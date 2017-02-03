Data Views
===============

A data view is a user built resource based on a custom selection of rows and columns over a specific table of a dataset. This data selection can include some level of modifications over the original data, such as headers, column formatting and data type selection, as well as parametric filters to query over large databases. 

There is a special API request for a data view to return all its data content: 

::
  
  GET   /api/v2/datastreams/{guid}/data.{format}
  
  
The path that ends in data. ``{format}``  returns a new  ``result`` field where the data of the resource origin is located

The ``{format}`` of the output can be one of the following:

-	json: Returns a json that follows the structure detailed in the next section.

-	ajson: Returns a json array of arrays inside a 'result' element. 

-	pjson: Returns a json array with labels for each element. It automatically uses the first row of the data view as the source for the labels.

-	jsonp: Returns the data in JSONP format

-	csv: Returns a comma separated value text document.

-	xml: Returns the data as an XML structure.

-	xls: Returns a link inside an fUri element to download an XLS version of the data view.

There is a special endpoint obtained replacing ``data.{format}`` with ``tableau.html`` which enables the Junar API to be used as a data source on the Tableau application.

JSON structure of the data.json output
---------------------------------------

The result is an Argument object as a recursive data structure with the following properties:

- fType: Indicates the data type of the Argument. Its values can be  ARRAY | TEXT | NUMBER | DATE | LINK. ARRAY data type indicates that the argument contains a TABLE. The default data type of content inside a TABLE is TEXT, unless it has been defined as another data type during the dataview creation process. Data types can be modified with each API query.
- When the data type is ARRAY, fRows y fCols indicate the number of rows and columns of a TABLE. In the same way, fArray contains the data of a TABLE as an Argument object array.
- When the data type is TEXT, the value is contained in fStr. When the data type is NUMBER the value is contained in fNum. When the data type is DATE the value is contained in fNum as epoch time.
- An Argument can also contain a LINK. In such cases when fType is LINK, the corresponding URI comes inside fUri and the text is contained in fStr.
- An array can also contain the value fHeader. If fHeader equals "true", that field is part of the headers defined for the data view.
- When a data type is ERROR, there was a problem executing the data view. The error message will be contained in fStr.
- When an error occurs when connecting to the source, the result is replaced with the last positive result from that source.
- To see if the results are up to date, there is an additional fTimestamp property. It contains the POSIX time from the **last valid access to the data source**. If fTimestamp is equal to 0, it means that the result was obtained at the same instant of the query.


Data output examples
-------------------------------

Output with *data.json* value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code-block:: json

	{
	  "result": {
		"fLength": 8,
		"fType": "ARRAY",
		"fTimestamp": 1457102225405,
		"fArray": [
		  {
			"fStr": "Congreso Nacional - Biblioteca del Congreso",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Cifras en miles de $",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Código BIP",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Nombre de Proyecto",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Monto Identificado",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Remodelación Administración Valparaíso",
			"fType": "TEXT"
		  },
		  {
			"fStr": "26,505",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fHeader": true,
			"fType": "TEXT"
		  },
		  {
			"fStr": "Bóveda y sala preservación colecciones valiosas",
			"fHeader": true,
			"fType": "TEXT"
		  },
		  {
			"fStr": "111,564",
			"fHeader": true,
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Raparaciones daños terremoto, Sector Biblioteca",
			"fType": "TEXT"
		  },
		  {
			"fStr": "66,440",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Proyectos de climatización en Santiago y Valparaíso",
			"fType": "TEXT"
		  },
		  {
			"fStr": "62,101",
			"fType": "TEXT"
		  },
		  {
			"fStr": "TOTAL IDENTIFICADO",
			"fType": "TEXT"
		  },
		  {
			"fStr": "",
			"fType": "TEXT"
		  },
		  {
			"fStr": "266,610",
			"fType": "TEXT"
		  }
		],
		"fRows": 8,
		"fCols": 3
	  },
	  "endpoint": "http://www.dipres.gob.cl/574/articles-74267_doc_xls.xls",
	  "description": "Inversiones BCN durante el año 2011 según art. 24 de Ley de Presupuestos N° 18.482",
	  "parameters": [],
	  "tags": [],
	  "created_at": "2012-06-04T14:12:52",
	  "title": "Nóminas de Iniciativas de Inversión (M$) Biblioteca del Congreso Nacional",
	  "link": null,
	  "user": "publicador",
	  "guid": "NOMIN-DE-BIBLI-DEL-12877",
	  "category_name": "Finanzas"
	}


