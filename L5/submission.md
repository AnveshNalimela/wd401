##  Dockerizing the application

First of all,Install the doccker in your machine. verify the installation using the command in terminal
```bash
docker --version
```
Create an docker and login into the acccount through terimnal
```bash
docker login
```

![image](https://github.com/user-attachments/assets/6f50b353-b61c-4529-a90c-43e640b4022c)

Create the Dockerfile in root directory of project.

```Dockerfile
# Stage 1: Build the application using Node.js
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# Build the application using Vite
RUN npm run build

# Stage 2: Serve the built application using a lightweight web server
FROM node:18-alpine AS production

# Install `serve` to serve the build directory
RUN npm install -g serve

# Set the working directory inside the container
WORKDIR /app

# Copy the build output from the previous stage
COPY --from=build /app/dist ./dist

# copy env varibales
COPY .env .

# Expose the port that `serve` will use
EXPOSE ${PORT:-5000}

# Start the server
CMD serve -s dist -l ${PORT:-5000}

```

#### Stage 1: Build the application
#### Base Image: 
Using node:18-alpine, which is a lightweight version of Node.js based on Alpine Linux.
#### Working Directory: 
Set to /app inside the container.
#### Dependency Installation: 
Copies package.json and package-lock.json and runs npm install to install the dependencies.
#### Copy Application Code:
Copies the entire application code to the working directory.
#### Build: 
Runs the build command using Vite to generate the production-ready files.

### Stage 2: Serve the built application
#### Base Image:
Again, using node:18-alpine.
#### Serve Installation: 
Installs the serve package globally, which is used to serve static files.
#### Working Directory:
Set to /app.
#### Copy Build Output: 
Copies the built files from the first stage.
#### Environment Variables:
Copies the .env file if any environment-specific configurations are needed.
### Expose Port: 
The port from the environment variable PORT, with a default of 5000, is exposed.
#### Start the Server:
The application is served using serve -s dist -l ${PORT}


#### Building the Docker Image
First, ensure you have your Dockerfile ready in the root of your project directory. 
Then, you can build the Docker image using the docker build command.
```bash
docker build -t sportbuzz-image:latest .
```
![image](https://github.com/user-attachments/assets/2b1dd4db-2712-4d8a-b9b1-9ba34e65b383)

#### Running the Docker Image
Once the image is built, you can run it locally to test if everything works as expected.


```bash
docker run -p 5000:5000 sportbuzz-image:latest
```
![image](https://github.com/user-attachments/assets/1b3aee7d-3e0d-4a61-ae6c-2d8580ef617d)

Verify the proper working by visiting the http://localhost:5000

![image](https://github.com/user-attachments/assets/04c37280-6dbd-482d-b959-9123558e8b35)

#### list of images
we can view the list of images built using the command
```bash
docker images
```
![image](https://github.com/user-attachments/assets/b7801f11-dddd-46b9-80fc-e2707d2ca624)

### Docker-compose
Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to manage multiple containers with a single configuration file (docker-compose.yml)
In our case a single service is required and configured in compose file
```docker-compose.yml
version: "3.8"
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "${PORT:-5000}:${PORT:-5000}"
    environment:
      - VITE_API_ENDPOINT=${VITE_API_ENDPOINT}
      - PORT=${PORT:-5000}
    env_file:
      - .env

```

#### Starting Services
To start all the services defined in your docker-compose.yml file, use:

```bash
docker-compose up
```
![image](https://github.com/user-attachments/assets/bfb688e3-a0ed-4dda-b2be-94807b3f0359)

#### Stopping Services
To stop all running services:

```bash
docker-compose down
```

#### Tagging the Docker Image
If we want to tag your Docker image with another tag, we can use the docker tag command.

```bash
docker tag sportbuzz-image anvesh269/sportbuzz-app:v1.0
```
### Pushing the Docker Image to Docker Hub
Finally, push our tagged Docker image to Docker Hub.

```bash
docker push anvesh269/sportbuzz-app:latest
```

![image](https://github.com/user-attachments/assets/8ad3f24b-fccd-4362-a895-ebac713504af)
