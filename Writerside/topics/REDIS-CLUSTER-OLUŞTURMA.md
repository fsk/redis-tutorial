# REDIS CLUSTER OLUŞTURMA

Bir redis cluster oluşturmak için öncelikle bir tane `redis.conf` dosyası oluşturmamız lazım.

```
port 8000
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
```

