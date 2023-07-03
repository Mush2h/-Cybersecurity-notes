```ruby
#!/bin/bash
color_rojo="\e[91m"
color_verde="\e[92m"

# Verifica la cantidad de argumentos
if [ $# -ne 2 ]; then
    echo "Uso: ./script <imagen> <diccionario>"
    exit 1
fi

# Asigna los argumentos a variables
imagen="$1"
diccionario_claves="$2"

# Verifica la existencia de la imagen
if [ ! -f "$imagen" ]; then
    echo "La imagen $imagen no existe."
    exit 1
fi

# Verifica la existencia del diccionario
if [ ! -f "$diccionario_claves" ]; then
    echo "El diccionario $diccionario_claves no existe."
    exit 1
fi

# Recorre el diccionario de claves
encontrado=false
while IFS= read -r clave
do
    # Intenta extraer el archivo oculto utilizando la clave actual
    steghide extract -sf "$imagen" -p "$clave" >/dev/null 2>&1

    # Verifica el resultado de la extracción
    exit_code=$?
    if [ $exit_code -eq 0 ]; then
        echo -e "${color_verde}[+] Archivo extraído correctamente con la clave: $clave ${color_verde}"
        encontrado=true
        break
    fi
done < "$diccionario_claves"

if [ "$encontrado" = false ]; then
    echo -e "${color_rojo}[-] Contraseña no encontrada.${color_}"
fi

```

## String a imagenes

Si le hacemos string a una imagen nos da información que puede contener en el interior 

```shell 
strings <imagen>.png
```