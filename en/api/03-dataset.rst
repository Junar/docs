Datasets
========

We can consume the metadata of dataset-type resources published in an Open Data Portal using a GET request.
These GET requests can return either a list of all published dataset resources, or a single GUID-identified resource.
::

    GET   /api/v2/datasets.json
    GET   /api/v2/datasets/{guid}.json

All GET requests return the following parameters:
- guid: Unique identifier of the dashboard,
- title: Resource title
- description: Resource description
- category_name: Name of the category
- endpoint: URL pointing to where the resource is located.
- tags : Array of tags separated by comma. Optional.
- user : Username of resource publisher.
- parameters : Null by default. Used to mantain regular structure with other data resources.
- created_at : Timestamp of resource revision creation.
- link : Link to the resource in the Open Data portal

For example::

 http://providencia.cloudapi.junar.com/api/v2/datasets/TRAFI-EN-CICLO-DE-PROVI.json?auth_key=MI_AUTH_KEY 


returns

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

Automating Dataset Publishing using the Junar API
--------------------------------------------------

If you are the owner of a private auth_key, you can automate the data publishing and/or data update process by using our API. You can create or update datasets from local files (which are pushed to the Junar storage servers) or you can define a new, absolute route to where your files are hosted. 

The publishing methods support POST/PUT/PATCH requests and expects the following parameters :

::

    POST  /api/v2/datasets.json
    PUT   /api/v2/datasets/:guid.json
    PATCH /api/v2/datasets/:guid.json



- title : Title of the dataset. Max. 100 characters.
- description : Description of the dataset. Max. 250 characters.
- category : Name of the category where the dataset will be assigned. Must be equal to a category already existing in the account.
- notes : Extended field for a more complete description of the dataset. Can contain enriched text in HTML (escaped), and supports up to 10.000 characters. Optional.
- end_point : Absolute URL pointing to where the source file or webpage is located. Mandatory if ``file`` hasn't been added.
- file : Route to the file to be uploaded onto the Junar storage servers. Mandatory if ``end_point`` hasn't been added.
- license : Licencing associated to this dataset. Optional
- spatial : Text field indicating the geographic zone where the dataset applies. Max. 100 characters. Optional.
- frequency : Dataset resource expected update frequency. Optional.
- mbox : Email address of the person responsible for data upkeep. Optional.
- tags : A list of comma separated tags. Optional

Possible values for the ``license`` field::

    http://creativecommons.org/licenses/by/4.0/
    http://creativecommons.org/licenses/by-sa/4.0/
    http://www.gnu.org/licenses/fdl-1.3.en.html
    http://opendefinition.org/licenses/odc-pddl/
    http://opendatacommons.org/licenses/by/
    http://opendatacommons.org/licenses/odbl/
    http://creativecommons.org/publicdomain/zero/1.0/


If you require a new licence to be added, please contact us at support@junar.com

Possible values for the ``frequency`` field::

    yearly
    monthly
    weekly
    daily
    hourly
    ondemand
    
If successful, a sample return from the API looks like this::

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

  
  When updating a dataset, you can change everything from the title and description to the filename and even the file format. Just be sure that, if there's any resource asociated to this dataset, changes on the data structure will affect it's outcome. While this in general poses no problem in terms of new records/rows, any new column added will not be represented on pre-existing data views as they are linked to the previous data structure and will not reflect new columns. The data view will have to be edited on the workspace to include any new columns added on a dataset update process.
