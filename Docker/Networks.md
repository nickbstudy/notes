#### Bridge Networks

This is the easiest type to set up.  It allows communication between containers **on the same machine**.

`docker network create --driver bridge <networkName>`

`docker network ls` lists all networks

`docker network rm <networkName>`

`docker network inspect <id>` shows details including connected containers

`docker network` shows all network commands

