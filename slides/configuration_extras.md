### Algunos extras sobre `redis.conf`

El fichero de configuración `redis.conf` permite el uso de `include` para incluir otro fichero de 
configuración dentro del actual

^^^^^^

### `include`

Se puede separar la configuración en múltiple ficheros:

```bash 
# /etc/redis.conf

include /etc/redis-common.conf
include /etc/redis-master.conf
```

^^^^^^

### Parámetros de aranque

El uso de parámetros de arranque permite sobreescribir la configuración que se carga del fichero.

```bash 
> redis-server /etc/redis.conf --port 6390 --loglevel debug
```

Independientemente de lo que ponga en el fichero de configuración, el puerto y el loglevel serán los
que se han pasado por consola.

^^^^^^

### `INFO STATS`

Nos devuelve estadísticas de uso de la instancia de Redis:

```redis-cli
redis-cli > info stats
# Stats
total_connections_received:144209
total_commands_processed:334041789
instantaneous_ops_per_sec:3
total_net_input_bytes:62472396057
total_net_output_bytes:34379553436
instantaneous_input_kbps:0.54
instantaneous_output_kbps:0.01
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:1
evicted_keys:0
keyspace_hits:32323050
keyspace_misses:9
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:438
migrate_cached_sockets:0 
``` 

^^^^^^

#### `INFO STATS`

El comando `CONFIG RESETSTAT` inicializa a cero algunos de los contadores que nos muestra el
comando `INFO STATS`:

* `keyspace_hits`
* `keyspace_misses`
* `total_commands_processed`
* `total_connections_received`
* `expired_keys`
* `rejected_connections`
* `latest_fork_usec`
