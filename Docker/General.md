#### To run interactive help:

**`docker run -d -p 80:80 docker/getting-started`** in PowerShell then visit localhost 

---

#### Terminology

- **Image** - The "Recipe for the cake" - defines the contents that are needed to run a container.  It will have a Dockerfile listing everything required to compile and run the container.  Contains your code, server code, environment variables, and security settings.  Designed to be universally run.

- **Container** - An isolated and secured shipping container created from an image that can be run, started, stopped, moved, and deleted.

---

Part of the container startup tags specifies detached `-d` (most common) or alternatively `-i` (or `--interactive`), which keeps the standard input open (even if not attached) and `-t` (or `--tty`), which allocates a pseudo TTY to provide a command line interface, allowing you to interact with it similar to a terminal session.

If you are in a session (such as if you had just run `docker container run -it --name test alpine sh`) you can detach with Ctrl + P, Ctrl + Q.  To reattach, use `docker attach appname`

---

