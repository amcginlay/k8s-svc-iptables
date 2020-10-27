# k8s-svc-iptables

- This repo contains text file results of calling `sudo iptables -n -t nat -L` whilst logged into any k8s node.
- `kube-proxy` is designed to ensure the ip table entries of all nodes are in sync at all time.

## The progression of this exercise is as follows:
- `a-before-deployment.txt` represents the starting position of iptables on any k8s node.
#### >> `kubectl create deployment echoserver --image k8s.gcr.io/echoserver:1.10`.
- result: `b-after-deployment.txt` - To demonstrate that creating pods will not cause any change in the iptables.
#### >> `kubectl expose deployment echoserver --port=80 --type=ClusterIP`.
- result: `c-expose-clusterip.txt` - To demonstrate that creating a service causes a new target for the **ClusterIP** to be inserted into the KUBE_SERVICES Chain. The target is itself the identifier of a new SVC Chain. The new SVC Chain also has a target which is the identifier of a new SEP (Pod) Chain. The new SEP Chain has a target for the natural **Pod IP**.
#### >> `kubectl scale --replicas=2 deployment/echoserver`.
- result: `d-scale-up.txt` - To demonstrate that increasing the number of pods causes additional SEP Chains to be created. The SVC Chain created in the previous step is now extended to provide a second target. 50% of the traffic will be targetted to each SEP Chain.
also has a target which is the identifier of a new SEP (Pod) Chain. 
