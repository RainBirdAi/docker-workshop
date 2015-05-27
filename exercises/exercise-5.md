# Exercise 5

In this exercise we're going to build a container that will run `curl` for us.
We'll then deploy this container on our _production_ server using our Docker 
repository.

## Create a basic `curl` container

1. On your _development_ server, create a directory to work in:

   ```
   $ cd
   $ mkdir curl-container
   $ cd curl-container
   ```
    
3. Create a `Dockerfile` using `vi Dockerfile`:
  
   ```dockerfile
   FROM debian:latest
   MAINTAINER Your Name <email@address.com>

   RUN apt-get update
   RUN apt-get install -y curl
   ```

   > **Tips:** press `i` to start typing in `vi`. You should see the text
   > `-- INSERT --` at the bottom. When you're done hit `ESCAPE` and type `:wq`
   > to save the file and quit.

4. Now we can build the image from the `Dockerfile`:

   ```
   $ docker build -t curl-container .
   ```
    
   This builds the Dockerfile in the current directory and gives it the tag 
   `curl-container`. It will automatically be tagged `:latest`. The `-t` flag
   allows you to give your images meaningful names.

5. Test the image:

   ```
   $ docker run curl-container curl www.google.com
   ```

## Update the image to automatically run curl for you

1. Edit the `Dockerfile` and add a `CMD` command at the end:
    
   ```dockerfile
   FROM debian:latest
   MAINTAINER Your Name <email@address.com>
    
   RUN apt-get update
   RUN apt-get install -y curl
    
   CMD curl www.google.com
   ```
    
   > **Tip:** Go to the bottom of the file using the down arrow, then press `o` 
   > to start editing on the next line.
    
   The `CMD` command tells Docker to run the given command when the image is 
   run as a container. We've provided the command in _shell format_, i.e. as 
   you would type it if you were entering the command from the shell. We'll
   look at different formats later.

2. Rebuild the image:

   ```
   $ docker build -t curl-container .
   ```
    
   This will update the `curl-container` image with the new image defined in
   the `Dockerfile`
    
3. Test the image:

   ```
   $ docker run curl-container
   ```
    
   > **Note:** We're no longer providing a command for the container to run.
   > This is because it's specified on the `Dockerfile`.

4. Retag the image for our Docker repository:

   ```
   $ docker tag curl-container:latest <your-repo-ip>:5000/curl-container
   ```
    
   > **Note:** Change `<your-repo-ip>` to the IP address of your Docker
   > repository (i.e. the IP address of your first machine).
    
5. Push the image to our Docker repository:

   ```
   $ docker push <your-repo-ip>:5000/curl-container
   ```

### Deploy your image

1. Using an SSH session to your _production_ server (the third machine on your
   list), pull the image we just pushed:

   ```
   $ docker pull <your-repo-ip>:5000/curl-container
   ```
    
3. Run the container on your _production_ server:

   ```
   $ docker run curl-container
   ```
   
