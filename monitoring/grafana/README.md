# Grafana Helm chart

## What it is?

This is a Helm 3 chart which is used for setup/deployment of Grafana, connecting to the datasource (Prometheus) and provisioning example Grafana dashboard which gives you important information about cpu/memory (limit/request/used) ratio's per each pod on the cluster out of the box, which can be very useful for fine tunning the performances of the your workload on k8s cluster.

This is an example of such simple Grafana dashboard:

![](https://github.com/bjusufbe/minimal-k8s-resources/blob/master/monitoring/grafana/img/grafana-dashboard.png)

## How to use it?

### Quick start

Easiest way to start is by running following command from `./monitoring/grafana` directory:
```
$ cd ./monitoring/grafana
monitoring/grafana $ helm install grafana . --namespace <NAMESPACE> 
...
NAME: grafana
LAST DEPLOYED: Tue Aug  4 15:57:46 2020
NAMESPACE: bakir
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Congratulations! You have successfully deployed Grafana and loaded pre-defined dashboards
```

To check that grafana is deployed successfully, get exposed NodePort:
```
$ kubectl get svc grafana -n bakir
NAME      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
grafana   NodePort   10.96.119.252   <none>        3000:32001/TCP   73m
```
and try to access it with IP address of one of the kubernetes cluster nodes. For example: `http://192.168.64.3:32001/`

Finally, connect with credentials that you set in `values.yaml` and you will be able to see that:
1. Datasource Prometheus is already added
2. "CPU & Memory optimisation per pod" dashboard is provisioned and can be used

### Additional options

By default, persistence is turned off, but it is expected that, in production environments, you want to have persistence enabled. You can do it by uncommenting out line in the `values.yaml` file:
```
grafana:
  ...
  persistence:
    enabled: "true"
```

If you want to specify other credentials for Grafana login (other than default ones), best way to do it is by specifying them when doing helm installation in following way:
```
$ cd ./monitoring/grafana
monitoring/grafana $ helm install grafana . --namespace <NAMESPACE> --set grafana.security.secret.username=<USERNAME> --set grafana.security.secret.password=<PASSWORD>
```

Also, you can turn on/off (by default it is turned on), provisioning of datasource and dashboards, by altering `values.yaml` in following way:
```
grafana:
  provisioning:
    dashboard: 
      enabled: "false" # if true, cpu-memory-dashboard will be provisioned
      ...
    datasource: 
      enabled: "false" # if true, prometheus datasource will be added
      ...
```

If you want to customize Grafana Helm chart, you can do it in two ways: declaratevely (by modifying `values.yaml` file before running helm install) or by passing arguments to helm install command which will override default values specified in `values.yaml` file. For example:
```
$ cd ./monitoring/grafana
monitoring/grafana $ helm install grafana . --namespace <NAMESPACE> --set grafana.replicaCount=2
```

To check all available options that are configurable, you can use following command:
```
helm show values .
```
or simply check `values.yaml` file.
