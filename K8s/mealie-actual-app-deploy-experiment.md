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
