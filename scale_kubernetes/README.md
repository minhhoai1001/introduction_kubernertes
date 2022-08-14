# Deploy the ReplicaSet
## deploy the ReplicaSet and service:
    kubectl apply -f whoami/
## check the resource:
    kubectl get replicaset whoami-web
## delete all the Pods:
    kubectl delete pods -l app=whoami-web
## show the detail about the ReplicaSet:
    kubectl describe rs whoami-web
## deploy the update:
    kubectl apply -f whoami/update/whoami-replicas-3.yaml
## check Pods:
    kubectl get pods -l app=whoami-web
## delete all the Pods:
    kubectl delete pods -l app=whoami-web
## check again:
    kubectl get pods -l app=whoami-web

# Updates to see how the ReplicaSets are managed
## deploy the Pi app:
    kubectl apply -f pi/web/
## check the ReplicaSet:
    kubectl get rs -l app=pi-web
## scale up to more replicas:
    kubectl apply -f pi/web/update/web-replicas-3.yaml

# Scale up the Pi application using Kubectl directly
## we need to scale the Pi app fast:
    kubectl scale --replicas=4 deploy/pi-web
## check which ReplicaSet makes the change:
    kubectl get rs -l app=pi-web
## now we can revert back to the original logging level:
    kubectl apply -f pi/web/update/web-replicas-3.yaml
## but that will undo the scale we set manually:
    kubectl get rs -l app=pi-web
## check the Pods:
    kubectl get pods -l app=pi-web