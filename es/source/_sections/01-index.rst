
============
Introducción
============

Bienvenidos a la documentación de la API de Junar. La documentación de esta API aplica de manera general para todos los portales de datos abiertos de todos los clientes de Junar.
Esta API está organizada en base a los conceptos REST. Es decir, la API está orientada a recursos y utiliza códigos de respuestas HTTP para indicar errores. Además se utilizan los verbos HTTP como GET, POST, PUT, PATCH, DELETE para las correspondientes acciones. La mayoría de las respuestas son en JSON ,salvo algunos casos particulares que se detallan mas adelante. 

También soportamos CORS (cross-origin resource sharing) para permitir la interacción con nuestra API desde otros clientes. 

Cada portal de Datos Abiertos tiene sus propios módulos de API con sus propias limitaciones respecto a consultas (requests) por minuto y totales mensuales. Por favor referirse a los sitios de desarrolladores de cada uno o contactennos a developers@junar.com para más detalles.
