# Exercise 12

In this exercise we'll look at using port mappings to provide Blue/Green
deployments using docker. Since we don't have a router configured we'll need to
simulate the effects manually.

## Blue/Green deployments

1. Using an SSH session to your _production_ server, stop and remove all running 
   containers.

3. Start the blue deployments:

   ```
   $ docker run -d -p 5001:5000 --name hello-world-1.0 <repo ip>:5000/hello-world:1.0
   ```
    
4. Test the deployment by opening a browser to the IP address of your third
   machine, port `5001`.

   > **Note:** Ordinarily our _router_ would be taking traffic from port 80 and
   > redirecting it to port 5001 for us. Since we don't have a router we'll 
   > have to manually add the port.
    
5. Start the green deployments:

   ```
   $ docker run -d -p 5002:5000 --name hello-world-1.1 <repo ip>:5000/hello-world:1.1
   ```

   > **Note:** Both instances are now running internally on port `5000`. We've 
   > mapped those ports to `5001` and `5002` on our host machine to allow them 
   > to run side by side. The router would still be configured to point to port 
   > `5001` so the users would only see `v1`.
    
6. Switch versions.

   To switch versions you would simply update the router to point to port
   `5002` instead of port `5001`. Simulate this by opening a browser to the IP 
   address of your _production_ server, on port `5001`.
    
7. Roll back. This is simply a case of updating the router to point back to 
   `5001` again. You can simulate this in your browser.
