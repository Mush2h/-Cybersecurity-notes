#theHarvester
* Herramienta gratuita de entornos linux , de **recoleccion de informacion de dominios, subdominios ,direcciones de correo de internet .**
* **IMPORTANTE**: 
	* Ya no funciona con google ni linkedin


## Uso
* Parametros destacados:
	* -d : Especifica el dominio sobre el que se va a realizar la búsqueda
	* -b : Especifica la fuente de datos desde donde se van a obtener los resultados. 
		Permite realizar  consultas sobre motores de búsqueda, servidores PGP, redes sociales  
	* -f : Permite exportar los resultados de la búsquedad a un informe en formato HTML oXML. Despues del párametro se deberá escribir la ruta y el nombre del archivo donde queramos guardarlo
	*  -l : Limita el número de resultados a mostrar 
	* -s : Realiza la consulta sobre [[Shodan]]


```console
kali@kali:~$ theHarvester
kali@kali:~$ theHarvester -d cocacola.es -b all
kali@kali:~$ theHarvester -d cocacola.es -b bing

```
