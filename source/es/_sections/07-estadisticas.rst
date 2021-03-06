Estadísticas del portal
=======================

Podemos obtener las estadísticas de uso del portal de datos abiertos a través de llamadas a la API, utilizando una API Key pública. Para crear una API key acceda `aquí <https://junar.github.io/docs/en/_sections/02-topics.html#basic-authentication>`_.

La URL que debemos utilizar se contruye de la siguiente manera:
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY``
  

Por defecto, esa llamada devolverá resultados para los últimos 365 días.

Es posible refinar los resultados utilizando las siguientes variables:

- days: La cantidad de días sobre los cuales queremos consultar. El valor por defecto será 365. Este argumento no puede utilizarse junto a hours, minutes, from y to. Si asi sucediera, el argumento tiene prioridad.
- hours: La cantidad de horas hacia atrás desde la fecha actual por la cual se acotará la consulta.
- minutes: La cantidad de minutos hacia atras desde la fecha actual por la cual se acotará la consulta. Este argumento no puede utilizarse junto a  from y to. Si asi sucediera, el argumento tiene prioridad.
- from: Fecha inicio de consulta en formato dd/mm/yyyy o mm/dd/yyyy dependiendo el idioma de la cuenta.
- to: Fecha hasta consulta en formato dd/mm/yyyy o mm/dd/yyyy dependiendo el idioma de la cuenta. En caso que from sea provisto, el valor por defecto del argumento to es la fecha actual. 
- channel: Canal por el cual acotar el ranking. Las opciones validas son API/WEB. Por defecto se deberá contemplar todos los canales.
- facets=geoinfo.country_name: consultas realizadas según la ubicación de los usuarios.
- limit: Cantidad recursos a incluir en el ranking
- group_by: El criterio de agrupamiento. Las opciones validas son date, year, month, day, hour y minute. Por defecto los resultados solo se agruparan por recurso.
- order: asc/dsc para consultar los menos y mas accedidos respectivamente




Ejemplos 
^^^^^^^^

- Consulta sobre los últimos 90 días
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&days=90``


- Consulta sobre las últimas 12 horas
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&hours=12``

- Consulta sobre los últimos 60 minutos
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&minutes=60``

- Consulta entre dos fechas, desde 1 de enero a 20 de enero de 2017.
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&from=01/01/2017&to=20/01/2017``

- Consulta sobre origen de las visitas
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&facets=geoinfo.country_name``

- Consulta sobre el canal Web
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&channel=0``

- Consulta sobre el canal API
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&channel=1``

- Agrupar los resultados por mes
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&group_by=month``

- Ordenar los resultados de forma ascendente según cantidad de visitas
	``GET /api/v2/stats/?auth_key=MI_AUTH_KEY&order=asc``






 