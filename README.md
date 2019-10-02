# Helm Chart: docker-kubernetes-pv-local-storage
This is a Helm chart that lets you create a Kubernetes storage class for creating local persistent volumes for a local Kubernetes cluster started with Docker Desktop.

## Install
1. Make sure hostpath is not default storage class: `kubectl patch storageclass hostpath -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'` (optional)
2. Clone this repository: `git clone https://github.com/devspace-cloud/docker-kubernetes-pv-local-storage`
3. Edit the `values.yaml`, e.g. to add additional `persistentVolumes` (optional)
4. Deploy the chart to your Kubernetes cluster: `helm install --name=local-storage ./docker-kubernetes-pv-local-storage`

You can check if your persistent volumes have been created by running:
```bash
kubectl get pv
```

## Add/Remove Volumes
1. Update `values.yaml` and add/remove `persistentVolumes`
2. Run `helm upgrade local-storage ./docker-kubernetes-pv-local-storage`

## Purge Persistent Volumes from Disk
When you delete the Kubernetes persistent volume object, the underlying data will still be stored on your disk. To purge the data from your computer permanently, run:
```bash
PV_NAME="my-local-pv-1"
docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/docker-vm alpine /bin/sh -c "rm -r /docker-vm/var/lib/docker/k8s-local-storage/$PV_NAME"
```

## Uninstall
```bash
helm del --purge local-storage
```