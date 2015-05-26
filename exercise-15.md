# Exercise 14

In this quick exercise we're going to look at how base images can affect the 
size of your images and containers. Large, bloated images can often take up lots
of disk space without you realising.

## Comparing base images

1. Using an SSH session into your _development_ server, pull the `ubuntu` image:

   ```
   $ docker pull ubuntu
   ```
    
3. Compare the relative size with the `debian` image:

   ```
   $ docker images
   ```

   > **Tip:** Images that share the same ID  don't take up any more disk space.
   > They are just the same image with different tags.
   
4. Remove the `ubuntu` image.
