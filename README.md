
## Guestbook Example

[![Docker Repository on Quay](https://quay.io/repository/deis/example-guestbook/status "Docker Repository on Quay")](https://quay.io/repository/deis/example-guestbook)

This example shows how to build a simple, multi-tier web application using [Helm Classic](https://helm.sh) and [Deis Workflow](https://deis.com/workflow/).

The example consists of:

- A web frontend which is installed as a Deis Workflow
- And a back-end [Redis](http://redis.io/) `master` (for storage) with a replicated set of Redis `slaves`

The web frontend interacts with the Redis `master` API via JavaScript calls.

### Prerequisites

This example requires a running [Kubernetes](https://kubernetes.io) cluster and you have installed [Helm Classic](https://helm.sh) and [Deis Workflow](https://deis.com/workflow/).

#### Backend install with the Helm Classic

1) We add the remote repo to Helm:
```
$ helmc up
$ helmc repo add demo-charts https://github.com/deis/demo-charts
$ helmc up
```

2) We install our back-end chart
```
$ helmc fetch demo-charts/redis-guestbook
$ helmc install redis-guestbook
```

#### Front-end install with deis cli

1) Clone the repo:
```
$ git clone https://github.com/deis/example-guestbook.git
$ cd example-guestbook
```

2) Create guestbook App:
```
$ deis create guestbook
```

3) Set env vars so the App knows where to connect to redis cluster:
```
$ deis config:set GET_HOSTS_FROM=env REDIS_MASTER_SERVICE_HOST=redis-master.default REDIS_SLAVE_SERVICE_HOST=redis-slave.default PORT=80 -a guestbook
```

4) Push to remote git repo:
```
$ git push deis master
```

5) Open the App in your browser:
```
$ deis open
```
