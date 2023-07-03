## Datos principales

* #Enumeracion de puertos
* #Telnet
* #Acceso sin credenciales / Credenciales por defecto
*  SO : #Linux

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap, en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.172.166
```

Puerto abierto:
	*23* --> Telnet

Para saber más acerca del servicio al que me estoy enfrentando vuelvo  a hacer otro nmap más exhaustivo que me diga que versión está corriendo del servicio.

```bash
nmap -p 23 -sCV  10.129.172.166
```

Podemos ver la siguiente informacion:

```console
PORT   STATE   SERVICE  VERSION
23/tcp open    telnet   linux telnetd
Service Info: OS: Linux
```

A partir de ahora sabiendo que esta el servicio telnet abierto vamos a probar a conectarnos con las credenciales por defecto para conectarnos a telnet con el comando:

```console
telnet <ip>
telnet 10.129.172.166
```

Para poder acceder al servicio tenemos que loguearnos para poder acceder, por lo tanto comprobamos los valores por defecto de telnet

### Valores por defecto de Telnet

* Login : root
* Password: (Vacío)

Una vez que estamos dentro de la máquina víctima comprobamos que privilegios tenemos , se puede ver somos root listamos los archivos y podemos ver la flag.txt.

```console
root@Meow:~# ls
flag.txt  snap
```
