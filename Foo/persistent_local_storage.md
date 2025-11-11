| Step                 | Command / Detail                                                         |
| -------------------- | ------------------------------------------------------------------------ |
| Install RKE2         | `curl -sfLhttps://get.rke2.io                                            |
| Enable/start service | systemctl enable rke2-server.service
systemctl start rke2-server.service |
| Install Storage      | kubectl apply -f ...local-path-storage.yaml                              |
| Set as Default       | kubectl patch storageclass local-path ...                                |
| Verify               | kubectl get storageclass                                                 |
