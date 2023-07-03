## Datos principales

* #Enumeracion 
* #Php 
* #gobuster
* #Acceso sin credenciales / Credenciales por defecto
* SO : #Linux

## Enumeración de puertos

Para  empezar el escaneo usamos la herramienta Nmap , en nuestra fase de descubrimiento , como se puede apreciar tenemos los siguientes puertos abiertos:

```ruby
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.125.209
```

Puertos abiertos:
80 --> http

Para averiguar más acerca de la página vamos a usar la aplicación Gobuster para extraer los posibles directorios usando un diccionario.

```ruby
gobuster dir -w <diccionario> -u <ip>
gobuster dir -w /usr/share/dirb/worlists/common.txt -u 10.129.125.209
```

Podemos ver que nos encontramos con un archivo que se llama *admin.php* en el navegador y  vamos a ver si podemos acceder.

![[Pasted image 20230409172230.png]]
Como podemos ver tenemos un formulario, vamos a probar con los usuario típicos por defecto …. Tras probar varios resultado finalmente he ha podido acceder con:

* Login: **admin**
* password: **admin**

Al acceder obtenemos la flag en el navegador.

Aunque lo suyo es usar alguna herramienta como Hydra para atacar el formulario 

