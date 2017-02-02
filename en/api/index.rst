
============
Junar APIv2
============


.. toctree::
    :maxdepth: 2
    :numbered:
    :caption: Base Documentation
    :hidden:
  
  Basic Principles <topics.rst>

.. toctree::
    :maxdepth: 2
    :numbered:
    :titlesonly:
    :caption: Open Data Resources
    :hidden:

  Datasets<datasets.rst>
  Data Views <dataviews.rst>
  Visualizations <visualizations.rst>
  Dashboards <dashboards.rst>
 

Welcome to the documentation for the Junar API. The current version of the API is 2.0.

This API documentation applies to all Open Data portals that are built and mantained on top of the Junar Open Data platform.
The API is organized based on REST verbs. This means the API is resource-oriented and uses HTTP response for error handling. HTTP verbs such as GET, POST, PUT, PATCH, DELETE are available. The majority of the responses are in JSON, except where detailed otherwise. 

By default our API is CORS enabled to allow interaction with other clients that are consuming data from our customers API. 

Each Open Data portal has its own API modules, each with it's own limits regarding monthly requests available per auth key. The maximum limit is 5 requests per second for all auth keys, regardless of the domain. If you have any comments, questions or feedback related to our API or to this documentation, please send us and email to developers@junar.com
