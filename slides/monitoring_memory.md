### Monitorizando la memoria

El comando `INFO MEMORY` nos da una métrica global del uso de la memoria de la instancia de Redis:

```redis-cli
redis-cli > INFO MEMORY
# Memory
used_memory:601525
used_memory_human:587.43K
used_memory_rss:5328896
used_memory_rss_human:5.08M
used_memory_peak:3889157
used_memory_peak_human:3.71M
used_memory_peak_perc:15.47%
used_memory_overhead:600309
used_memory_startup:550069
used_memory_dataset:1216
used_memory_dataset_perc:2.36%
allocator_allocated:568667
allocator_active:5291008
allocator_resident:5291008
total_system_memory:737300480
total_system_memory_human:703.14M
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:9.30
allocator_frag_bytes:4722341
allocator_rss_ratio:1.00
allocator_rss_bytes:0
rss_overhead_ratio:1.01
rss_overhead_bytes:37888
mem_fragmentation_ratio:9.37
mem_fragmentation_bytes:4760229
mem_not_counted_for_evict:210
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:49694
mem_aof_buffer:210
mem_allocator:libc
active_defrag_running:0
lazyfree_pending_objects:0 
```

^^^^^^

#### Monitorizando la memoria

En la documentación del comando [`INFO`](https://redis.io/commands/info) podemos ver la descripción
de algunos de estos valores.

^^^^^^

#### Monitorizando la memoria

El comando [`MEMORY USAGE`](https://redis.io/commands/memory-usage) nos da una estimación del tamaño que ocupa en memoria una clave:

```redis-cli
redis-cli > SET loremipsum "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum interdum, nulla vel vulputate elementum, mauris tellus rhoncus risus, nec auctor mauris nulla ut nunc. Integer magna tortor, gravida ac eleifend a, pretium vel nisi. Etiam interdum enim vitae nibh eleifend id tincidunt massa adipiscing. Nullam dictum ornare ultricies. Phasellus sollicitudin sapien hendrerit orci fringilla laoreet dignissim metus ornare."
OK
redis-cli > MEMORY USAGE loremipsum
(integer) 480 
```


^^^^^^

#### Monitorizando la memoria

El comando [`MEMORY USAGE`](https://redis.io/commands/memory-usage) no incluye 
la memoria que utiliza la clave ni la expiración de la clave:

```redis-cli
redis-cli > set nombre "Curso de redis"
OK
redis-cli > MEMORY USAGE nombre
(integer) 64
redis-cli > set nombre_de_la_clave_mucho_mas_largo "Curso de redis"
OK
redis-cli > MEMORY USAGE nombre_de_la_clave_mucho_mas_largo
(integer) 64  <---- Esto no incluye los bytes extra que hemos consumido
                    usando un nombre de clave mucho más largo ()
redis-cli > set nombre "Curso de redis" EX 300
OK
redis-cli > MEMORY USAGE nombre
(integer) 64 <---- Esto no incluye los bytes extra que hemos consumido
                   almacenando el tiempo de expiración de la clave (300 segundos)


```

^^^^^^

#### Monitorizando la memoria

Por último, el comando [`MEMORY STATS`](https://redis.io/commands/memory-stats) nos devuelve el uso de memoria de la instancia de redis
en formato [Array](https://redis.io/topics/protocol#array-reply) 

```redis-cli
redis-cli > MEMORY STATS
 1) "peak.allocated"
 2) (integer) 3889157
 3) "total.allocated"
 4) (integer) 603044
 5) "startup.allocated"
 6) (integer) 550069
 7) "replication.backlog"
 8) (integer) 0
 9) "clients.slaves"
10) (integer) 0
11) "clients.normal"
12) (integer) 49694
13) "aof.buffer"
14) (integer) 920
15) "lua.caches"
16) (integer) 0
17) "db.0"
18) 1) "overhead.hashtable.main"
    2) (integer) 488
    3) "overhead.hashtable.expires"
    4) (integer) 56
19) "overhead.total"
20) (integer) 601227
21) "keys.count"
22) (integer) 9
23) "keys.bytes-per-key"
24) (integer) 5886
25) "dataset.bytes"
26) (integer) 1817
27) "dataset.percentage"
28) "3.4299197196960449"
29) "peak.percentage"
30) "15.505776405334473"
31) "allocator.allocated"
32) (integer) 570229
33) "allocator.active"
34) (integer) 5291008
35) "allocator.resident"
36) (integer) 5291008
37) "allocator-fragmentation.ratio"
38) "9.278742790222168"
39) "allocator-fragmentation.bytes"
40) (integer) 4720779
41) "allocator-rss.ratio"
42) "1"
43) "allocator-rss.bytes"
44) (integer) 0
45) "rss-overhead.ratio"
46) "1.0071607828140259"
47) "rss-overhead.bytes"
48) (integer) 37888
49) "fragmentation"
50) "9.3451862335205078"
51) "fragmentation.bytes"
52) (integer) 4758667 
``` 

^^^^^^

#### Monitorizando la memoria

La opción `--bigkeys` del comando  `redis-cli` nos muestra un resumen de los tamaños medios 
de las claves que tenemos en nuestra instancia de redis:

^^^^^^

#### Monitorizando la memoria

```bash
> redis-cli --bigkeys

# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).

[00.00%] Biggest hash   found so far '436df0d2-8dd3-4053-a4bf-765535bdca7e' with 3 fields
[00.00%] Biggest hash   found so far '26d8f9ef-dc9d-4b8b-9d2a-b74ae5df2a17' with 8 fields
[00.00%] Biggest hash   found so far '3b077086-1bf1-4273-b62d-6d89f4fab2fe' with 9 fields
[00.00%] Biggest hash   found so far 'b8d4457f-ffae-459a-a279-88581dd79e97' with 10 fields
[01.22%] Biggest string found so far 'clave2' with 6 bytes
[05.56%] Biggest string found so far 'loreipsum' with 423 bytes
[98.14%] Biggest list   found so far 'productos' with 4 items
[100.00%] Sampled 1000000 keys so far

______ summary ______

Sampled 1000009 keys in the keyspace!
Total key length in bytes is 36000083 (avg len 36.00)

Biggest   list found 'productos' has 4 items
Biggest   hash found 'b8d4457f-ffae-459a-a279-88581dd79e97' has 10 fields
Biggest string found 'loreipsum' has 423 bytes

1 lists with 4 items (00.00% of keys, avg size 4.00)
1000001 hashs with 5503400 fields (100.00% of keys, avg size 5.50)
7 strings with 455 bytes (00.00% of keys, avg size 65.00)
0 streams with 0 entries (00.00% of keys, avg size 0.00)
0 sets with 0 members (00.00% of keys, avg size 0.00)
0 zsets with 0 members (00.00% of keys, avg size 0.00) 
```

notes:

Para ver este informe, podemos cargar un millón de claves usando el programa `generator.js`
situado en la carpeta `/usr/local/redis-random-data-generator` de la máquina virtual:

```bash
> node generator.js  hash 1000000
```

^^^^^^

#### Monitorizando la memoria

Por último, tenemos el comando [`MEMORY DOCTOR`](https://redis.io/commands/memory-doctor) que nos hace un
estudio de la situación de memoria de la instancia y nos ofrece posibles soluciones. 

^^^^^^

#### Monitorizando la memoria

```redis-cli
redis-cli > MEMORY DOCTOR
Sam, I detected a few issues in this Redis instance memory implants:

  High allocator fragmentation: This instance has an allocator external 
   fragmentation greater than 1.1. This problem is usually due either to a 
   large peak memory (check if there is a peak memory entry above in the report) 
   or may result from a workload that causes the allocator to fragment memory a lot. 
   You can try enabling 'activedefrag' config option.

  High total RSS: This instance has a memory fragmentation and RSS overhead greater 
   than 1.4 (this means that the Resident Set Size of the Redis process is much larger 
   than the sum of the logical allocations Redis performed). This problem is usually 
   due either to a large peak memory (check if there is a peak memory entry above in 
   the report) or may result from a workload that causes the allocator to fragment 
   memory a lot. If the problem is a large peak memory, then there is no issue. 
   Otherwise, make sure you are using the Jemalloc allocator and not the default 
   libc malloc. N o t e: The currently used allocator is "libc".

I'm here to keep you safe, Sam. I want to help you. 
```