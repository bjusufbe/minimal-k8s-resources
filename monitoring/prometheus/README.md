# Prometheus Helm chart

## What it is?

This is a Helm 3 chart which is used for setup/deployment of Prometheus and additional metrics scrappers (node-exporter and kube-state-metrics) on Kubernetes environment. This Prometheus setup will give you, out of the box, metrics exposed from multiple levels of the cluster on which Prometheus is running:
- cadvisor - container metrics exposed by kubelet on each kubernetes node
- node-exporter - metrics from the kubernetes cluster nodes
- kube-state-metrics - metrics from the kubernetes api about the kubernetes objects itself
- prometheus - metrics about the prometheus itself

## How to use it?

### Quick start

Easiest way to start is by running following command from `./monitoring/prometheus` directory:
```
$ cd ./monitoring/prometheus
monitoring/prometheus $ helm install prometheus . --namespace <NAMESPACE> 
...
NAME: prometheus
LAST DEPLOYED: Tue Aug  4 16:41:45 2020
NAMESPACE: bakir
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Congratulations! You have successfully deployed Prometheus with additional metric scrapers (NodeExporter and KubeStateMetrics)
```

To check that prometheus is deployed successfully, get exposed NodePort:
```
$ kubectl get svc prometheus -n <NAMESPACE>
NAME         TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
prometheus   NodePort   10.111.234.69   <none>        9090:32000/TCP   18m
```
and try to access it with IP addresss of one of the kubernetes cluster nodes. For example: `http://192.168.64.3:32000/graph`

Finally, verify that all metrics scrappers are available and running on the following path: `/targets`

### Additional options

By default, persistence is turned off, but it is expected that, in production environments, you want to have persistence enabled. You can do it by uncommenting out line in the `values.yaml` file:
```
prometheus:
  ...
  persistence:
    enabled: "true"
```

If you want to customize Prometheus Helm chart, you can do it in two ways: declaratevely (by modifying `values.yaml` file before running helm install) or by passing arguments to helm install command which will override default values specified in `values.yaml` file. For example:
```
$ cd ./monitoring/prometheus
monitoring/prometheus $ helm install prometheus . --namespace <NAMESPACE> --set prometheus.replicaCount=2
```

To check all available options that are configurable, you can use following command:
```
helm show values .
```
or simply check `values.yaml` file.