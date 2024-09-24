---
title: "Docker Demystified: Your Easy Guide to Containers, Builds, Volumes & Networking"
seoTitle: "Docker: Containers, Builds, Volumes, Networking"
seoDescription: "Learn Docker: A beginner's guide to containers, builds, volumes, networking, and multistage Dockerfile examples"
datePublished: Tue Sep 24 2024 13:51:56 GMT+0000 (Coordinated Universal Time)
cuid: cm1ghwebc00010akybwn3eub0
slug: docker-demystified-your-easy-guide-to-containers-builds-volumes-networking
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1726904604733/bcfbb4d7-3058-455b-ad66-07aa17b9f31a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1727185886874/cc5434c8-e811-40b5-b1bb-b8135d8db2bc.png
tags: docker, docker-compose, docker-images

---

## Introduction

On every YouTube channel or blog, you often hear that Docker is a container. Yes, that's true. But what is a container? It is like a box that can hold many things, such as different fruits, vegetables, or even books. In the case of software, it's like having applications in a sandbox that are secure and have the same environment as they would on a local host. Just like apples and oranges are stored in cartons to increase their shelf life, software in containers is protected and secured from harmful attacks.

For developers, a container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings.

## Architecture of Docker :

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726904105411/a9380cfe-e2c6-4a93-a532-dc61c1da3e61.png align="center")

So, the next step is how to start with Docker.

Docker has three main stages: build, run, and push.

1. `docker build` - builds Docker images from a Dockerfile.
    
2. `docker run` - runs a container from Docker images.
    
3. `docker push` - pushes the container image to public or private registries to share the Docker images.
    

You can use `docker run` directly, which will install and run in two steps, but it's better to do it separately.

### **Here are some terms related to Docker that everyone should know:**

**Docker Client**: The Docker client (`docker`) is a command-line tool that allows users to interact with the Docker daemon. When you run commands like `docker build`, `docker run`, or `docker push`, the client sends these commands to the Docker daemon, which then executes them.

**Docker Daemon**: The Docker daemon (`dockerd`) is a background service running on the host machine. It listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. The daemon communicates with other daemons to manage Docker services.

**Docker Desktop**: An easy-to-install application for Mac, Windows, or Linux that lets you build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.

**Docker Images:** Docker images are read-only templates used to create containers. They contain the application code, libraries, dependencies, and other necessary files to run the application. Images are built from a set of instructions written in a Dockerfile, which specifies the base image, the software to be installed, and the commands to be run.

**Docker Container:** A Docker container is a runnable instance of a Docker image. Containers are lightweight and portable, providing an isolated environment for applications to run. They share the host system's kernel but have their own filesystem, network interfaces, and process space, ensuring that applications run consistently across different environments.

**Docker Registries:** Docker registries are storage and distribution systems for Docker images. They allow users to store, share, and manage Docker images. Public registries like Docker Hub are available for anyone to use, while private registries can be set up for more controlled access. Registries enable developers to push and pull images, facilitating collaboration and deployment.

**Union File System (UnionFS)**: Docker uses UnionFS to create layers in a Docker image. UnionFS allows files and directories of separate file systems (layers) to be transparently overlaid, forming a single coherent file system. This makes images lightweight, as common layers can be shared between multiple images.

**Namespaces**: Docker uses namespaces to provide isolated workspaces called containers. When you run a container, Docker creates a set of namespaces for that container. These namespaces provide a layer of isolation, ensuring that processes in one container cannot see or affect processes in another container.

**Control Groups (cgroups)**: Docker uses cgroups to limit and allocate resources such as CPU, memory, and I/O for containers. Cgroups ensure that containers do not exceed their allocated resources and that the host system remains stable and responsive.Install Docker on your laptop.

To install Docker on your laptop, follow these steps:

1. **Visit the Docker Website**: Go to the official Docker website at [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop).
    
2. **Download Docker Desktop**: Choose the appropriate version for your operating system (Windows, macOS, or Linux) and download the installer.
    
3. **Run the Installer**:
    
    * **Windows**: Double-click the downloaded `.exe` file and follow the installation instructions.
        
    * **macOS**: Open the downloaded `.dmg` file and drag the Docker icon to the Applications folder.
        
    * **Linux**: Follow the specific instructions provided on the Docker website for your Linux distribution.
        
4. **Start Docker Desktop**: Once installed, launch Docker Desktop from your applications menu or start menu.
    
5. **Complete the Setup**: Follow any additional setup instructions provided by Docker Desktop, such as signing in with your Docker Hub account.
    
