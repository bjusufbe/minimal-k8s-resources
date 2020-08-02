# Selenium Grid 3 Helm chart

## What it is?

This is a Helm 3 chart which is used for setup/deployment of Selenium Grid 3 (Hub + Chrome/Firefox nodes) on Kubernetes environment.
Kubernetes, as an environment, is ideal place to deploy Selenium Grid since we can harness full power of distributed cluster which gives us ability to scale number of nodes in Selenium Grid very easily. This Helm chart makes it even easier to do the setup of needed resources and having Selenium Grid infrastructure up and running in a few moments.

## How to use it?

### Quick start

Easiest way to start is by running following command from `./selenium-grid3` directory:
```
$ cd ./selenium-grid3
selenium-grid3 $ helm install selenium-grid . --namespace <NAMESPACE> 
...
NAME: selenium-grid
LAST DEPLOYED: Sun Aug  2 21:08:45 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Congratulations! You have successfully deployed testing infrastructure (Selenium Grid)
```

To check that everything is deployed successfully (both Hub and nodes), you can do following:
```
$ kubectl get pods -n <NAMESPACE>
...
NAME                            READY   STATUS      RESTARTS   AGE
node-chrome-5ddd685f5f-ddw8f    1/1     Running     0          65m
node-firefox-5544948989-n8nsg   1/1     Running     0          65m
selenium-hub-658f5cd7b-pzsw4    1/1     Running     0          65m
```

To check that nodes are connected to the hub successfully (if selenium hub is deployed with Service type NodePort which is by default), you can do following:
```
$ kubectl get svc selenium-hub -n <NAMESPACE>
...
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
selenium-hub   NodePort    10.105.190.148   <none>        4444:31500/TCP   68m
```
In your browser, try to access the 31500 port with IP address of one of the kubernetes cluster nodes. For example: `http://192.168.64.3:31500/grid/console`

### Additional options

If you want to customize Selenium Grid Helm chart, you can do it in two ways: declaratevely (by modifying `values.yaml` file before running helm install) or by passing arguments to helm install command which will override default values specified in `values.yaml` file. For example:
```
$ cd ./selenium-grid3
selenium-grid3 $ helm install selenium-grid . --namespace <NAMESPACE> --set chrome.replicaCount=2
```

To check all available options that are configurable, you can use following command:
```
helm show values .
```
or simply check `values.yaml` file