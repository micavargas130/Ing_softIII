## Trabajo Práctico 4 - Arquitectura de Microservicios

### 4- Desarrollo:


#### 1- Instanciación del sistema
- Clonar el repositorio https://github.com/microservices-demo/microservices-demo
```bash
mkdir -p socks-demo
cd socks-demo
git clone https://github.com/microservices-demo/microservices-demo.git
```
- Ejecutar lo siguiente
```bash
cd microservices-demo
docker-compose -f deploy/docker-compose/docker-compose.yml up -d
```
- Una vez terminado el comando `docker-compose` acceder a http://localhost
- Generar un usuario

![imagen](https://user-images.githubusercontent.com/48757979/136665655-6509b1f2-c19e-4351-89fc-1ba2103e587c.png)

- Realizar búsquedas por tipo de media, color, etc.
- Hacer una compra - poner datos falsos de tarjeta de crédito ;)

![imagen](https://user-images.githubusercontent.com/48757979/136665666-e1025f2f-d3fe-4533-a57b-6f722056bf4d.png)


#### 2- Investigación de los componentes
1. Describa los contenedores creados, indicando cuales son los puntos de ingreso del sistema

Se crearon los siguentes contenedores:

- weaveworksdemos/front-end: se encarga de todo el diseño de la pagina  
- weaveworksdemos/edge-router: Es un punto de ingreso al sistema.
- weaveworksdemos/carts: maneja las acciones que se hacen sobr el carrito de compras
- weaveworksdemos/catalogue: se trata el catalogo de medias en donde estan cargados todos los productos. Es un punto de ingreso del sistema.
- weaveworksdemos/queue-master: administra los pedidos dentro de la pagina
- weaveworksdemos/orders: maneja las ordenes de compras que se hacen en la pagina
- weaveworksdemos/user: maneja la informacion del usuario. Es un punto de entrada del sistema
- weaveworksdemos/payment: maneja la informacion del metodo de pago (la tarjeta)
- weaveworksdemos/shipping: maneja la informacion del envio que ingresa el usuario

3. Clonar algunos de los repositorios con el código de las aplicaciones
```bash
cd socks-demo
git clone https://github.com/microservices-demo/front-end.git
git clone https://github.com/microservices-demo/user.git
git clone https://github.com/microservices-demo/edge-router.git
.
.
```
3. ¿Por qué cree usted que se está utilizando repositorios separados para el código y/o la configuración del sistema? Explique puntos a favor y en contra.

En mi opinion, se utilizan repositorios separados para facilitar el deploy de codigo y hacer que sea mas flexible y mas facil de mantener. Como podemos ver, en un repositorio se incluye todo lo relativo al codigo y en otro se pone todo lo que es la configuracion. 

Puntos a favor: 
- Flexible
- Facil de administrar.
- Facil de mantener
- Mas ordenado 

Desventajas:
- Fragmentacion de los equipos de trabajo
- Las bibliotecas van a tener que actualizarse a medida que se incluyan cambios. 

4. ¿Cuál contenedor hace las veces de API Gateway?

   El contenedor que hace de API gateway es el del ```front-end```

5. Cuando ejecuto este comando:
```bash
curl http://localhost/customers
```
6. ¿Cuál de todos los servicios está procesando la operación?

    La operacion esta siendo procesada por el servicio ```User```

7. ¿Y para los siguientes casos?
```bash
curl http://localhost/catalogue
curl http://localhost/tags
```

Para ambos casos se utiliza el servicio ```Catalogue```

8. ¿Como perisisten los datos los servicios?
    Tenemos multiples bases de datos en donde van a persistir todos los datos de la pagina. Las BD ´´ḿongo´´ almacenan    toda la informacion del usuario (nombre, correo, nombre de usuario e informacion de la tarjeta) y tambien todo lo que se guarda en el carrito de compras, luego tenemos una BD mysql que se utiliza para guardar la informaicion del catalogo de medias de la pagina. 

9. ¿Cuál es el componente encargado del procesamiento de la cola de mensajes?
   El componente que procesa los mensajes es RabbitMQ

10. ¿Qué tipo de interfaz utilizan estos microservicios para comunicarse?

Los microservicios estan utilizando la interfaz rabbitmq
