Kubernetes Logging into Elasticsearch

This repository contains a docker images for a fluentd logshipper, kubernetes configs to deploy a basic elasticsearch cluster with kibana frontend, and documentation. These files should show how to setup a fluentd logshipper as kubernetes daemonset and pipe all container logs into an elasticsearch cluster. The logs are enriched with Metadata like pod_name, pod_id, docker_id.


To setup the Elasticsearch Cluster and a kibana frontend run:

kubectl create -f elastic-search-rc.yaml

kubectl create -f elasticsearch-svc.yaml

kubectl create -f kibana-rc.yaml

kubectl create -f kibana-svc.yaml



To start the fluentd daemonset run:

kubectl create -f fleuntd-daemonset.yaml

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
