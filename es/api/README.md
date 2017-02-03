La API de DATAL debe seguir en la medida de lo posible las guias de 
diseño del Gobierno de Chile, expuestas en 
[https://github.com/e-gob/api-standards](https://github.com/e-gob/api-standards). 

En resumen, se estructurarán las URLs de la API siguiendo el patrón REST, asignando 
URLs a recursos, utilizando los verbos GET, POST, PUT, PATCH, DELETE para las correspondientes 
acciones y la extension del formato en que se recibiran los resultados, por ejemplo:

GET /api/v2/datasets/12345.json

para solicitar un dataset identificado por el ID 12345 en formato JSON.

# Autorización
1. Lo primero que se hace es tratar de identificar la cuenta (Account) mediante el dominio. Si no se puede identificar no se permite el acceso. 
2. Luego se intenta identificar la auth_key (privada o pública). Si no se puede indentificar no se permite el acceso.
3. Si la auth_key es publica se chequea el referer. Si no se puede identificar no se permite el acceso.
4. Se trata de identificar al usuario. Si no se puede identificar se trata de un usuario 'anónimo'

# Recursos

Todos los recursos de la api salvo que aparezca alguna excepción retornan una objeto con los siguientes atributos


- **guid**: Id de la vista.
- **title**: Titulo de la vista.
- **description**: Descripción de la vista.
- **user**: Usuario propietario de la vista.
- **tags**: Tags correspondientes a la vista.
- **created_at**: Fecha de creacón.
- **endpoint**: Fuente de los datos de la vista.
- **link**: Link a la vista en el portal de datos abiertos.
- **category_name**: Nombre de la categoría de la vista.
- **parameters (solo en datastreams)** : parametros dinámicos para el contenido de un datastream
- **result (solo en datastreams)** : cuando se llama al metodo data de un datastream
- **type (solo en busquedas de recursos)**: un string de dos caracters ('ds', 'dt', 'vz')

```
{
  "guid": "INDIC-DIARI", 
  "title": "Indicadores Diarios", 
  "description": "Datos obtenidos en l\u00ednea del Banco Central de Chile.", 
  "user": "cne", 
  "tags": [], 
  "created_at": "2015-07-28 11:20:24", 
  "endpoint": "https://docs.google.com/spreadsheets/d/1wiUQlRiIPMbTWFDVn3Ug8Rp15yHuXwsCU3ZGz39xJhg/pub?gid=0&single=true&output=csv", 
  "link": "http://datos.energiaabierta.cne.cl/datastreams/94287/indicadores-diarios/", 
  "category_id": 41111, 
  "category_name": "Internacional",
  "parameters": [
    {
      "name": "param0",
      "position": 0,
      "description": "Un parametro",
    },
    {
      "name": "param0",
      "position": 0,
      "description": "Un parametro",
    }
  ],
  "result": {
    "fType":"ARRAY",
    "fArray": [
      {
        "fStr":"Indicador",
        "fType":"TEXT",
        "fHeader":true
      },{
        "fStr":"Valor",
        "fType":"TEXT",
        "fHeader":true
      },{
        "fStr":"UF",
        "fType":"TEXT"
      },{
        "fNum":25152.06,
        "fType":"NUMBER",
        "fDisplayFormat": {"fPattern":"#,###.##", "fLocale":"es"}
      },{
        "fStr":"UTM (Agosto)",
        "fType":"TEXT"
      },{
        "fNum":44067.0,
        "fType":"NUMBER",
        "fDisplayFormat":{"fPattern":"#,###.##","fLocale":"es"}
      },{
        "fStr":"Dólar Observado",
        "fType":"TEXT"
      },{
        "fNum":689.36,
        "fType":"NUMBER",
        "fDisplayFormat":{"fPattern":"#,###.##","fLocale":"es"}
      },{
        "fStr":"Dólar Observado19-08-2015",
        "fType":"TEXT"
      },{
        "fNum":697.25,
        "fType":"NUMBER",
        "fDisplayFormat":{"fPattern":"#,###.##","fLocale":"es"}
      }
    ],
    "fRows":4,
    "fCols":2,
    "fTimestamp":1439937039829,
    "fLength":0
  }, 
}
```

Los recursos que maneja la API son:

- datasets
- datastreams
- visualizations

# Métodos 

# GET Listados

Busca un litado de recursos 

```
ET /api/v2/resources
```

Busca un listado de datastreams.

```
GET /api/v2/datastreams
```

Busca un listado de datasets.

```
GET /api/v2/datasets
```

Busca un listado de visualizaciones.

```
GET /api/v2/visualizations
```

Si no hay parametros GET la API devuelve todos los recursos que le corresponden a la cuenta 
y sino se pueden pasar los siguientes parametros para ordenar o filtrar la búsqueda.

- **query**: texto que se busca en todos los parametros del datastream
- **offset**: a partir de que datastream traer el resultado (se usa para paginar)
- **limit**: junto con offset se usa para limitar la cantidad de resultados 
- **order**: pudiendo ser su valor *top*, *viewed*, *downloaded* o *last* y ordenando el resultado según esos criterios.
- **categories**: listado de los nombres de categorias separados por coma
- **resources**: listado de recursos (Ej. "dt,ds,vz")

Cuando se aplican limites a las busquedas el resultado se devuelve encapsulando los objetos en otros paramteros.

- **count**: el total de los resultados si no hubiera paginado
- **next**: un link a la próxima petición de la api según el paginado
- **previous**: un lina a la previa petición de la api según el paginado
- **result**: un array con el resultado final

Un ejemplo de resultados con limit igual a 2 sería asi

```
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
```

## GET

Trae la información asociada a una vista

```
GET /api/v2/datastreams/:guid
``` 

Trae la información asociada completa (incluye los valores de la vista) a una vista

```
GET /api/v2/datastreams/:guid/data
``` 

Trae la información asociada a un dataset

```
GET /api/v2/datasets/:guid
``` 

Trae la información asociada a una visualización

```
GET /api/v2/visualizations/:guid
``` 

Un ejemplo de resultado de un datastream sería así

```
{
    "endpoint": "http://datastorage.mineduc.cl/tablas/Nivel_Calificaciones_anio_2006.csv",
    "description": "Contiene indicadores para promedio anual de calificaciones (para Enseñanza Básica de Niños y Enseñanza Media de Jóvenes).",
    "parameters": [],
    "tags": [],
    "created_at": "2012-06-04T14:12:52",
    "title": "Nivel Calificaciones 2006",
    "link": null,
    "user": "publicador",
    "guid": "NIVEL-DE-CALIF-2006-53010",
    "category_name": "Educación"
}
```

## POST / PUT / PATCH

Creación, Edición y Edición Parcial de recursos.

Al momento solo podemos crear y editar datasets y vistas. Y para poder 
hacerlo hay que tener acceso a una clave privada que esté asociada a un usuario.

Por lo tanto la operación de edición y creación se realiza sobre las últimas revisiones de los recursos y no sobre laas últimas revisiones publicadas de los mismos.

### Datasets


```
POST /api/v2/datasets
PUT/PATCH /api/v2/datasets/:guid
``` 

- **title**: Título del conjunto de datos
- **description**: Descripción del conjunto de datos
- **category**: Slug de la categoría para clasificar los recursos. Debe coincidir con alguna de las categorías de la cuenta
- **notes**: Opcional. Texto de la nota del conjunto de datos
- **end_point**: Opcional. Url apuntando al recurso con los datos (archivos o página web).
- **file**: Opcional. Archivo a subir a la plataforma
- **license**: Opcional. Tipo de licencia que aplica sobre el conjunto de datos
- **spatial**: Opcional. Zona geográfica a la cual aplica el conjunto de datos
- **frequency**: Opcional. Tipo de licencia que aplica sobre el conjunto de datos
- **mbox**: Opcional. Correo electronico de quien administra el conjunto de datos
- **tags**: Opcional. Tags separados por coma.


### Datastreams
```
POST /api/v2/datastreams
``` 

- **title**: Título del conjunto de datos
- **description**: Descripción del conjunto de datos
- **category**: Slug de la categoría para clasificar los recursos. Debe coincidir con alguna de las categorías de la cuenta
- **notes**: Opcional. Texto de la nota del conjunto de datos
- **table_id**: Opcional. Indice de la tabla en el conjunto de datos, comenzando de cero.
- **meta_text (solo en datastreams)**: Meta text del dataastream.
- **header_row**: Opcional. Indice de la fila a usar como cabecera de la tabla comenzando de cero. Por defecto es vacio
- **dataset**: GUID del conjunto de datos asociado a la vista
- **tags**: Opcional. Tags separados por coma.

Ejemplo de llamada para crear un datastream 

```
curl -H 'Accept: application/json; indent=4' -H "Content-Type: application/json" -X POST -d @api/tests/datastream-ok.json "http://api.dev:8080/api/v2/datastreams.json?auth_key=576bba0dd5a27df9aaac12d1d7ec25c8411fe29e"
```
