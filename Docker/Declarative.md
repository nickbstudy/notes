Instead of using command lines, create a configuration file and hand it to a platform.  A compose (`compose.yaml`) file might look like:

```
networks:
  counter-net:

volumes:
  counter-vol:
  
services:
  web-fe:
    build: .
    command: python app.py
    ports:
      - target: 8080
        published: 5001
    networks:
      - counter-net
    volumes:
      - type: volume
        source: counter-vol
        target: /app
  redis:
    image: "redis:alpine"
    networks:
      counter-net:
```

First it defines a volume (storage) and a network, then the services.  This is a multi-container app (web-fe and redis).  It referred to the "Desired State".

To compile all of this, make sure Docker Desktop is running, then in the terminal navigate to the folder containing `compose.yaml` and run `docker compose up -d`

To unload them use `docker compose down` and optionally `--volumes` to remove volumes (unless you need to persist data)