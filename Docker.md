####
- при запуске через докер контейнер может получать разные IP адреса, НО с единым именем
```
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres
    ports:
      - "8001:5432"
```
### Internal and external PORTS
- при настройке сети ЖЕЛАТЕЛЬНО использовать container_name насколько возможно:
- `8001 - HOST_PORT` - внешний, from container to HOST
```postgres://{DOCKER_IP}:8001```
<hr>

- `5432 - CONTAINER_PORT` - внутренний, service to service connection between containers
```postgres://db:5432 ```

### Миграция с `docker-compose` V1 to `docker compose` V2
- улучшен CLI
- V1 не поддерживается с июля 2023
- cборка по BuildKit (надо почитать)
- теперь вместо двух разных cli комманд (первая docker, вторая docker-compose), можно ввести составную одну команду `docker --log-level=debug --tls compose up`
- в V1 использовался underscore (_) как разделитель, когда V2 использовует дефис, hyphen (-)
- Нижние подчеркивания является НЕВАЛИДНЫМИ символами для DNS hostnames
