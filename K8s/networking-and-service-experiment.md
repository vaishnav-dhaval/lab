### Pods level
- networking (ip, endpoints slices and all) happens at pods level via CNI
- this is by default assign by cluster to pods so that pods can communicate using `localhost` between containers 
- `k port-forward pods/mealie-66d67cdc89-b74h5 9000`
- after this command you still have to run command as front service, the moment you close terminal the command get stop (same behaviour like ctrl+c) but we can set pod to listen external request

### CNI plugin
stored in 📍 `/etc/cni` directory in any OS
⇒ this is the core network interface for networking (its software)
⇒ ip assignment within cluster for everything

### Services

expose command

`k expose deployment mealie --port 8080 --target-port 90000`

`k edit svc mealie`  ⇒ to edit the service, because still you haven't created service yaml

`k get svc -o yaml > mealie-service.yaml`  ⇒ to create service yaml file

now you can change `spec.type` to `LoadBalancer` and now you can access pod via external ip 
to get external ip run command `k get svc -o wide`

##### types of service
- clusterIp
	- this default option when you run expose command
- NodePort
	- export port on node and then you can access pods via node via NodePort
- LoadBalancer
	- this mainly use in cloud

what is cluster ip and external ip
