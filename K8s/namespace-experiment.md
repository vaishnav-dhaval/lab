# Namesapce in k8s

> *Namespace* ⇒ isolation for resources in cluster (its like scope in JS/TS)

- namespace use for RBAC in team

How to create default namespace file
`k create namespace my-ns -o yaml --dry-run > my-ns.yaml`

How to create namespace
- `k create namespace <namespace>` or `k create ns <namespace>`

How to delete namespace
- `k delete ns my-ns`

> [!danger] Deleting NS
> > when you delete NS its also delete all its resources

> so its helpful testing out or learning somethings and *easy to clean up*


##### How to create pods and other resources in created NS
`k run daksh --image=nginx -ns my-ns`

##### How to get pods and other resources info
`k get pods -n <ns name>`

##### How to set newly create NS as your default NS
`k config set-context --current --namespace=my-ns`
now you can get and see your pods of this ns by default


## Mealie project

### Create Namespace

Lets create namespace file and set it is as default namespace
- `mkdir mealie && cd mealie`
- `k create namespace mealie-ns --dry-run=client -o yaml > mealie-ns.yaml`
- `k apply -f mealie-ns.yaml`
- `k config set-context --current --namespace=mealie-ns`

### create deployment 

lets create deployment file and create pods

`k create deployment mealie --image=nginx -o yaml --dry-run=client > mealie-deployment.yaml`

- update image to `ghcr.io/mealie-recipes/mealie:v1.2.`
- add ports and containerPort section

```yaml
spec:
  containers:
    - image: ghcr.io/mealie-recipes/mealie:v1.2.0
      name: mealie
	  ports:
	  - containerPort: 9000
```

now check :
`k get pods`
or
`k get pods -o wide`  ⇒ it will show IP address 

> [!tip] Understanding
> Created Deployment ⇒ creates ReplicaSet ⇒ creates pods

### PORT binding / forward
just remember now this command
- `k port-forward pods/<pod_name from "k get pods"> <port>`
- e.g. `k port-forward pods/mealie-66d67cdc89-b74h5 9000`
- its like non detached mode of docker or node app is running

NOW CHECK `http://localhost:9000`
