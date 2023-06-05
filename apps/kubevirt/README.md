# Kubervirt


## Fix terminating namespace

```bash
kubectl get namespace "<NAMESPACE>" -o json   | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/"   | kubectl replace --raw /api/v1/namespaces/<NAMESPACE>/finalize -f -
```

## Upload image to CDI

```bash
export VOLUME_NAME=lunar-pvc
export IMAGE_PATH=lunar-server-cloudimg-amd64.img
export VOLUME_TYPE=pvc
export SIZE=16G
export PROXY_ADDRESS="136.243.32.27"

virtctl image-upload $VOLUME_TYPE $VOLUME_NAME --image-path=$IMAGE_PATH \
    --access-mode=ReadWriteOnce \
    --size=$SIZE \
    --uploadproxy-url=https://$PROXY_ADDRESS \
    --wait-secs=60
```