6. **Verify Installation**: Open a terminal or command prompt and run the command `docker --version` to ensure Docker is installed correctly.
    

By following these steps, you can successfully install Docker on your laptop and start using it for containerized applications.

**Docker Example Project: Website Container**

**Project Structure**

```bash
website-docker/
|—— Dockerfile
|—— index.html
|—— server.js
|—— package.json
```

**Files Explanation**

* **Dockerfile**: contains instructions for building a Docker image
    
* **index.html**: a simple HTML file
    
* **server.js**: a Node.js server that serves the HTML file
    
* **package.json**: specifies the dependencies for the Node.js application
    

**Dockerfile**

```dockerfile
# Use an official Node.js 14 image as the base
FROM node:14

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in package.json
RUN npm install

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Define environment variable
ENV NAME World

# Run app.js when the container launches
CMD ["node", "server.js"]
```

Certainly! Here's a clear, concise, and readable explanation of the provided Docker file:

1. `FROM node:14`: This line specifies the base image to be used for the Docker container. In this case, it's using the official Node.js 14 image.
    
2. `WORKDIR /app`: This sets the working directory inside the container to `/app`. All subsequent commands will be executed in this directory.
    
3. `COPY . /app`: This command copies the current directory contents (the source code) into the `/app` directory inside the container.
    
4. `RUN npm install`: This runs the `npm install` command to install the dependencies specified in the `package.json` file.
    
5. `EXPOSE 8080`: This line informs Docker that the container will listen on port 8080 at runtime.
    
6. `ENV NAME World`: This sets the environment variable `NAME` with the value `World`.
    
7. `CMD ["node", "server.js"]`: This line specifies the command that will be executed when the container is run. In this case, it runs the `server.js` file using the Node.js runtime.
    

In summary, this Docker file:

1. Starts with the official Node.js 14 image as the base.
    
2. Sets the working directory to `/app`.
    
3. Copies the current directory contents (the source code) into the container.
    
4. Installs the dependencies specified in the `package.json` file.
    
5. Exposes port 8080 for the application to listen on.
    
6. Sets an environment variable named `NAME` with the value `World`.
    
7. Runs the `server.js` file when the container is started.
    

This Docker file can be used to build a Docker image that contains a Node.js application, and the resulting image can be run as a container to deploy the application.

**index.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Webpage</title>
</head>
<body>
    <h1>Hello World</h1>
    <p>My name is ${NAME}</p>
</body>
</html>
```

**server.js**

```javascript
const http = require('http');
const fs = require('fs');

const HOST = '0.0.0.0';
const PORT = 8080;

const server = http.createServer((req, res) => {
    if (req.url === '/') {
        const index = fs.readFileSync('index.html', 'utf8');
        res.write(index.replace('${NAME}', process.env.NAME));
        res.end();
    }
});

server.listen(PORT, HOST);

