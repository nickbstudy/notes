#### Common commands

`docker image build -t yourname/repositoryname:appname` - (`-t` is for tag)
This should be run in a folder with a Dockerfile in it to build an image

`docker image ls` - list all running docker images (include `-a` to see stopped ones too)

`docker image push yourname/repositoryname:appname`
Publish your image to the Docker Hub

`docker container run -d --name appname -p 8000:8080 yourname/repositoryname:appname`

1. **`docker container run`** - This is the base command used to run a new container from a Docker image.
2. **`-d`** - This flag stands for "detached" mode. It tells Docker to run the container in the background. You won't see any output directly in your terminal from the container's processes, allowing you to continue using the terminal for other tasks.
3. **`--name appname`** - The `--name` flag allows you to assign a custom name to your container, which in this case is appname. This name can be used to reference the container within a Docker environment, making it easier to manage especially if you are running multiple containers.
4. **`-p 8000:8080`**  - This is the port mapping:
8000 is the port on the host (your machine).
8080 is the port inside the container.
This mapping allows network traffic that hits your host machine on port 8000 to be forwarded internally to port 8080 on the container. This is commonly used to expose a service running inside the container to the outside world.

`docker container stop appname` Stops the app. 
`docker container start appname` If you have just stopped the app you can start it again in your container, otherwise it needs to be retrieved from the repository with the above.

`docker container rm appname` Removes the image (must be stopped first, unless you override with `-f`).  Can be reloaded from the repository.