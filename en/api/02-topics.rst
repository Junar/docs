Basic Authentication
====================

There is a three step process in identifying who's making an API request

1. First tries to identify an account through the domain. 
2. Then tries to match the auth_key in the request to a list of authentication keys enabled for the domain of the account.
3. Finally, it tries to identify the user of that auth_key. If it cannot identify it, it'll consider it an anonymous user.

The system has two types of auth_keys: public and private. Private auth_keys are defined and delivered to an authorized customer contact by the Junar team. Public auth keys can be obtained from the  ``/developers`` path in the Open Data portal. 
All auth keys are anonymous by default, and are generated on the fly with each request. Anonymous keys have less privileges than identified users.

Authorization
============

If the domain is not associated to an active account, access is denied

If the auth_key is not recognized or does not belong to the account, access is denied

If the auth_key is public, it checks the referer. If it cannot identify the referer, access is denied. This feature can be configured on a need-to basis by the Junar team

If five (5) requests per seconds limit is surpassed, access is denied for one (1) second

Public auth_keys allow only for read operations

Error Handling
==============

The Junar API uses regular HTTP responses to indicate success or failure of API requests. Following this HTTP standard, codes in the 2XX range indicate success and on the 4XX indicate errors.


- ParseError: Data parsing error "400 Bad Request".
- AuthenticationFailed: Authentication error "401 Unauthenticated" 
- NotAuthenticated: Query does not contain authentication "401 Unauthenticated"
- PermissionDenied: Access is not allowed "403 Forbidden".
- NotFound: Resource is not found "404 Not Found".
- MethodNotAllowed: There's been a request for a method (POST, PUT, ect) that is not allowed "405 Method Not Allowed".
- NotAcceptable: An invalid request has been made over a data type "406 Not Acceptable".
- UnsupportedMediaType: An invalid file format has been attempted to upload "415 Unsupported Media Type".
- Throttled: The access limit has been beaten "429 Too Many Requests".
- ValidationError: Invalid parameters "400 Bad Request".
- UnexpectedError: An unexpected error. Please contact support@junar.com "500 internal Server".


Pagination
==========

All listed API methods have a pagination function that can receive any or all of the following parameters: 

- limit: amount of results per query
- offset: result position over which the query applies
- page: page of results of the query, size related to ``limit``

When these parameters are present, the response of the API is affected, adding the following fields

- count: total amount of results
- next: link to the next query
- preview: link to the previous query
- result: a list of results

A sample with a ``limit`` equal to 2

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

Basic Metrics
=============

You can obtain an ordered list of basic metrics related to the performance of your open data site. You can use the ``order`` parameter to get one of the following results.

- viewed: list of most viewed resources (hits on the portal)
- downloaded: most downloaded resources (including API requests)
- top: an aggregation of the two previous methods
- last: ordered by last update.

Metadata Filters
================

You can retrieve data resources that fits a ``query`` parameter over title, description, or tags of the open data resource. You can also filter over a specific category by using the ``categories`` parameter, which receives any number of category_name separated by commas.

Resource Type Filtering
=======================

You can also retrieve resources of a specific type by using the ``resources`` parameter on the request, which can contain any of the following values, separated by commas.

- dt: Datasets
- ds: Dataviews
- vz: Visualizations
- db: Dashboards

On the response array there's a ``type`` parameter which identifies the resource

Full catalog queries
====================

You can filter, order, search and paginate over the entire catalog with one query by using the following GET method

::

  GET   /api/v2/resources.json

For instance  ´´http://cne.cloudapi.junar.com/api/v2/resources.json?auth_key=MY_AUTH_KEY&limit=2&offset=8&query=balance&order=top´´ returns

.. code-block:: json
    {
        "count": 101,
        "next": "http://cne.cloudapi.junar.com/api/v2/resources.json/?auth_key=7a392227077e0efbfdb16f843fd12a09bea78210&limit=2&offset=10&order=top&query=balance",
        "previous": "http://cne.cloudapi.junar.com/api/v2/resources.json/?auth_key=7a392227077e0efbfdb16f843fd12a09bea78210&limit=2&offset=6&order=top&query=balance",
        "results": [
            {
            "result": null,
            "endpoint": "http://dataset.cne.cl/Energia_Abierta/Balances%20Energeticos/bne_2014.xls",
            "description": "Fuente: Elaborado por la División de Prospectiva y Política Energética del Ministerio de Energía ",
            "parameters": [
                
            ],
            "tags": [
                "consumo",
                "Sector Publico",
                "comercial",
                "residencial",
                "cpr",
                "bne 2014"
                    ],
            "timestamp": 1472489817419,
            "created_at": 1449000423,
            "title": "BNE 2014 - Consumo Sector Comercial, Público, Residencial (CPR)",
            "modified_at": 1455897663,
            "category_id": "41209",
            "link": "http://datos.energiaabierta.cne.cl/dataviews/111743/bne-2014-consumo-sector-comercial-publico-residencial-cpr/",
            "user": "cne",
            "guid": "BNE-2014-CONSU-SECTO-COMER",
            "category_name": "Balance Energético",
            "type": "ds"
            },
            {
            "result": null,
            "endpoint": "http://dataset.cne.cl/Energia_Abierta/Balances%20Energeticos/bne_2014.xls",
            "description": "Fuente: Elaborado por la División de Prospectiva y Política Energética del Ministerio de Energía ",
            "parameters": [
                
            ],
            "tags": [
                "consumo",
                "sector industrial",
                "sector minero",
                "bne 2014"
            ],
            "timestamp": 1472049616033,
            "created_at": 1448996916,
            "title": "BNE 2014 - Consumo Sector Industrial y Minero",
            "modified_at": 1455897663,
            "category_id": "41209",
            "link": "http://datos.energiaabierta.cne.cl/dataviews/111697/bne-2014-consumo-sector-industrial-y-minero/",
            "user": "cne",
                "guid": "BNE-2014-CONSU-SECTO-INDUS",
            "category_name": "Balance Energético",
            "type": "ds"
            }
        ]
    }

Version
=========

Current API version is 2.0.
Maintenance and support for APIv1.0 related issues will not be available beginning on September, 2016. While v1.0 requests will continue to work, we strongly recommend to integrate all developments over to the current version of the API. 
