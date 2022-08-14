## Start kubernetes cluster
    minikube start --node 2 --driver=docker
## Run docker
    kubectl run hello-kubernetes --image=hoaitran1001/hello-kubernetes --restart=Never
## List all the Pods in the cluster
    kubectl get pods
## Show detailed information about the Pod
    kubectl describe pod hello-kubernetes 
## Port forward To the Pod on port 80
    kubectl port-forward pod/hello-kubernetes 8080:80
## Delete pod
    kubectl delete pod hello-kubernetes
## Create pod with config
    kubectl apply -f Getting_kubernetes/pod.yaml
