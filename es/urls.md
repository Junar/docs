# Guias de diseño para URLs

DATAL está dividido en 3 aplicaciones DJANGO

- API
- MICROSITES
- WORKSPACE

cada una con su API interna. Las siguientes recomendaciones se tendrán en
cuenta al añadir o modificar URLs de DATAL.

Las URL que son expuestas a los usuarios no administrativos deberán ser
acompañadas de un slug (una cadena de texto simplificado, relacionado al contenido)
por consideraciones relacionadas a la optimización de resultados de motores de
busqueda (SEO).

Se utilizarán nombres en inglés para las URL que indiquen, en la medida de lo posible,
su funcion o relación con
- un modelo de django (para el modelo Dataset, /datasets/12345)
- un método o acción (para suscribir a un usuario, /auth/signup)


Si una URL pertenece a un grupo distinguible (p. ej. autenticación), se recomienda
añadir un prefijo que ayude a distinguirla como tal (/auth/signup y /auth/signin).

- Autenticacion /auth/signin, /auth/signup, etc
- Assets estáticos /static/js, /static/css/


## AJAX

Las URLs accedidas mediante solicitudes AJAX deberán respetar el patrón REST. La guia
de diseño del Gobierno de Chile, [https://github.com/e-gob/api-standards](https://github.com/e-gob/api-standards) describe este patrón.
