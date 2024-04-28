Uses a YAML file to

- Build Services
- Start Up Services
- Tear Down Services

Services are really just containers, but docker compose calls them services.  Indentation is an important part of the file too.

---

Create a file called `docker-compose.yml` with these parts:

`version: '1.2'` or whatever version it is

`services:` to define all the containers (detail below)

```
networks:
    nodeapp-network:
        driver: bridge
```
to create the network/s - defined in each service with:
```
services:
    app:
        ...
        networks:
         - nodeapp-network
        
    anotherapp:
        ...
        networks:
         - nodeapp-network
```

---

#### Service details
`container_name: goeshere`
`image: goeshere`
```
build:
    context: .
    dockerfile: yourDockerfile
```
will build the image in location `context` from named Dockerfile.
```
ports:
 - "3000:3000"
networks:
 - nodeapp-network
```
standard network stuff

```
depends_on:
 - anotherapp
```
This will force compose to start up the `anotherapp` service before this one, but doesn't actually check it's ready to accept connections.  If this were a database your web server depended on, be ready to allow for this.

---


#### Commands:

`docker-compose build` Builds a single image based on the services in your YAML file.

`docker-compose up` Starts the services

`docker-compose down` Deletes the services.

`docker-compose --help` to list all commands