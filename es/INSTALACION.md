Herramientas necesarias
-----------------------

Vangrant: https://www.vagrantup.com/

VirtualBox: https://www.virtualbox.org/

Instalacion
-----------

Luego de haber clonado el repositorio, dentro del mismo ejecutar:

1. git submodule init
2. git submodule update
3. Agregar los siguientes hosts a nuestro archivo de hosts apuntando a localhost. Ejemplo suponiendo que el IP local es 127.0.0.1

    127.0.0.1 admin.dev api.dev datastore.dev microsite.dev  workspace.dev

4. Iniciar la virtual con el comando:

    vagrant up --provision

5. Iniciar el servicio Web:

    vagrant ssh

    sudo supervisorctl start uwsgi

6. Para probar la demo, en tu navegador ingresá a http://workspace.dev:8080/


Usuarios y claves para acceder a la demo
----------------------------------------

Administrador: administrador/administrador

Editor: editor/editor

Publicador: publicador/publicador


Acceso a la maquina virtual
-------------------

Para acceder a la virtual via SSH

  vagrant ssh


Actualizacion
-------------

1. git pull origin master
2. vagrant provision (si la virtual esta corriendo) o vagrant up --provision (si la virtual esta apagada)


Cambiar los host del sistema
----------------------------

Para Linux y Mac el archivo es /etc/hosts, para Windows es \WINDOWS\system32\drivers\etc y para Windows 8 hay que tener en cuenta este detalle: 
http://www.howtogeek.com/122404/how-to-block-websites-in-windows-8s-hosts-file/