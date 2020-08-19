# fluent-bit-on-kubernetes
Kubernetes resources to deploy [Fluent Bit](https://github.com/fluent/fluent-bit).

In this repository you will find the basic Kubernetes resources to deploy Fluent Bit on Kubernetes out of the HELM world.

A really cool tool to manage Kubernetes resources is K9S which I recommend

## Resources
### Deployment
Will deploy Fluent Bit v1.5 over 3 replicas

### Configmap
Contains only a simple INPUT and OUTPUT plugins. The input reads from TCP and the OUTPUT just displays what was 
received over the standard output, which is the logs.

### Service
Just to expose the deployment, as ClusterIP, and the ports.

## Deploy
```shell script
kubectl apply -f ./
```
Once is up, you will see in the Fluent Bit logs something like
```shell script
Fluent Bit v1.5.3                                                                                                                                          │
│ * Copyright (C) 2019-2020 The Fluent Bit Authors                                                                                                           │
│ * Copyright (C) 2015-2018 Treasure Data                                                                                                                    │
│ * Fluent Bit is a CNCF sub-project under the umbrella of Fluentd                                                                                           │
│ * https://fluentbit.io                                                                                                                                     │
│ [2020/08/19 16:16:55] [ info] [engine] started (pid=1)                                                                                                     │
│ [2020/08/19 16:16:55] [ info] [storage] version=1.0.5, initializing...                                                                                     │
│ [2020/08/19 16:16:55] [ info] [storage] in-memory                                                                                                          │
│ [2020/08/19 16:16:55] [ info] [storage] normal synchronization mode, checksum disabled, max_chunks_up=128                                                  │
│ [2020/08/19 16:16:55] [ info] [input:tcp:tcp.0] listening on 0.0.0.0:5170                                                                                  │
│ [2020/08/19 16:16:55] [ info] [http_server] listen iface=0.0.0.0 tcp_port=2020                                                                             │
│ [2020/08/19 16:16:55] [ info] [sp] stream processor started
```

## Testing
Once the resources are deployed, you can do port-forwarding to port tcp:5170 to your localhost. Once done, you can run

```shell script
echo '{"key 1": 123456789, "key 2": "abcdefg"}' | nc 0.0.0.0 5170
```
And you will in the Fluent Bit logs something like this:
```shell script
[0] tcp.0: [1597853873.242894573, {"key 1"=>123456789, "key 2"=>"abcdefg"}]
```

## Destroy
```shell script
kubectl delete -f ./
```