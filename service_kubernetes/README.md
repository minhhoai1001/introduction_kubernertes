# Deploy two Pods—you can ping one Pod from the other
## create two deployments, which each run one Pod:
    kubectl apply -f sleep/sleep1.yaml -f sleep/sleep2.yaml
## wait for the Pod to be ready:
    kubectl wait --for=condition=Ready pod -l app=sleep-2
## check the IP address of the second Pod:
    kubectl get pod -l app=sleep-2 --output jsonpath='{.items[0].status.podIP}'
## use that address to ping the second Pod from the first:
    kubectl exec deploy/sleep-1 -- ping -c 2 $(kubectl get pod -l app=sleep-2 -- output jsonpath='{.items[0].status.podIP}')

## check the current Pod’s IP address:
    kubectl get pod -l app=sleep-2 --output jsonpath='{.items[0].status.podIP}'
## delete the Pod so the deployment replaces it:
    kubectl delete pods -l app=sleep-2
## check the IP address of the replacement Pod:
    kubectl get pod -l app=sleep-2 --output jsonpath='{.items[0].status.podIP}'


# You deploy a Service using a YAML file and the usual Kubectl apply command
## deploy the Service defined in listing 3.1:
    kubectl apply -f sleep/sleep2-service.yaml
## show the basic details of the Service:
    kubectl get svc sleep-2
## run a ping command to check connectivity—this will fail:
    kubectl exec deploy/sleep-1 -- ping -c 1 sleep-2


# No Services for this app yet, and it won’t work correctly because the website can’t find the API.
## run the website and API as separate deployments: 
    kubectl apply -f numbers/api.yaml -f numbers/web.yaml
## wait for the Pod to be ready:
    kubectl wait --for=condition=Ready pod -l app=numbers-web
## forward a port to the web app:
    kubectl port-forward deploy/numbers-web 8080:80

# Create the Service for the API so the domain lookup works and traffic gets sent from the web Pod to the API Pod.
## deploy the Service from listing 3.2:
    kubectl apply -f numbers/api-service.yaml
## check the Service details:
    kubectl get svc numbers-api
## forward a port to the web app:
    kubectl port-forward deploy/numbers-web 8080:80

# The API Pod is managed by a Deployment controller, so you can delete the Pod and a replacement will be created
## check the name and IP address of the API Pod:
    kubectl get pod -l app=numbers-api -o custom-columns=NAME:metadata.name,POD_IP:status.podIP
## delete that Pod:
    kubectl delete pod -l app=numbers-api
## check the replacement Pod:
    kubectl get pod -l app=numbers-api -o custom-columns=NAME:metadata.name,POD_IP:status.podIP 
## forward a port to the web app:
    kubectl port-forward deploy/numbers-web 8080:80
