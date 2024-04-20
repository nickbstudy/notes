#### Dockerfile

At the start of a Dockerfile:

```
FROM node:current-alpine
```

Specifies the base image from which you are building your container.  The components of the command are:

1. **`node`** - The docker image name - in this case the official Node.js image, which is provided and maintained by the Node.js foundation.  Includes the runtime and `npm`.

2. **`current`** - The version to use (updated regularly as new versions are released)

3. **`alpine`** Indicates it should be built using Alpine Linux (known for being lightweight and minimal, popular for Docker images aiming to be small and efficient in resource usage).  The `alpine` version of the Node.js image contains just the necessary components needed to run Node.js, built on top of an Alpine Linux distribution.

---
