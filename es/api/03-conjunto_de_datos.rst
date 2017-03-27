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

Los valores posibles para el campo ``license`` son::

http://creativecommons.org/licenses/by/4.0/
http://creativecommons.org/licenses/by-sa/4.0/
http://www.gnu.org/licenses/fdl-1.3.en.html
http://opendefinition.org/licenses/odc-pddl/
http://opendatacommons.org/licenses/by/
http://opendatacommons.org/licenses/odbl/
http://creativecommons.org/publicdomain/zero/1.0/

Si usted requiere otra licencia, por favor contáctese con support@junar.com

Los valores posibles para el campo ``frequency`` son::

yearly
monthly
weekly
daily
hourly
ondemand

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
