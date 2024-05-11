[[Docker]]
## Quick Start
```
docker run --name postgres \
-e POSTGRES_PASSWORD=mypassword \
-e POSTGRES_USER=whoop \
-e POSTGRES_DB=whoop \
-p 5432:5432 \
-d postgres
```

## Reference
[How to use Postgres Docker Official Image](https://www.docker.com/blog/how-to-use-the-postgres-docker-official-image/)