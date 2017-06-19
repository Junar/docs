Vistas de Datos
===============

Una vista de datos es un recurso construido por un usuario a partir de una selección personalizada de filas y columnas en base a un conjunto de datos. Esta selección de datos puede incluir modificaciones adicionales sobre los datos originales, como incluir cabeceras, cambio en el tipo de dato o cambios en el formato de salida de los números


En el recurso vista existe una llamada especial para mostrar todos sus datos: 

::
  
  GET   /api/v2/datastreams/{guid}/data.{format}
  
  
El path que termina en data. ``{format}``  permite retornar un nuevo campo ``result``: donde están los datos del origen del recurso

El ``{format}``  puede tomar uno de los siguientes valores:

-	json: Retorna un json con la estructura definida en el siguiente apartado.

-	ajson: Retorna un array de arrays json dentro de un elemento 'result'. 

-	pjson: Retorna un array con etiquetas para cada elemento. Utiliza siempre la primera fila como la vista de origen para las etiquetas.

-	jsonp: Retorna los datos en formato JSONP

-	csv: Retorna un documento de texto separado por comas.

-	xml: Retorna los datos como una estructura XML.

-	xls: Retorna un enlace dentro de un elemento fUri para descargar una versión en XLS de la vista de datos.

Existe además una salida especial reemplazando ``data.{format}`` por ``tableau.html``, la cual permite la integración con la aplicación Tableau al utilizar este nuevo endpoint como origen de datos desde la aplicación.


Estructura JSON de la salida data.json
--------------------------------------

El resultado es un objeto Argument, el cual es una estructura recursiva de datos que contiene las siguientes propiedades:

- fType: Indica el tipo de dato del Argument. Sus valores pueden ser ARRAY | TEXT | NUMBER | DATE. El tipo ARRAY indica que el argumento contiene una TABLA. El tipo por defecto de los datos dentro de una TABLA es TEXT, a menos que se haya definido otro tipo de datos durante la creación. El tipo de dato puede ser modificado en cada consulta via API.
- Cuando el tipo de datos es un ARRAY, fRows y fCols indican el número de filas y columnas de la TABLA. De la misma manera, fArray contiene los datos de la TABLA como un arreglo de objetos Argument.
- Cuando el tipo de datos es TEXT el valor está contenido en fStr. Para un tipo de dato NUMBER el valor esta contenido en fNum. Para un tipo de dato DATE el valor está contenido en fNum como epoch time.
- Un Argument puede contener un enlace LINK. En esos casos, fType contiene LINK, la uri correspondiente viene en fUri y el texto a mostrar está contenido en fStr.
- Un array puede contener también el valor fHeader. Si fHeader es igual a "true", significa que ese campo es parte de las cabeceras seleccionadas en la vista de datos.
- Cuando el tipo de datos es ERROR, ocurrió un error al ejecutar la vista de datos. El mensaje de error estará contenido en fStr.
- Cuando un error ocurre en la conexión con la fuente, el resultado es reemplazado con el último resultado que fue ejecutado correctamente.
- Para reconocer si el resultado está actualizado, existe una propiedad adicional llamada fTimestamp. Contiene el tiempo POSIX de cuando fue **accedida la fuente de datos** de forma exitosamente por última vez. Si fTimestamp tiene un valor igual a 0, significa que el resultado fue obtenido en ese instante.


Ejemplos de salida de los datos
-------------------------------

Ejemplo sobre el retorno de *data.json*
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


Ejemplo sobre el retorno de *data.pjson*
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


Ejemplo sobre el retorno de *data.ajson*
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

	
Ejemplo sobre el retorno de *data.xml*
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

Ejemplo sobre el retorno de *data.csv*
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
	

Ejemplo sobre el retorno de *data.xls*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: json

	{
	  "fUri": "http://datastore.dev:8888/resources/datal_temp/2016-03-10/temp_1386265881861839185.xlsx",
	  "fNum": 302,
	  "fType": "REDIRECT"
	}

	
Consumiendo una vista de datos con parámetros
-----------------------------------------------

