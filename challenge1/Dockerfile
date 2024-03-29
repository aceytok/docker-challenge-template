FROM nginx:latest
COPY public/index.html /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]


# Use the official NGINX image as the base image
# Copy custom NGINX configuration file to the container
# Copy static website files to the container's default HTML directory
# Expose port 80
# Start NGINX when the container starts~