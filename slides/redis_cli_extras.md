### Algunos extras sobre `redis-cli`

^^^^^^

### Ejecutar comandos

Para ejecutar un comando de Redis basta con añadirlo como último argumento de `redis-cli`

```bash
> redis-cli HMSET user:1 nombre Miguel apellido "de Unamuno"
OK
> _ 
```

^^^^^^

#### Ejecutar comandos

Una vez ejecutados los comandos, `redis-cli` devuelve el control a la terminal.

^^^^^^

### Formato CSV

Podemos pedir los datos en formato CSV:

```bash
> redis-cli --csv HGETALL user:1
"nombre","Miguel","apellido","de Unamuno"
> _ 
```

^^^^^^

### Ejecución de un comando en bucle

La opción `-r` nos permite ejecutar el mismo comando un número definida de veces.

La opción `-i` añade un retraso en segundos entre esas ejecuciones:

```bash
> redis-cli -r 5 -i 1 INFO MEMORY | grep used_memory:
used_memory:601525
used_memory:601525
used_memory:601525
used_memory:601525
used_memory:601525
```

^^^^^^

### Búsqueda

```bash
> bin/redis-cli --scan --pattern 'session:*7ab0'
session:e9788147-a4dd-470d-af6b-df1e2fae7ab0
session:ef04be00-b4f2-4778-8023-82994d237ab0
session:4764e56f-a73e-4e64-bc68-03e51e067ab0
session:b030fb27-b560-4c39-8892-c5f6989f7ab0
session:51588697-191f-4e71-9a69-6540ff397ab0
session:37f0259c-f7ea-4fde-9883-ec3298e77ab0
session:c45db486-bfb2-4240-91b3-fd9850ba7ab0
session:7226bb09-b03a-4393-b98d-5f3f59097ab0
```

^^^^^^

### Más información

```bash
> redis-cli -h
```


