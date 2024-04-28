#### Starting a container

**`docker run -p <externalport>:<internalport> <imagename>`**

`-p` (short for `--publish`) defines the ports, `<externalport>` being the one you will use to access it, and `<internalport>` the one opened by the image.

`<imagename>` will include a tag, something like `nginx:alpine`

`-d` (short for `--detached`) won't lock your terminal into the session, put it before the image name

Optional: `--name namegoeshere` before the image name will give the container a name, instead of the randomly generated one.

add `--net=<networkName>` or `--network=<networkName>` to join a network, a name will help for that

---

`docker ps` (ps is short for "process status") lists all running containers, use `-a` to include stopped ones.

`docker stop <id>` will stop a container (first few chars of the ID is enough)

`docker rm <id>` will delete a container (first few chars of the ID is enough).  This doesn't delete the image, only the container.