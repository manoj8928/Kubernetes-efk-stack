#references: https://github.com/kubernetes/examples/tree/master/staging/elasticsearch/production_cluster
# https://github.com/iAmPlus/eliza-devops/tree/master/infrastructure/staging/elk

Kubernetes Logging into Elasticsearch

This repository contains a docker images for a fluentd logshipper, kubernetes configs to deploy a basic elasticsearch cluster with kibana frontend, and documentation. These files should show how to setup a fluentd logshipper as kubernetes daemonset and pipe all container logs into an elasticsearch cluster. The logs are enriched with Metadata like pod_name, pod_id, docker_id.


To setup the Elasticsearch Cluster and a kibana frontend run:

kubectl create -f elastic-search-rc.yaml

kubectl create -f elasticsearch-svc.yaml

kubectl create -f kibana-rc.yaml

kubectl create -f kibana-svc.yaml



To start the fluentd daemonset run:

kubectl create -f fluentd-daemonset.yaml

After this setup you can check the pods you have deployed. The command is:

kubectl --namespace=kube-system get pods
The output should look like this:

NAME                             READY     STATUS    RESTARTS   AGE
elasticsearch-logging-v1-15qsf   1/1       Running   0          2m
elasticsearch-logging-v1-278v2   1/1       Running   0          2m
fluentd-logging-mcegp            1/1       Running   0          10s
kibana-logging-v1-mmfv4          1/1       Running   0          2m
kube-addon-manager-minikubevm    1/1       Running   0          4m
kubernetes-dashboard-ms0el       1/1       Running   0          4m


ToDo
- elasticsearch
  remove persistent storage - we rebuild, if needed
    looks like already taken care of because of `emptyDir` volume type
  pick later version (5.2 is out)
- fluentd
  support authentication
    place auth info in secret
- nginx
  what the hell is this for?
  # GEORGE MALARY: I say let's delete the nginx yaml.  works without deploying and yaml makes no sense.  probably for debugging of was trying # to add some new functionality.

- Implement log retention and rotation: look int curator add on
