# STEP 1: Creating simple nodejs hello world app

Assuming that nodejs installed.

## Installation

create new directory and initialize npm:

```bash
mkdir nodejs-hello
cd nodejs-hello
npm init
```
confirm default values with enter. 

npm will create a package.json which holds the dependencies of our app and following code will add express framework as a dependency.

```bash
npm install express --save
```
### creating index.js with simple http server that will serve on port 8080

```nodejs
//now it load express module with `require` directive
var express = require('express')
var app = express()

//Define request response in root URL (/) and response with text Hello World!
app.get('/', function (req, res) {
  res.send('Hello World!')
})

//Launch listening server on port 8080 and consoles the log.
app.listen(8080, function () {
  console.log('app listening on port 8080!')
})
```
### run the app

```bash
node index.js
```
this is show the log as that app listening on port 8080. You can stop it with CTRL + C. And check out http://localhost:8080/ in your browser, it will response Hello World!. Alternatively if you live in terminal you can do:

```bash
curl http://localhost:8080/
```
which will return same Hello World! in the terminal 


## STEP 2: dockerize our app

lets assume docker is installed 

### create Dockerfile to root directory

```docker
## it uses node js image alpine version from image registries.
FROM node:alpine
## it sets directory in the container to /app to store files and launch our app.
WORKDIR /app
## it copies the app to /app directory with dependencies.
COPY package.json /app
RUN npm install
COPY . /app
## it commands to run our app which is index.js.
CMD node index.js
##  it exposes the port where our app is running that is port 8080.
EXPOSE 8080
```

### to build this image
```docker
docker build -t web .
```

## STEP 3: using docker compose
```docker
version: '3' # version of docker-compose

services: # defining service/s
  web: # name of the service
    image: nodejs/hello # image named given
    build: . # directory what to build, here it is root directory
    ports:
      - "8080:8080" # defining port for our app
```

### how to run docker compose
```bash
docker-compose up ## {-d for detached mode or background}
```
or for new fresh build use this
```bash
docker-compose up --build ## {--detach for running in background}
```

to remove the container
```bash
docker-compose rm
```