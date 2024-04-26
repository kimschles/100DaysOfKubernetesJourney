# it's HTTP! 

## Introduction 
`kubectl` is the command-line interface you use if you interact with a Kuberetes cluster. The kube-apiserver exposes the Kuberntes API, which is a REST interface, and `kubectl` is an HTTP client like `cURL` or a browser like Firefox. `kubectl` sends HTTP requests to the Kubernetes API, and knowing the underlying mechanics of how `kubectl` translates its commands into HTTP requests will help you better understand how Kubernetes is organized. In this blog post, I'll walk you through the details.


## Prerequisites
In order to follow along with this article, you will need access to a Kubernetes cluster through `kubectl`. If you don't have a cluster easily available, you can find one from yourbrowser at the [Killer Coda CKAD Scenario](https://killercoda.com/killer-shell-ckad). 

You will need to sign in to Killer Coda (you can use your Github, Gitlab, Google Account, or email), select the CKAD Certification option, and then choose Playground. Click the START button, and you'll see a command prompt that says `controlplane $`. Run your commands in the terminal for the control plane. 


## Seeing the API Endpoints 
Every object controlled by Kubernetes has an API endpoint that you, or an automated system, can interact with through HTTP Methods. The most common HTTP methods are `GET`, `POST`, `PUT` and `DELETE`. 

To see the API paths available in your cluster, run the command. `kubectl get --raw /`. If you've got [jq](https://stedolan.github.io/jq/) installed, make it pretty with `kubectl get --raw / | jq .` Either way, you'll see a longer version of this JSON object:  

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
    "/apis/apiextensions.k8s.io",
    "/apis/apiextensions.k8s.io/v1",
    "/apis/apiregistration.k8s.io",
    "/apis/apiregistration.k8s.io/v1",
    "/apis/apps",
    "/apis/apps/v1",
    "/apis/authentication.k8s.io",
    "/apis/authentication.k8s.io/v1",
    "/apis/authorization.k8s.io",
    "/apis/authorization.k8s.io/v1",
    "/apis/autoscaling",
    "/apis/autoscaling/v1",
    "/apis/autoscaling/v2",
    "/apis/batch",
    "/apis/batch/v1",
    "/apis/certificates.k8s.io",
    "/apis/certificates.k8s.io/v1",
    "/apis/coordination.k8s.io",
    "/apis/coordination.k8s.io/v1",
    "/apis/crd.projectcalico.org",
    "/apis/crd.projectcalico.org/v1",
    "/apis/discovery.k8s.io",
    "/apis/discovery.k8s.io/v1",
    "/apis/events.k8s.io"
    ...
    [
{
```

The `--raw` flag allows you to look at the response from the Kubernetes API as JSON instead of the formatted tables that you're used to seeing with `kubectl`. 

If you'd like to see the the supported API versions in your cluster, run the command `kubectl api-versions`. Each line shows you the api group, then a forward slash `/`, then the API version. For example: `apps/v1`. With that command, you'll see a version of this list: 

``` 
admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1
apiregistration.k8s.io/v1
apps/v1
authentication.k8s.io/v1
authorization.k8s.io/v1
autoscaling/v1
autoscaling/v2
batch/v1
certificates.k8s.io/v1
coordination.k8s.io/v1
crd.projectcalico.org/v1
discovery.k8s.io/v1
events.k8s.io/v1
flowcontrol.apiserver.k8s.io/v1
flowcontrol.apiserver.k8s.io/v1beta3
networking.k8s.io/v1
node.k8s.io/v1
policy/v1
rbac.authorization.k8s.io/v1
scheduling.k8s.io/v1
storage.k8s.io/v1
v1
```

Finally, if you'd prefer to see the Kubernetes resources and their API version organized in columns, run the command `kubectl api-resources`. You will see a longer version of this table: 

```
NAME                              SHORTNAMES   APIVERSION                        NAMESPACED   KIND
bindings                                       v1                                true         Binding
componentstatuses                 cs           v1                                false        ComponentStatus
configmaps                        cm           v1                                true         ConfigMap
endpoints                         ep           v1                                true         Endpoints
events                            ev           v1                                true         Event
limitranges                       limits       v1                                true         LimitRange
namespaces                        ns           v1                                false        Namespace
nodes                             no           v1                                false        Node
persistentvolumeclaims            pvc          v1                                true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                false        PersistentVolume
pods                              po           v1                                true         Pod
``` 

## Translating `kubectl` commands to HTTP Requests
When you run a `kubectl` command, `kubectl` sends an HTTP request as a string of JSON to the Kubernetes API.  

If you need to review (or learn for the first time!) how HTTP requests are formatted, I recommend you check out [HTTP Requests
](https://sematext.com/glossary/http-requests/) by Sematext. 

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


## Conclusion 
Kubernetes can be an intimidating system to start learning, especially if you're a web app developer who is new to the DevOps side of the tech industry. Exploring the Kubernetes API through the different endpoints might be a good place to start. Good luck!  

### Recommended Resources 
- [HTTP Requests](https://sematext.com/glossary/http-requests/) 
- [Detailed overview on Kubernetes API Server](https://www.golinuxcloud.com/kube-apiserver/)
- [K8s Docs: kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
- [K8s Docs: HTTP Proxy Access](https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/)
- [Kubernetes v1.21 API Docs](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.21/)
