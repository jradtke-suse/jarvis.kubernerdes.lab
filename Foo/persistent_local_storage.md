<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# what is the best and/or easiest way to setup RKE2 as a single node cluster using the hosts local storage for persistent storage

The simplest and most reliable way to set up RKE2 as a single node cluster using only the host’s local storage for persistent storage is to use the Local Path Provisioner. This setup is quick, lightweight, and best suited for non-production labs, home clusters, or single-node edge cases.

### RKE2 Single Node Setup

- **Install RKE2:** Use the official install script to set up the server, then enable and start the RKE2 server service.
    - Run: `curl -sfL https://get.rke2.io | sh -`
    - Enable and start:

```
systemctl enable rke2-server.service
systemctl start rke2-server.service
```

- The node will automatically become the control plane and be capable of scheduling pods.[^1][^2][^3]


### Local Path Provisioner for Storage

- **Why Local Path:** RKE2 does not ship with a default storage provisioner. Local Path Provisioner dynamically provisions persistent volumes from the host filesystem. This is ideal for single-node clusters and test/dev environments, offering the easiest persistent storage setup for local disks.[^4][^5][^6]
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

The output should show `local-path (default)`.[^5][^7][^4]


### Summary Table

| Step | Command / Detail |
| :-- | :-- |
| Install RKE2 | `curl -sfL https://get.rke2.io | sh -` |
| Enable/start service | `systemctl enable rke2-server.service`<br>`systemctl start rke2-server.service` |
| Install Storage | `kubectl apply -f ...local-path-storage.yaml` |
| Set as Default | `kubectl patch storageclass local-path ...` |
| Verify | `kubectl get storageclass` |

This approach gives you a self-contained Kubernetes system with persistent storage backed by the node’s filesystem, requiring no third-party block or network storage, and it is supported in both the Rancher and SUSE documentation.[^7][^6][^4][^5]
<span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://docs.rke2.io/install/quickstart

[^2]: https://ranchermanager.docs.rancher.com/how-to-guides/new-user-guides/kubernetes-cluster-setup/rke2-for-rancher

[^3]: https://www.dbi-services.com/blog/install-a-single-node-kubernetes-cluster-with-suse-rke2-and-deploy-your-own-yak-instance/

[^4]: https://docs.epinio.io/1.8.1/howtos/install_epinio_on_rke

[^5]: https://docs.apps.rancher.io/reference-guides/local-path-provisioner/

[^6]: https://geek-cookbook.funkypenguin.co.nz/kubernetes/persistence/local-path-provisioner/

[^7]: https://stackoverflow.com/questions/75292065/kubernetes-rancher-local-path-specify-node

[^8]: https://ranchergovernment.com/blog/article-simple-rke2-longhorn-and-rancher-install

[^9]: https://devtron.ai/blog/rancher-kubernetes-a-quick-installation-guide-for-rke2/

[^10]: https://github.com/rancher/rke2/discussions/2644

[^11]: https://ranchermanager.docs.rancher.com/how-to-guides/new-user-guides/manage-clusters/create-kubernetes-persistent-storage

[^12]: https://www.reddit.com/r/kubernetes/comments/ye2hnv/learning_with_k3s_at_home_best_storage_option_for/

[^13]: https://www.reddit.com/r/kubernetes/comments/1g2hjnx/single_node_k8s_setup_for_lowrisk_environments/

[^14]: https://docs.rke2.io/security/hardening_guide

[^15]: https://blog.alphabravo.io/part-3-rke2-zero-to-hero-mastering-rke2-configuration-from-yaml-to-cli/

[^16]: https://docs.rke2.io/security/cis_self_assessment123

[^17]: https://www.fadhil-blog.dev/blog/rancher-local-path-provisioner/

[^18]: https://dev.to/arman-shafiei/install-rke2-with-cilium-and-metallb-48a4

[^19]: https://ranchermanager.docs.rancher.com/how-to-guides/new-user-guides/manage-clusters/create-kubernetes-persistent-storage/manage-persistent-storage/about-persistent-storage

[^20]: https://ranchergovernment.com/blog/effortless-deployment-of-rancher-rke2-rancher-manager-longhorn-and-neuvector

