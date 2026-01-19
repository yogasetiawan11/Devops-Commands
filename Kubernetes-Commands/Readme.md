
```sh
kubectl rollout restart [deployment name]
```
> Used to gracefully restart a deployment after modifying associated resources like ConfigMaps or Secrets, ensuring a new pod is healthy before the old one is terminated 


```sh
kubectl rollout undo [deployment name] 
```
> Allows rolling back a faulty deployment to a previous working version


```sh
kubectl rollout history [deployment name] 
```
> Displays the full rollout history with revision numbers, useful for tracking changes 

```sh
kubectl exec [pod name] -- df -h 
```
> Checks mounted volumes and their storage usage within a pod 

```sh
kubectl get secret [secret name] -o jsonpath='{.data.[key]}' | base64 --decode
```
> Decodes the value of a secret from its encoded form 

```sh
kubectl exec [pod name] -- wget -qO- http://localhost:3000
# http is an address of your application or pods
```

> Manually executes a readiness probe to confirm application health and readiness to accept traffic 

```sh
kubectl describe pod [pod name]
```
> Lists all volumes mounted on a pod and their respective mount paths

```sh
kubectl get endpoints [service name]
```
> Displays service endpoints and the actual IP addresses of the pods they connect to

```sh
kubectl get events --all-namespaces --sort-by=.lastTimestamp 
```
> Shows all events in particular namespace, useful for debugging 

```sh
kubectl patch pvc [pvc name] -p '{"spec":{"resources":{"requests":{"storage":"[new size]"}}}}'
```
> Resizes a Persistent Volume Claim (PVC) to increase its storage 

```sh
kubectl exec [pod name] -- kill 1  
```
> Simulates a crash loop back-off error by killing the main process within a pod 

```sh
kubectl autoscale deployment [deployment name] --cpu-percent=[percentage] --min=[min pods] --max=[max pods] -n webapp
```
> Enables Horizontal Pod Autoscaling (HPA) based on CPU usage

```sh
watch kubectl top pod
```
> Provides live streaming of pod CPU and memory usage, But you have Install Kubernetes metrics first 

```sh

```