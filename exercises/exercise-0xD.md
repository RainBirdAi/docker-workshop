# Exercise 0xD

In this exercise we're going to look at how we can map directories located on 
the host machine to the containers file system. We'll use this to set up a 
development environment that uses Docker to run our code, but the host machine
to edit it.

## Mapping Directories

1. Using an SSH session on your _development_ server, create a `dev` directory:

   ```
   $ cd
   $ mkdir dev
   $ cd dev
   ```
    
2. Create a `Dockerfile` that will run node on any given file:

   ```
   FROM node:0.10.38-slim
   EXPOSE 5000
        
   RUN mkdir code
   WORKDIR code
    
   ENTRYPOINT ["node"]
   ```
    
   > **Tip:** The `WORKDIR` command sets the current working directory in the
   > container.
    
3. Build the image:

   ```
   $ docker build -t dev-env .
   ```
    
4. Copy our old NodeJS application as a basis for us to work on:

   ```
   $ cp ../hello-world/server.js .
   ```
    
5. Run our application from the local file:

   ```
   $ docker run -d -p 5001:5000 -v /home/core/dev:/code --name dev-env dev-env server.js
   ```

   > **Tip:** The `-v` flag maps the directory `/home/core/dev` on our host
   > machine to `/code` in the container. We could, if we wanted, have specified
   > an individual file in this case, however, a directory allows us to have
   > more than one file if our application needs it.
    
6. Check it's working by pointing a browser at the IP address of your 
   _development_ machine, port `5001`.
    
7. The code is still using your name, so lets change that back. Edit `server.js`
   and replace your name with `everyone!`.
    
8. Restart the container to pick up the changes:

   ```
   $ docker restart dev-env
   ```
    
9. Check it's picked up the change by reloading the browser page.

   > **Tip:** The changes will only take effect when you restart the container
   > because NodeJS does not dynamically read files. You can test this by 
   > making another change and reloading the pagge without restarting the 
   > container.
