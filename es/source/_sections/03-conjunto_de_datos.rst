Conjunto de Datos
==================

Podemos consumir la metadata de los recursos de tipo conjunto de datos (dataset) publicados en Junar a través de una simple llamada GET.
Las llamadas GET nos pueden traer una lista completa de todos los conjuntos de datos o solo de un GUID específico.
Las llamadas GET nos devuelven los siguientes parámetros :

::

    GET   /api/v2/datasets.json
    GET   /api/v2/datasets/{guid}.json

- guid : Identificador del recurso,
- title : Título del conjunto de datos
- description : Descripción del conjunto de datos
- categories : Nombre de la categoría
- endpoint : Url apuntando al recurso con los datos (archivos o página web)
- tags : Opcional. Tags separados por coma.
- user : Usuario que publica el recurso.
- parameters : Parámetros  que tiene el recurso.
- created_at : Fecha de creación de la versión del recurso
- link : Link a la vista del recurso en el portal

Por ejemplo::

 http://providencia.cloudapi.junar.com/api/v2/datasets/TRAFI-EN-CICLO-DE-PROVI.json?auth_key=MI_AUTH_KEY 


retorna

.. code-block:: json

    {
    "result": null,
    "endpoint": "http://tv.providencia.cl/ws/ws-css3.php",
    "description": "Toma la estadística de 1 mes hacia atrás, desde el dia de la consulta, y publica el promedio por dia de la semana, por hora y sentido.",
    "parameters": null,
    "tags": [
        "trafico",
        "Ciclovias",
        "providencia",
        "Promedio"
    ],
    "timestamp": null,
    "created_at": "2016-05-23T14:25:53",
    "title": "Tráfico en Ciclovías de Providencia",
    "modified_at": "2016-05-23T14:27:20",
    "category_id": 41246,
    "link": null,
    "user": null,
    "guid": "TRAFI-EN-CICLO-DE-PROVI",
    "category_name": "Ciclovias"
    }

Publicación de conjuntos de datos usando API
--------------------------------------------

Si usted posee una auth key privada, puede realizar la publicación de conjuntos de datos de manera sistemática a través de la API. Puede crear conjuntos de datos desde archivos locales (haciendo un PUSH hacia los servidores de Junar) o definiendo la ruta absoluta en la cual se encuentra el archivo. 

El metodo de publicación soporta llamadas POST/PUT/PATCH y recibe los siguientes parámetros :

::

    POST  /api/v2/datasets.json
    PUT   /api/v2/datasets/:guid.json
    PATCH /api/v2/datasets/:guid.json



- title : Título del conjunto de datos. Máximo 100 caracteres.
- description : Descripción del conjunto de datos. Máximo 250 caracteres.
- category : Slug de la categoría para clasificar los recursos. Debe coincidir con alguna de las categorías de la cuenta
- notes : Opcional. Texto de la nota del conjunto de datos. Máximo 10.000 caracteres. Soporta texto enriquecido.
- end_point : Url apuntando al recurso con los datos (archivos o página web). Obligatorio si no se ha agregado un ``file``.
- file : Archivo a subir a la plataforma. Obligatorio si no se ha agregado un ``end_point``.
- license : Opcional. Tipo de licencia que aplica sobre el conjunto de datos.
- spatial : Opcional. Zona geográfica a la cual aplica el conjunto de datos. Máximo 100 caracteres.
- frequency : Opcional. Frecuencia de actualización del recurso.
- mbox : Opcional. Correo electronico de quien administra el conjunto de datos.
- tags : Opcional. Tags separados por coma.
- status: Opcional. Estado del conjunto de datos.

Los valores posibles para el campo ``license`` son:
     

- `Attribution (CC BY) <http://creativecommons.org/licenses/by/4.0/>`_
- `Attribution ShareAlike (CC BY-SA) <http://creativecommons.org/licenses/by-sa/4.0/>`_
- `The GNU Free Documentation License <http://www.gnu.org/licenses/fdl-1.3.en.html>`_
- `Open Data Commons Public Domain Dedication and Licence (PDDL) <http://opendefinition.org/licenses/odc-pddl/>`_
- `Open Data Commons Attribution License <http://opendatacommons.org/licenses/by/>`_
- `Open Data Commons Open Database License (ODbL) <http://opendatacommons.org/licenses/odbl/>`_
- `Creative Commons CC0 Public Domain Dedication <http://creativecommons.org/publicdomain/zero/1.0/>`_


Si usted requiere otra licencia, por favor contáctese con support@junar.com

