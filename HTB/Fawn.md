## Datos principales

* #Enumeracion de puertos
* #FTP
* #Acceso sin credenciales / Credenciales por defecto
* SO : #Linux

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap, en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.16.197
```

Puerto abierto:
	*21* --> FTP

Podemos apreciar que nos encontramos con el puerto 21 abierto y el servicio ftp.
Nos conectamos al servicio FTP:

```console
ftp <ip>
ftp 10.129.16.197
```


### Valores por defecto de FTP

* Login : anonymous
* Password: anonymous

Vemos que podemos acceder con las credenciales por defecto.

Como el comando *cat* o cualquier otro visualizador de documentos no se puede hacer en el servicio ftp vamos a descargarnos en nuestra máquina el fichero con el comando:

```console
ftp> get flag.txt
```