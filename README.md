# Docker Workshop

## Prerequisites
* Connected to WiFi
* Downloaded Docker
* Have Docker Hub account
* Run `docker pull python:3-slim` -> just run in background

## Basics

* Client Server Model
    * Daemon = Server
* Useful commands
    * `docker info` -> detailed info about server
    * `docker version` -> also tells you if the server is running
    * `docker kill <container>`
    * `docker ps` -> what's running

## Containers & Images

* What's a container -> An image that's being run
* What's an image -> A list of filesystem layers
* What's a repository -> a place to store your Docker images and filesystem layers

* How to load other people's containers
    * Exercise: `docker run -it alpine` -> run the latest alpine (a reduced slim Linux-based OS)
    * Exercise: `docker pull alpine:edge` -> run the edge version of alpine

* Dockerfiles
    * How to extend other people's containers and publish the result
        * Dockerfile is a set of instructions to Docker daemon on how to construct your image
        * Exercise: Write a Dockerfile based on Alpine
            * Create new folder
            * Create text file
            * Create "Dockerfile"
                ```
                    FROM alpine
                    ADD test.txt test.txt
                ```
            * Run `docker build .` -> Builds current directory into image, shows you ID
            * What just happened?
                * Client sends local context (Dockerfile, files and folders) to Docker daemon
                * Daemon builds an image based on the Dockerfile, saves the result
                * Client shows ID of freshly created image

            * Run `docker run -it <id>` -> Run the container you just built, you can `cat test.txt` to see your file
            * Run `docker tag <id> <username>/test-01` -> Tag it with a human readable name
            * Run `docker push <username>/test-01` -> Push it to DockerHub

### BREAK 15 Minutes

* Now let's make something useful
    * Exercise: Create a webserver
        * Create a new folder
        * `cd` to it
        * Create `index.html` with whatever HTML content you like
        * Create Dockerfile
            ```
                FROM python:3-slim
                WORKDIR /usr/src/app
                COPY . .

                EXPOSE 8000

                CMD [ "python", "-m", "http.server" ]
            ```
        * Note: "EXPOSE" command tells Docker that this application offers service on the following ports
            * Since local computer is Docker server, this allows us to access containers on localhost
        
        * Run `docker run -p 8080:8000 -it <id>`

        * Browse to [https://localhost:8080]
        * Note: Host port is different from container port

## Stacks & Compose Files
* How to specify a stack -> Docker compose file

* Exercise: Create a WordPress installation running on MySQL
    * Copy the Docker compose file from folder "03-compose-files"
    * Run `docker-compose up -d`

* What are stacks? -> multiple containers to run, with:
    * image to use
    * storage
    * networking

* Checkpoint: Wait for everyone to have wordpress running locally

* Exercise: Change the compose file:
    * Use different versions of the images -> check Docker Hub for "tags" you can ask Docker for
        * Note: Only differences in images between versions are downloaded

* What happens "under the hood"?
    * Docker creates the containers required in the stack. It also creates the volumes required, connects the specified network services, and respects the dependencies between multiple containers in a stack.

### BREAK, 20 minutes

* Explore time
    * Have a project you want to make into a container? Let's try it
    * Want to try building your own docker-compose file?
    * Try to change configs, try to break it