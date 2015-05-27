# Exercise 4

In this exercise we're going to make a `curl` server, similar to the ping server 
you made in Exercise 1. We'll see how Docker images sometimes require extra 
steps to create, and revisit committing containers as images.

## Curl Server

1. Using an SSH session to your _development_ server:

   ```
   $ docker run debian apt-get install -y curl
   Reading package lists...
   Building dependency tree...
   Reading state information...
   E: Unable to locate package curl
   ```
   
   This command has failed because `apt-get` hasn't been initialised properly
   yet.
   
   > **Tip:** The `-y` flag stops `apt-get` from asking if we're sure we want
   > to proceed. We need to suppress this as the container is not an interactive
   > one.
   
2. Update `apt-get`:

   ```
   $ docker run debian apt-get update
   ```

3. Commit the updated container so we can run further commands against it.

   The Docker `commit` command uses the syntax:
   
   ```
   docker commit <container_id> <some_name>
   ```
   
   > **Tip:** You will need to use the `-a` flag when finding the container ID.
   
4. Re-run `apt-get install` command, and commit the resultant container.

5. Use the newly committed image to run `curl` on `www.google.com`

> **Question:** Take a look at the images and docker containers that are left
> after exercises 3 and 4. Do you know why these are here?
