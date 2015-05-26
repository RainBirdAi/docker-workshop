## Exercise 0

In this exercise we'll make sure that everyone has access to the servers they're
going to need during the workshop. We'll also finish off some basic setup that's
needed in a later exercise.

### Setting up

1. If you're running on Windows and _don't_ have an SSH client installed you'll
   need to install something like [PuTTY][putty].
   
   > **Tip:** Mac and Linux users have a built in SSH client and can skip this
   > step.
   
2. Get a copy of `docker.pem` from the memory stick circulating round the room.

   > **Tip:** Mac and Linux users should put this file in `~/.ssh` and then run
   > `chmod 600 ~/.ssh/docker.pem`.
   
   > **Tip:** Windows users can store the file anywhere. You'll need to open it 
   > with your SSH client later. A good set of instructions on how to use the
   > keyfile can be found [here][instructions] (scroll down to the section 
   > _Setting Up an SSH Session with SSH Keys in PuTTY_)
   
3. Note down 3 IP addresses from the sheet of paper that's going around the room
   with the memory stick. The first IP will be used for your _Docker 
   repository_, the second will be used as a _development environment_ and the
   third is your _live environment_.
   
4. Ensure you can connect to all three machines. You will need to run the 
   following commands to complete the setup:
   
   ```
   $ sudo systemctl daemon-reload
   $ sudo systemctl restart docker
   ```
   
   > **Tip:** Docker is installed by default on _CoreOS_, however, we're going
   > to be using an insecure repository during the workshop. The above commands
   > load in a pre-generated configuration file that allows for this and
   > restarts Docker so it can read the file.
   
   > **Tip:** If you leave open the three connections to the three machines you
   > can re-use them in the future exercises. 

[putty]: http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe
[instructions]: https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-putty-on-digitalocean-droplets-windows-users
