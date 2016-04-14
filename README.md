## Guestbook Example

This example shows how to build a simple, multi-tier web application using [Helm](https://helm.sh) and [Deis Workflow PaaS](https://deis.com/).

The example consists of:

- A web frontend which is installed as a Deis Workflow PaaS App
- And a back-end [redis](http://redis.io/) master (for storage) with a replicated set of redis 'slaves'.

The web frontend interacts with the redis master via javascript redis API calls.

### Prerequisites

This example requires a running [Kubernetes](https://kubernetes.io) cluster and you have installed [Helm](https://helm.sh) and [Deis Workflow Paas](https://github.com/deis/workflow).


### Install

#### Backend install with the Helm

1) We add the remote repo to Helm:
```
$ helm up
$ helm repo add demo-charts https://github.com/deis/demo-charts
$ helm up
```

2) We install our back-end chart
```
$ helm fetch demo-charts/redis-guestbook
$ helm install redis-guestbook
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
$ deis config:set  GET_HOSTS_FROM=env -a guestbook
$ deis config:set  REDIS_MASTER_SERVICE_HOST=redis-master.default -a guestbook
$ deis config:set  REDIS_SLAVE_SERVICE_HOST=redis-slave.default -a guestbook
```

4) Push to remote git repo:
```
$ git push deis master
```

5) Open the App in your browser:
```
$ deis open
```
