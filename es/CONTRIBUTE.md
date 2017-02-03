# Entorno de desarrollo

Entre a la máquina virtual y detenga uWSGI usando 
```
$ sudo supervisorctl stop uwsgi
```
luego inicie el servidor de desarrollo de django para la aplicación que desea modificar usando, por ejemplo, para workspace:
```
$ python manage.py runserver_plus 0.0.0.0:3015 --settings=workspace.settings
```

El puerto 3015 está vinculado al 8015 de la maquina host. 
Abra en un navegador la url `http://workspace.dev:8015/`. 

De la misma forma, para las demás aplicaciones django, existen puertos vinculados (vea `/Vagrantfile` para mas información).


# Base de datos y Migraciones

Si haces cambios en los modelos dentro de `core/models.py`, debers usar django migrations para crear una migración de base de datos. Primero modifica el modelo que deseas cambiar. Luego ejecuta, dentro del entorno de la máquina virtual, 
```
$ python manage.py schemamigration core --auto --settings=core.settings
```
Verás que dentro de la carpeta `core/migrations` se ha creado un nuevo archivo. Para ejecutar la migración que creaste, ejecuta
```
$ python manage.py migrate core --settings=core.settings
```
Puedes acceder al shell de la base de datos usando 
```
$ python manage.py dbshell
```
para controlar tus tablas.

Verifica que el cambio que deseas esté reflejado correctamente en ese archivo de migración y agregalo al repositorio.
