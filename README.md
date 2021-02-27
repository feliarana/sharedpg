# sharedpg
Docker Postgres Shared Volume

Requirements:
- Docker
- docker-compose

Steps:

1) Configure port, network, user and password as you wish. Default port is 5433.
2) `docker-compose up -d`

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
  database: testing_development
  username: postgres
  password: postgres
```
