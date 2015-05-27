# Exercise 9

It's likely that you've got a number of containers and images lying around on 
your three servers which will be using disk space, and that are possibly still 
running. In this exercise we're going to tidy things up a little bit.

## Tidying up

1. Log into server running the Docker repository (the first machine on your 
   list). Stop and remove all containers _except_ for the docker registry 
   container. You should leave this running.

   > **Tip:** Don't forget to use the `-a` flag to list all containers, even
   > stopped ones.

2. Remove all images except for the docker registry image

3. Log into your _development_ server (the second machine on your list). Stop 
   and remove all containers, then remove all images except the `debian` ones.
    
4. Log into your _production_ server (the third machine on your list). Stop and 
   remove all containers and images.
