Portal Statistics
=======================

We can obtain statistics from the open data portal using API calls with public API Key.

The URL to use looks like as follows:
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY``
  
By default, that call will return results for the last 365 days.

It is possible to refine the results using the following variables:

- days: The number of days we want to consult. The default value is 365. This argument can not be used together with hours, minutes, from and to. If so, the argument has priority.
- hours: The number of hours back from the current date by which the consultation will be narrow.
- minutes: The amount of minutes back from the current date by which the query will narrow. This argument cannot be used next to from and to. If so, the argument has priority.
- from: Start date of query in dd/mm/yyyy format or mm/dd/yyyy depending on the language of the account.
- to: Upto Date to query in dd/mm/yyyy or mm/dd/yyyy format, depending on the language of the account. If provided from, the default value of the to argument is the current date. 
- channel: Channel to limit the ranking. Valid options are API and WEB. By default all channels are considered.
- facets=country_name: Queries made according to the location of users.
- limit: Number of resources to include in the ranking
- group_by: The grouping criterion. Valid options are date, year, month, day, hour and minute. By default the results will only be grouped by resource.
- order: Ascending and descending, to consult the less and more accessed respectively



Examples 
^^^^^^^^

- Query on the last 90 days
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&days=90``


- Query on the last 12 hours
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&hours=12``

- Query on the last 60 minutes
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&minutes=60``

- Query between dates, from January 1 to January 20, 2017.
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&from=01/01/2017&to=20/01/2017``

- Query according to the location of users.
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&facets=country_name``

- Web Channel Query
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&channel=0``

- API Channel Query
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&channel=1``

- Group results by month
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&group_by=month``

- Sort results by ascending according to number of visits
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&order=asc``






 