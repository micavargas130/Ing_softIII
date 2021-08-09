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
git remote add origin https://github.com/micavargas130/Ing_softIII.git
luego subo todos los cambios locales a github haciendo un git push
