====================
Tópicos
====================

Autenticación Básica
====================

Existen 3 pasos para intentar identificar quien está llamando a la API

1. Lo primero que se hace es tratar de identificar la cuenta (Account) mediante el dominio. 
2. Luego se intenta identificar la auth_key que debe coincidir con una de las auth_key dentro de las habilitadas en la cuenta.
3. Por último, se trata de identificar el usuario y en caso negativo se lo considera un usuario anónimo.

El sistema tiene dos tipos de auth_key; públicas y privadas. Las privadas son solicitadas directamente al equipo de Junar, quienes la entregan únicamente a un contacto autorizado por la cuenta, mientras que las publicas se pueden obtener entrando al path ``/developers`` dentro del portal de la cuenta.
Las auth_keys son anónimas y se generan dinámicamente para cada solicitud. 

Autorización
============

Si el dominio no es un dominio que corresponde con una cuenta activa, se deniega el acceso

Si la auth_key no se reconoce o no corresponde a la cuenta, se deniega el acceso

Si la auth_key es pública se chequea el referer y si no se puede identificar (configurable por sistema) no se permite el acceso.

Si se exceden las cinco (5) solicitudes por segundo, se deniega el acceso por 1 segundo.

Las auth_key públicas solo permiten operaciones de lectura.

Errores
=======

La API de Junar usa respuesta HTTP convencionales para indicar el éxito o el fracaso de una llamada a la API. Siguiendo los lineamientos HTTP los códigos de rango 2xx indicana éxito y los códigos de rango 4xxx indicana un error.


- ParseError: Error de parseo de datos "400 Bad Request".
- AuthenticationFailed: Error de autenticación "401 Unauthenticated" 
- NotAuthenticated: El query viene sin autenciación "401 Unauthenticated"
- PermissionDenied: Error de acceso no permitido "403 Forbidden".
- NotFound: El recurso no se encuenta "404 Not Found".
- MethodNotAllowed: Se quiere ejecutar un método (POST, PUT, ect) no permitido "405 Method Not Allowed".
- NotAcceptable: Se pide que se devuelva la inforamción en un tipo de dato que no es válido "406 Not Acceptable".
- UnsupportedMediaType: Se intenta subir un tipo de dato que no es válido "415 Unsupported Media Type".
- Throttled: Se super el límite de accesos "429 Too Many Requests".
- ValidationError: Parámetros inválidos "400 Bad Request".
- UnexpectedError: Error inesperado "500 internal Server".

Paginación
==========

Todos los métodos de listado de recursos de la API tienen el funcionamiento de paginado. Cualquiera puede recibir los siguientes parámetros: 

- limit: cantidad de resultados por búsqueda
- offset: número de resultado a partir del cual se realiza la búsqueda.

Cuando aparecen estos parámetros la respuesta se ordena de otra manera y agrega los siguientes campos

- count: la cantidad todal de resultados
- next: un link a la llamada siguiente
- preview: un link a la llamada previa
- result: el listado de resultados.

Un ejemplo de respuesta con limit igual a 2 podría ser

.. code-block:: json

  {
    "count": 20,
    "next": "http://api.dev:8080/api/v2/datasets/?auth_key=576bba0dd5a27df9aaac12d1d7ec25c8411fe29e&limit=2&offset=2",
    "previous": null,
    "results": [
        {
            "endpoint": "http://datastorage.mineduc.cl/tablas/Nivel_Calificaciones_anio_2006.csv",
            "description": "Descripcion del conjuto de datos",
            "parameters": [],
            "tags": [
                ""
            ],
            "created_at": 1337611920,
            "title": "Nivel de Calificaciones 2006",
            "link": "http://microsites.dev:8080/datasets/61649/nivel-de-calificaciones-2006",
            "user": "publicador",
            "guid": "DATASET-ID-61649",
            "category_name": "Educación"
        },
        {
            "endpoint": "http://datastorage.mineduc.cl/tablas/Nivel_Rendimiento_anio_2010.csv",
            "description": "Descripcion del conjuto de datos",
            "parameters": [],
            "tags": [
                ""
            ],
            "created_at": 1337611453,
            "title": "Nivel de Rendimiento 2010",
            "link": "http://microsites.dev:8080/datasets/61648/nivel-de-rendimiento-2010",
            "user": "publicador",
            "guid": "DATASET-ID-61648",
            "category_name": "Educación"
        }
    ]
  }



También pueden paginarse los datos que devuelve una llamada a una vista. En este caso, deben utulizarse los siguientes parámetros: 

- limit: cantidad de resultados por búsqueda
- page: página sobre la cual se retornan los resultados, según lo especificado en ``limit``

Por ejemplo, esta llamada devuelve 50 filas y se ubica en la página 3:

http://junardemo.cloudapi.junar.com/api/v2/datastreams/INFRA-INFRA-TOTAL-SUM-FROM/data.json/?auth_key=7f9ef43c9132fd3766d69d65a881134cc2ffbfcd&limit=50&page=3


Ordenamiento
============

Para poder ordenar los listados de todos los recursos se usa el parámetro ``order`` que puede tener uno de los siguientes valores.

- viewed: se ordena por los mas vistos (vistos en el portal)
- downloaded: se ordena por los recursos mas descargados (acceso a través de la API).
- top: se ordena por una suma de los dos campos anteriores
- last: se ordena por fecha de actualización.



Filtros
=======

Los filtros aplican también a los listados de todos los recursos de la API.

Las búsquedas se realizan utilizando el parámetro ``query`` y actualmente se puede filtrar por categorias utilizando el parámetro ``categories`` que recibe los nombres de las categorias separadas por coma.

Busquedas Múltiples
===================

Existe la posibilidad de tener un listado de múltiples recursos y para ello se creo el siguiente path:

``GET /api/v2/resources``

Para poder filtrar los tipos de recursos se utiliza el parámetro ``resources`` que puede tener más de uno de los siguientes valores separados por coma.

- dt: Conjunto de datos
- ds: Vistas
- vz: Visualizaciones
- db: Colecciones

En los resultados se agrega el parámetro ``type`` para identificar el recurso

``GET /api/v2/resources/?auth_key=MI_AUTH_KEY&type=dt``


Además, es posible buscar recuros por palabras utilizando el parámetro ``query``

``GET /api/v2/resources/?auth_key=MI_AUTH_KEY&query=termino de busqueda``


Mediante el valor ``score`` es posible ordenar de forma ascendente ``:asc`` o descendente ``:desc`` los resultados según la relevancia del término buscado. 
La relevancia se define calculando cuán similar es la ortografía de la palabra encontrada al término de búsqueda original.

``GET /api/v2/resources/?auth_key=MI_AUTH_KEY&query=termino de busqueda&order=_score:asc``


Versiones
=========

Actualmente la api se encuentra en su versión 2. 

Para obtener información de sobre la Versión 1.0 de la API, ingresar a la documentación de esa versión asociada al proyecto `Energía Abierta
<http://junar.github.io/VizCNE/doc/build/index.html>`_.