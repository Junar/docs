## Respuestas internas JSON

El microstio y workspace requieren muchas veces respuestas internas desde ajax.  
En estos casos estanadarizamos y reutilizamos templates para cada caso tratando de simplificar este proceso.  
  
Hay templates especificos para micrositios o workspace, algunos de uso común están en
 [core/templates.py](https://github.com/datal-org/datal/blob/develop/core/templates.py).  
Las respuestas básicas son de la forma:  

```
{ 
    "status": true|false,
    "messages": ["lista", "de", "mensajes"] 
}
```

La clase **DefaultAnswer** permite emitir este tipo de respuestas. Usa 
[este](https://github.com/datal-org/datal/blob/develop/core/templates/defaul_answer.json) template.  
  
Para las respuestas no normalizadas (sin *status* por ejemplo) se puede 
usar la clase **DefaultDictToJson** que 
simplemente transforma un diccionario a respuesta Json. Estas llamadas deben ser usadas 
temporalmente hasta avanzar en la normalización.


#### Pendientes

  - Usar los estados HTTP como parte de la respuesta para evitar el uso de *status*.
  - No permitir llamadas que no formen partes de un template o no estén documentadas