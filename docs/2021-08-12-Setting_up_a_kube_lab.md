---
layout: post
title: Setting up a Kubernetes Lab with Monitoring and State Alerts
subtitle: Self Managed Control plane with Prometheus and State Alerting
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /assets/img/post5/npm-installer.png
share-img: /assets/img/post5/npm-installer.png
tags: [supplychain, sdlc]
---

#### Getting Started

This post assumes you've already set up your Kubernetes cluster using  `kubeadm`. It's pretty straightforward to install and run `kubeadm` to set up a single node or multi-node cluster.
My lab consists of a 2 node cluster, with a dedicated control plane running on AWS ec-2 instances.


#### Why Prometheus?
It's an open source monitoring framework which helps you view and set alerting on your cluster.
You can also use it to visualize and export data or metrics about your cluster that might be useful for wrangling the extraordinary amount of datatypes Kubernetes generates. It's pretty easy even in a small cluster to lose track of some of the secrets or such that are being handled by the system.



#### Dependencies  - Kube State Metrics

// kube-state-metrics is a simple service that listens to the Kubernetes API server and generates metrics about the state of the objects.  

Something that is not super well covered in the standing documentation about Prometheus is it's dependency on `kube-state-metrics.`
Here's the [Git repo for that.](https://github.com/kubernetes/kube-state-metrics )
I would suggest installing the [Bitnami version](https://bitnami.com/stack/kube-state-metrics/helm) of kube-state-metrics via helm, since this makes it really easy to reverse anything that's gone wrong.

I'm going to task helm to install kube-state-metrics in the kube-system namespace (so that it stays on my control plane node). I'm going to name it 'kube-state-metrics' as thats what Prometheus will look for by default later on.

 `helm repo add bitnami https://charts.bitnami.com/bitnami`
 `helm install kube-state-metrics -n kube-system bitnami/kube-state-metrics`

That's it! Now when you go ahead and install Prometheus, it should just pick up the metrics API and start working off the bat.




####   Installing Prometheus

I liked the bibinwilson version of this project, which came with some great default configs and a dedicated namespace.

`git clone https://github.com/bibinwilson/kubernetes-prometheus`

##### Create a NameSpace and ClusterRole

If you don't do this, Prometheus will install on the default namespace, and you don't want that.

`kubectl create namespace monitoring`

Prometheus will need a `clusterRole.yaml` with the permissions `get` `list` `watch` for nodes, endpoints, pods, services and ingresses bound to the `monitoring` namespace.

`touch clusterRole.yaml`
`vi clusterRole.yaml`

`apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: prometheus
rules:
- apiGroups: [""]
  resources:
  - nodes
  - nodes/proxy
  - services
  - endpoints
  - pods
  verbs: ["get", "list", "watch"]
- apiGroups:
  - extensions
  resources:
  - ingresses
  verbs: ["get", "list", "watch"]
- nonResourceURLs: ["/metrics"]
  verbs: ["get"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: prometheus
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: prometheus
subjects:
- kind: ServiceAccount
  name: default
  namespace: monitoring`

`kubectl create -f clusterRole.yaml`

This repo https://github.com/bibinwilson/kubernetes-prometheus contains some great basic configs and rules to get started on alerting.
execute `kubectl create -f config-map.yaml` & `kubectl create -f prometheus.yaml` to get them set up.


Next it's time to build the deployment. This is how Prometheus will start up and get instructions from the scheduler to stay online.
We'll use the [official image](https://hub.docker.com/r/prom/prometheus/) to do so.

`apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus-deployment
  namespace: monitoring
  labels:
    app: prometheus-server
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus-server
  template:
    metadata:
      labels:
        app: prometheus-server
    spec:
      containers:
        - name: prometheus
          image: prom/prometheus
          args:
            - "--storage.tsdb.retention.time=12h"
            - "--config.file=/etc/prometheus/prometheus.yml"
            - "--storage.tsdb.path=/prometheus/"
          ports:
            - containerPort: 9090
          resources:
            requests:
              cpu: 500m
              memory: 500M
            limits:
              cpu: 1
              memory: 1Gi
          volumeMounts:
            - name: prometheus-config-volume
              mountPath: /etc/prometheus/
            - name: prometheus-storage-volume
              mountPath: /prometheus/
      volumes:
        - name: prometheus-config-volume
          configMap:
            defaultMode: 420
            name: prometheus-server-conf

        - name: prometheus-storage-volume
          emptyDir: {}`

This doesn't contain any persistent storage just yet.

You can now drop the deployment on the cluster:

`kubectl create  -f prometheus-deployment.yaml`

It'll take about 40 seconds to spool up

`kubectl get deployments --namespace=monitoring`

##### Viewing your new dashboard


Pull the pod name so you can apply it to the next command:
`kubectl get pods --namespace=monitoring`

![prometheus](/assets\img\post6\get pods monitoring.png){: .mx-auto.d-block :}


Set up port forwarding to http://localhost:8080
`kubectl port-forward prometheus-deployment-name 8080:9090 -n monitoring`

You can now view your new dashboard.

![prometheus](/assets\img\post6\prometheus up and running.png){: .mx-auto.d-block :}


#### Final Notes

If something has gone wrong with your installation process, the best bet is to remove the helm chart, `kubectl delete` commands won't stop the `ReplicateSet` from rescheduling a new pod.
e.g:
`helm uninstall kubemetrics -n kube-system`
