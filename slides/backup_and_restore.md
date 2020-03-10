### Copias de seguridad y Recuperación

La forma recomendada de hacer copias de seguridad es a través de ficheros RDB.

^^^^^^

#### Copia de seguridad

Podemos crear una copia de seguridad ejecutando el comando BGSAVE:

* De manera automática a través de Redis usando el parámetro de configuración `save`
* Lanzando el comando [`BGSAVE`](https://redis.io/commands/bgsave) a través de una tarea cron
* Lanzando el comando [`BGSAVE`](https://redis.io/commands/bgsave) a través de un agente
* etc...

^^^^^^

#### Restauración de la copia de seguridad

En la instancia de redis en la que queramos restaurar el fichero RDB, deshabilitamos AOF

```redis-cli
redis-cli > CONFIG SET appendonly no
OK
redis-cli > CONFIG REWRITE
OK
```

^^^^^^

#### Restauración de la copia de seguridad

Este paso es importante ya que si redis encuentra un fichero AOF en la carpeta `/var/lib/redis`
y `appendonly` está habilitado, utilizará este fichero y no leerá nuestro fichero RDB.

^^^^^^

#### Restauración de la copia de seguridad

Paramos la instancia de redis:

```redis-cli
redis-cli > SHUTDOWN 
```
 
^^^^^^

#### Restauración de la copia de seguridad

Borramos los ficheros *.aof y *.rdb existentes:

```bash 
> rm /var/lib/redis/*.aof /var/lib/redis/*.rdb 
```
 
^^^^^^

#### Restauración de la copia de seguridad

Copiamos/movemos el backup a la carpeta `/var/lib/redis`:

```bash
> cp 2020310-backup.rdb /var/lib/redis/dump.rdb
```

^^^^^^

#### Restauración de la copia de seguridad

Levantamos Redis:

```bash
> sudo rc-service redis start
```

^^^^^^

#### Restauración de la copia de seguridad

Volvemos a activar AOF:

```redis-cli
redis-cli > CONFIG SET appendonly yes
OK
redis-cli > CONFIG REWRITE
OK
```

