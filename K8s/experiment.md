
## pods

- k config current-context
- k config get-contexts
- use-context
- k run nginx-daksh --image=nginx
- k describe pod nginx-daksh

kubectl is like curl ⇒ k8s expose API through control pane

control pane have *api server*, to that server kubectl talk or send operation to execute


- `k run nginx --dry-run=client --image=nginx -o yaml` => print yaml file in terminal
- `k run nginx --dry-run=client --image=nginx -o yaml > nginx.yaml`  ⇒ create yaml file named with ==nginx.yaml== 
- K create command ⇒ create new pods only
	- `k create -f nginx.yaml` 
- k apply  ⇒ its apply config new change to pods, as well as create if no pods
	- `k apply -f nginx.yaml`


VIM with paste mode ⇒ because paste yaml file its indentation is not stay correct
open file using vim then run this command it will open file in ==insert (paste)== mode
`:set paste`


- `k exec -it <podname> -- /bin/bash.`  ⇒ exec to pods with bash tty 
- `k get pods -o wide`  ⇒ this is give you list of pods with its IP Addr

## Deployment

- `k create deploy daksh-deploy --image=httpd --replicas=3`
	- daksh-deploy is ==deployment name==
- `k edit deployments.apps daksh-deploy`
	- the change you do, its automatically apply on cluster like increase replicas or decrease replica its auto apply as you change file
-  `k apply -f deploy.yaml` apply the file ==deploy.yaml== into cluster
- `k get deployments.app` or `k get deployments.app <deployment-name>` 
- replica set never be manage by human its conventions, but you can change
- `k create deploy daksh-deploy --image=httpd --replicas=3 --dry-run=client -o yaml > deploy.yaml`  ⇒ use of  ==--dry-run== and creating deploy yaml file using redirect of stdout
  
  ```yaml
	strategy:
	  type: RollingUpdate
	  rollingUpdate:
		maxUnavailable: 1
	    maxSurge: 1
  ```
  
- above changes in deployment file in spec section
- maxUnavail ⇒ max pods unavail limit when changes apply to the cluster
- maxUnavail ⇒ max pods create limit when changes apply to the cluster