Una vista de datos puede contener parámetros. Los parámetros pueden agregarse a la vista de datos solamente durante el proceso de creación. Estos parámetros pueden estar mapeados contra un formulario en un sitio web, directamente contra la URL de la fuente de datos o contra columnas de datos dentro de la tabla sobre la cual se crea la vista. La sintaxis apropiada para agregar parámetros en una solicitud API es
pArgumentN=X
Donde N es la posición del parámetro en la vista, empezando desde cero y X es el valor que tendrá dicho parámetro.

Ejemplo::


    http://cne.cloudapi.junar.com/api/v2/datastreams/BALAN-NACIO-ENERG-POR-5269/data.ajson?auth_key=MI_AUTH_KEY&pArgument0=2014


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




Filtrar los resultados de una vista
------------------------------------

La API de Datos Abiertos de Junar permite a sus usuarios filtrar los resultados obtenidos durante la solicitud de una vista de datos utilizando la siguiente sintaxis::

	http://api.datosabiertos.chilecompra.cl/api/v2/datastreams/TRANS-ENTRE-PROVE-E-INSTI/data.ajson/?auth_key=MI_AUTH_KEY&filter0=column4[>]1000000000&filter1=column0[==]Mobiliario&where=(filter0 and filter1)


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


Esto retorna todos los datos que sean mayores a 1.000.000.000 en la columna 4 y sean iguales a la palabra "Mobiliario" en la columna 0.

Los filtros pueden ir desde 0 a N (filter0, filter1...filterN) y tienen la siguiente sintaxis::

	operando0 | operador lógico | operando1

Los operando0 pueden ser rownum (número de fila) o columnN (columna N, donde N es un entero que va desde 0 a N). El operando1 por lo general es una cadena de texto, número o fecha. 

Los operadores lógicos pueden ser::
	
	[==], [>], [<], [!=], [contains], [>=], [<=] 

Los corchetes [] deben ser incluidos. Los operandos son sensibles a mayúsculas si el contenido es una cadena de texto. En el caso del operador lógico [contains], el orden de los operandos debe invertirse.

La operación where tiene una expresión lógica para concatenar filtros de tipo AND u OR. En este caso, se utilizaría (filter0 and filter1). Las expresiones and y or sirven para diferenciar la relación entre los filtros y pueden concatenarse tanto como fuera necesario para cumplir una cierta condicion por ejemplo::
	
	(filter0 and filter1) or filter2.

Si se utiliza como operando un número o fecha, la misma debe venir formateada como tal en la vista de datos. Si no viene formateada, puede aplicarse un formato a través de la API (ver apartado siguiente).

Cuando se agrega una fecha como parámetro debe incluirse utilizando el formato MM/dd/aaaa.


Ordenar los Datos
-----------------

La API permite ordenar los resultados obtenidos durante la solicitud de una vista de datos utilizando el parámetro ``orderBy``. Debe indicarse la columna sobre la cual deseamos ordenar los resultados y entre corchetes si el orden debe ser ascedente [A] o descendente [D].

	``http://api.datosabiertos.chilecompra.cl/api/v2/datastreams/TRANS-ENTRE-PROVE-E-INSTI/data.ajson/?auth_key=MI_AUTH_KEY&orderBy0=column0[D]``


Formateo de Datos
-----------------

Permite a los desarrolladores el dar formato a columnas de datos con un tipo y formato que afectará cómo son devueltos los datos de la consulta. Usa la siguiente sintaxis y debe aplicarse a una columna que ya haya sido objeto de un filtro :

::

	format={"table":[{"id":"column10", "type":"DATE", "format":{"country":"ES", "lang":"es", "style": "dd/MM/yyyy"}}]}

Donde : 

- id : Corresponde a la posición de la columna a filtrar. Esta columna debe haber sido objeto de un filtro para poder ser formateada.
- type: El tipo de dato que contiene la columna. Por defecto todas las columnas se consideran de tipo texto (TEXT), pero pueden cambiarse a fecha (DATE) o número (NUMBER).
- format : Dependiendo del tipo elegido puede requerir la siguiente información.

El formato DATE requiere country (país), lang (idioma) y style (estilo). Los valores de country y lang corresponden al formato ISO, mientras que posibles valores de style pueden encontrarse aquí:

http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html

