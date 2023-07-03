#recon-ng
* Framework desarrollado en python, ofrece al usuario la recoleccion de informacion y reconocimiento de redes de forma automatizada.
* Interfaz similar a [[Metasploit]] basa su funcionamiento en el uso de diferentes módulos 
* Para usar algunos modulos se requiere de una api y ya la pagina se encuntra caída

* Ya instalada en kali, para instalarla sin estamos en otro linux
```bash
apt-get install recon-ng
```

## ¿Cómo se usa?

```bash
recon-ng 
```

una vez inicie la aplicacion podemos hacer las siguientes opciones

### Workspaces
La aplicacion nos da la posibilidad  de trabajar con workspaces 

```bash
[recon-ng][default] > workspaces
```

* Listar workspaces
 ```console
 [recon-ng][default] > workspaces list
```

* Crear workspaces (por defecto estamos en workspaces default)
```console
[recon-ng][default] > workspaces create [Nombre]
[recon-ng][default] > workspaces create Prueba1
```

* Cargar un workspace
```console
[recon-ng][default] > workspaces load prueba1
[recon-ng][prueba1] > 
```

* Borrar un workspace
```console
[recon-ng][default] > workspaces remove [NombreWorkspace]
[recon-ng][default] > workspaces remove prueba1
```

### Trabajar con la base de datos 

* Podemos trabajar con la base de datos para guardar toda la informcacion y poder constultarla más tarde.

* ¿Como  se guarda la informacion?
```console
[recon-ng][default] > db schema
```

* Borramos infomación de la base de datos 
```console
[recon-ng][default] > db delete [información a borrar]
```

* Insertamos un dominio en la base de datos:
```console 
[recon-ng][default] > db insert domains
domain (TEXT): tesla.com
```

### Mostramos información almacenada

* Para poder ver la informacion recolectada como dominio , credenciales , host,podemos usar el siguiente comando:
```console
[recon-ng][default] > show [informacion]
[recon-ng][default] > show hosts
[recon-ng][default] > show domains
[recon-ng][default] > show credentials
```

### Creando snapshots

* Si queremos crear snapchots o backups de los workspaces hacemos el siguiente comando:

```console
[recon-ng][default] > snapshot take
```

* Borrar snapshots

```console
[recon-ng][default] > snapshot remove [nombre snapshots]
```

* Listar snapshots
```console
[recon-ng][default] > snapshots list
```

* Cargar snapshots
```console
[recon-ng][default] > snapschot load [nombre snapshots]
```

### Modulos
* Son las herramientas que podemos descargarnos del marketplace para poder utilizarlas con el finde de automatizar tareas de recolección:
* Listamos las herramientas del marketplace

```console
[recon-ng][default] > marketplace search
```

Obtenemos el resultado de todos los módulos disponibles y una información adicional 

* Path: Es la ruta o el nombre del módulo
* Versión : Es la version del módulo 
* Status: Si la tenemos instalada o no
* Updated: La fecha en la que se modifico por última vez
* D: Si el módulo tiene dependencias o no
* K: Son claves de terceros que tendremos que proporcionar para poder correr el módulo de manera satisfactoria

* #### Busqueda más acotada
	* Si queremos unas busquedad a partir de una (keyword)

```console
[recon-ng][default] > marketplace search [keyword]
[recon-ng][default] > marketplace search credentials
```

### Instalar modulos

Si queremos intalar algun módulo que tenemos en el Marketplace:
```console
[recon-ng][default] > marketplace install [path]
[recon-ng][default] > marketplace install /recon/companies-domains/pen
```

Tambien podemos instalarlos todos con :
```console 
[recon-ng][default] > marketplace install all
```

Puede que nos de un **error** eso significa que tenemos que cargar en nuestras keys, la key que se nos genera al darnos de alta en la web:
* **Página caída**

Usamos el modulo 
Hay algunos modulos que no necesitan cargar la api con los hashes 

```console 
[recon-ng][default] > modules load [módulo]
[recon-ng][default] > modules load recon/companies-contacts/pen
[recon-ng][default][pen] >

```

* Listamos los valores que podemos definir
```console
[recon-ng][default][pen] > options list
```

* Definimos un valor 
```console
[recon-ng][default][pen] > options set [campo][valor]
[recon-ng][default][pen] > options set SOURCE ejemplo.com
```

## Ejecutamos un modulo

```console
[recon-ng][default][pen] > run
```

* Salimos del módulo

```console
[recon-ng][default][pen] > back
```


## Añadimos una API key

```console
[recon-ng][default] > keys add [nombre api] [key]
[recon-ng][default][pen] > keys add shoda_api 123456789
```

* Listamos las key 
```console
[recon-ng][default][pen] > keys list
```

* Borramos API Keys que tenemos cargada 
```console
keys remove [nombre API key]
key remove shodan_api
```