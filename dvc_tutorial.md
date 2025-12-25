
## DVC Uso

Normalmente iniciamos con: 
```bash
git init
dvc init
```

Para crear un dvc init en un folder cuando el main esta en otra carpeta: 
```bash
dvc init --subdir
```

En caso nosotros queramos versionar una carpeta de imagenes, acá especificamos la data que queremos versionar:
```bash
dvc add ./images
```

Primero añadimos images.dvc y gitignore
```bash
git add images.dvc .gitignore
```

Ahora realizamos un commit de nuestros cambios
```bash
git commit -m "Added raw data"
```

La información de nuestras imagenes se guardara en el cache de .dvc/cache/ (el valor hash). Lo unico que guarda dvc es la información de estas imagenes (la localización de los imagenes y sus valores hash). Podemos almacenar nuestras imagenes en Google Drive, Google Cloud Stored.
```bash
dvc remote add -d <name-of-the-storage> <storage-uri>
```
En este ejemplo, lo guardaremos en google drive
```bash
dvc remote add -d gdrive gdrive://id-of-the-folder-drive
```

Ahora, tenemos que subir nuestra información. Primero instalamos la libreria especifica de dvc para gdrive.
```bash
pip install "dvc[gdrive]"
dvc push
```

Para traernos nuestras imagenes de google drive
```bash
dvc pull
```

Para verificar cambios de nuevos archivos en dvc
```bash
dvc status
```

Lo que hacemos es de nuevo añadir estos cambios
```bash
dvc add ./images
```

Ahora, lo que hacemos es agregar estos cambios al repo de git
```bash
git add images.dvc
git commit -m "updated my dataset"
dvc push
```

Queremos volver a una vieja versión de mi data,  porque mi modelo performa peor. Para hacerlo seguimos los siguientes pasos.
Primero, con git log podemos ver nuestros commits antiguos
```bash
git log
```
Luego, para ir a un commit previo usamos checkout
```bash
git checkout id-del-commit-sacado-de-log
```
Ahora, borremos las imagenes y el cache. 
```bash
rn .rf ./images
ls .dvc/cache
rm -rf .dvc/cache
```

Si yo hago ahora dvc pull, solo me bajare las imagenes existentes en mi primer commit.
```bash
dvc pull
```

Para verificar que data tienes subida en tu repositorio. Necesitas poner tus credenciales cada vez q uses el comando. 
```bash
dvc list <repo-url> <directory.path>
```

Para descargar esta informacion, usamos dvc get 
```bash
dvc get <repo-url> <directory.path>
```

Si queremos usar dvc import, tenemos que estar dentro del dvc repository. Este comando sirve para clonar el carpeta.dvc, que trackea nuestras imagenes. Cuando nosotros queramos clonar nuestra data en un nuevo repo, tenemos que usar dvc import.

```bash
mkdir new_project
cd new_project
git init 
dvc init
dvc import <repo-url> <directory.path> 
```