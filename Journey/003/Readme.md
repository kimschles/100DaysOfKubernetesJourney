# Go Faster with `kubectl` 

## 3 Ways to Go Faster Using `kubectl`

### 1. Alias `k` to `kubectl` 

Typing `kubectl` every time you want to run a Kubernetes command is time consuming and easy to mess up. You can fix this by using an alias which means that anytime you type `k`, your shell knows to use the command `kubectl`. 

If you're unfamiliar with aliases, I recommend you check out this article called [An Introduction to Useful Bash Aliases and Functions](https://www.digitalocean.com/community/tutorials/an-introduction-to-useful-bash-aliases-and-functions).

If you just want to try the alias out in your current shell, run this command: 
    `alias k="kubectl"`

If you want to make that part of every terminal window you open, add `alias k="kubectl"` to your shell configuration file (likely a file called .bashrc, .bash_profile, .zshrc, or something like that), then source that file or open a new terminal window to make sure `k` invokes `kubectl`. 

You can alias all sorts of commands, and Ahmet Alp Balkan has an excellent blog post on his favorite kubectl bash aliases, and you can find it [here](https://ahmet.im/blog/kubectl-aliases/). 


### 2. Enable `kubectl` autocomplete

The second thing that will make you faster using `kubectl` is to enable autocomplete. This means that if you start typing a `kubectl` command or resource name, when you hit tab `kubectl` will complete the name on your command line. You can find instructions on enabling autocomplete in the [kubectl cheatsheet docs](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete). 

### 3. Learn a Few K8s Resources Shortnames 

Finally, you can speed up your `kubectl` commands by learning a few resource shortnames. To find the Kubernetes resources that have shortnames, run the command `k api-resources`. You'll see a table similar to this one: 

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
serviceaccounts                   sa           v1                                     true         ServiceAccount
services                          svc          v1                                     true         Service
```

The second column from the left shows resources that have shortnames, so instead of typing `k get namespaces` you could type `k get ns`, or instead of `k get configmaps` you could type `k get cm`. Memorize a few shortnames, and your K8s life will get a little easier. Good luck! 


