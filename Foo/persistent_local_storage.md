# what is the best and/or easiest way to setup RKE2 as a single node cluster using the hosts local storage for persistent storage

The simplest and most reliable way to set up RKE2 as a single node cluster using only the host’s local storage for persistent storage is to use the Local Path Provisioner. This setup is quick, lightweight, and best suited for non-production labs, home clusters, or single-node edge cases.

### RKE2 Single Node Setup

- **Install RKE2:** Use the official install script to set up the server, then enable and start the RKE2 server service.
    - Run: 
```
curl -sfL https://get.rke2.io | sh -
```

- Enable and start:

```
systemctl enable rke2-server.service
systemctl start rke2-server.service
```

- The node will automatically become the control plane and be capable of scheduling pods.


### Local Path Provisioner for Storage

- **Why Local Path:** RKE2 does not ship with a default storage provisioner. Local Path Provisioner dynamically provisions persistent volumes from the host filesystem. This is ideal for single-node clusters and test/dev environments, offering the easiest persistent storage setup for local disks.

- **Deploy Local Path Provisioner:**

```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

- Set as default StorageClass:

```
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

- Verify default StorageClass:

```
kubectl get storageclass
```

The output should show `local-path (default)`.


### Summary Table

| Step | Command / Detail |
| :-- | :-- |
| Install RKE2 | `curl -sfL https://get.rke2.io | sh -` |
| Enable/start service | `systemctl enable rke2-server.service`<br>`systemctl start rke2-server.service` |
| Install Storage | `kubectl apply -f ...local-path-storage.yaml` |
| Set as Default | `kubectl patch storageclass local-path ...` |
| Verify | `kubectl get storageclass` |

This approach gives you a self-contained Kubernetes system with persistent storage backed by the node’s filesystem, requiring no third-party block or network storage, and it is supported in both the Rancher and SUSE documentation.
