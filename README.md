# Node-Express App on Docker

Sample NODE.js application deployed in docker in minimalistic manner

## Getting Started

Why should I dockerize my application

You might've heard about the whole Docker thing, but that still doesn't answer the question: "why bother?" Well, that's why:

You can launch a fully capable development environment on any computer supporting Docker; you don't have to install libraries, dependencies, download packages, mess with config files etc.
The working environment of the application remains consistent across the whole workflow. This means the app runs exactly the same for developer, tester, and client, be it on development, staging or production server.
In short, Docker is the counter-measure for the age-old response in the software development: "Strange, it works for me!"

### Prerequisites

Install Node.js

If you've never worked with Node.js before, kick off with installing the npm manager: nodejs.org/en/download/package-manager

Install NPM and Express Framework

```
$ mkdir helloworld
$ cd helloworld
$ npm init
```

### Installing

```
$ npm install express --save
```

*The file should look like this now:*
```
{
  "name": "helloworld",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.2"
  }
}
```

*Details of Hello World

With everything installed, we can create an index.js file with a simple HTTP server that will serve our Hello World website:*

```
//Load express module with `require` directive
var express = require('express')
var app = express()

//Define request response in root URL (/)
app.get('/', function (req, res) {
  res.send('Hello World!')
})

//Launch listening server on port 8081
app.listen(8081, function () {
  console.log('app listening on port 8081!')
})
```
*The application is ready to launch:*

```
$ node index.js
```

### Write Dockerfile

## Line 1: Use another Docker image for the template of my image. We shall use the official Node.js image with Node v7.
## Line 2: Set working dir in the container to /app. We shall use this directory to store files, run npm, and launch our application:
## Copy application to /app directory and install dependencies. If you add the package.json first and run npm install later, Docker won't have to install the dependencies again if you change the package.json file. This results from the way the Docker image is being built (layers and cache), and this is what we should do:

```
FROM node:7
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
CMD node index.js
EXPOSE 8081
```

### Build Docker image

With the instructions ready all that remains is to run the docker build command, set the name of our image with -t parameter, and choose the directory with the Dockerfile:

```
$ docker build -t hello-world .
```

### Run Docker container

The application has been baked into the image. Dinner time! Execute the following string to launch the container and publish it on the host with the same port 8081:

```
docker run -p 8081:8081 hello-world
```

