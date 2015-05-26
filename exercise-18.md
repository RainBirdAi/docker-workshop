# Exercise 18

In this exercise we look at the `ONBUILD` command. This allows us to make our
images extensible by deferring some commands until this image is used as the
base for a future image.

## Building the base image

Our base image will be a simple example that demonstrates how we could use
`ONBUILD` to setup development and production images.

1. Using an SSH session to your _development_ server, create a new directory:

   ```
   $ cd
   $ mkdir -p onbuild/parent
   $ cd onbuild/parent
   ```
   
2. Create a dummy `Dockerfile` that simulates setting up the environment
   required for your application:
   
   ```dockerfile
   FROM debian:latest
   
   RUN echo "Setting up the environment"
   ONBUILD RUN echo "Performing the build"
   ```
   
   We defer the building of the environment because the developer will be
   controlling that, possibly creating debug, or non-standard builds to test
   things.
   
3. Build the image:

   ```
   $ docker build -t onbuild-parent .
   ```
   
   Note it only runs the `RUN` command, not the `ONBUILD`, despite providing
   output for both. The `ONBUILD` command is simply registered with the image.
   
## Build the child image

1. Create a directory for the child:

   ```
   $ mkdir ../child
   $ cd ../child
   ```
   
2. Create a dummy `Dockerfile` that simulates deploying to production:

   ```
   FROM onbuild-parent
   RUN echo "Finishing the production image"
   ```
   
3. Build the image:

   ```
   $ docker build -t onbuild-child .
   ```

   Notice how the build performed the `echo` command from the `ONBUILD` as a 
   built trigger.
