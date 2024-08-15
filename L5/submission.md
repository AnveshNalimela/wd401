##  Dockerizing the application

Create the Dockerfile in root directory of project
```Dockerfile
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


docker  --version
docker ps



```
docker build -t sportbuzz-app-v2 .
```

```
docker run -p 5000:5000 wd301-sportbuzz-app
```

```
docker compose up
```
docker login
'''docker 

docker ps


docker images


docker tag sportbuzz-app-v2 anvesh269/sportbuzz-app:latest

docker push anvesh269/sportbuzz-app:latest
