# Exercise 8

In this exercise we're going to look at how we can control and interrogate long
running containers. We will use a simple container that prints the time every
second as our example, but the container could be running any application.

## Create the `clock-container`

1. Using an SSH session to your _development_ server, make a new directory for 
   your `clock` container:

   ```
   $ cd
   $ mkdir clock-container
   $ cd clock-container
   ```
    
3. Create a Dockerfile for the container using `vi Dockerfile`:

   ```
   FROM debian:latest    
   CMD while true; do date; sleep 1; done
   ```
    
4. Build the image:

   ```
   $ docker build -t clock-container .
   ```
    
5. Run the image:

   ```
   $ docker run -d --name clock clock-container
   ```

   > **Tip:** The `-d` flag runs the container in _detatched_ mode. Nothing
   > will be output to the terminal from the container, just the ID is
   > displayed.
   >
   > The `--name` flag allows us to name the container something memorable.
    
## Stopping, starting and interrogating the container
    
1. We can see the container running using the `ps` command. the `logs` command
   allows us to see any output the container has produced:

   ```
   $ docker ps
   $ docker logs clock
   ```
    
   You should see several lines showing the time and current date.
    
2. Stop the container and interrogate it again:

   ```
   $ docker stop clock
   $ docker ps
   $ docker ps -a
   ```
   
   The container may take a moment to stop. Once stopped the container will no 
   longer show in the Docker process list unless the `-a` flag is used.
   
   ```
   $ docker logs clock
   ```
   
   > **Tip:** We can still see the output from the container, even though it's
   > stopped.
   
3. Restart the container and interrogate it again:

   ```
   $ docker start clock
   $ docker ps
   $ docker logs clock
   ```
   
   You should see a gap in the times where the container was not running.
   
4. Kill the container.

   ```
   $ docker kill clock
   ```
    
   > **Note:** `docker stop` will attempt to stop the container nicely.
   > `docker kill` will stop the container immediately. The container can skill
   > be rerun but may not be in a good state depending on how the underlying
   > processes handled the kill.
   
## Clean up    
    
1. Clean up:

   ```
   $ docker rm clock
   $ docker ps -a
   ```
    
   > **Tip:** Notice the clock container is no longer listed, even with the `-a` 
   > flag
    
   ```
   $ docker images
   $ docker rmi clock-container
   $ docker images
   ```
    
   > **Tip:** The `rmi` command can be used to remove unused Docker images.
