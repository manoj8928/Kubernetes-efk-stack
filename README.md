# Kubernetes Logging into Elasticsearch via fluentd

This repository contains a docker images for a fluentd logshipper, kubernetes configs to deploy a basic elasticsearch cluster with kibana frontend, and documentation. These files should show how to setup a fluentd logshipper as kubernetes daemonset and pipe all container logs into an elasticsearch cluster. The logs are enriched with Metadata like pod_name, pod_id, docker_id.

### Setup
To setup the Elasticsearch Cluster and a kibana frontend run:

```sh
$ kubectl create -f elastic-search-rc.yaml
$ kubectl create -f elasticsearch-svc.yaml
$ kubectl create -f kibana-rc.yaml
$ kubectl create -f kibana-svc.yaml
```

To start the fluentd daemonset run:

```sh
$ kubectl create -f fleuntd-daemonset.yaml
```

### Validation
After this setup you can check the pods you have deployed. The command is:

```sh
$ kubectl --namespace=kube-system get pods
```
Expected output:
```
NAME                             READY     STATUS    RESTARTS   AGE
elasticsearch-logging-v1-15qsf   1/1       Running   0          2m
elasticsearch-logging-v1-278v2   1/1       Running   0          2m
fluentd-logging-mcegp            1/1       Running   0          10s
kibana-logging-v1-mmfv4          1/1       Running   0          2m
kube-addon-manager-minikubevm    1/1       Running   0          4m
kubernetes-dashboard-ms0el       1/1       Running   0          4m
```

#### ToDo

- elasticsearch
  1. Persistent Storage - remove
     * follow, "discard and rebuild" strategy
        _looks like already taken care of because of `emptyDir` volume type_ 
  2. Version - 5.2 is out
     * this version
       * con: it's old
       * pro: has small footprint
  3. Curator
     * add a curator component to clear out old logs
     * configMap vars to define a retention policy
  4. Production-ready
     * consider [es-prod] - elasticsearch production
- fluentd
  1. authentication
     * place auth info in secret
  2. Configuration
     * add configMap
       1. exec into pod
       2. copy files out to config map
       3. mount as volume in `/etc`

    [es-prod]: <https://github.com/kubernetes/examples/tree/master/staging/elasticsearch/production_cluster>

