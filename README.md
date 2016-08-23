# How to use this image
## start a redis instance
```
$ docker run --name some-redis -d stepsaway/redis
```
This image by default is unix socket only. Therefore, no TCP connections will be accepted.

## start with persistent storage
```
$ docker run --name some-redis -d stepsaway/redis --appendonly yes
```
If persistence is enabled, data is stored in the VOLUME /data, which can be used with --volumes-from some-volume-container or -v /docker/host/dir:/data (see docs.docker volumes).

For more about Redis Persistence, see http://redis.io/topics/persistence.

## connect to it from an application
The client will find the socket at /var/run/redis/redis.sock
```
$ docker run --name some-app --volumes-from some-redis -d application-that-uses-redis
```
... or via redis-cli
```
$ redis-cli -s /var/run/redis/redis.sock
```

Additionally, If you want to use your own redis.conf ...
You can create your own Dockerfile that adds a redis.conf from the context into /data/, like so.
```
FROM redis
COPY redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]
```

## maxmemory
The maxmemory default is 128GB, different applications will have different requirements. Therefore, this value should be set at runtime.

```
$ docker run --name some-redis -d stepsaway/redis --maxmemory 2GB
```
