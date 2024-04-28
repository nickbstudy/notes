A **Dockerfile** is a text document that contains all the commands a user could call on the command line to assemble an image.  Each image is essentially a layered file system.  An example of some from a node Dockerfile:

- **`FROM`** defines the programming language and OS to use (`node:alpine`).  You can find a list of images on https://hub.docker.com/
If no version is specified it will always get the latest.  This is not always desirable as it can introduce inconsistencies, so consider specifying a version.
- **`LABEL`** allows inserting metadata (`author="Bob Ross"` or a creation date etc)
- **`ENV`** defines environment variables (`NODE_ENV=production`) - to access in the file later prefix with `$`**
- **`WORKDIR`** is the starting directory, e.g. folder containing index.html for a web server.
- **`COPY . .`** indicates what is to be copied.  First value is the source (`.` being the folder containing the Dockerfile) and the second is the `WORKDIR` location.  Any folders you want to skip (e.g. node_modules) can be listed in a file called `.dockerignore`
- **`RUN`** runs a command, such as `npm install` 
- **`EXPOSE`** opens a port to listen on
- **`ENTRYPOINT`** is the first command to be run on start (`["node", "server.js"]` or .NET or other language equivelant)

A detailed list of all available commands is available on the Docker website: https://docs.docker.com/reference/dockerfile/

Also, you don't have to call it Dockerfile - this is just a convention.  An example of all of the above:

```
FROM        node:alpine

LABEL       author="Bob Ross"

ENV         NODE_ENV=production
ENV         PORT=3000

WORKDIR     /var/www
COPY        package.json package-lock.json ./
RUN         npm install

COPY        . ./
EXPOSE      $PORT

ENTRYPOINT  [ "npm", "start" ]
```