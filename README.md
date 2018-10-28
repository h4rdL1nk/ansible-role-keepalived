# Ansible role for docker-based etcd cluster deployment ( molecule driven tests )

This role installs and configure docker community-edition with devicemapper storage driver.

The ```molecule.yml``` file has been modified to add an additional disk for docker storage only.

## General module testing procedure

### Export required variables
```
MOLECULE_EPHEMERAL_DIRECTORY="${PWD}/.molecule" 

export MOLECULE_EPHEMERAL_DIRECTORY
```

### Launch molecule role testing commands
```
molecule create
molecule converge
molecule idempotence
molecule destroy
```

### Launch complete molecule test
```
molecule test
```

### Etcd cluster test from inside any instance
```
[vagrant@instance-3 ~]$ sudo docker exec etcd /etcdctl --endpoints https://172.28.128.12:2379,https://172.28.128.13:2379,https://172.28.128.14:2379 --cert-file /ssl/peer/peer.pem --key-file /ssl/peer/peer.key --ca-file /ssl/ca/ca.pem cluster-health
member 9e5457ae5fb9acd5 is healthy: got healthy result from https://172.28.128.12:2379
member b23c3d966927f407 is healthy: got healthy result from https://172.28.128.13:2379
member d2bdee87db4f4958 is healthy: got healthy result from https://172.28.128.14:2379
cluster is healthy

[vagrant@instance-3 ~]$ sudo docker exec etcd /etcdctl --endpoints https://172.28.128.12:2379,https://172.28.128.13:2379,https://172.28.128.14:2379 --cert-file /ssl/peer/peer.pem --key-file /ssl/peer/peer.key --ca-file /ssl/ca/ca.pem member list
9e5457ae5fb9acd5: name=instance-1 peerURLs=https://172.28.128.12:2380 clientURLs=https://172.28.128.12:2379 isLeader=false
b23c3d966927f407: name=instance-2 peerURLs=https://172.28.128.13:2380 clientURLs=https://172.28.128.13:2379 isLeader=false
d2bdee87db4f4958: name=instance-3 peerURLs=https://172.28.128.14:2380 clientURLs=https://172.28.128.14:2379 isLeader=true

vagrant@instance-3 ~]$ sudo docker exec etcd /etcdctl --endpoints https://172.28.128.12:2379,https://172.28.128.13:2379,https://172.28.128.14:2379 --cert-file /ssl/peer/peer.pem --key-file /ssl/peer/peer.key --ca-file /ssl/ca/ca.pem mkdir test

[vagrant@instance-3 ~]$ sudo docker exec etcd /etcdctl --endpoints https://172.28.128.12:2379,https://172.28.128.13:2379,https://172.28.128.14:2379 --cert-file /ssl/peer/peer.pem --key-file /ssl/peer/peer.key --ca-file /ssl/ca/ca.pem set /test/key value

[vagrant@instance-3 ~]$ sudo docker exec etcd /etcdctl --endpoints https://172.28.128.12:2379,https://172.28.128.13:2379,https://172.28.128.14:2379 --cert-file /ssl/peer/peer.pem --key-file /ssl/peer/peer.key --ca-file /ssl/ca/ca.pem get /test/key
value
```
