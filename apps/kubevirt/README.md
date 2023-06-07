# Kubervirt


## Fix terminating namespace

```bash
kubectl get namespace "<NAMESPACE>" -o json   | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/"   | kubectl replace --raw /api/v1/namespaces/<NAMESPACE>/finalize -f -
```

## Upload image to CDI

```bash
## PVC
export VOLUME_NAME=debian12-pvc
export NAMESPACE="debian12"
export IMAGE_PATH=debian-12-generic-amd64-daily.qcow2
export VOLUME_TYPE=pvc
export SIZE=16Gi
export PROXY_ADDRESS=$(kubectl get svc cdi-uploadproxy-loadbalancer -n cdi -o json | jq --raw-output '.spec.clusterIP')
# $(kubectl get svc cdi-uploadproxy -n cdi -o json | jq --raw-output 

virtctl image-upload $VOLUME_TYPE $VOLUME_NAME \
    --size=$SIZE \
    --image-path=$IMAGE_PATH \
    --uploadproxy-url=https://$PROXY_ADDRESS:443 \
    --namespace=$NAMESPACE \
    --insecure
    
## DV
export VOLUME_NAME=debian12-datavolume
export NAMESPACE="debian12"
export IMAGE_PATH=debian-12-generic-amd64-daily.qcow2
export VOLUME_TYPE=dv
export SIZE=16Gi
export PROXY_ADDRESS=$(kubectl get svc cdi-uploadproxy-loadbalancer -n cdi -o json | jq --raw-output '.spec.clusterIP')

virtctl image-upload $VOLUME_TYPE $VOLUME_NAME \
    --size=$SIZE \
    --image-path=$IMAGE_PATH \
    --uploadproxy-url=https://$PROXY_ADDRESS:443 \
    --insecure \
    --namespace=$NAMESPACE \
    --access-mode=ReadWriteOnce 
```
