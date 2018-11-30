# Installation



## Installation

With an introduction to the core concepts in the previous section, lets move onto setting up foremast and using it.

### Install All-in-One

#### Minikube

If you don't have kubenertes cluster, you can have you own minikube setup in your local environment. Please check out minikube settings [https://kubernetes.io/docs/setup/minikube/](https://kubernetes.io/docs/setup/minikube/).  
Once you got minikube installed in your local, please use our suggested minikube startup shell, [https://github.intuit.com/dev-containers/foremast/blob/master/deploy/minikube.sh](https://github.intuit.com/dev-containers/foremast/blob/master/deploy/minikube.sh) The shell gives enough memory to make sure all pods will be started correctly.

#### Prometheus

```text
$ kubectl create -Rf deploy/prometheus-operator/
$ kubectl get pods -n monitoring

NAME                                  READY   STATUS    RESTARTS   AGE
alertmanager-main-0                   2/2     Running   0          6h
alertmanager-main-1                   2/2     Running   0          6h
alertmanager-main-2                   2/2     Running   0          6h
grafana-5b68464b84-vvth6              1/1     Running   0          6h
kube-state-metrics-58dcbb8579-7dt95   4/4     Running   0          6h
node-exporter-zt8qh                   2/2     Running   0          6h
prometheus-k8s-0                      3/3     Running   1          6h
prometheus-k8s-1                      3/3     Running   1          6h
prometheus-operator-587d64f4c-lvzsn   1/1     Running   0          6h
```

#### Foremast:

```text
$ kubectl create -Rf deploy/foremast/

$ kubectl get pods -n foremast

NAME                               READY   STATUS    RESTARTS   AGE
barrelman-85c67bbbb5-nh5sl         1/1     Running   0          1h
elasticsearch-0                    1/1     Running   0          5h
foremast-ai-api-867fc9f756-pt6lz   1/1     Running   0          5h
foremast-engine-748d45cc98-qph54   1/1     Running   0          5h
```

### Install Foremast with existing Prometheus

#### Prometheus settings

Change yaml file `deploy/foremast/barrelman/spring-boot-deployment-metadata.yaml`, put the correct prometheus endpoint

```text
spec:
  metrics:
    dataSourceType: prometheus
    endpoint: http://prometheus-k8s.monitoring.svc.cluster.local:9090/api/v1/
```

#### Foremast:

```text
$ kubectl create -Rf deploy/foremast/

$ kubectl get pods -n foremast

NAME                               READY   STATUS    RESTARTS   AGE
barrelman-85c67bbbb5-nh5sl         1/1     Running   0          1h
elasticsearch-0                    1/1     Running   0          5h
foremast-ai-api-867fc9f756-pt6lz   1/1     Running   0          5h
foremast-engine-748d45cc98-qph54   1/1     Running   0          5h
```

## Run example

#### Run foo v1

```text
$ kubectl create -Rf examples/foo/v1/
```

#### Wait around 1 hour to let the system generate the historical running metrics

#### Run foo v2

```text
$ kubectl replace -f examples/foo/v2/foo_v2.yaml
```

Use following command to check the deployment status

```text
$ kubectl get deploymentmonitor foo -o yaml
```

It will show following information, if the deployment is considered as unhealthy, it will show "Unhealthy" and roll it back the v1.

```text
status:
  anomaly: {}
  expired: false
  jobId: 6fff8856b554115bc3c94ce9eb89fd86808f94458ed559c6c3fe67c91342e4b2
  phase: Running
```