Output with *data.pjson* value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: json
	
	{
  	"result": [
    	{
      	"GASTO-REGISTRADO": "",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "Versión : Ejecución DIPRES",
      	"MINISTERIO-DE-HACIENDA": "Dirección de Presupeustos"
    	},
    	{
      	"GASTO-REGISTRADO": "",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "Moneda Nacional - Miles de Pesos - Monto Devengado",
      	"MINISTERIO-DE-HACIENDA": ""
    	},
    	{
      	"GASTO-REGISTRADO": "",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "11  PARTIDA : MINISTERIO DE DEFENSA NACIONAL",
      	"MINISTERIO-DE-HACIENDA": ""
    	},
    	{
      	"GASTO-REGISTRADO": "Ejecución acumulada al Primer Trimestre",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "Clasificación Económica",
      	"MINISTERIO-DE-HACIENDA": "Subt."
    	},
    	{
      	"GASTO-REGISTRADO": "350,239,182",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "INGRESOS",
      	"MINISTERIO-DE-HACIENDA": ""
    	},
    	{
      	"GASTO-REGISTRADO": "1,787,369",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "RENTAS DE LA PROPIEDAD",
      	"MINISTERIO-DE-HACIENDA": "06"
    	},
    	{
      	"GASTO-REGISTRADO": "85,459,417",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "INGRESOS DE OPERACION",
      	"MINISTERIO-DE-HACIENDA": "07"
    	},
    	{
      	"GASTO-REGISTRADO": "2,464,229",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "OTROS INGRESOS CORRIENTES",
      	"MINISTERIO-DE-HACIENDA": "08"
    	},
    	{
      	"GASTO-REGISTRADO": "228,441,645",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "APORTE FISCAL",
      	"MINISTERIO-DE-HACIENDA": "09"
    	},
    	{
      	"GASTO-REGISTRADO": "1,553",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "VENTA DE ACTIVOS NO FINANCIEROS",
      	"MINISTERIO-DE-HACIENDA": "10"
    	},
    	{
      	"GASTO-REGISTRADO": "-200,000",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "VENTA DE ACTIVOS FINANCIEROS",
      	"MINISTERIO-DE-HACIENDA": "11"
    	},
    	{
      	"GASTO-REGISTRADO": "32,284,969",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "RECUPERACION DE PRESTAMOS",
      	"MINISTERIO-DE-HACIENDA": "12"
    	},
    	{
      	"GASTO-REGISTRADO": "0",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "SALDO INICIAL DE CAJA",
      	"MINISTERIO-DE-HACIENDA": "15"
    	},
    	{
      	"GASTO-REGISTRADO": "309,580,095",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "GASTOS",
      	"MINISTERIO-DE-HACIENDA": ""
    	},
    	{
      	"GASTO-REGISTRADO": "216,709,098",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "GASTOS EN PERSONAL",
      	"MINISTERIO-DE-HACIENDA": "21"
    	},
    	{
      	"GASTO-REGISTRADO": "50,929,915",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "BIENES Y SERVICIOS DE CONSUMO",
      	"MINISTERIO-DE-HACIENDA": "22"
    	},
    	{
      	"GASTO-REGISTRADO": "292,887",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "PRESTACIONES DE SEGURIDAD SOCIAL",
      	"MINISTERIO-DE-HACIENDA": "23"
    	{
      	"GASTO-REGISTRADO": "6,926,828",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "TRANSFERENCIAS CORRIENTES",
      	"MINISTERIO-DE-HACIENDA": "24"
    	},
    	{
      	"GASTO-REGISTRADO": "295,054",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "INTEGROS AL FISCO",
      	"MINISTERIO-DE-HACIENDA": "25"
    	},
    	{
      	"GASTO-REGISTRADO": "72,619",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "OTROS GASTOS CORRIENTES",
      	"MINISTERIO-DE-HACIENDA": "26"
    	},
    	{
      	"GASTO-REGISTRADO": "1,096,186",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "ADQUISICION DE ACTIVOS NO FINANCIEROS",
      	"MINISTERIO-DE-HACIENDA": "29"
    	},
    	{
      	"GASTO-REGISTRADO": "825,448",
      	"INFORME-DE-EJECUCION-TRIMESTRAL-PERIODO-2012": "INICIATIVAS DE INVERSION",
      	"MINISTERIO-DE-HACIENDA": "31"
    	},
    
	    {
      	"timestamp": 1466534470176,
      	"length": 27
    	}
  	],
  	"endpoint": "http://www.sampleurl.gov/573/87684_public_record.xls",
  	"description": "json",
  	"parameters": [
	    
  	],
  	"tags": [
	    
  	],
  	"timestamp": null,
  	"created_at": "2012-06-04T14:12:52",
  	"title": "prueba json",
  	"modified_at": "2016-06-21T14:59:52",
  	"category_id": 40524,
  	"link": null,
  	"user": "administrador",
  	"guid": "PRUEB-JSON",
  	"category_name": "Seguridad Pública"
	}


Output with *data.ajson* value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: json


	{
	  "result": [
		[
		  "Congreso Nacional - Biblioteca del Congreso",
		  "",
		  ""
		],
		[
		  "",
		  "",
		  "Cifras en miles de $"
		],
		[
		  "Código BIP",
		  "Nombre de Proyecto",
		  "Monto Identificado"
		],
		[
		  "",
		  "Remodelación Administración Valparaíso",
		  "26,505"
		],
		[
		  "",
		  "Bóveda y sala preservación colecciones valiosas",
		  "111,564"
		],
		[
		  "",
		  "Raparaciones daños terremoto, Sector Biblioteca",
		  "66,440"
		],
		[
		  "",
		  "Proyectos de climatización en Santiago y Valparaíso",
		  "62,101"
		],
		[
		  "TOTAL IDENTIFICADO",
		  "",
		  "266,610"
		]
	  ],
	  "endpoint": "http://www.dipres.gob.cl/574/articles-74267_doc_xls.xls",
	  "description": "Inversiones BCN durante el año 2011 según art. 24 de Ley de Presupuestos N° 18.482",
	  "parameters": [],
	  "tags": [],
	  "created_at": "2012-06-04T14:12:52",
	  "title": "Nóminas de Iniciativas de Inversión (M$) Biblioteca del Congreso Nacional",
	  "link": null,
	  "user": "publicador",
	  "guid": "NOMIN-DE-BIBLI-DEL-12877",
	  "category_name": "Finanzas"
	}

	
Output with *data.xml* value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: xml

	<?xml version="1.0" encoding="UTF-8"?>
	<table>
		<row>
			<column>Congreso Nacional - Biblioteca del Congreso</column>
			<column/>
			<column/>
		</row>
		<row>
			<column/>
			<column/>
			<column>Cifras en miles de $</column>
		</row>
		<row>
			<column>Código BIP</column>
			<column>Nombre de Proyecto</column>
			<column>Monto Identificado</column>
		</row>
		<row>
			<column/>
			<column>Remodelación Administración Valparaíso</column>
			<column>26,505</column>
		</row>
		<row>
			<column/>
			<column>Bóveda y sala preservación colecciones valiosas</column>
			<column>111,564</column>
		</row>
		<row>
			<column/>
			<column>Raparaciones daños terremoto, Sector Biblioteca</column>
			<column>66,440</column>
		</row>
		<row>
			<column/>
			<column>Proyectos de climatización en Santiago y Valparaíso</column>
			<column>62,101</column>
		</row>
		<row>
			<column>TOTAL IDENTIFICADO</column>
			<column/>
			<column>266,610</column>
		</row>
	</table>

Output with *data.csv* value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

	"Congreso Nacional - Biblioteca del Congreso","",""
	"","","Cifras en miles de $"
	"Código BIP","Nombre de Proyecto","Monto Identificado"
	"","Remodelación Administración Valparaíso","26,505"
	"","Bóveda y sala preservación colecciones valiosas","111,564"
	"","Raparaciones daños terremoto, Sector Biblioteca","66,440"
	"","Proyectos de climatización en Santiago y Valparaíso","62,101"
	"TOTAL IDENTIFICADO","","266,610"
	

Output with *data.xls* value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: json

	{
	  "fUri": "http://datastore.dev:8888/resources/datal_temp/2016-03-10/temp_1386265881861839185.xlsx",
	  "fNum": 302,
	  "fType": "REDIRECT"
	}

	
Requesting a data view with parameters
-----------------------------------------------

A data view can require parameters to retrieve data. Parameters can be added to a data view only during the creation process. These parameters can be mapped against a form in the case of an HTML dataset source, against a URL or against columns of data over a specific table. The proper syntax to request parameters of a dataview is 
pArgumentN=X
Where N is the position of the parameter of the data view and X is the value of said parameter.
The position of the parameter is informed when querying over the data view **without** the data element.
Example::


    http://cne.cloudapi.junar.com/api/v2/datastreams/BALAN-NACIO-ENERG-POR-5269?auth_key=MY_AUTH_KEY

From there we know that there is only one parameter, so we build out query like this::


    http://cne.cloudapi.junar.com/api/v2/datastreams/BALAN-NACIO-ENERG-POR-5269/data.json?auth_key=MY_AUTH_KEY&pArgument0=2014

.. code-block:: json

	{
  	"result": [
	    [
    	  "Año",
      	"Sección",
      	"Item",
      	"Combustible",
      	"Valor"
    	],
    	[
      	"2014",
      	"Energético Primario",
      	"Producción Primaria",
      	"Petróleo Crudo",
      	"4,809.00"
    	],
    	[
      	"2014",
      	"Energético Primario",
      	"Producción Primaria",
      	"Gas Natural",
      	"7,381.00"
    	],
    	[	
    	"2014",
      	"Energíargético Primario",
      	"Producción Primaria",
      	"Carbón",
      	"29,147.00"
    	],
    	[
      	"2014",
      	"Energético Primario",
      	"Producción Primaria",
      	"Biomasa",
      	"73,752.00"
    	],
    	[
      	"2014",
      	"Energético Primario",
      	"Producción Primaria",
      	"Energía Hídrica",
      	"20,104.00"
    	],
    	[
      	"2014",
      	"Energético Primario",
      	"Producción Primaria",
      	"Energía Eólica",
      	"1,241.00"
    	],
    	(...)
    	[
      	"2014",
      	"Sector de Consumo",
      	"Sector Industrial y Minero",
      	"Gas Corriente",
      	"6.00"
    	],
    	[
      	"2014",
      	"Sector de Consumo",
      	"Sector Industrial y Minero",
      	"Metanol",
      	"-"
    	],
    	[	
    	"2014",
      	"Seccióntor de Consumo",
      	"Sector Industrial y Minero",
	  	"Total",
      	"1457102225405,105.00"
    	]
  	],
  		"endpoint": "file://5995/5316/185277278134828680067533944176086411863",
  		"description": "Fuente: CNE. Datos desde 2008 a 2014 con el balance nacional energético consolidado en formato base de datos.",
  		"parameters": [
    	{
      	"default": "2014",
      	"position": 0,
      	"name": "Año",
      	"description": "Año de la consulta en formato AAAA"
    	}
  	],
  	"tags": [
	    "Balance",
	    "nacional",
	    "energ tico",
	    "bne",
    	"energia",
    	"Chile"
  		],
  	"timestamp": null,
  	"created_at": "2015-11-11T17:27:41",
  	"title": "Consolidado Balances Energéticos (2014 - 2008)",
  	"modified_at": "2016-06-15T16:29:49",
  	"category_id": 41209,
  	"link": null,
  	"user": "cne",
  	"guid": "BALAN-NACIO-ENERG-POR-52693",
  	"category_name": "Balance Energético"
	}



Filtering data view results
------------------------------------

The Junar API allows users to filter the results of a data view request by adding custom filters over any number of columns, which apply logical operators over values and can be concatenated using AND/OR sentences::

	http://api.datosabiertos.chilecompra.cl/api/v2/datastreams/TRANS-ENTRE-PROVE-E-INSTI/data.ajson/?auth_key=MY_AUTH_KEY&filter0=column4[>]1000000000&filter1=column0[==]Mobiliario&where=(filter0 and filter1)


.. code-block:: json

	{
  		"result": [
    	[
      		"Convenio Marco",
      		"Institución",
      		"Nombre Empresa",
      		"Cantidad OC",
      		"Monto OC"
    	],
    	[
      		"Mobiliario",
      		"Ejército de Chile",
      		"MUEBLES TIMAUKEL LTDA.",
      		"17.00",
      		"2,443,853,748.52"
    	]
  		],
  		"endpoint": "file://6745/9345/70289701374408125008067787804389705863",
  		"description": "Datos agrupados en ordenes de compra y montos totales, en pesos chilenos, de cada transaccion realizada en un Convenio Marco para Enero 2016",  
  		"parameters": [], 
  		"tags": 
  			[
  			"transacciones",
  			"ordenes de compra",
  			"Proveedor",
  			"instituciones públicas",
  			"enero","2015",
  			"convenio marco"
  			],  
  		"timestamp": null,  
  		"created_at": "2016-05-26T18:15:36",  
  		"title": "Transacciones entre Proveedores e Instituciones en Convenio Marco - Enero 2015",  
  		"modified_at": "2016-05-26T19:07:49",  
  		"category_id": 41338,  
  		"link": null,  
  		"user": "chilecompra",  
  		"guid": "TRANS-ENTRE-PROVE-E-INSTI",  
  		"category_name": "Convenio Marco"
	}


That sample request returns all values that are over 1.000.000.000 in column 4 where the string is equal to "Mobiliario" in column 0.

Filters can go from 0 to N (filter0, filter1...filterN) and have the following syntax::

	operand0 | logical operator | operand1

The values for operand0 can be rownum (the number of row/record in the table) o columnN (where N is a whole number from 0 to N). The value for operand1 can be a string of text, a number or a date. 

The possible values for logical operator are::
	
	[==], [>], [<], [!=], [contains], [>=], [<=] 

Square brackets [] must be included. All operands are case sensitive. In the case of the logical operator [contains], the order of operands must be reversed (operand2[contains]operand1).

A *where* operation must be included to concatenate two or more filters with and/or expressions (in lowercase). In the example case the where function is equal to (filter0 and filter1). The and/or expressions enable a developer to set the relation between filters and can be mixed to fulfill a certain condition, such as ::
	
	(filter0 and filter1) or filter2.

If an operand contains a number or a date, the data view must have those columns already formatted as either NUMBER or DATE. If they are not formatted in the data view, the API enables you to perform such operation on the fly (see next section).

When a date is added as an operand value it must be typed in using the US format MM/dd/yyyy.


Data column formatting 
----------------------

The API allows to define column formats for queries to enable filtering over NUMBER or DATE records that were not properly formatted during the data view creation stage. This transformation is made on the fly and affects only that query, it does not modifies the original data view. It must be applied over a column that is being filtered on the same query. The syntax is as follows :

::

	format={"table":[{"id":"column10", "type":"DATE", "format":{"country":"ES", "lang":"es", "style": "dd/MM/yyyy"}}]}

Where : 

- id : The position of the column to filter. This column must be affected by a filter in the same API query in order to properly apply format.
- type: The desired data type for the column. By default, all columns are defined as TEXT, but they can also be set as DATE or NUMBER.
- format : Dependiendo del tipo elegido puede requerir la siguiente información.

When DATE format is applied, we requere the parameters country, lang (language) and style (print) to be defined. Valued for country and lang correspond to standard ISO format, while values for style can be found in:

http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html

::

	format={"table":[{"id":"column10", "type":"DATE", "format":{"country":"CL", "lang":"es", "style": "dd/MM/yyyy"}}]}
	

When NUMBER format is applied, we require the parameters country, lang (language) and pattern in which the original number comes informed in the data view. The country and language indicates the default separators for thousands and decimals, and the pattern the general structure of the number. Additional parameters *decimals* and *thousands* can also be defined in case they are different from country and language. 

::
	format={"table":[{"id":"column4", "type":"NUMBER", "format":{"country":"US", "lang":"es", "pattern":"", "decimals":"", "thousands":""}}]}

Ejemplo

::

	..../invoke/SACRA-ANNUA-CRIME-STATS?...&filter0=column0[==]Homicide&filter1=column4[>]0&where=(filter0 or filter1)&format=
		{"table":[{"id":"column4", "type":"NUMBER", "format":"format":{"country":"US", "lang":"es", "pattern":"#,###.##", "decimals":".", "thousands":","}}]}
	
	
.. code-block:: json
	
	{
	  "id": "SACRA-ANNUA-CRIME-STATI",
	  "title": "Sacramento Annual Crime Statistics",
	  "description": "Year to date information on different types of crimes and variation 2012 2013",
	  "user": "sacramento",
	  "result": {
		"fType": "ARRAY",
		"fArray": [
		  {
			"fStr": "Homicide",
			"fType": "TEXT"
		  },
		  {
			"fStr": "7",
			"fType": "TEXT"
		  },
		  {
			"fStr": "10",
			"fType": "TEXT"
		  },
		  {
			"fNum": 3.0,
			"fType": "NUMBER"
		  },
		  {
			"fStr": "42.9%",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Robbery",
			"fType": "TEXT"
		  },
		  {
			"fStr": "275",
			"fType": "TEXT"
		  },
		  {
			"fStr": "309",
			"fType": "TEXT"
		  },
		  {
			"fNum": 34.0,
			"fType": "NUMBER"
		  },
		  {
			"fStr": "12.4%",
			"fType": "TEXT"
		  },
		  {
			"fStr": "Burglary",
			"fType": "TEXT"
		  },
		  {
			"fStr": "944",
			"fType": "TEXT"
		  },
		  {
			"fStr": "1,084",
			"fType": "TEXT"
		  },
		  {
			"fNum": 140.0,
			"fType": "NUMBER"
		  },
		  {
			"fStr": "14.8%",
			"fType": "TEXT"
		  }
		],
		"fRows": 3,
		"fCols": 5,
		"fTimestamp": 0,
		"fLength": 0
	  },
	  "tags": [
		"Sacramento",
		"POLICE",
		"crime"
	  ],
	  "created_at": "2013-05-28 00:27:27",
	  "source": "http://www.sacpd.org/crime/stats/",
	  "link": "http://sacramento.opendata.junar.com/datastreams/77447/sacramento-annual-crime-statistics/"
	}


Publishing and updating data views using the API
-------------------------------------------------

In a similar way to publishing datasets using the API, you can create new data views over existing datasets using the API. You need a private API key with publishing grants to do so. All private API keys are delivered on demand exclusively to the official contact point of the account of the Junar team.
When publishing/updating you can use basic POST/PUT/PATCH verbs:

::  
  
  POST  /api/v2/datastreams.json
  PUT   /api/v2/datastreams/:guid.json
  PATCH /api/v2/datastreams/:guid.json
  
The parameters expected for each creation/edition are:

- title : Title of the resource. Max 100 characters. Mandatory
- description : Description of the resource. Max 250 characters. Mandatory.
- category : Slug name of the category where the resource will be included. Mandatory. It must coincide with a category already existing in the account. To obtain a full category list of the account check http://account.apidomain/api/v2/categories/?auth_key=my_auth_key  
- notes : A field to place any extra information beyond the lenght of the original description. Supports enriched text and up to 7000 characters. Optional.
- dataset : GUID of the source dataset. Mandatory. Full list of published datasets available at http://account.apidomain/api/v2/datasets/?auth_key=my_auth_key  
- header_row : The position number of the row to be defined as header for the data view, starting from zero. Syntax is rowN (where N is the position number). By default this value is empty. Optional.
- table_id : Position of the table inside the document, starting from zero. In case of multisheet documents (XLS), it indicates the index number of the document sheet to be used. In case of HTML documents, the position of the table. Syntax: tableN (where N is the position number). Mandatory.
- tags : Any number of tags, separated by comma. Optional.

If succesfull, a response will return with a  ``result`` field where all the information of the affected resource is informed:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://1995/46721/313214253556015558595838280659574174401",
    "description": "prueba mesa copypaste",
    "parameters": [ ],
    "tags": [ ],
    "created_at": "2016-02-23T10:34:42",
    "title": "prueba",
    "link": null,
    "user": "junarcity",
    "guid": "PRUEB",
    "category_name": "Financial"
  }
