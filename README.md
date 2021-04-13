# sharedpg
Docker Postgres Shared Volume

Requirements:
- Docker
- docker-compose

Steps:

1) Configure port, network, user and password as you wish. Default port is 5433:

Create a bridge network. For example, in order to create a network named "shared", you can run this command: 
`docker network create --driver bridge shared`

For more information:
https://docs.docker.com/network/bridge/


2) `docker-compose up -d`
3) If you're having trouble with yarn packages, DON'T run yarn install on your local machine and do it in the docker container.

By using the default docker-compose.yml of this proyect, your containers will connect to the 5432 port using the shared network.
If you want to connect outside docker (e.g, sqlpro or pgAdmin), you can do it by connecting through localhost at port 5433.

Now, on your local docker postgres, you can set your connection to be sharedpg. In your proyect's docker-compose.yml, you can set the network to bridge so it can connect to this postgres server, like this:

```
networks:
  shared:
    driver: bridge
    external: true

services:
  your-service:
    ...
    networks:
      - shared

volumes:
  sharedpg:
    external: true
```

If you're using Rails, here's an example of how my database.yml looks like:

```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  host: sharedpg
  port: 5432
  database: mydb_development
  username: postgres
  password: postgres
```

You can autostart your container whenever your OS boots up, running this command:
`docker start sharedpg`
