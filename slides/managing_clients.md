### Gestión de clientes

Redis nos facilita diferentes comandos para obtener información sobre qué clientes tenemos
conectados a nuestra instancia de redis.

^^^^^^

#### Gestión de clientes

Vamos conectar dos clientes a la instancia de redis para ver los comandos en acción:

```bash 
> nohup redis-benchmark -c 5 -n 100000 -r 1000 -d 1000 &
> nohup redis-benchmark -c 5 -n 100000 -r 1000 -d 1000 &
```

^^^^^^

#### Gestión de clientes

El primer comando que podemos utilizar para obtener información sobre los clientes es 
[`INFO`](https://redis.io/commands/info)

```redis-cli
redis-cli > INFO CLIENTS
connected_clients:11
client_recent_max_input_buffer:2
client_recent_max_output_buffer:3284800
blocked_clients:0 
```

^^^^^^

#### Gestión de clientes

Es siguiente comando que podemos utilizar es [CLIENT LIST](https://redis.io/commands/client-list)

```redis-cli
redis-cli > CLIENT LIST
id=166 addr=127.0.0.1:45338 fd=13 name= age=11 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=16157 oll=6 omem=98544 events=r cmd=lrange
id=167 addr=127.0.0.1:45340 fd=14 name= age=11 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=16157 oll=6 omem=98544 events=r cmd=lrange
id=168 addr=127.0.0.1:45342 fd=15 name= age=11 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=lrange
id=169 addr=127.0.0.1:45344 fd=16 name= age=11 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=lrange
id=170 addr=127.0.0.1:45346 fd=17 name= age=11 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=lrange
id=164 addr=127.0.0.1:45334 fd=11 name= age=32 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=16157 oll=18 omem=295632 events=r cmd=lrange
id=165 addr=127.0.0.1:45336 fd=12 name= age=32 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=16157 oll=18 omem=295632 events=r cmd=lrange
id=130 addr=127.0.0.1:45266 fd=20 name= age=72 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=26 qbuf-free=32742 obl=0 oll=0 omem=0 events=r cmd=client
id=161 addr=127.0.0.1:45328 fd=8 name= age=32 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=16157 oll=18 omem=295632 events=r cmd=lrange
id=162 addr=127.0.0.1:45330 fd=9 name= age=32 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=16157 oll=18 omem=295632 events=r cmd=lrange
id=163 addr=127.0.0.1:45332 fd=10 name= age=32 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=4 omem=65696 events=rw cmd=lrange 
```

notes:

El significado de cada elemento de la línea se puede ver en la documentación del comando [CLIENT LIST](https://redis.io/commands/client-list)

^^^^^^

#### Gestión de clientes

Para matar un cliente utilizamos el comando [CLIENT KILL](https://redis.io/commands/client-kill)

```redis-cli
redis-cli > CLIENT KILL 127.0.0.1:45328
OK
```