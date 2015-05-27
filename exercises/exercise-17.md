# Exercise 17 

In this exercise we look at how we can mitigate Dockerfiles creating too many
layers.

## Slim images

1. Using an SHH connection to your _development_ server, create a new directory:

   ```
   $ cd
   $ mkdir slim-image
   $ cd slim-image
   ```

2. Make a note of how many images you currently have on your server:
   
   ```
   $ docker images -a | wc -l
   ```
      
2. Create a new `Dockerfile` using `vi Dockerfile`:

   ```dockerfile
   FROM debian:latest
   
   RUN mkdir a; mkdir b; mkdir c
   ```
   
   This `Dockerfile` is functionally identical to the previous one we created, 
   however, it runs the three commands on the same line.
   
3. Build the image:

   ```
   $ docker build -t slim-image .
   ```

4. Check how many images we now have:

   ```
   $ docker images -a | wc -l
   ```
   
   You'll see there is now only one more image.
