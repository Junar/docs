Visualizaciones
====================

Las llamadas GET nos devuelve los siguientes parámetros  :


::

  GET   /api/v2/visualizations.json
  GET   /api/v2/visualizations/{guid}.json


Y los parámetros  son los siguientes.

- guid: Identificador del recurso,
- title: Título del recursos.
- description: Descripción del recursos.
- category_name: Nombre de la categoría.
- endpoint: Viene en null y se utiliza para concoordar con otros recursos de datos
- tags : Opcional. Tags separados por coma.
- user : Usuario que publica el recurso.
- parameters : Viene en null y se utiliza para concoordar con otros recursos de datos
- created_at : Fecha de creación de la versión del recurso
- link : Link a la vista del recurso en el portal

Por ejemplo el ``GET   /api/v2/visualizations/{guid}.json`` muestra los siguientes datos: 

.. code-block:: json

  {
    "result": null,
    "endpoint": null,
    "description": "prueba apeso 2",
    "parameters": [ ],
    "tags": [ ],
    "created_at": "2016-03-04T17:43:04",
    "title": "prueba p",
    "link": null,
    "user": "junarcity",
    "guid": "PRUEB-P",
    "category_name": "Financial"
  }