console.log(`Server running on http://${HOST}:${PORT}`);
```

**package.json**

```json
{
    "name": "website-docker",
    "version": "1.0.0",
    "description": "",
    "main": "server.js",
    "scripts": {
        "start": "node server.js"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {}
}
```

**Build the Docker Image**

```bash
docker build -t website-docker .
```

**Run the Docker Container**

```bash
docker run -d --name website-docker -p 8080:8080 website-docker
```

**Verify the Website**

Open your web browser and navigate to [`http://localhost:8080`](http://localhost:8080). You should see the webpage with the title "My Webpage" and a paragraph with the text "My name is World".

## Multistage Docker :

**Need of multistage docker**

As projects or services grow larger with thousands of microservices and lots of data, the size of the Docker container can reach several gigabytes. This is not ideal from a business perspective because it can lead to significant data loss if something goes wrong, and it also increases costs. Additionally, many libraries are copied repeatedly to run the container. For example, if the application is built on Node.js and is a microservice, there will be a lot of redundant data for the Node modules across all services. This is not efficient. Moreover, there is a security risk. To fix this, we use multistage Docker.

By leveraging multistage Docker builds, you can create optimized, secure, and maintainable Docker images that are well-suited for modern, complex applications.

This can be done in three ways:

1. **Using Multiple FROM Statements**: In a multistage Docker build, you can use multiple `FROM` statements in your Dockerfile. Each `FROM` statement starts a new build stage. You can selectively copy artifacts from one stage to another, which helps in reducing the final image size by only including necessary files.
    
2. **Minimizing Layers**: By combining multiple commands into a single `RUN` statement, you can minimize the number of layers in your Docker image. This helps in reducing the image size and improving build performance.
    
3. **Using Build Arguments**: Build arguments (`ARG`) can be used to pass variables at build time. This allows you to customize the build process and create different variations of the same image without duplicating the Dockerfile.
    

By following these practices, you can create efficient and secure Docker images that are easier to maintain and deploy.

**Example of Multistage Dockerfile**

Here is an example of a multistage Dockerfile for a Node.js application:

```Dockerfile
# Stage 1: Build the application
FROM node:14 AS build

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install the dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Create the production image
FROM node:14-alpine

# Set the working directory
WORKDIR /app

# Copy the built application from the build stage
COPY --from=build /app/dist ./dist

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install only production dependencies
RUN npm install --only=production

# Expose the application port
EXPOSE 8080

# Set the environment variable
ENV NODE_ENV=production

# Start the application
CMD ["node", "dist/server.js"]
```

### Explanation:

1. **Stage 1: Build the application**
    
    * `FROM node:14 AS build`: Uses the official Node.js 14 image as the base for the build stage.
        
    * `WORKDIR /app`: Sets the working directory inside the container to `/app`.
        
    * `COPY package*.json ./`: Copies the `package.json` and `package-lock.json` files to the working directory.
        
    * `RUN npm install`: Installs the dependencies specified in the `package.json` file.
        
    * `COPY . .`: Copies the rest of the application code to the working directory.
        
    * `RUN npm run build`: Builds the application.
        
2. **Stage 2: Create the production image**
    
    * `FROM node:14-alpine`: Uses the official Node.js 14 Alpine image as the base for the production stage.
        
    * `WORKDIR /app`: Sets the working directory inside the container to `/app`.
        
    * `COPY --from=build /app/dist ./dist`: Copies the built application from the build stage to the production stage.
        
    * `COPY package*.json ./`: Copies the `package.json` and `package-lock.json` files to the working directory.
        
    * `RUN npm install --only=production`: Installs only the production dependencies.
        
    * `EXPOSE 8080`: Exposes port 8080 for the application.
        
    * `ENV NODE_ENV=production`: Sets the environment variable `NODE_ENV` to `production`.
        
    * `CMD ["node", "dist/server.js"]`: Specifies the command to start the application.
        

This multistage Dockerfile helps in creating a smaller, optimized production image by separating the build and runtime environments.

One can also decrease the size of the image by using the operating system scratch. This approach allows for a minimal base image, often as small as 2 KB, which contains only the essential components. By doing this, we can add only the specific libraries and dependencies that are required for our application, avoiding any unnecessary bloat. This method not only reduces the overall image size but also improves security by minimizing the attack surface. Additionally, it can lead to faster deployment times and more efficient use of resources.

One can also decrease the size of the images by using multiple copy commands combined into a single command. This technique helps in reducing the number of layers in the Docker image, which in turn minimizes the overall image size. By consolidating the copy operations, we can streamline the Dockerfile, making it more efficient and easier to maintain. Additionally, this approach can lead to faster build times, as fewer layers mean less overhead during the image creation process. Combining copy commands is a simple yet effective optimization that contributes to a leaner and more performant Docker image.

## Docker Volumes and Build mounts

In the past, there was no way to access the operating system's files. If the front end or back end went down, data would be lost and unreadable. Sometimes, containers might fail or be deleted by the system because the OS might kill them to optimize resource usage. As a result, the data stored by the containers could be lost. To prevent this, data needs to be stored persistently, even if a container fails. This is where **volumes** come in. Volumes are created by Docker on the **host** and are mounted on the containers. They can be shared between containers as needed. They are similar to bind mounts but have a better lifecycle. They can be managed using the Docker CLI and can be connected to external sources as well.

They are managed using these commands:

```sh
docker volume create name
docker volume run name
docker volume inspect name
```

**Build mount**: Binding a specific directory on the host to a specific directory in the container.

```bash
docker run -d --mount source=sourcename,target=/app/nginx
```

Docker automatically creates space for volumes if you create them without specifying the exact size needed. Therefore, it is a good practice to delete the volume after destroying the container.

## Docker Networking

Networking in Docker allows containers to communicate with each other and the host system. Container networking provides isolation and multiple solutions.

* Containers can communicate with each other.
    
* Containers are isolated from each other.
    

Docker creates a virtual ethernet for containers to communicate with the host.

* By default, Docker creates 'Bridge networking' for container and host communication. Without this bridge networking (virtual ethernet), containers cannot be accessed by users or clients.
    

### Security Concerns with Complete Operating Systems and Common Networking in Containers:

* Using a complete operating system in a container can lead to package issues unrelated to the application being used.
    
* Common networking, like host networking, poses security risks in Docker, prompting the consideration of overlay networking for improved security in container orchestration scenarios.
    

## Docker Compose

It is a tool created by Docker Inc. for building multi-container applications. In microservices, the goal is not just to run Docker services but to run them in the right order based on the need. If the database is available, then we build the database; otherwise, there is no need to create it, saving resources and costs.

In Docker Compose, if there is a change in the container or the business domain, then every developer working on the service has to take it into consideration and fix the bug. As the application grows, complexity increases, which in turn raises the chances of errors. Hence, it is not a good option from a business perspective. To address this, Docker Compose comes into play. We can write every configuration in a compose file and change it according to the requirements. It is very easy to write:

```bash
docker compose up
```

Though we still have to write the Docker files, they can be managed by a single configuration file.

**docker-compose.yaml**

```bash
version: '3'
services:
  web:
    image: my-web-app:latest
    ports:
      - "8080:8080"
    depends_on:
      - db
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
```

### **Explanation:**

1. **version: '3'**: Specifies the version of the Docker Compose file format.
    
2. **services**: Defines the services that make up the application.
    
    * **web**: The web service.
        
        * **image**: Specifies the Docker image to use for the web service.
            
        * **ports**: Maps port 8080 on the host to port 8080 in the container.
            
        * **depends\_on**: Specifies that the web service depends on the db service.
            
    * **db**: The database service.
        
        * **image**: Specifies the Docker image to use for the database service.
            
        * **environment**: Sets environment variables for the database service.
            

Though we still have to write the Docker files, they can be managed by a single configuration file.

## Docker init

Initialize a project with all the files you need to run it in a container—how exciting is that?

Docker Desktop brings you the awesome `docker init` CLI command! Just run `docker init` in your project directory, and it will create these essential files with smart defaults for your project:

* .dockerignore
    
* Dockerfile
    
* compose.yaml
    
* README.Docker.md
    

If any of these files already exist, no worries! A prompt will appear, letting you know and giving you the option to overwrite them. If you have a `docker-compose.yaml` file instead of `compose.yaml`, `docker init` can overwrite it while keeping the name `docker-compose.yaml`.

After running `docker init`, get ready to choose from these amazing templates:

* ASP.NET Core: Perfect for an ASP.NET Core application.
    
* Go: Ideal for a Go server application.
    
* Java: Great for a Java application using Maven and packaged as an uber jar.
    
* Node: Awesome for a Node server application.
    
* PHP with Apache: Fantastic for a PHP web application.
    
* Python: Superb for a Python server application.
    
* Rust: Excellent for a Rust server application.
    
* Other: A versatile starting point for containerizing your application.
    

Once `docker init` is done, you might need to tweak the created files to perfectly fit your project. Check out the following links to learn more about these files:

* [.dockerignore](https://docs.docker.com/reference/dockerfile/#dockerignore-file)
    
* [Dockerfile](https://docs.docker.com/reference/dockerfile/)
    
* [compose.yaml](https://docs.docker.com/compose/intro/compose-application-model/)[**Options**](https://docs.docker.com/reference/cli/docker/init/#options)
    

### [**Example of running** `docker init`](https://docs.docker.com/reference/cli/docker/init/#example-of-running-docker-init)

The following example shows the initial menu after running `docker init`. See the additional examples to view the options for each language or framework.

```bash
 docker init

Welcome to the Docker Init CLI!

This utility will walk you through creating the following files with sensible defaults for your project:
  - .dockerignore
  - Dockerfile
  - compose.yaml
  - README.Docker.md

Let's get started!

? What application platform does your project use?  [Use arrows to move, type to filter]
> PHP with Apache - (detected) suitable for a PHP web application
  Go - suitable for a Go server application
  Java - suitable for a Java application that uses Maven and packages as an uber jar
  Python - suitable for a Python server application
  Node - suitable for a Node server application
  Rust - suitable for a Rust server application
  ASP.NET Core - suitable for an ASP.NET Core application
  Other - general purpose starting point for containerizing your application
  Don't see something you need? Let us know!
  Quit
```

## Conclusion

Understanding the basics of containers and Docker is crucial for modern DevOps. This guide gives beginners the knowledge to containerize applications, optimize Docker images, manage networking and storage, and get ready for advanced topics like Kubernetes. By following these principles and best practices, DevOps engineers can achieve efficient, secure, and scalable application deployments.