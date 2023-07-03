#googlehacking
Tabla resumen:
* or: Busquedas en las que algun termino de varios salga
* -: Busquedad donde no esté el termino detras del simbolo
* " ": Busqueda en la que se encuentre exactamente el texto de dentro
* * : Busqueda con "comodin" dentro de cualquier palabra
* site: Define donde se hace la búsquedad
* related: Busquedas de sitios web similares al dominio especificados detras
* linl: contienen enlaces al dominio que se especifique detras del operador
* cache: Comprobar el aspecto que tenia el sitio web especificando la última vez indexado por google
* filetype: Buscara documentos con la extension posterior 
* inurl: Buscara el término que se especifique en aquellas páginas que url coincide con lo especificado
* intext: Busca el termino especificado dentro del cuerpo 
* intittle Busca el termino especificado dentro del titulo
* innachor: Busca el termino , se realiza dentro del texto que tiene los hipervinculos.

## Ejemplos
	site:youtube.com and intext:"Apple"
	nurl:pepe.com
	inurl: "esto esjemplo" --> (puede salir un captcha)
	filetype:pdf ciberseguridad
	ext:py --> (Ficheros que tengan extension de python)
	intittle:ciber *
	
## Ejemplos Pros:
	ext:php intext:"root:x:0:0:root:/root:/bin/bash"

