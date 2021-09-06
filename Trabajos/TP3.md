## Trabajo Práctico 3 - Arquitectura de Sistemas Distribuidos

### Desarrollo:


#### 1- Sistema distribuido simple 
  - Ejecutar el siguiente comando para crear una red en docker
  ```bash
  docker network create -d bridge mybridge
  ```
  - Instanciar una base de datos Redis conectada a esa Red.
  ```bash
   docker run -d --net mybridge --name db redis:alpine
   ```
  - Levantar una aplicacion web, que utilice esta base de datos
  ```bash
    docker run -d --net mybridge -e REDIS_HOST=db -e REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest
  ```
  
  ![imagen](https://user-images.githubusercontent.com/48757979/131233894-d22edfdf-b077-46eb-9f20-54796eb68bd3.png)

  - Abrir un navegador y acceder a la URL: http://localhost:5000/
 
  ![imagen](https://user-images.githubusercontent.com/48757979/131233899-ef9df48c-4dc1-4f9d-9d8b-61f0984cc11d.png)

  - Verificar el estado de los contenedores y redes en Docker, describir:
    - ¿Cuáles puertos están abiertos?
   
    Los puertos que estan abiertos son: el 5000 para la aplicacion web y el 6379 para la base de datos Redis.
    
    ![imagen](https://user-images.githubusercontent.com/48757979/131233908-99118d63-13d6-430d-a1cc-48c1fb4e3af3.png)
    - Mostrar detalles de la red `mybridge` con Docker.
    
    ![imagen](https://user-images.githubusercontent.com/48757979/131233925-51972b95-c858-49f6-aa8d-263b39d51b4b.png)

    - ¿Qué comandos utilizó?
    
    Para ver los puertos abiertos utilicé el comando `docker ps -a` y para ver detalles de la red utilicé `docker network inspect mybridge`


#### 2- Análisis del sistema 
  - Siendo el código de la aplicación web el siguiente:
```python
import os

from flask import Flask
from redis import Redis


app = Flask(__name__)
redis = Redis(host=os.environ['REDIS_HOST'], port=os.environ['REDIS_PORT'])
bind_port = int(os.environ['BIND_PORT'])


@app.route('/')
def hello():
    redis.incr('hits')
    total_hits = redis.get('hits').decode()
    return f'Hello from Redis! I have been seen {total_hits} times.'


if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True, port=bind_port)
```
  - Explicar cómo funciona el sistema
 
    La forma en la que funciona el sistema es la siguiente: se especifica un puerto que va a ser el que escucha la aplicacion, y por cada vez que se ingrese a la d     ireccion de ese puerto va a ir incrementando el contador de "visitas".

  - ¿Para qué se sirven y porque están los parámetros `-e` en el segundo Docker run del ejercicio 1?

     El `-e` sirve para pasar parametros a un comando, en este caso, se lo utiliza para especificar que puerto va a estar "escuchando" la aplicacion web.
  
  - ¿Qué pasa si ejecuta `docker rm -f web` y vuelve a correr ` docker run -d --net mybridge -e REDIS_HOST=db -e REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest` ?
  
    Si hacemos `docker rm -f web` se va a destruir el contenedor que tiene la aplicacion web, y con el  docker run -d --net mybridge -e REDIS_HOST=db -e      REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest` lo que va a pasar es que se va a volver a intalar la aplicacion.
  
  - ¿Qué occure en la página web cuando borro el contenedor de Redis con `docker rm -f db`?
  
     Si borro el contenedor de Redis, la aplicacion web no va a poder andar ya que necesita de los datos que le provee Redis y por lo tanto, si voy al    localhost:5000 me muestra un mensaje de error.
  
  - Y si lo levanto nuevamente con `docker run -d --net mybridge --name db redis:alpine` ?
 
    Si levanto de nuevo el base de datos, la aplicacion web vuelve a funcionar normalmente.
  
  - ¿Qué considera usted que haría falta para no perder la cuenta de las visitas?
  
  - Para eliminar los elementos creados corremos:
  ```bash
  docker rm -f db
  docker rm -f web
  docker network rm mybridge
  ```
  
#### 3- Utilizando docker compose 
  - Normalmente viene como parte de la solucion cuando se instaló Docker
  - De ser necesario instalarlo hay que ejecutar:
  ```bash
  sudo pip install docker-compose
  ```
  - Crear el siguente archivo `docker-compose.yaml` en un directorio de trabajo:

```yaml
version: '3.6'
services:
  app:
    image: alexisfr/flask-app:latest
    depends_on:
      - db
    environment:
      - REDIS_HOST=db
      - REDIS_PORT=6379
    ports:
      - "5000:5000"
  db:
    image: redis:alpine
    volumes:
      - redis_data:/data
volumes:
  redis_data:
```

  - Ejecutar `docker-compose up -d`

![imagen](https://user-images.githubusercontent.com/48757979/131234320-999a2607-7383-4362-b161-e0b20341faef.png)

  - Acceder a la url http://localhost:5000/
  - Ejecutar `docker ps`, `docker network ls` y `docker volume ls`
  - ¿Qué hizo **Docker Compose** por nosotros? Explicar con detalle.
  - Desde el directorio donde se encuentra el archivo `docker-compose.yaml` ejecutar:
  ```bash
  docker-compose down
  ```
  ![imagen](https://user-images.githubusercontent.com/48757979/131234330-c4360806-4a77-4f52-afb9-a08a577700db.png)

 
#### 4- Aumentando la complejidad, análisis de otro sistema distribuido.
Este es un sistema compuesto por:

- Una aplicación web de Python que te permite votar entre dos opciones
- Una cola de Redis que recolecta nuevos votos
- Un trabajador .NET o Java que consume votos y los almacena en...
- Una base de datos de Postgres respaldada por un volumen de Docker
- Una aplicación web Node.js que muestra los resultados de la votación en tiempo real.

Pasos:
- Clonar el repositorio https://github.com/dockersamples/example-voting-app
- Abrir una línea de comandos y ejecutar
```bash
cd example-voting-app
docker-compose -f docker-compose-javaworker.yml up -d
```
- Una vez terminado acceder a http://localhost:5000/ y http://localhost:5001
- Emitir un voto y ver el resultado en tiempo real.
![imagen](https://user-images.githubusercontent.com/48757979/131234341-7ac6116f-e921-438a-abdb-449d68068620.png)
- Para emitir más votos, abrir varios navegadores diferentes para poder hacerlo
Abro el http://localhost:5000/  desde otro navegador

![imagen](https://user-images.githubusercontent.com/48757979/131234353-c50fd956-0bac-4d6d-afdd-dd6f7d3a0e7d.png)

- Explicar como está configurado el sistema, puertos, volumenes componenetes involucrados, utilizar el Docker compose como guía.

#### 5- Análisis detallado
- Exponer más puertos urtos para ver la configuración de Redis, y las tablas de PostgreSQL con alguna IDE como dbeaver.
- Revisar el código de la aplicación Python `example-voting-app\vote\app.py` para ver como envía votos a Redis.
La forma en la que se envian los votos es que la app genera cookies de forma aleatoria y cuando se hace click sobre uno de los cuadrados para votar, se hace un post con la cookie del votante y la eleccion.
- Revisar el código del worker `example-voting-app\worker\src\main\java\worker\Worker.java` para entender como procesa los datos.

- Revisar el código de la aplicacion que muestra los resultados `example-voting-app\result\server.js` para entender como muestra los valores.

- Escribir un documento de arquitectura sencillo, pero con un nivel de detalle moderado, que incluya algunos diagramas de bloques, de sequencia, etc y descripciones de los distintos componentes involucrados es este sistema y como interactuan entre sí.

