# Docker Challenge

Challenges were completed on Windows 11.

Challenge 1
Uses Docker and NGINX to host a simple web server that serves static web pages.

1. Create a folder labeled "public" in the "challenge1" folder.
2. In the "public" folder, create an "index.html" file.
3. Using appropriate HTML tags, put your name and student ID into a <DIV>.
4. Create a file labeled "DockerFile" into the root "challenge1" directory.
5. In the DockerFile, add the following commands:
```
        FROM nginx:latest
        COPY public/index.html /usr/share/nginx/html
        EXPOSE 80
        CMD ["nginx", "-g", "daemon off;"]
```
    This uses the latest version of the NGINX image, copies the contents of "index.html" to NGINX's default HTML directory, exposes port 80, and runs NGINX when the container starts.

6. In the command prompt, change directories to the "challenge1" folder and run the command "docker build -t {Tag Name} ." with a tag name that you choose.
7. On Docker desktop, go to "Images" and click on the play button of the newly created image.
8. On the popup window, click on the drop-down menu for "Optional Settings" and set the "Host Port" to 8080 (a container name if desired) and click "Run".
9. On a web browser, go to http://localhost.8080/ and if done correctly, you should see your name and student ID being displayed.

Challenge 2
Uses Docker, NGINX, and Node.js to create a dynamic application and a "docker-compose.yml" file that is responsible for the web server and creating the application.

1. Download the "challenge2.zip" folder provided by your instructor.
2. Extract the files and copy them into the "challenge2" folder of the project.
3. In the "challenge2" folder, create a file labeled "DockerFile" and add the following commands:

```
        FROM node:latest
        WORKDIR /
        COPY package*.json ./
        RUN npm install
        COPY . .
        EXPOSE 3000
        CMD ["node", "server.js"]
```

        This uses the latest version of the Node.js image, the working directory is set to the root directory, copies the "package.json" and "package-lock.json" to the working directory, runs "npm install" inside the container to install required dependencies, copies all files and directories from the project to the Docker container's working directory, exposes port 3000, and runs "node" and "server.js" when the container starts.

4. Create another file labeled "docker-compose.yml" and add the following commands:

```
        version: '3.8'

        services:
        nginx:
            image: nginx:latest
            ports:
            - "8080:80"
            depends_on:
            - api
            networks:
            - app-network

        api:
            build: ./
            ports:
            - "8080:3000"
            networks:
            - app-network
            environment:
            - PORT=3000

        networks:
        app-network:
            driver: bridge
```

    This uses the latest version of NGINX Docker image and maps the port 80 of the NGINX container to 8080 on the local host, as well as sets the services dependencies.
    The API uses the current directory to build the Docker image, using the "Dockerfile", maps port 3000 to port 8080, and set an environment variable to 3000, which the API will be listening on for incoming requests.
    
5. On the command prompt, in the "challenge2" directory, run the command "docker-compose up --build" to build the Docker image.
6. On Docker desktop, go to "Images" and click on the play button for the newly created image.
7. On the popup window, click on the drop-down menu for "Optional Settings" and changes the "Host Port" to 8080 (and a container name if desired) and click on "Run".
8. On a web browser, go to http://localhost:8080/api/books/, and if done correctly, you should see the full list of books being displayed.
9. Go to http://localhost:8080/api/books/1/, and if done correctly, you should see the first book being displayed.