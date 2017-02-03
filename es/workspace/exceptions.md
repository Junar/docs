# Errores
Los errores en DATAL se manejan en un middlware que funciona de la siguiente manera. 

Si el usuario no está logueado o si la excepción no es hija de DATALEXception se loguea información al respecto y se deja pasar la excepción para que la maneja DJANGO; es decir, se devolverá un 404 o un 500.

En cambio si la excepción es hija de DATALException se detecta si el la respuesta tiene que ser un json o un html y se genera el resultado correspondiente.

Las excepciones tienen las siguientes variables de clase que todas pueden ser redefinidas por una clase que herede de DATALException o no definirlas y heredarlas.

- title: debe utilizar el metodo gettext _() para manejar traducciones
- description: debe utilizar el metodo gettext _() para manejar traducciones
- tipo: este campo todavía no tiene uso pero se lo creo para futuras implementaciones
- status_code: se utilizar para la respuesta (404, 400, etc)
- template: debe crearse un archivo con extensión json y otro con extensión html
- _context: un diccionario para las variables en traducciones (ver la sección correspondiente mas adelante en este documento)

# Acciones

Los errores en DATAL tenen la opción de poder generar un listado de acciones a tomar para solucionar o manejar los problemas, es por eso que cada excepción heredada tiene que definir sus acciones en el methodo `get_actions`

# Ubicación de Archivos

Para saber donde codear una excepción es importante saber los lugares de la aplicación donde se puede generar la misma. Las opciones son tres; en la api, en el workspace o en el microsite. 

Si la excepción aparece en mas de uno de estos tres debe crearse dentro del archivo exceptions en la carpeta core, de lo contrario en el archivo exceptions dentro de la carpeta que le corresponda.

# Traducciones

Tanto las excepciones como las accines funconan con la traducción de django. En todo el proyecto por un tema de claridad no se ponen los textos en ningún idioma sino que se los define como un string (tipo id) en ingles que después se va traduciendo en los diferentes django.po.

Los textos de los titulos y de las descripciones pueden tener variables del estilo %(nombre_variable)s de tenerlos la manera de pasarselos en en el método de creación de DATALException donde los argumentos con nombre definidos se transforman en el contexto que se le pasa tanto al título como a la descripción. 

Veamos un ejemplo de como utilizar una cantidad en una definición de una excepcion hija.

```python
class ChildNotApprovedException(LifeCycleException):
    """ Exception for resources that need approved child to change state """
    title = _('EXCEPTION-TITLE-CHILD-NOT-APPROVED')
    # Translators: Ejemplo, "Existen %(count)s hijos sin aprobar"
    description = _('EXCEPTION-DESCRIPTION-CHILD-NOT-APPROVED')
    tipo = 'child-not-approved'
    _context = {
        'count': 0, #valor default
    }

    def __init__(self, datastream_list):
        self.datastreams = datastreams
        super(ChildNotApprovedException, self).__init__(count=len(self.datastreams))

    def get_actions(self):
        return map(lambda x: ReviewDatastreamExceptionAction(x)], self.datastreams)
```

En las acciones lo que se hace es pasar la variable 'url' a la descripción de la misma por si se la quiere usar en una descripción que contenga html, por ejemplo 

     "Buscar una solución en el siguiente <a href='%(url)s'>link</a>"
     
**NOTA: Si una traducción no define una variable no habrá ningún problema, en cambio si una traducción define una variable que nuca se le pasa a la excepción en el momento de la creación se imprimira el texto %(nombre-variable)s. No es terrible pero es importante tenerlo en cuenta**


