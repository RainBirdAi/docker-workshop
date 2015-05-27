# Exercise 16 

In this exercise we look at how Dockerfiles and images can easily eat up disk 
space. We'll then see how we can mitigate this in the next exercise.

## Fat images

1. Using an SHH connection to your _development_ server, create a new directory:

   ```
   $ cd
   $ mkdir fat-image
   $ cd fat-image
   ```

2. Make a note of how many images you currently have on your server:
   
   ```
   $ docker images -a | wc -l
   ```
   
   The actual number will be 1 less than what was output as the first line is a
   header, however, when we check again later the result will also be off by 1
   so we'll know how many more images we've created.
   
2. Create a new `Dockerfile` using `vi Dockerfile`:

   ```dockerfile
   FROM debian:latest
   
   RUN mkdir a
   RUN mkdir b
   RUN mkdir c
   ```
   
   This `Dockerfile` simply creates an image with three directories, `a`, `b`,
   and `c`.
   
3. Build the image:

   ```
   $ docker build -t fat-image .
   ```

4. Check how many images we now have:

   ```
   $ docker images -a | wc -l
   ```
   
   You'll see there are now 3 more images, not 1.
