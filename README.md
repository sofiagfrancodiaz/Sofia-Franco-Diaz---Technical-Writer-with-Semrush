# Guide to Deploying a Python Application with Docker and Kubernetes 

In this tutorial, you'll learn how to containerize a Python application using Docker and then prepare it for deployment in a Kubernetes cluster. We'll start from the basics, including setting up the necessary directory structure, creating the application, and writing a Dockerfile to package the application.

## Before you begin

Make sure you have Docker installed on your computer. If not, follow [this guide](https://docs.docker.com/get-docker/) to install Docker. 

## 1\. Set Up Your Project Directory Structure 

To keep your project organized, create a structured directory layout. This will help you manage and maintain your project files more effectively.

**Create the Project Directory**  
Create a new directory for your project by running the following command:

\`\`\` mkdir quickstart\_docker \`\`\` 

This creates a directory called "quickstart\_docker" that will serve as the root directory for your project.

**Create the Application and Docker Directories**  
Inside the "quickstart\_docker" directory, create the following subdirectories:

\`\`\` cd quickstart\_docker  
mkdir application  
mkdir docker  
mkdir docker/application \`\`\` 

The directory structure should now look like this:

quickstart\_docker/ \# Catalog of all project  
├──application/ \# Here code of the application  
└──docker/ \# Here some stuff for docker  
└──application/ \# …here Dockerfile for application

By creating this structured directory layout, you'll be able to better organize and manage the different components of your project as you continue to develop and expand it. 

## 2\. Write a Simple Python Application

**Navigate to the Application Directory**   
Navigate to the quickstart\_docker/application directory by running the following command:

\`\`\`bash cd /path/to/quickstart\_docker/application \`\`\` 

Replace /path/to/quickstart\_docker/ with the actual path to the "quickstart\_docker" directory on your file system.

**Create the Application File**  
In the quickstart\_docker/application directory, create a new file named application.py using a text editor, such as nano:

nano application.py 

**Add the Application Code**  
In the text editor, add the following Python code to the application.py file:

\`\`\` import http.server import socketserver   
PORT \= 8000 Handler \= http.server.SimpleHTTPRequestHandler httpd \= socketserver.TCPServer(("", PORT), Handler)   
print("serving at port", PORT) httpd.serve\_forever() \`\`\` 

**Note:** This script sets up a basic HTTP server that listens on port 8000\. 

Save the changes to the application.py file and exit the text editor.

## 3\. Create a Dockerfile to Containerize the Application 

In this step, you'll create a Dockerfile to build a Docker image for your Python application.

A Dockerfile is a script that contains a set of instructions for building a Docker image. The image can then be used to run your application in a containerized environment.

**Navigate to the Docker Application Directory**  
Navigate to the quickstart\_docker/docker/application directory by running the following command:

\`\`\`bash /path/to/quickstart\_docker/docker/application \`\`\` 

Replace /path/to/quickstart\_docker/ with the actual path to the "quickstart\_docker" directory on your file system.

**Create the Dockerfile**  
In the quickstart\_docker/docker/application directory, create a new file named Dockerfile using a text editor, such as nano:

\`\`\`nano Dockerfile \`\`\` 

**Add the Dockerfile Instructions**  
In the text editor, add the following instructions to the Dockerfile:

\`\`\` \# Use the official Python image from Docker Hub as the base image FROM python:3.5   
\# Set the working directory in the container WORKDIR /app   
\# Copy the application code into the container COPY ./application /app   
\# Expose port 8000 to the host machine EXPOSE 8000   
\# Define the command to run the application CMD \["python", "/app/application.py"\] \`\`\` 

Save the changes to the Dockerfile and exit the text editor.

**Note:** This Dockerfile uses a Python 3.5 base image, copies your application code into the container, and specifies the command to run the Python script when the container starts. 

## 4\. Build the Docker Image 

Now that you have created the Dockerfile, you can use it to build a Docker image for your Python application.

**Navigate to the Project Directory**  
Navigate to the quickstart\_docker directory by running the following command:

\`\`\`bash cd /path/to/quickstart\_docker \`\`\` 

Replace /path/to/quickstart\_docker/ with the actual path to the "quickstart\_docker" directory on your file system.

**Build the Docker Image**  
In the quickstart\_docker directory, run the following command to build the Docker image:

docker build . \-f docker/application/Dockerfile \-t exampleapp

Wait for the image building process to complete. Docker will download the necessary base image and apply the instructions specified in the Dockerfile to create the final image.

After the build process is finished, you should have a Docker image named exampleapp ready to be used.

## 5\. Verify the Docker Image 

After building the Docker image, you can verify that it was created successfully.

**List Docker Images**  
Run the following command to list all Docker images on your system:

\`\`\`bash docker images \`\`\` 

This command will display a table of all the Docker images currently available on your system.

**Inspect the Created Image**  
Look for the image you just created, which should be named exampleapp and have the latest tag.

The output should look similar to this:

\`\`\`REPOSITORY TAG IMAGE ID CREATED SIZE  
exampleapp latest 83ioe0edc28a 2 seconds ago 154MB  
python 3.6 05stv8636w3f 6 weeks ago 154MB\`\`\`

**Verify the Image Details**  
Inspect the details of the exampleapp image by running the following command:

 \`\`\` docker inspect exampleapp \`\`\` 

This will display a JSON-formatted output containing detailed information about the image, such as its layers, configuration, and metadata.

By verifying the image in the Docker images list and inspecting its details, you can confirm that the Docker image was created successfully based on the Dockerfile you created earlier.

## 6\. Push the Image to a Docker Repository 

If you need to deploy your application in a Kubernetes cluster, you'll need to push your Docker image to a Docker registry, such as Docker Hub or a private registry. This will make the image accessible to your Kubernetes cluster.

**Log In to Your Docker Account**  
Log in to your Docker Hub account by running the following command:

\`\`\`bash docker login \`\`\` 

**Tag the Docker Image**  
Tag your exampleapp image with your Docker Hub username and the latest tag:

\`\`\`bash docker tag exampleapp your\_dockerhub\_username/exampleapp:latest \`\`\` 

Replace your\_dockerhub\_username with your actual Docker Hub username.

**Note:** This command creates a new tagged version of the exampleapp image that can be pushed to Docker Hub.

**Push the Image to Docker Hub**  
Push the tagged exampleapp image to Docker Hub:

\`\`\`bash docker push your\_dockerhub\_username/exampleapp:latest \`\`\` 

Replace your\_dockerhub\_username with your actual Docker Hub username.

Docker will upload the image layers to Docker Hub, making the image available in your Docker Hub repository.  
After completing these steps, your exampleapp Docker image will be available in your Docker Hub account and can be pulled by your Kubernetes cluster for deployment.
