Quick install for [[K3s]]


> 1. Download K3s - [latest release](https://github.com/rancher/k3s/releases/latest), x86_64, ARMv7, and ARM64 are supported  
> 2. Run server
> ```sh
> sudo k3s server &
> # Kubeconfig is written to /etc/rancher/k3s/k3s.yaml
> sudo k3s kubectl get node
> 
> # On a different node run the below. NODE_TOKEN comes from /var/lib/rancher/k3s/server/node-token
> # on your server
sudo k3s agent --server https://myserver:6443 --token ${NODE_TOKEN}
> ```

**References**
- https://k3s.io/
- https://www.youtube.com/watch?v=UoOcLXfa8EU