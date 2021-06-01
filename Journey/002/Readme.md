# it's HTTP! 

## Introduction 
When I started thinking about what to write about in this entry, I knew I wanted to explore about how the Kubernetes CLI tool, `kubectl`, interacts with the kube-apiserver. 

<picture>

What I learned through my research is that the kube-apiserver exposes the Kuberntes API, which is a REST interface, and `kubectl` is an HTTP client like `cURL` or a browser like Firefox. I'd never put it together that `kubectl` sends HTTP requests to the Kubernetes API. 

## Seeing the API Endpoints 
Every object controlled by Kubernetes has an API endpoint that you or an automated system can interact with through  HTTP Methods. The most common HTTP methods are `GET`, `POST`, `PUT` and `DELETE`. 

To see the API paths available in your cluster, run the command. `kubectl get --raw /`. You'll see a longer version of this json object:  

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

If you'd prefer to see these resources and their API version organized in columns, run the command `kubectl api-resources`. You will see a longer version of this table: 

``` md
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

## Translating `kubectl` to an HTTP Request

| `kubectl` command  | HTTP Method  | API path   |
|--------------------|--------------|------------|
| `kubectl get pods -A`   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |








Kubernetes Resource Objects 






! 

The kube-apiserver exposes a REST API that can exchange information with `kubectl`, `kubeadm`, or with raw http requests. 

## HTTP Clients 





The http requests are sent as json  

How does authn and authz work? 


https://kubernetes.io/docs/concepts/security/controlling-access/

RESTful API calls 

The kube-apiserver is the only kubernetes component that can communicate with etcd. 




Is that the http header? 

`GET  /api/vi/namespaces/test/pods` 
`POST /api/v1/namespaces/test/pods` 


### Recommended Resources 
- [K8s Docs: kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
- [K8s Docs: HTTP Proxy Access](https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/)
- [Detailed overview on Kubernetes API Server](https://www.golinuxcloud.com/kube-apiserver/)
- [Kubernetes v1.21 API Docs](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.21/)
- [Anatomy of an HTTP Request
](https://betterprogramming.pub/the-anatomy-of-an-http-request-728a469ecba9)




