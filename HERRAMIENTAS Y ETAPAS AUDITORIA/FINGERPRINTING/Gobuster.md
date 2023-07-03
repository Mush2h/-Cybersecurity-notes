#gobuster 

Gobuster herramineta utilizada para realizar fuerza bruta a: URIs(directorios y archivos) en sitios web,subdominios DNS.

Tiene tres modos disponibles:
* dir: Modo clasico de fuerza bruta contra subdominios DNS y "vhost"
* help: Muestra la ayuda de nivel superior de Gobuster
	* Puede mostrar ayuda especifica con help dir

## Opciones interesantes

* -u: define la URL en evaluacion.
* -t: define el numero de hilos concurrentes
* -w: define el archivo cnteniendo una lista de palabras
* -x: define las extesiones de los archivos a buscar

### Ejemplos básico

```shell
gobuster dir -u http://<ip>/<directorio> -t 20 -w /usr/

gobuster dir -w /usr/share/dirb/worlists/common.txt -u <ip>

gobuster dns -d example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3 small.txt

gobuster dns -d google.com -w ~/wordlists/subdomains.txt
```

