Visualizations
====================

We can get a full list of all published visualizations related to an Open Data portal, and also query for an individual published resource using a GET method. You'll need to include in each request your auth key from the portal's developer page.

::

  GET   /api/v2/visualizations.json
  GET   /api/v2/visualizations/{guid}.json


From which we get the data with the following parameters :

- result: Null by default. Used to mantain regular structure with other data resources.
- guid: Unique identifier of the dashboard,
- title: Resource title
- description: Resource description
- category_name: Name of the category
- endpoint: Null by default. Used to mantain regular structure with other data resources.
- tags : Array of tags separated by comma. Optional.
- user : Username of resource publisher.
- parameters : Null by default. If the visualizations contains parameters, a more detailed description of requirements will be informed here.
- created_at : Timestamp of resource revision creation.
- link : Link to the resource in the Open Data portal

For example, if we ``GET   http://api.data.cityofsacramento.org/api/v2/visualizations/SACRA-UCR-YEAR-TO-YEAR.json?auth_key=MY_AUTH_KEY`` we receive the following response: 

.. code-block:: json

  [
  {
    "result": null,
    "endpoint": null,
    "description": "Sacramento UCR Year-to-Year Comparison",
    "parameters": [
      
    ],
    "tags": [
      ""
    ],
    "timestamp": 1472482779000,
    "created_at": 1472482779,
    "title": "Sacramento UCR Year-to-Year Comparison",
    "modified_at": 1472482779,
    "category_id": "38920",
    "link": "http://data.cityofsacramento.org/visualizations/10470/sacramento-ucr-year-to-year-comparison/",
    "user": "rnarvaez",
    "guid": "SACRA-UCR-YEAR-TO-YEAR",
    "category_name": "Public Safety"
  }
  ]

