Part of the container startup tags specifies detached `-d` (most common) or alternatively `-i` (or `--interactive`), which keeps the standard input open (even if not attached) and `-t` (or `--tty`), which allocates a pseudo TTY to provide a command line interface, allowing you to interact with it similar to a terminal session.

If you are in a session (such as if you had just run `docker container run -it --name test alpine sh`) you can detach with Ctrl + P, Ctrl + Q.  To reattach, use `docker attach appname`

