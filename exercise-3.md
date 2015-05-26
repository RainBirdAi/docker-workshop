# Exercise 3

In this exercise we will run a copy of Debian on CoreOS and get it to display 
_"Hello World!"_. This shows how Docker can be used to run different Linux
distributions in a container.

## Running Debian on CoreOS

1. Open an SSH session to your _development_ server (the second server on your
   list).

2. Pull a Debian image from Docker Hub:
 
   ```
   $ docker pull debian
   ```

3. Run the echo command on your container so it displays _"Hello World!"_.

> **Tip:** You can refer back to exercise 1 and 2 for a reminder on the syntax
> running a command in a container.
