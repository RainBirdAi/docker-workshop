# Exercise 7

In this exercise we're going to look at overriding an entry point to allow us to
potentially debug an image. We'll do this by running a `bash` shell in the 
container.

## Invoking an interactive shell within a container that uses `CMD`

1. With an SSH session to your _development_ server, run the `curl-container` 
   using the default command, then override this by passing in a command line 
   argument:

   ```
   $ docker run curl-container
   $ docker run -i -t curl-container bash
   ```
    
   > **Tip:** The `-i` flag attaches `STDIN` to the container, the `-t`
   > attaches a pseudo-tty. This allows you to interact with the shell. 

   This works as `curl-container` just uses the `CMD` command, and we override
   this when providing a command in our arguments list when running the
   container.

3. Exit the container:

    ```
    # exit
    ```
 
## Invoking an interactive shell within a container that uses `ENTRYPOINT`
   
1. Attempt to attach `bash` to the `curl-binary` image.

   ```
   $ docker run -i -t curl-binary bash
   curl: (6) Could not resolve host: bash
   ```
   
   Since `curl-binary` is using `ENTRYPOINT` this simply takes anything passed 
   on the command line and attempts to use it as an argument. 
    
## Overriding the entry point
    
1. We can override the entry point on `curl-binary` using the following command:

    ```
    $ docker run -i -t --entrypoint bash curl-binary
    ```
    
    The `--entrypoint` flag allows us to specify an entry point, overriding any
    that was defined in the container.

6. Exit the container.

    ```
    # exit
    ```
