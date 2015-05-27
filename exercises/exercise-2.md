# Exercise 2

In this exercise, you'll create a container running Docker's official _registry_ 
image. You will push an image to, and then pull the same image from, this 
_registry_.

This a good exercise for understanding the basic interactions a client has with 
a local registry.

> **Note:** The following commands will need to be run on **one** of the 
> machines assigned to you. Use the first machine on your list. We'll refer to 
> this machine as your _Docker repository_ in future exercises. 

## Running a Docker image from the public registry

1. In an SSH session to your _Docker repository_ server, run the `hello-world` 
   image from the Docker public registry:

   ```
   $ docker run hello-world
   ```
	
   The `run` command automatically pulls a `hello-world` image from Docker's
   official images.

## Setting up and using a local registry

1. Start a registry on your localhost:

   ```
   $ docker run -d -p 5000:5000 registry:0.9.1
   ```
	
   This pulls version `0.9.1` of the official Docker registry image and runs it 
   on your host using port `5000`. We'll revisit the `-d` and `-p` flags later.
    
	
2. List the images running on you repository server:
	
   ```
   $ docker images
   REPOSITORY     TAG     IMAGE ID      CREATED       VIRTUAL SIZE
   registry       2.0     bbf0b6ffe923  3 days ago    545.1 MB
   hello-world    latest  e45a5af57b00  3 months ago  910 B
   ```
  
   Your list should include a `hello-world` image from the earlier run.

3. Re-tag the `hello-world` image for your local repository:

   ```
   $ docker tag hello-world:latest localhost:5000/hello-mine:latest
   ```

   The command labels a `hello-world:latest` using a new tag in the
   `[REGISTRYHOST/]NAME[:TAG]` format.  The `REGISTRYHOST` is this case is
   `localhost`. In a Mac OSX environment, you'd substitute `$(boot2docker
	ip):5000` for the `localhost`.
	
4. List your new image:

   ```     
   $ docker images
   REPOSITORY                  TAG     IMAGE ID      CREATED       VIRTUAL SIZE
   registry                    2.0     bbf0b6ffe923  3 days ago    545.1 MB
   hello-world                 latest  e45a5af57b00  3 months ago  910 B		 
   localhost:5000/hello-mine   latest  ef5a5gf57b01  3 months ago  910 B
   ```
	
   You should see your new image in your listing.

5. Push this new image to your local registry:

   ```
   $ docker push localhost:5000/hello-mine:latest
   The push refers to a repository [localhost:5000/hello-mine] (len: 1)
   e45a5af57b00: Image already exists 
   31cbccb51277: Image successfully pushed 
   511136ea3c5a: Image already exists 
   Digest: sha256:a1b13bc01783882434593119198938b9b9ef2bd32a0a246f16ac99b01383ef7a
   ```
		
6. Use the `curl` command and the Docker Registry API v1 to list your image in 
   the registry:
   
   ```
   curl -v -X GET http://localhost:5000/v1/repositories/library/hello-mine/tags
   * About to connect() to localhost port 5000 (#0)
   *   Trying 127.0.0.1...
   * Adding handle: conn: 0x7f83e2bf66f0
   * Adding handle: send: 0
   * Adding handle: recv: 0
   * Curl_addHandleToPipeline: length: 1
   * - Conn 0 (0x7f83e2bf66f0) send_pipe: 1, recv_pipe: 0
   * Connected to localhost (127.0.0.1) port 5000 (#0)
   > GET /v1/repositories/library/hello-mine/tags HTTP/1.1
   > User-Agent: curl/7.30.0
   > Host: localhost:5000
   > Accept: */*
   > 
   < HTTP/1.1 200 OK
   * Server gunicorn/19.1.1 is not blacklisted
   < Server: gunicorn/19.1.1
   < Date: Fri, 22 May 2015 15:02:24 GMT
   < Connection: keep-alive
   < Expires: -1
   < Content-Type: application/json
   < Pragma: no-cache
   < Cache-Control: no-cache
   < Content-Length: 78
   < 
   * Connection #0 to host localhost left intact
   {"latest": "91c95931e552b11604fea91c2f537284149ec32fff0f700a4769cfd31d7696ae"}
   ```

## Tidying up and testing

1. Remove all the unused images from your local environment:

   ```
   $ docker rmi -f $(docker images -q -a )
   ```

   This command is for illustrative purposes; removing the image forces any 
   `run` to pull from a registry rather than a local cache. You may get errors 
   on running it. You can usually ignore these. If you run `docker images` 
   after this you should not see any instance of `hello-world` or `hello-mine` 
   in your images list.
	
   ```
   $ docker images
   REPOSITORY      TAG      IMAGE ID      CREATED       VIRTUAL SIZE
   registry        2.0      bbf0b6ffe923  3 days ago    545.1 MB
   ```

2. Try running `hello-mine`:

   ```
   $ docker run hello-mine
   Unable to find image 'hello-mine:latest' locally
   Pulling repository hello-mine
   FATA[0001] Error: image library/hello-mine:latest not found 
   ```
		
   The `run` command fails because your new image doesn't exist in the Docker 
   public registry.

3. Now, try running the image but specifying the image's registry:

   ```
   $ docker run localhost:5000/hello-mine
   ```

   If you run `docker images` after this you'll fine a `hello-mine` instance. 
	
> This exercise is based on the documenation from 
> [https://github.com/docker/distribution/blob/master/docs/deploying.md]().
> Refer to that document for more information on making a production ready 
> registry. 
