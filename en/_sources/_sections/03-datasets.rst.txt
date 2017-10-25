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

For example

  http://junardemo.cloudapi.junar.com/api/v2/datasets/INFRA-INFRA-TOTAL-SUM-FROM.json/?auth_key=MI_AUTH_KEY 

returns

.. code-block:: json

    {
      "result": null,
      "endpoint": "file://73121/71821/147721638324488614901557308458266348545",
      "description": "Investment data at current prices in local currency (millions), USD (millions) and % of GDP",
      "parameters": null,
      "tags": ["InfraLatam", "Infrastructure"],
      "timestamp": null,
      "created_at": "2017-10-02T17:04:02Z",
      "title": "[InfraLatam] Infrastructure total - sum from all infrastructure sectors",
      "modified_at": "2017-10-02T17:16:17Z",
      "category_id": 83353,
      "sources": [{
        "source__id": 2109,
        "source__name": "InfraLatam",
        "source__url": "http://infralatam.info/"
      }],
      "frequency": "",
      "link": null,
      "user": null,
      "guid": "INFRA-INFRA-TOTAL-SUM-FROM",
      "category_name": "Default Category"
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
- status: Status of the dataset. Optional.


Possible values for the ``license`` field::

- http://creativecommons.org/licenses/by/4.0/
- http://creativecommons.org/licenses/by-sa/4.0/
- http://www.gnu.org/licenses/fdl-1.3.en.html
- http://opendefinition.org/licenses/odc-pddl/
- http://opendatacommons.org/licenses/by/
- http://opendatacommons.org/licenses/odbl/
- http://creativecommons.org/publicdomain/zero/1.0/


If you require a new licence to be added, please contact us at support@junar.com

Possible values for the ``frequency`` field::

- yearly
- monthly
- weekly
- daily
- hourly
- ondemand

The possible values for the ``status`` field are:

- published
- draft
- pending_review

If the parameter is not sent the default value is pending_review.


If successful, a sample return from the API looks like this:

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


###################
POST method example
###################

The POST method must be used when creating a new dataset.

.. code-block:: text 

    curl -X  POST \
  http://demo.cloudapi.junar.com/api/v2/datasets.json \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: 58b98775-8b83-4f7e-ed39-cd2225a586f5' \
  -F auth_key=238415d936545e1994476b900d41db07883eff72 \
  -F 'title=Uploaded from API' \
  -F description=Uploaded from API using post method \
  -F category=GIS \
  -F file=@example.csv


In case the dataset has been successfully created, we will receive this response:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://72121/15031/128335633035380119355724536018989239175",
    "description": "postman",
    "parameters": null,
    "tags": [],
    "timestamp": null,
    "created_at": "2017-06-16T15:20:38Z",
    "title": "Uploaded from API",
    "modified_at": "2017-06-16T15:20:38Z",
    "category_id": 83274,
    "link": null,
    "user": null,
    "guid": "UPLOA-FROM-API",
    "category_name": "GIS"
  }



##################
PUT method example
##################

The PUT method is used to update a data set already created. This method requires the sending of all values. So, if a value is missing for the required fields it will return error. In case the field is not obligatory it will send it empty.

.. code-block:: text 

    curl -X PUT \
  http://junarops.cloudapi.junar.com/api/v2/datasets/SUBID-DESDE-API.json \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: 5ff0caf2-6639-07dc-9c70-c01ba59bac9a' \
  -F auth_key=238415d936545e1994476b900d41db07883eff72 \
  -F 'title=New title' \
  -F 'description=This dataset had been updated using put method' \
  -F category=GIS \
  -F file=@example.xlsx


In case the dataset has been successfully update, we will receive this response:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://72121/15031/272512842402740134577364585136929799929",
    "description": "This dataset had been updated using put method",
    "parameters": null,
    "tags": [],
    "timestamp": null,
    "created_at": "2017-06-12T13:15:06Z",
    "title": "New title",
    "modified_at": "2017-06-16T17:31:02Z",
    "category_id": 83274,
    "link": null,
    "user": null,
    "guid": "UPLOA-FROM-API",
    "category_name": "GIS"
    }



####################
PATCH method example
####################

The PATCH method is used to update a data set already created. In this case only the values to be changed must be sent.The rest of the values are maintained.

In this example we will update only the dataset's decription:

.. code-block:: text 

    curl -X PATCH \
    http://demo.cloudapi.junar.com/api/v2/datasets/SUBID-DESDE-API.json \
    -H 'cache-control: no-cache' \
    -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
    -H 'postman-token: 0527ec7f-f4ac-3bd6-a5f7-9e521076347e' \
    -F auth_key=238415d936545e1994476b900d447407883eff72 \
    -F description=Updating description using patch method \


In case the dataset has been successfully update, we will receive this response:

.. code-block:: json

  {
    "result": null,
    "endpoint": "file://72121/15031/272512842402740134577364585136929799929",
    "description": "Updating description using patch method",
    "parameters": null,
    "tags": [],
    "timestamp": null,
    "created_at": "2017-06-12T13:15:06Z",
    "title": "New title",
    "modified_at": "2017-06-16T17:31:02Z",
    "category_id": 83274,
    "link": null,
    "user": null,
    "guid": "UPLOA-FROM-API",
    "category_name": "GIS"
    }