Django - Docker
====================

Steps for start project in Django  using Docker, We use `<projectname>` as reference to project name

### Prereq

* Installed  **Docker**.

## Step 1 - Structure.

**_Download the structure of the project_**

Se descarga el proyecto que contiene la estructura general.
```
git clone https://github.com/lagowski/docker-django.git <projectname>
```

**_Remove Git folder_**

Remove  `.git` and create new repository.

```
cd <projectname>
rm -rf .git/
```

**_Create new repository inside of project_**

Initialize Version Control inside of folder.

```
git init
```

## Step 2 - Docker Image

**_Create Docker Image (NOT NECESSARY STEP)_**

Create Docker Image of the project. Image will contains installation of requerements that are inside of `requirements.txt`.

The `requirements.txt` contains basic requirements for start and run app with Django. If you need add sometnig more this is the moment.

```
docker build -t <projectname>:1.0 .
```

Always when you modify file `requirements.txt` you have to repeat this step.


**_Configure Docker Compose_**

In file `docker-compose.yml` you have to modify name of image that will be used. The name of image described in last step. You have to modify this line:
```
image: <projectname>:1.0
```


## Step 3 -  Create Django Project

**_Project creation_**
Create the project with same commandslike in Django.
```
docker-compose run web django-admin startproject <projectname> .
```

**_Try the system_**
To see if the system is working propertly Para probar si el sistema está funcionando correctamente se ejectua el siguiente comando. En el navegador se puede revisar la aplicación en la siguiente dirección `http://<ip-máquina:8000>`. El puerto de salida puede ser configurado en el fichero `docker-compose.yml`.
```
docker-compose up
```

**_Stop the system_**
Se detiene el sistema de ser necesario para continuar con las configuraciones.
```
Ctrl-C
```

## Step 4 - Create Application Inside of Project

For create app we need use a django command, but we want it inside of project:
```
docker-compose run web sh -c "cd <projectname>; python manage.py startapp app"
```

## Step 5 - Create User"

Another command of django
```
docker-compose run web sh -c "cd <projectname>; python manage.py createsuperuser"
```

## Step 6 - Production Environment

To use this project in production environment we nedd make some changes

At the end  of file `projectname/settings.py` add this line:

```
STATIC_ROOT = './static/'
```

Add line `command: ./run-production.sh` to file `docker-compose.yml` thats finally could be like that:

```
web:
  image: projectname:1.0
  command: ./run-production.sh
  volumes:
    - .:/code
  ports:
    - "8000:80"
```

Finally modify name  `projectname` inside of `conf/app.ini`.

### Usefull links

* <a target="_blank" href="https://docs.docker.com/compose/django/">Docker Compose con proyectos Django</a>
* <a target="_blank" href="https://docs.djangoproject.com/es/1.9/intro/tutorial01/">Primeros pasos en projectos con Django</a>
