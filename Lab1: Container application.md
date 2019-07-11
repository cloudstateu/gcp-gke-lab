<img src="https://avatars1.githubusercontent.com/u/47143554?s=400&u=7c55eeec6479b4ff59df7cad452501a41635b0e4&v=4" alt="Cloudstate logo" width="200" align="right">
<br><br>
<br><br>
<br><br>

# Lab 1: Nodejs container application

## LAB Overview

#### This lab will demonstrate:


## Requirements
* Docker: https://www.docker.com/products/docker-desktop
* NodeJs current https://nodejs.org/en/

## TASK 1: Create NodeJS application and contenerized it.
1. Create new folder be-rtd
2. Create new nodejs procejct using command in terminal <code>npm init</code>
3. Provide project name be-rtd.
4. Install packages:
* Express packege <code>npm i -s express</code>
* Mongoose package <code>npm i -s mongoose</code>
* Socket.io <code>npm i -s socket.io</code>
* Http <code>npm i -s http</code>
5. Create file index.js
6. Import libraries on top of the file:
```
var mongoose = require('mongoose');
var express = require('express');
var bodyParser  = require( 'body-parser');
```
7. Add code to read environment variables:
```
var password = process.env.DB_PASSWORD;
var username = process.env.DB_USERNAME;
var dbUrl = "mongodb://"+username+':'+password+'@'+process.env.DBURL;;
console.log("DB url",dbUrl);
var port = 3000;
var timeout = process.env.TIMEOUT;
```
8. Initialize server:
```
var app = express();
var server = app.listen(port, () => {
    console.log('server is running on port', server.address().port);
});
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
var http  = require('http').Server(app);
var io = require('socket.io')(http);
io.attach(server);
```
9. Connect to MongoDB:
```
mongoose.connect(dbUrl);
```
10. Define MongoDB metrics entity model:
```
var Metrics = mongoose.model('Metrics', { a1: Number, b1: Number, Time: Date });

```
11. Define API call:
```
app.get('/metrics', (req, res) => {
    Metrics.find({}, (err, messages) => {
        res.send(messages);
    });
});
```
12. Define socket IO connection:
```
var client = [];
io.on('connection', (socket) => {
    client.push(socket);
    console.log('Clent connected', socket);
});
```
13. Initialize data generation:
```
initializeDataGeneration(timeout,client);

//Message generation mechanism
function initializeDataGeneration(timeout, socketClient) {
    setTimeout(() => {
        var date = new Date();
        var message = {
            a1: Math.random() * 200 + 100,
            b1: Math.random() * 10 + 230,
            Time: date.getTime()
        }
        var metrics = new Metrics(message, socketClient);
        metrics.save((err) => {
            if (err) {
                console.log('Error occured', error);
                sendDataToClient({ msgType: 'error', error: err }, socketClient);
            } else
                sendDataToClient({ msgType: 'metrics', data: message }, socketClient);
        });
        initializeDataGeneration(timeout,socketClient);
    }, timeout);
}
// Send message to client.
function sendDataToClient(message, client) {
    if (client) {
        client.forEach(element => {
            element.emit('message',JSON.stringify(message));
            console.log('Message sended to client',client.length, message);
        });
        console.log('No client connected');
    } else
        console.log('No client connected');
}
```
14. Create Dockerfile in your app directory.
15. Insert to file commands:
* Set base image: <code>FROM node:10</code>
* Set working directory: <code>WORKDIR /usr/src/app<code>
* Copy package.json: <code>COPY package*.json ./</code>
* Install dependencies: <code>RUN npm install</code>
* Bundel app file: <code>COPY . .</code>
    
* Create env for MongoDB connection string: 
```
ENV DB_PASSWORD 'secret'
ENV DB_USERNAME 'admin'
ENV DBURL 'localhost:27017'
```
* Create env for event generation time: <code>ENV TIMEOUT 5000</code>
* Expose port 3000: <code>EXPOSE 3000</code>
* Run main proces:<code>ENTRYPOINT [ "node", "index.js" ]</code>
16. Build container app typing in console: 
* <code>docker build -t berealtime .</code>
17. Pull image of MongoDB from dockerhub:
* <code>docker pull mongo</code>
18. Run image with command:
* <code>docker run -e MONGO_INITDB_ROOT_PASSWORD=secret -e MONGO_INITDB_ROOT_USERNAME=admin -p 27017:27017 mongo </code>
19. Run whole application using command:
* <code>docker run -e DB_PASSWORD='secret' -e DB_USERNAME='admin' -e  DBURL='localhost:27017' -e TIMEOUT=5000 -p 3000:3000 berealtime</code>


## TASK 2: Create Container Repository and push image
1. Open terminal.
2. Run command: <code>gcloud auth configure-docker</code>
3. Tag image with repository by typing in console: 
* <code>docker tag berealtime *[HOSTNAME]/[PROJECT-ID]/*/berealtime </code>
List of the hosts:
* <code>gcr.io hosts</code> the images in the United States, but the location may change in the future
* <code>us.gcr.io hosts</code> the image in the United States, in a separate storage bucket from images hosted by gcr.io
* <code>eu.gcr.io hosts</code> the images in the European Union
* <code>asia.gcr.io</code> hosts the images in Asia
8. Push image to repository by typing in console: 
* <code> docker push *[HOSTNAME]/[PROJECT-ID]/*/berealtime </code>
9. Open cloud console and choose from left menu Tools -> Container Registry and check your image there.

