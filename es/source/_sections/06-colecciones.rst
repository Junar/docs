Colecciones
===========
Podemos obtener un listado de las colecciones pubicadas pertenecientes a un portal de datos abiertos, además de un listado de los recursos incluidos en cada una de las colecciones publicadas a través de un método GET

::

  GET   /api/v2/dashboards.json
  GET   /api/v2/dashboards/{guid}.json


Las llamadas GET nos devuelve los siguientes parámetros :

- guid: Identificador del recurso,
- title: Título del recurso
- description: Descripción del recurso
- category_name: Nombre de la categoría
- endpoint: Viene en null y se utiliza para concoordar con otros recursos de datos
- tags : Opcional. Tags separados por coma.
- user : Usuario que publica el recurso.
- parameters : Viene en null y se utiliza para concoordar con otros recursosde datos
- created_at : Fecha de creación de la versión del recurso
- link : Link a la vista del recurso en el portal

Por ejemplo el ``GET   /api/v2/dashboards/{guid}.json`` muestra los siguientes datos: 

.. code-block:: json

	{
		"result": null,
		"endpoint": null,
		"description": "ph",
		"parameters": null,
		"tags": [ "" ],
		"created_at": "2016-02-11T09:07:19",
		"title": "Prueba HTML",
		"link": null,
		"user": "junarcity",
		"guid": "PRUEB-HTML",
		"category_name": "Category",
		"resources": 
		[
			{
				"doc_type": "datastream",
				"w": 4,
				"parameters": "",
				"h": 4,
				"last_revision": 
				{
					"slug": "api-publish-test",
					"modified_at": "2016-02-11T09:08:08",
					"id": 211856,
					"parameters": [],
					"title": "API Publish Test"
				},
		 
				"order": 0,
				"link": "http://junarcity.site.beta.junar.com/dataviews/225402/-/",
				"last_published_revision": 
				{
					"slug": "api-publish-test",
					"modified_at": "2016-02-11T09:08:08",
					"id": 211856,
					"parameters": [],
					"title": "API Publish Test"
				},
					"y": 0,
					"x": 0,
					"guid": "API-PUBLI-TEST-50073",
					"type": "ds",
					"id": 225402
			},
			{
				"parameters": "",
				"h": 4,
				"html": "<iframe width='420' height='315' src='https://www.youtube.com/embed/v2I9eSUTPjY' frameborder='0' allowfullscreen></iframe>",
				"w": 4,
				"y": 0,
				"x": 4,
				"type": "html",
				"order": 0
			}
		]
	}
	
	
	
	









