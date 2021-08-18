## Trabajo Práctico 2 - Introducción a Docker
### Desarrollo

#### 1 - Instalar Docker Community Edition

 - Diferentes opciones para cada sistema operativo
 - https://docs.docker.com/
```
Instalacion en Ubuntu
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

 - Ejecutar el siguiente comando para comprobar versiones de cliente y demonio.
``` 
 docker version
```

![imagen](https://user-images.githubusercontent.com/48757979/129815065-f59086ae-ce99-4aeb-a3bb-bb165ef28082.png)


#### 2 - Explorar DockerHub
-  Registrase en docker hub: https://hub.docker.com/
-  Familiarizarse con el portal

#### 3 - Obtener la imagen BusyBox

  - Ejecutar el siguiente comando, para bajar una imagen de DockerHub
  ```bash
  docker pull busybox
  ```
  ![imagen](https://user-images.githubusercontent.com/48757979/129813407-e9af7045-13f7-4831-b067-021516504a6c.png)

  - Verificar qué versión y tamaño tiene la imagen bajada, obtener una lista de imágenes locales:
```bash
docker images
```
![imagen](https://user-images.githubusercontent.com/48757979/129813426-1c02d9e7-3e0b-4650-8807-dfea4057b49f.png)

#### 4 - Ejecutando contenedores
  - Ejecutar un contenedor utilizando el comando **run** de docker:
```bash
docker run busybox
```

  - Explicar porque no se obtuvo ningún resultado

  - Especificamos algún comando a correr dentro del contendor, ejecutar por ejemplo:
```bash
docker run busybox echo "Hola Mundo"
```
![imagen](https://user-images.githubusercontent.com/48757979/129813460-514244bb-a8a7-435e-af23-658c884f5306.png)


  - Ver los contendores ejecutados utilizando el comando **ps**:
```bash
docker ps
```

  - Vemos que no existe nada en ejecución, correr entonces:
```bash
docker ps -a
```
![imagen](https://user-images.githubusercontent.com/48757979/129813509-05bb83ba-dbb1-4602-bf34-c56316bd977f.png)

  - Mostrar el resultado y explicar que se obtuvo como salida del comando anterior.

#### 5- Ejecutando en modo interactivo

  - Ejecutar el siguiente comando
```bash
docker run -it busybox sh
```

  - Para cada uno de los siguientes comandos dentro de contenedor, mostrar los resultados:
```bash
ps
uptime
free
ls -l /
```
  - Salimos del contendor con:
```bash
exit
```

![imagen](https://user-images.githubusercontent.com/48757979/129813703-b1fbd3dc-badc-4056-a588-5e2bdf5d2041.png)

#### 6- Borrando contendores terminados

  - Obtener la lista de contendores 
```bash
docker ps -a
```
![imagen](https://user-images.githubusercontent.com/48757979/129813736-b3ff1593-9752-4ce8-be87-782cc293e435.png)

  - Para borrar podemos utilizar el id o el nombre (autogenerado si no se especifica) de contendor que se desee, por ejemplo:
```bash
sudo docker rm superset
```
  - Para borrar todos los contendores que no estén corriendo, ejecutar cualquiera de los siguientes comandos:
```bash
docker rm $(docker ps -a -q -f status=exited)
```
```bash
docker container prune

```

#### 7- Montando volúmenes

Hasta este punto los contenedores ejecutados no tenían contacto con el exterior, ellos corrían en su propio entorno hasta que terminaran su ejecución. Ahora veremos cómo montar un volumen dentro del contenedor para visualizar por ejemplo archivos del sistema huésped:

  - Ejecutar el siguiente comando, cambiar myusuario por el usuario que corresponda. En linux/Mac puede utilizarse /home/miusuario):
```bash
docker run -it -v C:\Users\misuario\Desktop:/var/escritorio busybox /bin/sh
```
  - Dentro del contenedor correr
```bash
ls -l /var/escritorio
touch /var/escritorio/hola.txt
```
  - Verificar que el Archivo se ha creado en el escritorio o en el directorio home según corresponda.

![imagen](https://user-images.githubusercontent.com/48757979/129814840-61debb04-46d5-4f1e-ab98-7df2fe969144.png)


#### 8- Publicando puertos

En el caso de aplicaciones web o base de datos donde se interactúa con estas aplicaciones a través de un puerto al cual hay que acceder, estos puertos están visibles solo dentro del contenedor. Si queremos acceder desde el exterior deberemos exponerlos.

  - Ejecutar la siguiente imagen, en este caso utilizamos la bandera -d (detach) para que nos devuelva el control de la consola:

```bash
docker run -d daviey/nyan-cat-web

```
![imagen](https://user-images.githubusercontent.com/48757979/129814902-3648f57e-75a3-4385-9a20-16b312c6ad49.png)

  - Si ejecutamos un comando ps:
```bash
PS D:\> docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS               NAMES
87d1c5f44809        daviey/nyan-cat-web   "nginx -g 'daemon of…"   2 minutes ago       Up 2 minutes        80/tcp, 443/tcp     compassionate_raman
```
  - Vemos que el contendor expone 2 puertos el 80 y el 443, pero si intentamos en un navegador acceder a http://localhost no sucede nada.

  - Procedemos entonces a parar y remover este contenedor:
```bash
docker kill compassionate_raman
docker rm compassionate_raman
```
  - Vamos a volver a correrlo otra vez, pero publicando uno de los puertos solamente, el este caso el 80

```bash
docker run -d -p 80:80 daviey/nyan-cat-web
```
  - Accedamos nuevamente a http://localhost y expliquemos que sucede.
![imagen](https://user-images.githubusercontent.com/48757979/129814993-075bae64-e94d-44c1-8807-892c748f848a.png)


#### 9- Utilizando una base de datos
- Levantar una base de datos PostgreSQL

```bash
mkdir $HOME/.postgres
docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -v $HOME/.postgres:/var/lib/postgresql/data -p 5432:5432 -d postgres:9.4
```
- Ejecutar sentencias utilizando esta instancia

```bash
docker exec -it my-postgres /bin/bash
psql -h localhost -U postgres
#Estos comandos se corren una vez conectados a la base
\l
create database test;
\connect test
create table tabla_a (mensaje varchar(50));
insert into tabla_a (mensaje) values('Hola mundo!');
select * from tabla_a;
\q
exit
```
- Conectarse a la base utilizando alguna IDE (Dbeaver - https://dbeaver.io/, eclipse, IntelliJ, etc...). Interactuar con los objectos objectos creados.
- Explicar que se logro con el comando `docker run` y `docker exec` ejecutados en este ejercicio.

