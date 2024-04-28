#### `docker build -t <registryName>/<imageName>:<tag> .`

`-t` is short for `--tag` (if none is specified it will be `latest` by default)
`.` at the end is the folder to build from (usually the one you're running the command from, but this can be changed)
Tag is a good place to keep the version number.
e.g. `docker build -t bobross/webserver/1.0 .`
If you have a Dockerfile name anything different use `-f` (short for `--file`)


`docker images` will list all images
`docker rmi <image id>` will remove an image, note you don't need the entire ID generally just the first few characters is enough.

