#### `docker run -p <ports> -v /var/www/logs <imageToRun>`
`-v` (short for `--volume`) will store location listed on the container host (doesn't have to be the one listed obviously).

To specify a folder on Windows PowerShell use:
`-v ${PWD}:/var/www/logs`

Linux is the same except `$(pwd)`

This is the most simple type of volume, they can be run on docker instead of just locally, go to https://docs.docker.com/storage/volumes for more details.