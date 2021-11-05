# Kubernetes Basics

`minikube start` to get things going
this starts a virtual machine and creates a Kubernetes cluster in the VM

`minikube dashboard` and switch back to original tab
deployments and services can be created from the dashboard 

`kubectl` is the main cli that will be used to interact with the cluster

`kubectl cluster-info` provides a bit of information on the cluster at the upper level
`kubectl get nodes|deployments|services` provides a list of the active nodes/deployments/services

A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster.

Pods are the atomic unit on the Kubernetes platform. When we create a Deployment on Kubernetes, that Deployment creates Pods with containers inside them (as opposed to creating containers directly). Each Pod is tied to the Node where it is scheduled, and remains there until termination (according to restart policy) or deletion. 

The Control Plane is used to manage the cluster. It is the layer through which actions on nodes are administrated.

Deployments, or Kubernetes Deployment configurations, instructs Kubernetes how to create and update instances of your application. These instances are monitored constantly by a Deployment Controller so that if one should go down or be deleted, it will be replaced (a self-healing mechanism).

`kubectl` is using an API endpoint to communicate with whatever application we are running.

`kubectl proxy` will create a connection between our terminal window (the host) and the cluster. The proxy will not show any output (it is usually run in a separate terminal tab) but will remain active until killed with CTL + C.

Once the proxy is active, we can call `curl http://localhost:8001/version` with no issue.

`curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/`

The below environment variable will help us to call other different API endpoints that are exposed thanks to the `proxy` being active.

```zsh
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
```