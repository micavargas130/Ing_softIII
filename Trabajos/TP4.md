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
- Realizar búsquedas por tipo de media, color, etc.
- Hacer una compra - poner datos falsos de tarjeta de crédito ;)

#### 2- Investigación de los componentes
1. Describa los contenedores creados, indicando cuales son los puntos de ingreso del sistema

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
El contenedor que hace de API gateway es docker-compose_front-end_1

6. Cuando ejecuto este comando:
```bash
curl http://localhost/customers
```
6. ¿Cuál de todos los servicios está procesando la operación?
7. ¿Y para los siguientes casos?
```bash
curl http://localhost/catalogue
curl http://localhost/tags
```
8. ¿Como perisisten los datos los servicios?
9. ¿Cuál es el componente encargado del procesamiento de la cola de mensajes?
10. ¿Qué tipo de interfaz utilizan estos microservicios para comunicarse?
