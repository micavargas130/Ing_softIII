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
Para esta parte decidi instalar la herramienta P4V y configuré el Tortoise para que soporte esta herramienta.


