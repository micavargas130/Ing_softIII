# Trabajo Práctico 1
### Desarrollo:
#### 1 -Instalar Git
Comenzamos instalando Git desde https://git-scm.com/ siguiendo todos los pasos provistos por la pagina y 
luego instalamos un cliente visual, en mi caso opte por instalar TortoiseGit en Windows.

#### 2 - Crear un repositorio local y agregar archivos.
 - Creo una carpeta en la direccion Desktop/Universidad/4to/IngDeSoftwareIII
 - Para hacer que esa carpeta sea un repositorio utilizo el comando git init 
 - Desde Git Bash introduzco el comando cat > README.md para crear el README
 - Luego hago git add . para añadir todos los cambios
 - Por ultimo hago un commit de los cambios con el comando git commit -m “Cree el Readme”

#### 3 - Crear un repositorio remoto
Creo un nuevo repositorio utilizando la pagina github y asocio este repositorio remoto que se encuentra vacío 
con mi repositorio local utilizando el comando:
```
git remote add origin https://github.com/micavargas130/Ing_softIII.git
```
luego subo todos los cambios locales a github haciendo un git push

#### 4 - Familizarizarse con el concepto Pull Request
 Un pull request es una accion que se realiza para pedirle al propietario de un repositorio que nos deje
incorporar los cambios (commits) que hicimos en nuestro fork, el cual es un "clon" del repositorio original en el cual podemos realizar cambios. Si el propietario acepta el pull request, los cambios que hicimos en el fork se adieren al repositorio original.

Creo un branch local llamado "local" y agrego cambios en dicho branch.


```

git checkout -b local
nano README.md
git commit -a -m "Modifico README desde local"
```
subo el archivo README.md modificado a ese branch y hago un pull request

```
git push origin local
```

![image](https://user-images.githubusercontent.com/48757979/128758911-b7493f16-35a8-4e2c-9e88-621480ca2814.png)

![image](https://user-images.githubusercontent.com/48757979/128758990-1a30b386-8596-4616-a7e2-a2a5a3f2d41e.png)

#### 5 - Mergear codigo con conflictos
Para esta parte decidi instalar la herramienta Beyond Compare y configuré el Tortoise para que soporte esta herramienta.

Luego clone en un segundo directorio el repositorio creado en github
```
git clone https://github.com/micavargas130/Ing_softIII.git
```
En el clon inicial modifico el README.md agregando "Modifico desde el repositorio clonado"

Hago un commit y subo el cambio al master en git

```
git add .
git commit -m "Cambios desde clon"
git push
```
Luego desde el segundo clon modifico el README.md donde agrego "Cambio desde clon 2.0".
Cuando intento subir el cambio haciendo un commit y un push me muestra el siguiente error:

![image](https://user-images.githubusercontent.com/48757979/129221517-a4ff9573-c73e-44fe-8f83-46e5155416de.png)

usando Tortoise:

![image](https://user-images.githubusercontent.com/48757979/129221873-96fd3637-7bf5-4dd8-9f48-48b5551c7e71.png)

Para arreglar este error utilizo Beyond Compare en donde elijo que el README.md que se va a subir al master en github sea el del clon 2

![image](https://user-images.githubusercontent.com/48757979/129222718-ac32a9fc-8221-426a-b42c-fa0b2d5c88c7.png)

Una vez que el conflicto es marcado como "resuelto" puedo hacer commit y push y subirlo al master.

![image](https://user-images.githubusercontent.com/48757979/129223217-29a16084-31a6-493e-8520-beec6f37c7f0.png)



##### Explicar las versiones LOCAL, BASE y REMOTE.
- Local: Es el archivo que va a tener los cambios que hicimos sobre la base y que va a estar en un directorio local.
- Remote: Es un archivo que se encuentra en el repositorio remoto, el cual va a tener cambios que se le hicieron a la base y que se le hizo push para subirlo al repositorio.
-  Base: Es una version que se construye para poder resolver conflictos que surgieron al querer pushear codigo al repositorio remoto. Una vez que resolvemos el conflicto podemos subir la base al repositorio.


