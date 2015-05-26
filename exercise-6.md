# Exercise 6

In this exercise we're going to look at how the `ENTRYPOINT` command works, and
how it allows us to treat a container as a binary.

## Creating a `curl` server as a binary

1. With an SSH session to your _development_ server, make a `curl-binary` 
   directory and copy the `curl-container` Dockerfile:

   ```
   $ cd
   $ mkdir curl-binary
   $ cd curl-binary
   $ cp ../curl/container/Dockerfile .
   ```
    
3. Ammend the last line of the `Dockerfile` using `vi Dockerfile`:
  
   ```dockerfile
   FROM debian:latest
   MAINTAINER Your Name <email@address.com>

   RUN apt-get update
   RUN apt-get install -y curl
    
   ENTRYPOINT ["curl"]
   ```
    
   Like `CMD`, `ENTRYPOINT` will accept the _shell_ format for commands. Here
   we're using _exec_ format. The _exec_ format supports a comma separated list
   of strings, where the first string is the command to run, and all subsequent
   strings are arguments to that command. As we'll see in a bit, you can also
   use `CMD` to provide arguments.
    
4. Build a new image called `curl-binary`:

   ```
   $ docker build -t curl-binary .
   ```

5. Test the image, providing a URL to visit:

   ```
   $ docker run curl-binary www.google.com
   ```
   
6. Test the image with no arguments.

   ```
   $ docker run curl-binary
   curl: try 'curl --help' or 'curl --manual' for more information
   ```
   
   Since we've not provided any arguments, `curl` will fail. To get round this
   we can provide some default arguments.
   
## Adding some default arguments
    
1. Amend the `Dockerfile` to include a default URL:

   ```
   FROM debian:latest
   MAINTAINER Your Name <email@address.com>
    
   RUN apt-get update
   RUN apt-get install -y curl
        
   ENTRYPOINT ["curl"]
   CMD ["www.google.com"]
   ```
   
   Here the `CMD` line is providing arguments to `ENTRYPOINT`. If we provide
   arguments when we run the image the `CMD` line will be ignored.
    
2. Rebuild the `curl-binary` image:

   ```
   $ docker build -t curl-binary .
   ```

3. Test the image with no arguments:

   ```
   $ docker run curl-binary
   ```
   
4. Test the image with a URL of your choice as an argument.
