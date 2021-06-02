# it's HTTP! 

## Introduction 
When I started thinking about what to write about in this entry, I knew I wanted to explore about how the Kubernetes CLI tool, `kubectl`, interacts with the kube-apiserver. This was a 🪜 K8s concept I was familiar with, but wanted to learn more deeply. 

What I learned through my research is that the kube-apiserver exposes the Kuberntes API, which is a REST interface, and `kubectl` is an HTTP client like `cURL` or a browser like Firefox. I'd never put it together that `kubectl` sends HTTP requests to the Kubernetes API. 

I learned _so much_ from GoLinuxCloud's article called [Detailed overview on Kubernetes API Server](https://www.golinuxcloud.com/kube-apiserver/). If you want a detailed description of the kube-apiserver and Kubernetes API, I highly recommend it.  

## Prerequisites
In order to follow along with this article, you will need access to a Kubernetes cluster through `kubectl`. You can explore a Kubernetes cluster from your browser in the [KataCoda Kubernetes Playground].(https://www.katacoda.com/courses/kubernetes/playground). If you use the KataCoda playground, run your commands in the terminal for the control plane. 


## Seeing the API Endpoints 
Every object controlled by Kubernetes has an API endpoint that you or an automated system can interact with through HTTP Methods. The most common HTTP methods are `GET`, `POST`, `PUT` and `DELETE`. 

To see the API paths available in your cluster, run the command. `kubectl get --raw /`. If you've got [jq](https://stedolan.github.io/jq/) installed, make it pretty with `kubectl get --raw / | jq .` Either way, you'll see a longer version of this json object:  

```json 
{
  "paths": [
    "/.well-known/openid-configuration",
    "/api",
    "/api/v1",
    "/apis",
    "/apis/",
    "/apis/admissionregistration.k8s.io",
    "/apis/admissionregistration.k8s.io/v1",
    "/apis/admissionregistration.k8s.io/v1beta1",
    "/apis/apiextensions.k8s.io",
    "/apis/apiextensions.k8s.io/v1",
    "/apis/apiextensions.k8s.io/v1beta1",
    "/apis/apiregistration.k8s.io",
    "/apis/apiregistration.k8s.io/v1",
    "/apis/apiregistration.k8s.io/v1beta1",
    "/apis/apps",
    "/apis/apps/v1",
    "/apis/authentication.k8s.io",
    "/apis/authentication.k8s.io/v1",
    "/apis/authentication.k8s.io/v1beta1",
    "/apis/authorization.k8s.io",
    "/apis/authorization.k8s.io/v1",
    "/apis/authorization.k8s.io/v1beta1",
    "/apis/autoscaling",
    "/apis/autoscaling/v1",
    "/apis/autoscaling/v2beta1",
    "/apis/autoscaling/v2beta2",
    "/apis/batch",
    "/apis/batch/v1",
    "/apis/batch/v1beta1",
    "/apis/certificates.k8s.io",
    "/apis/certificates.k8s.io/v1",
    "/apis/certificates.k8s.io/v1beta1",
    "/apis/coordination.k8s.io",
    "/apis/coordination.k8s.io/v1",
    "/apis/coordination.k8s.io/v1beta1",
    "/apis/discovery.k8s.io",
    "/apis/discovery.k8s.io/v1beta1"
  ]
}
```

The `--raw` flag allows you to look at the response from the Kubernetes API as JSON instead of the formatted tables that we're used to seeing with `kubectl`. 

If you'd like to see the the supported API versions in your cluster, run the command `kubectl api-versions`. Each line shows you the api group, the a forward slash`/`, then the API version. You'll see a longer version of this list: 

``` 
admissionregistration.k8s.io/v1
admissionregistration.k8s.io/v1beta1
apiextensions.k8s.io/v1
apiextensions.k8s.io/v1beta1
apiregistration.k8s.io/v1
apiregistration.k8s.io/v1beta1
apps/v1
authentication.k8s.io/v1
```

Finally, if you'd prefer to see the Kubernetes resources and their API version organized in columns, run the command `kubectl api-resources`. You will see a longer version of this table: 

```
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
pods                              po           v1                                     true         Pod
podtemplates                                   v1                                     true         PodTemplate
replicationcontrollers            rc           v1                                     true         ReplicationController
resourcequotas                    quota        v1                                     true         ResourceQuota
secrets                                        v1                                     true         Secret
``` 

## Translating `kubectl` command to HTTP Requests
When you run a `kubectl` command, `kubectl` sends an HTTP request as a string of JSON. 

If you need to review (or learn for the first time!) how HTTP requests are formatted, I recommend [Anatomy of an HTTP Request](https://betterprogramming.pub/the-anatomy-of-an-http-request-728a469ecba9) by Patrick Devine. 

Here's a table showing a few `kubectl` commands and how they map to HTTP methods and the Kubernetes API paths.

| `kubectl` command  | Description | HTTP Method| API path   |
|--------------------|------------ | -----------|-----------|
| `kubectl -n kube-system get pods` | See all pods in the kube-system namepsace | `GET` | `/api/v1/namespaces/kube-system/pods`|
| `kubectl -n kube-system create -f deployment.yaml` | Create a deployment in the kube-system namespace | `POST`| `/apis/apps/v1/namespaces/kube-system/deployments`|
| `kubectl -n kube-system apply -f deployment.yaml`  | Update the deployment in the kube-system namespace | `PUT`| `/apis/apps/v1/namespaces/kube-system/deployments` |
| `kubectl -n kube-system delete -f deployment.yaml`  | Delete the deployment in the kube-system namespace  | `DELETE` | `/apis/apps/v1/namespaces/kube-system/deployments` |
| `kubectl -n kube-system get deployments`| See all deployments in the kube-system namepsace | `GET` | `/apis/apps/v1/namespaces/kube-system/deployments`|


## HTTP Clients 
The most common HTTP clients we use to communicate with the Kubernetes api are `kubectl` and `kubeadm`. You can also setup an HTTP proxy so you can make requests to the Kubernetes API using `cURL`, `wget` or your browser. You run the command `kubectl proxy --port=4000` (or whatever port your want) and start exploring! You can find more detailed instructions [K8s Docs: HTTP Proxy Access](https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/)


## An Unfinished Thought
- The kube-apiserver is the only kubernetes component that can communicate with etcd. All of the data the Kubernetes API shows and updates comes from etcd. 

## Conclusion 
Kubernetes can be an intimidating system to start learning, especially if you're a web app developer who is new to the devops side of the tech industry. Exploring the Kubernetes API through the different endpoints might be a good place to start exploring. Good luck!  

### Recommended Resources 
- [Detailed overview on Kubernetes API Server](https://www.golinuxcloud.com/kube-apiserver/)
- [K8s Docs: kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
- [K8s Docs: HTTP Proxy Access](https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/)
- [Kubernetes v1.21 API Docs](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.21/)