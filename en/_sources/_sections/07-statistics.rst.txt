Portal Statistics
=======================

REST Endpoint
^^^^^^^^^^^^^

Method: GET
All usages and examples of this endpoint use the GET method. 

Base URL:
	``/api/v2/stats/?auth_key=MI_AUTH_KEY``

MY_AUTH_KEY is your authorization key for accessing the REST APIs. If you do not have an auth key see `this <https://junar.github.io/docs/en/_sections/02-topics.html#basic-authentication>`_.

| 

Purpose:
	Retrieve portal statistics regarding user visits for a time period. Called with no additional parameters the statistics will include results from the last 365 days for all channels, grouped by resource. 

| 

Response: 
	The response is given by default in Django REST framework, but it could be also get it in JSON and JSONP format.
	Called with no additional parameters the information returned includes total hits, hits by resource type and hits by resource. In each resource, it includes the number of hits, the resource title, description, category and link. 
 	 
| 
Parameters: 
	Results may be refined by providing additional parameters. Each parameter should be added to the end of the Base URL. Unless noted otherwise, parameters may be used in combination. 

|

&days=365  
	Number of days prior to today to include in the results. This parameter should not be used in combination with other date range parameters. If days is specified other date parameters will be ignored. If no date related parameters are specified it is equivalent to specifying &days=365. 

&hours=24 
	Number of hours prior to now to include in the results; if used with from and to parameters, the from and to values will be ignored.  

&minutes=60 
	Number of minutes prior to now to include in the results. if used with from and to parameters, the from and to values will be ignored.  

&from=dd/mm/yyyy or mm/dd/yyyy 	
	The beginning date for retrieving statistics for a specific date range; may be used if days, hours, or muinutes parameters are not specified. The date format used is dependent on the language used with your account. 

&to=dd/mm/yyyy or mm/dd/yyyy 	
	End date for retrieving statistics for a specific date range; may be used if days, hours, or minutes parameters are not specified. If from is defined omitting the to parameter will return statistics from the start date to today. The date format used is dependent on the language used with your account. 

&channel=0 or 1 
	Limit statistics to a specific channel. By default all channels are included. 
	0=WEB 
	1=API 

&facets=geoinfo.country_name 
	Limit statistics to users’ location.

&limit=N 
	Number of results to be included in the response

&group_by=date or year or month or hour or minute 
	Statistics may be computed for unique values for the group_by target provided. For instance, group_by=month will provide statistics calculated for each unique month for which there is data. If not specified statistics are grouped by resource 

&order=asc or desc 
	Statistics may be returned in ascending (asc) number of visits; fewer visits to greater number of visits. Or results may be returned in descending (desc) number of visits; most visits to fewest visits. 


^^^^^^^^^^^^^

Examples 
^^^^^^^^

- Query on the last 90 days
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&days=90``

- Query on the last 12 hours
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&hours=12``

- Query on the last 60 minutes
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&minutes=60``

- Query between dates, from January 1 to January 20, 2017.
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&from=01/01/2017&to=20/01/2017``

- Query according to the location of users (you do not have to change 'country_name' value)
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&facets=country_name``

- Web Channel Query
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&channel=0``

- API Channel Query
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&channel=1``

- Group results by month
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&group_by=month``

- Sort results by ascending according to number of visits
	``/api/v2/stats/?auth_key=MI_AUTH_KEY&order=asc``


