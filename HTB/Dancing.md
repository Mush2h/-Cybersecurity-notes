## Datos principales

* #Enumeracion de puertos
* Servicio #smb
* #Acceso sin credenciales / Credenciales por defecto
*  SO : #Windows

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap, en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.69.135
```

Puerto abierto:
	 *135* -->  msrpc
	 *139* -- > netbios-ssn
	 *445* --> **microsoft-ds**
	 *47001* --> winrm
	 *49664* --> ???
	 *49665* --> ???
	 *49666* --> ???
	 *49667* --> ???
	 *49668* --> ???
	 *49669* --> ???

Vamos a hacer un escaneo más exhaustivo a los puertos encontrados para ver si podemos sacar más información con nmap y con la opción *-sCV* para que use los scripts y así podamos extraer más información.

```bash
nmap -p 135,139,445,47001,49664,49665,49666,49667,49668,49669 -sCV 10.129.69.135
```

Nos vamos a centrar en el puerto 445 que se encuentra abierto con el servicio de Microsoft-ds.

Vamos a intentar conectarnos al servicio smb

```bash
smbclient -L //<ip>
smbclient -L //10.129.69.135
```

El parámetro "-L" en smbclient se utiliza para listar los recursos compartidos disponibles en un servidor que utiliza el protocolo SMB (Server Message Block). Cuando se utiliza el comando "smbclient -L", se establece una conexión con el servidor SMB especificado y se solicita una lista de los recursos compartidos disponibles en ese servidor.

La sintaxis completa del comando es la siguiente:

```bash
smbclient -L <nombre_del_servidor> [-U <nombre_de_usuario>%<contraseña>]
```

Donde:

-   `<nombre_del_servidor>` es el nombre o la dirección IP del servidor que se va a consultar.
-   `<nombre_de_usuario>` y `<contraseña>` son las credenciales de inicio de sesión que se utilizarán para establecer la conexión con el servidor (opcional).

Si se omite el nombre de usuario y la contraseña, se utilizará el usuario y la contraseña actuales del sistema operativo en el que se está ejecutando el comando. Si se proporcionan credenciales de inicio de sesión, se utilizarán para establecer la conexión con el servidor SMB.

vemos que nos pide una contraseña pero le damos `Enter` y nos lista los grupos de trabajo existentes , los que tiene un '$' no podemos acceder a ellos pero vemos que existe workShare por lo tanto vamos a intentar entrar.

```bash
   Sharename   type    Comment
   ---------   ----    -------
   ADMIN$      Disk    Remote Admin
   C$          Disk    Default share
   IPC$        IPC     Remote IPC
   WorkShares  Disk
```

Para entrar usamos el siguiente comando:

```bash
smbclient \\\\<ip>\\WorkShares
smbclient \\\\10.129.125.36\\WorkShares
```

De esta manera podemos acceder con éxito y sin contraseña, nos aparecera una terminal tal qeu asi:

```console
Password for [WORKGROUP\root]
smb:\>
```

Para ver los directorios que podemos acceder usamos *ls* y podemos ver que hay dos directorios con archivos que tambien podemos usar *cd* para acceder al directorio

```console
Password for [WORKGROUP\root]
smb:\> ls
	.
	..
	Amy.J
	James.P

```

Para poder descargar la flag y verla en nuestro dispositivo lo hacemos con el siguiente comando: 

```console
smb:\James.P\> get flag.txt 
```