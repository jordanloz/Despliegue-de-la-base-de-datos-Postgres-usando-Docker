
-------# Despliegue-de-la-base-de-datos-Postgres-usando-Docker
------------------------------------------------------------------
creamos la red 

docker network create pg_network
------------------------------------------------------------------

docker run -d --name pg_server -e POSTGRES_PASSWORD=3125186957 -p 5432:5432 -v pg_db:/var/lib/postgresql/data --network pg_network postgres:15-bookworm
docker run -d --name pg_server -e POSTGRES_PASSWORD=3125186957 -p 5432:5432 -v pg_db:/var/lib/postgresql/data --network pg_network postgres
------------------------------------------------------------------

entramos al contenedor de postgres para ingresar y crear la base de datos

docker exec -it pg_server psql -U postgres

------------------------------------------------------------------


creamos la base de datos y la tabla e ingresamos el mensaje, todo esto dentro de pg_server
CREATE DATABASE tarea_db;
\c tarea_db
CREATE TABLE pg_tabla (
    id SERIAL PRIMARY KEY,
    mensaje VARCHAR(255)
);
INSERT INTO pg_tabla (mensaje) VALUES ('hola mundo');
------------------------------------------------------------------

creamos el cliente lo ponemos con la red para conectarse con pg_server; conectará a la base de datos tarea_db en el servidor pg_server.

docker run -it --name pg_client --network pg_network postgres psql -h pg_server -p 5432 -U postgres tarea_db
------------------------------------------------------------------


Selecionamos la base de datos para ver si se recibio el mensaje en la tabla 
SELECT * FROM pg_tabla;

------------------------------------------------------------------

docker inspect pg_server
docker network inspect pg_network

------------------------------------------------------------------

Detener los pg

docker stop pg_server pg_client
------------------------------------------------------------------

eliminar los contenedores, volumen y la red
docker rm pg_server pg_client
docker volume rm pg_db
docker network rm pg_network

Enlace al video de youtube: https://www.youtube.com/watch?v=gzrTW-Hjisc
