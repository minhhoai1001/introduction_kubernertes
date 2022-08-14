# How Kubernetes builds the container filesystem
## deploy a sleep Pod:
    kubectl apply -f sleep/sleep.yaml
## write a file inside the container:
    kubectl exec deploy/sleep -- sh -c 'echo ch05 > /file.txt; ls /*.txt'
## kill all processes in the container—causing a Pod restart:
    kubectl exec -it deploy/sleep -- killall5
## look for the file you wrote—it won’t be there:
    kubectl exec deploy/sleep -- ls /*.txt

# Emptydir
## update the sleep Pod to use an EmptyDir volume:
    kubectl apply -f sleep/sleep-with-emptyDir.yaml
## list the contents of the volume mount:
    kubectl exec deploy/sleep -- ls /data
## create a file in the empty directory:
    kubectl exec deploy/sleep -- sh -c 'echo ch05 > /data/file.txt; ls /data'
## kill the container processes:
    kubectl exec deploy/sleep -- killall5
## read the file in the volume:
    kubectl exec deploy/sleep -- cat /data/file.txt


# Storing data on a node with volumes and mounts
## deploy the Pi application:
    kubectl apply -f pi/v1/
## check the cache in the proxy
    kubectl exec deploy/pi-proxy -- ls -l /data/nginx/cache
## delete the proxy Pod: 
    kubectl delete pod -l app=pi-proxy
## check the cache directory of the replacement Pod:
    kubectl exec deploy/pi-proxy -- ls -l /data/nginx/cache