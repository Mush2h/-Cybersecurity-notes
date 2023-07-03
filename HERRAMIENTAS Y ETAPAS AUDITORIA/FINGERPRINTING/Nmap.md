#Nmap

* Creado inicialmente como herramienta de código abierto para eralizar escaneos de puertos que corría bajo Linux.
* Es un potente  escáner de redes disponible para sistemas operativos 
* Las principales tareas :
	* Descubrimiento de hosts de una red
	* Deteccion de los puertos abiertos en cada equipo 
	* Determinación de los servicios que está corriendo en cada maquina 
	* Descubrimiento del sistema operativo y su version 
	* Obtencion de características del hardware de red instalado
	* Tiene Script desarrollados que se denominan NSE 

## Descubrimiento de host

* Esta funcion contamos con el comando *nmap*
```bash
nmap 192.168.1.130
nmap 192.168.1.1-254
nmap 192.168.1.0/24
nmap microsoft.com
```

### Opciones avanzadas de parámetros

* -iL *fichero* : Buscará hosts en todas las direcciones incluidas en el fichero especificado.
* -excludefile *fichero* : Buscará en todas las direcciones posibles excepto en aquellas que están incluidas en el fichero especificado.
* -PS *puerto* : busca host enviando un **TCP SYN** al puerto n especificado
* -PA *puerto* : busca hosts enviando un **TCP ACK** al puerto especificado.
* -PU puerto : busca hosts enviando un datagrama **UDP** al puerto especificado
* -n : no realizará la resolución de DNS
* -R : resolverá los DNS de todos los hosts objetivo que encuentre.
* -treceroute : Trazará la ruta de los objetivos que encuentre


## Análisis de puertos

* Conocer el estado de todos los puertos de un objetivo determinado, los 65500.

```bash
nmap <ip> -p-
nmap 192.168.1.1 -p-
```

* También podemos analizar los principales 100 puertos más comunes .

```bash
nmap <ip> -F
nmap 192.168.1.1 -F
```

### Opciones avanzadas de parámetros

* -sS : sondeo TCP SYN
* -sT : sondeo TCP connect
* -sA : sondeo TCP ACK
* -sw : sondeo de ventana TCP
* -sM : sondeo TCP Maimon (Un tipo de escaneo sutil y no muy agresivo para pasar desapercibido)

## Detección de Servicios y Sistema Operativo

En funcion de los puertos abiertos , es capaz de averiguar el sistema operativo y las versiones de todos ellos .
* -sV : activa la detencción de versiones
* --allports : no excluye ningún puerto de la detección de versiones
* --version-intensity *intensidad* : Establece la intensidad con la que se va a realizar de 0-9 siendo menos a mas agresivo, pero requiere mas recursos.
* -sR : sondeo RCP
* -**O** : Detección del sistema operativo
* -v / -V :aumenta o disminuye el nivle de detalle del análisis

## Evasión de Cortafuegos y detectores de instrusión

* Son comandos pueden ser utilizados para intentar burlar aquellos sistemas de seguridad que haya instalados.

* No se trata de un trabajo mecánico

### Opciones avanzadas
*  -f : Fragmenta los paquetes enviados
*  -D <señuelo1,señuelo2> : esconde el análisis con señuelos
*  -S *ip* : falsea la dirección IP de origen.
*  -g *puerto* : falsea el puerto de origen
*  -spoof-mac *direccion mac* : falsea la direccion MAC de origen
* -e *interfaz* : especifica la interfaz a utilizar
* --data-length *numero* : Añade datos aleatorios a los paquetes enviados en el sondeo

```bash
nmap -f <ip>
```

## Opciones de control de tiempo y rendimiento

* -T *número* : establece la velocidad que se realiza el análisis. El número es de 0 a 5 cuanto mas bajo sea ,sera mas sigiloso y llevará más tiempo 
* --min-parallelism *número* / ----max-parallelism *número* ajustan el número de sondas enviadas en paralelo.
* --host-timeout *tiempo* : desecha equipos objetivos lentos

## Exportar la salida a un formato en concreto

* -oX *fichero* : salida XML
(en este formato se puede importa a mestasploit)
* -oG fichero : salida "grepeable"
* -oS *fichero* : salida Script Kiddie
* -oA *fichero* : todos los formatos

```bash
nmap -p- -sS 192.168.37.136 --min-rate 9000 -oG resultado_nmap.grep
nmap -p- -sS 192.168.37.136 --min-rate 9000 -oX resultado_nmap.xml
```

## Ejecución de script

* -sC : ejecuta el ánalisis con los scripts por defecto 
* -script *script* : ejecuta el o los scripts específicado
* -script-args *argumento=valor* : proporciona argumentos y sus valores correspondientes al scripts a ejecutar
* -script-trace :  muestra los intercambios de información de entrada y salida que se produzcan 
* -6 : activa el sondeo en IPv6.

```bash
nmap -p 21 --script ftp-brute <ip>
```


## Escaneo de menos agresivo a más agresivo.

```bash
nmap <ip>
nmap -sS -sU <ip>
nmap -sS -T4 -A -v <ip>
nmap -sS -sU -sV <ip>
nmap -sS -sU -sV -sC --script all <ip>

```


## Escaneos interesantes

```bash
nmap --script=vuln --script=auth -v --min-rate 9000 192.168.37.136
```

* Lanza scripts con autenticacion por defecto en los servicios que encuentre

```bash
nmap --script=firewall-bypass -v 192.168.1.1
```

* Lanza script para intentar bypasear los firewall 

## Escaneo silenciosos pero tardan mucho 

```shell 
nmap -T2 -Pn -n -sV <ip>
nmap -sS -T4 -Pn -p 1-65535 -n --min-rate 1000 --max-retries 0 <objetivo>

#puertos udp
nmap -sU -T4 -Pn -p 1-65535 -n --min-rate 1000 --max-retries 0 <objetivo>
```