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

Challenge 3
Uses Docker, NGINX, mariadb, and Node.js to create a dynamic application and a "docker-compose.yml" file that is responsible for the web server and creating the application.

1.	Click on this link to open the GitHub repository that contains the Docker challenge template.

2.	Click on the fork button to create a fork(copy) of the repository.
Note: You can opt to download the repository as a .zip file, and just extract the necessary files onto your challenge 1 and 2 repository, as forked repositories are not able to be made private.

3.	If you are not using the same repository used for challenges 1 and 2, create a folder on your system wherever you would like to clone the repository to.

4.	Open the command prompt to where you would like to clone the repository and type ‘git clone <link to GitHub repository>’ to create a clone of the forked repository, if you are not using the old repository.

5.	Open Visual Studio Code and open the root directory folder for the docker-challenge-template-main.

6.	Download the ‘challenge3.zip’ provided by your instructor and extract the contents into the “challenge3” folder in the docker challenge repository.

7.	In the ‘challenge3’ folder, create a file labeled ‘.env’ and input the following lines to create the environment variables used for the challenge.

```
DB_ROOT_PASSWORD=<password>
DB_DATABASE=<database>
DB_USERNAME=<username>
DB_HOST=<host>
MYSQL_ROOT_PASSWORD=<password>
MYSQL_DATABASE=<database>
MYSQL_USER=<user>
MYSQL_PASSWORD=<password>
MYSQL_HOST=<host>
```

Note: You can input what you like for the passwords and usernames. For both hosts, simply input ‘db’. For more information, click here.

8.	In the ‘challenge3’ folder, create a file labeled ‘docker-compose.yml’ and configure it accordingly to use NGINX, node-service, and db services.
Note: This builds a container, using the context, for the database, API, and NGINX, maps port 3000 and 80 of the containers to 3000 and 8080 respectively, and uses the environment variables that we setup in Step 7.

9.	Navigate to the ‘challenge3’ folder in the File Explorer and type ‘cmd’ in the address bar to open the command prompt from this location.

10.	On the command prompt, enter the following command to start the build for the challenge3 application; ‘docker-compose up –build’.

11.	The build will begin and shortly complete in the command prompt terminal.
Note: If you encounter any errors during the building process, ensure the paths in docker files correctly line up with the actual path of the files. If issues persist, please refer to the proper documentation to handle the errors.

12.	Once the build is completed, on a web browser, head to http://localhost.8080/api/books/, and then http://localhost.8080/api/books/1/. If done correctly, you should see the full list of books, and the first book on the second link.

13.	To check the services that are running, and their status, enter ‘docker-compose ps’ in a new command prompt terminal, in the ‘challenge3’ directory.

Challenge 4
Scales up the application from challenge 3

1.	Using the application from challenge 3, while it is running, head to http://localhost.8080/api/stats/ to make a GET request. 

2.	If done correctly, you should see “success” next to the status, contents containing the information about free and available memory, a PID, hostname, and counter.

3.	In the command prompt terminal that is running the services, press ‘CTRL+C’ to stop running the services.

4.	After the build has successfully stopped running, enter this command into the command prompt; ‘docker-compose up –build –scale node-service=3’
Note: This will scale the node-service instances to 3.

5.	After the build is completed, refresh the page in the web browser, or head to the address from Step 1 again. If done correctly, you should see the same success message but with a different host name, contents, and counter.

6.	In a different command prompt, enter this command; ‘docker-compose ps’ to view the services running, and notice how it differs from challenge 3 by having three node-services running.