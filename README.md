# workflow_orchestration_kestra

**docker_compose.yaml**
**to run this go to the pipeline dir and run "docker-compose up" in the terminal**
**to stop it, run "docker-compose down"**

```
services:
  pgdatabase:
    image: postgres:18
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "data:/var/lib/postgresql/data"
    ports:
      - "5432:5432"
```
```
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
    volumes:
      - "pgadmin_data:/var/lib/pgadmin"
    ports:
      - "8085:80"

volumes:
  data:
  pgadmin_data:
```

**to add data into postgreSQL DB we need to have**
**a separate container with .py file**
**run the following:**
```
docker build -t pipeline_project .
```
**pipeline_default - network created by docker-compose**
**pipeline_project:v001 - docker container for our script**

```
docker run -it --rm\
    --network=pipeline_default \
    pipeline_project \
    --pg-user=root \
    --pg-pass=root \
    --pg-host=pgdatabase \
    --pg-port=5432 \
    --pg-db=ny_taxi \
    --table_name=yellow_taxi_trips_2021_4 \
    --year=2021 \
    --month=04
```
  **For zones**
```
docker run -it --rm\
    --network=pipeline_default \
    pipeline_project \
    --pg-user=root \
    --pg-pass=root \
    --pg-host=pgdatabase \
    --pg-port=5432 \
    --pg-db=ny_taxi \
    --table_name=zones
```
**For taxi_trips**
```
docker run -it --rm\
    --network=pipeline_default \
    pipeline_project \
    --pg-user=root \
    --pg-pass=root \
    --pg-host=pgdatabase \
    --pg-port=5432 \
    --pg-db=ny_taxi \
    --table_name=taxi_trips
```