::

	{"table":[{"id":"column10", "type":"DATE", "format":{"country":"CL", "lang":"es", "style": "dd/MM/yyyy"}}]}
	El formato NUMBER requiere country (país), lang (idioma) y pattern (patrón). Los patrones permiten definir cómo se separarán los miles y los decimales o si las cifran irán agrupadas de acuerdo a estándares asociados al país e idioma elegidos
	{"table":[{"id":"column4", "type":"NUMBER", "format":{"country":"US", "lang":"es", "pattern":"", "decimals":"", "thousands":""}}]}

Ejemplo

::

	..../invoke/SACRA-ANNUA-CRIME-STATS?...&filter0=column0[==]Homicide&filter1=column4[>]0&where=(filter0 or filter1)&format=
		{"table":[{"id":"column4", "type":"NUMBER", "format":"format":{"country":"US", "lang":"es", "pattern":"", "decimals":".", "thousands":","}}]}
	
	
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

Agrupaciones y Funciones sobre vistas de datos
----------------------------------------------

Puedes aplicar algunas FUNCIONES y AGRUPACIONES sobre los datos de una vista. Las operaciones se realizan a demanda sobre un juego de columnas definido en una llamada API y asociados a través de dos parámetros pGroupBy y pFunction. El resultado puede ser reutilizado como una fuente tipo web service REST/JSON para crear nuevos recursos de datos (vistas, visualizaciones) en el área de trabajo. Las funciones disponibles actualmente son SUM (suma), COUNT (contar), y AVG (promedio).

En primer lugar definiremos la columna que servirá para agrupar mediante el parametro pGroupBy seguido de un número que indica la jerarquía (pGroupBy0=column0, pGroupBy1=column2...). Luego, aplicamos una función FUNCTION aplicada en la columna sobre la que vamos a operar. Debes incluir paréntesis cuadrados (brackets) al ingresar la columna, pudiendo concatenar mas de una pFunction agregandole un número entero empezando desde cero (pFunction0=SUM[column0], pFunction1=COUNT[column10]).

Por ejemplo, si quisieramos responder a la pregunta por la cantidad de incidencias totales agrupadas según cada estación de respuesta del servicio 911 de la ciudad de Sacramento, el request a la API sería 

::

http://api.data.cityofsacramento.org/api/v2/datastreams/SACRA-FIRE-DEPAR-911-73507/data.csv/?auth_key=8fdd7b3db6ef82b08bcf4715b5f0dfce54fc6bf1&pGroupBy0=column2&pGroupBy1=column3&pFunction0=COUNT[column3]


Este request retorna los datos como un CSV. Cualquier otro valor de data.{format} puede ser utilizado también.

El formato de salida **data.ajson** puede además ser reutilizado para crear un conjunto de datos en Junar usando la API como un servicio web REST/JSON. El path a los datos en todos los casos sería *$.result*. El uso del caché para el dataset es altamente recomendado, ya que la operación es realizada en línea y el exceso de uso puede degradar el rendimiento de la solicitud. 



Publicación y actualización via API
--------------------------------------

De manera similar a la publicación de conjuntos de datos via API, usted puede crear nuevas vistas de datos sobre datasets ya existentes usando la API de Junar. Para esto utilice las llamadas POST/PUT/PATCH las cuales reciben los siguientes parámetros:

::  
  
  POST  /api/v2/datastreams.json
  PUT   /api/v2/datastreams/:guid.json
  PATCH /api/v2/datastreams/:guid.json
  
- title : Título del recurso. Máximo 100 caracteres.
- description : Descripción del recurso. Máximo 250 caracteres.
- category : Slug de la categoría para clasificar los recursos. Debe coincidir con alguna de las categorías de la cuenta  
- notes : Opcional. Texto de la nota del conjunto de datos. Máximo 10.000 caracteres. Soporta texto enriquecido.
- dataset : GUID del conjunto de dataos asociado a la vista.
- header_row : Opcional. Indice númerico de la fila a usar como cabecera de la tabla comenzando de cero. Por defecto es vacio
- table_id : Indice numérico de la tabla en el conjunto de datos, comenzando de cero.
- tags : Opcional. Tags separados por coma.

El path que termine en data.{format} permite retornar un nuevo campo ``result``: donde están los datos del origen del recurso 

Todas las llamadas en caso de éxito devuelven lo mismo,	por ejemplo:

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


Nuevos tipos de salida se irán incluyendo con el tiempo.
