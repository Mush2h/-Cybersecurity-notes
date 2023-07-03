#metasploit

## Configuración inicial

La primera vez que usamos metasploit debemos crear la estructura de la base de datos , para ello lo  primero es iniciar el sistema gestor de base de datos el cual se trata de PostgreSQL 

Iniciamos el servicio:
```shell 
service postgresql start
```

Creamos la estrucutra de la base de datos para ellos usaremos el comando 

```shell
msfdb init
```

Comprobamos el estado con el siguiente comando 
```shell
msfconsole -q -x db_status
```

si todo ha ido bien deberia salir la siguiente salida
```shell
[*] Connected to msf.Connection type: postgresql
```

### Creamos un workspace 

Podemos crear zonas de trabajo para organizar las maquinas que estamos auditando 

```shell

#Ayuda de workspace
workspace -h 

#Creamos workspace
workspace -a <nombre>
workspace -a maquina1

#Listamos los workspace
workspace
```

### Nmap desde metasploit Automatico

Tenemos la opcion de inicar el escaneo con nmap directamente desde metasploit para ello utilizamos el comando siguiente

```shell
db_nmap -sP <ip>
```

Los dispositivos se agregaran automaticamente a la base de datos, con comando hosts podemos ver los hosts agregados.

```shell
hosts
```

```shell
Hosts                                                                                                                                                                                                                                       
=====                                                                                                                                                                                                                                                                                                                                                                                                  
address         mac                name  os_name  os_flavor  os_sp  purpose  info  comments                                                                                                                                                 
-------         ---                ----  -------  ---------  -----  -------  ----                                                                                                                                                  
192.168.37.136  00:0C:29:0A:E6:45                                                                                                                                                                                                           
         
```

#### Manual

Primero hacemos el escaner y lo pasamos a un formato xml para importarlo desde metasploit

```shell
nmap -p- -sS -O 192.168.37.136 --min-rate 9000 -oX resultado_nmap.xml
```

Desde metasploit lo importamos de la siguiente manera

```shell
msf6 > db_import /home/kali/Desktop/resultado_nmap.xml
```

Podemos comprobarlo con la siguiente opcion:
```shell
msf6> services
```

Aparecen todos los puertos que se encuentran en la maquina