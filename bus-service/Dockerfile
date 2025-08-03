# Use official nginx image as the base image
FROM nginx:alpine

# Copy the HTML file to the nginx web server directory
COPY index.html /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Command to run nginx
CMD ["nginx", "-g", "daemon off;"]
