# Exercise 14

Ordinarily, data stored in a Docker container is not persisted between
invocations. We can allow this by using _volumes_.
 
## Container storage doesn't ordinarily persist

1. Using an SSH session to your _development_ server, run the following:

   ```
   $ docker run debian sh -c 'echo hello world > testing.txt'
   $ docker run debian cat testing.txt
   ```

   This first command puts the text _"hello world"_ into a file called
   `testing.txt`. The second command uses the same base image to see if the
   file exists. It does not, since data does not persist between invocations, we
   use the same, clean `debian` image for each run.
   
## Use a volume to persist the data.

1. Rerun the `echo` command specifying a volume. We need to move where we're
   outputting the text to so it's located within the volume:
   
   ```
   docker run -v /volume --name="vtest" debian sh -c 'echo hello world > /volume/testing.txt'
   ```
   
2. Rerun the `cat` command specifying that we want to use a volume from `vtest`:

   ```
   docker run --volumes-from=vtest debian cat /volume/testing.txt
   ```

   As long as `vtest` exists (even if it's not running) the data in the volume
   is persisted.
