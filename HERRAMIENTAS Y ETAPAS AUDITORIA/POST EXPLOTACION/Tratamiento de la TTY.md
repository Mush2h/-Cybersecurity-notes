Una vez accedemos a un equipo Linux con una reverse shell de Netcat, tenemos una nueva shell reversa pero con un formato diferente al que estamos acostumbrados

- Cargamos una pseudoconsola sobre el sistema

Tenemos 2 formas de hacer esto, la primera es la siguiente:

```shell
script /dev/null -c bash
```

Otra de ellas es a través de python, para ello se recomienda aplicar un `whereis python` a nivel de sistema para comprobar las versiones que se encuentran presentes en el sistema, así tendremos que aplicar el siguiente comando seguido de su versión:

```shell
python -c 'import pty;pty.spawn("/bin/bash")'
```

- Configuramos las variables de entorno correctamente

A continuación presionamos la tecla **Ctrl+Z**, esto lo que hará será dejar en segundo plano nuestra sesión (no hay que asustarse). Una vez hecho, aplicamos los siguientes comandos:

```shell
stty raw -echo;fg
		reset xterm
```

Tras introducir el primero, es normal que al escribir **fg** no veamos lo que se está escribiendo, sin embargo se están introduciendo los caracteres. Este comando lo que hará será retornanos a la sesión que teníamos vía **Netcat**. Con el comando **reset** reconfiguraremos nuestra sesión, preguntándonos en la mayoría de los casos a continuación con qué tipo de terminal queremos tratar.

Puede ser que no nos pregunte por el tipo de terminal, en caso de que sí lo haga, introducimos `xterm`, en caso de que no e incluso aunque lo pida, posteriormente aplicamos los siguientes comandos:

```shell
export TERM=xterm
export SHELL=bash
```

Una vez hecho, lo único que queda (paso opcional), es configurar correctamente el redimensionamiento de la terminal, pues en caso de abrir algún editor como nano, veremos que las proporciones no cuadran. Para ello, lo más recomendable es poner a tamaño completo la terminal.

Abrimos otra terminal en nuestro sistema con el mismo redimensionamiento, y aplicamos el siguiente comando:

```shell
stty -a
speed 38400 baud; rows 44; columns 190; line = 0;
```

Este comando vemos las columnas y las filas que ocupa nuestra terminal y le ponemos las mismas proporciuones a la terminal anterior

```shell 
stty rows 44 columns 190
```
