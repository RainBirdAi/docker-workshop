# Exercise 11

In this exercise we're going to look at one method of using Docker to provide a
release, updated and rollback type mechanism for a deployment. We will create
an image which we will release. We'll then update the image and deploy it as a
new container. We'll the roll back to the original release.

## Preparing the release

We already have a working image that we can use for this example, so we'll just
tag it and push it to the Docker repository.

1. Using an SSH session to your _development_ server, tag the `hello-world` 
   container from the previous example:

   ```
   $ docker tag hello-world:latest <repo ip>:5000/hello-world:1.0
   ```

   > **Tip:** Replace `<repo ip>` with the IP address of your Docker 
   > repository.
   
   We're tagging this with a version `1.0` label.
    
3. Push the image to your Docker repository:

   ```
   $ docker push <repo ip>/hello-world:1.0
   ```
    
### Deploy v1.0

We can now pull the image from the Docker repository and run it on our 
production machine.

1. Using an SSH session to your production server, pull the `v1.0` Docker image 
   of `hello-world`.

   ```
   $ docker pull <repo ip>:5000/hello-world:1.0
   ```
    
2. Run the `hello-world` container on port `5000`:

   ```
   $ docker run -d -p 5000:5000 --name hello-world-1.0 <repo ip>:5000/hello-world:1.0
   ```
   
3. Test the deployment by pointing a web browser to the IP address of your 
   _proudction_ machine on port `5000`.

### Create v1.1

Lets update the image to make it more personal and call this version 1.1.

1. Using an SSH session to your _development_ server:

   ```
   $ cd
   $ cd hello-world
   $ vi server.js
   ```
    
   Change the world `world` to be your name. Save and exit with `:wq`.
    
2. Build the new image:

   ```
   $ docker build --t hello-world .
   ```
    
3. Tag the image as `v1.1`:

   ```
   $ docker tag hello-world:latest <repo ip>:5000/hello-world:1.1
   ```
    
4. Push the image to your Docker repository:

   ```
   $ docker push <repo ip>/hello-world:1.1
   ```
    
### Deploy v1.1

We can now pull the `v1.1` release and run that instead of `v1.1`

1. Using an SSH session to your production server, pull the `v1.1` image:

   ```
   $ docker pull <repo ip>:5000/hello-world:1.1
   ```
    
2. Shutdown the old container and run v1.1 in a single command to minimise
   downtime:

   ```
   $ docker stop hello-world-1.0; docker run -d -p 5000:5000 --name hello-world-1.1 <repo ip>:5000/hello-world:1.1
   ```
    
3. Test the deployment by pointing a web browser to the IP address of your
   _production_ machine on port 5000.

### Rollback

A user has pointed out that not everyone shares your name, making this update
nonsensical for some users. We need to roll back! This can be done in a single
line.

1. Rollback:

   ```
   $ docker stop hello-world-1.1; docker start hello-world-1.0
   ```

2. Test the rollback has worked.