Los valores posibles para el campo ``frequency`` son:

- yearly
- monthly
- weekly
- daily
- hourly
- ondemand

Los valores posibles para el campo ``status`` son:

- published
- draft
- pending_review

Si no se envía el parámetro el valor por defecto es pending_review


Todas las llamadas en caso de éxito devuelven el mismo resultado, por ejemplo:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://1995/46721/71341786542282142096488420671282999110",
    "description": "res",
    "parameters": null,
    "tags": [ "" ],
    "created_at": "2016-02-10T17:10:39",
    "title": "resto",
    "link": null,
    "user": "junarcity",
    "guid": "RESTO",
    "category_name": "Financial"
  }



#######################
Ejemplo del método POST
#######################

El método POST debe usarse cuando se desea crear un nuevo conjunto de datos. 

.. code-block:: text 

    curl -X  POST \
  http://demo.cloudapi.junar.com/api/v2/datasets.json \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: 58b98775-8b83-4f7e-ed39-cd2225a586f5' \
  -F auth_key=238415d936545e1994476b900d41db07883eff72 \
  -F 'title=Subido desde API' \
  -F description=Subido desde API utilizando el método POST \
  -F category=GIS \
  -F file=@example.csv


En caso que el conjunto de datos haya sido creado satisfactoriamente, recibiremos esta respuesta:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://72121/15031/128335633035380119355724536018989239175",
    "description": "postman",
    "parameters": null,
    "tags": [],
    "timestamp": null,
    "created_at": "2017-06-16T15:20:38Z",
    "title": "Subido desde API",
    "modified_at": "2017-06-16T15:20:38Z",
    "category_id": 83274,
    "link": null,
    "user": null,
    "guid": "SUBID-DESDE-API",
    "category_name": "GIS"
  }



######################
Ejemplo del método PUT
######################

El método PUT es utilizado para actualizar un conjunto de datos ya creado. Este método requiere el envío de todos los valores, es decir, si falta un valor para los campos obligatorios devolverá error; en caso que el campo no sea obligatorio lo enviará vacío.

.. code-block:: text 

    curl -X PUT \
  http://junarops.cloudapi.junar.com/api/v2/datasets/SUBID-DESDE-API.json \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: 5ff0caf2-6639-07dc-9c70-c01ba59bac9a' \
  -F auth_key=238415d936545e1994476b900d41db07883eff72 \
  -F 'title=Nuevo titulo' \
  -F 'description=Este dataset ha sido actualizado utilizando el método PUT' \
  -F category=GIS \
  -F file=@example.xlsx


En caso que el conjunto de datos haya sido actualizado satisfactoriamente, recibiremos esta respuesta:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://72121/15031/272512842402740134577364585136929799929",
    "description": "Este dataset ha sido actualizado utilizando el método PUT",
    "parameters": null,
    "tags": [],
    "timestamp": null,
    "created_at": "2017-06-12T13:15:06Z",
    "title": "Nuevo titulo",
    "modified_at": "2017-06-16T17:31:02Z",
    "category_id": 83274,
    "link": null,
    "user": null,
    "guid": "SUBID-DESDE-API",
    "category_name": "GIS"
    }



########################
Ejemplo del método PATCH
########################

El método PATCH es utilizado para actualizar un conjunto de datos ya creado. En este caso debe enviarse solo los valores que se desean cambiar; el resto de los valores se mantiene. 

En este ejemplo actualizaremos sólo la decripción del conjunto de datos:

.. code-block:: text 

    curl -X PATCH \
    http://demo.cloudapi.junar.com/api/v2/datasets/SUBID-DESDE-API.json \
    -H 'cache-control: no-cache' \
    -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
    -H 'postman-token: 0527ec7f-f4ac-3bd6-a5f7-9e521076347e' \
    -F auth_key=238415d936545e1994476b900d447407883eff72 \
    -F description=Actualizado desde API utilizando el método PATCH \


En caso que el conjunto de datos haya sido actualizado satisfactoriamente, recibiremos esta respuesta:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://72121/15031/272512842402740134577364585136929799929",
    "description": "Esta descripción ha sido actualizado utilizando el método PUT",
    "parameters": null,
    "tags": [],
    "timestamp": null,
    "created_at": "2017-06-12T13:15:06Z",
    "title": "Nuevo titulo",
    "modified_at": "2017-06-16T17:31:02Z",
    "category_id": 83274,
    "link": null,
    "user": null,
    "guid": "SUBID-DESDE-API",
    "category_name": "GIS"
    }