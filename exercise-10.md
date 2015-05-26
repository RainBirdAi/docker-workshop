# Exercise 10

In this exercise we're going to use the `COPY` and `EXPOSE` `Dockerfile` 
commands to create a simple NodeJS server which we'll connect to using a web 
browser.

1. With an SSH session to your _development_ server, make a new directory for 
   your `hello-world` container:

   ```
   $ cd
   $ mkdir hello-world
   $ cd hello-world
   ```
    
3. Create the server using `vi server.js`:

   ```
   var http = require('http');
    
   var server = http.createServer(function (request, response) {
       response.writeHead(200, {"Content-Type": "text/plain"});
       response.end("Hello World\n");
   });
    
   server.listen(5000);
    
   console.log("Server running on port 5000");
   ```

   This is the standard _Hello World_ NodeJS application that simply accepts
   HTTP requests on the given port and responds wth the text _"Hello World"_.

4. Create a Dockerfile for the container using `vi Dockerfile`:

   ```
   FROM node:0.10.38-slim
    
   COPY server.js server.js
    
   EXPOSE 5000
    
   ENTRYPOINT ["node"]
   CMD ["server.js"]
   ```
   
   The `COPY` command will copy `server.js` from our local file system and place
   it into the container.
    
5. Build the image:

   ```
   $ docker build -t hello-world .
   ```
    
5. Run the image in detached mode, exposing port `5000`:

   ```
   $ docker run -d -p 5000:5000 --name hello-world hello-world
   ```

   > **Tip:** The `-p` flag maps port `5000` from the host to port `5000` in 
   > our container.
    
6. Connect to the server using a web browser pointing to your development 
   servers IP address on port 5000.
