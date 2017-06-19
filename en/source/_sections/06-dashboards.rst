Dashboards
===========
We can get a full list of all published dashboards related to an Open Data portal, including also each individual resource published inside a particular dashboard using a GET method. You'll need to include in each request your auth key from the portal's developer page.

::

  GET   /api/v2/dashboards.json
  GET   /api/v2/dashboards/{guid}.json


From which we get the data with the following parameters :

- guid: Unique identifier of the dashboard,
- title: Resource title
- description: Resource description
- category_name: Name of the category
- endpoint: Null by default. Used to mantain regular structure with other data resources.
- tags : Array of tags separated by comma. Optional.
- user : Username of resource publisher.
- parameters : Null by default. Used to mantain regular structure with other data resources.
- created_at : Timestamp of resource revision creation.
- link : Link to the resource in the Open Data portal



Example ``GET   https://api.data.arlingtonva.us/api/v2/dashboards.json?auth_key=MY_AUTH_KEY`` returns: 

.. code-block:: json

	[
  		{
    		"result": null,
    		"endpoint": null,
    		"description": "Maps for bike related services and terrain",
    		"parameters": null,
    		"tags": [
      		""
    		],
    		"timestamp": "2016-06-21T16:22:07Z",
    		"created_at": "2016-06-21T16:09:09Z",
    		"title": "Transportation (Bike)",
    		"modified_at": "2016-06-21T16:22:07Z",
    		"category_id": "Transportation",
    		"link": "https://data.arlingtonva.us/dashboards/19313/transportation-bike/",
    		"user": "atran",
    		"guid": "TRANS-26367",
    		"category_name": "Transportation",
    		"resources": [
		      
    		]
  		},
  		{
    			"result": null,
    		"endpoint": null,
    		"description": "Maps for ART and Metro services",
    		"parameters": null,
    		"tags": [
      		""
    		],
    		"timestamp": "2016-06-21T16:21:46Z",
    		"created_at": "2016-02-22T20:02:29Z",
    		"title": "Public Transportation (Metro + Bus)",
    		"modified_at": "2016-06-21T16:21:46Z",
    		"category_id": "Transportation",
    		"link": "https://data.arlingtonva.us/dashboards/19314/public-transportation-metro-bus/",
    		"user": "atran",
    		"guid": "PUBLI-TRANS-METRO-BUS",
    		"category_name": "Transportation",
    		"resources": [
		      
    		]
  		},
  		{
    		"result": null,
    		"endpoint": null,
    		"description": "All economic Indicators",
    		"parameters": null,
    		"tags": [
      		""
    		],
    		"timestamp": "2016-06-16T18:59:54Z",
    		"created_at": "2016-04-25T19:56:05Z",
    		"title": "Economic Indicators",
    		"modified_at": "2016-06-16T18:59:54Z",
    		"category_id": "Demographics and Economic Indicators",
    		"link": "https://data.arlingtonva.us/dashboards/19456/economic-indicators/",
    		"user": "atran",
    		"guid": "ECONO-INDIC-60379",
    		"category_name": "Demographics and Economic Indicators",
    		"resources": [
		      
    		]
  		},
  		{
    		"result": null,
    		"endpoint": null,
    		"description": "Pool, Food, and Hotel Inspections with critical violations.",
    		"parameters": null,
    		"tags": [
      		""
    		],
    		"timestamp": "2016-02-24T15:09:21Z",
    		"created_at": "2016-02-24T15:09:19Z",
    		"title": "Inspections",
    		"modified_at": "2016-02-24T15:09:21Z",
    		"category_id": "Health and Human Services",
    		"link": "https://data.arlingtonva.us/dashboards/19311/inspections/",
    		"user": "atran",
    		"guid": "INSPE",
    		"category_name": "Health and Human Services",
    		"resources": [
		      
    		]
  		}
	]
	
	
	
	









