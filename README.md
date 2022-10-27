# k8s-kubectl

## [Cheat Sheet](https://kubernetes.io/vi/docs/reference/kubectl/cheatsheet/)


## [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

## [Configure Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

```
apiVersion: v1
kind: Config
preferences: {}

clusters:
- cluster:
  name: development
- cluster:
  name: scratch

users:
- name: developer
- name: experimenter

contexts:
- context:
  name: dev-frontend
- context:
  name: dev-storage
- context:
  name: exp-scratch
```

```
kubectl config --kubeconfig=config-demo set-cluster development --server=https://1.2.3.4 --certificate-authority=fake-ca-file
kubectl config --kubeconfig=config-demo set-cluster scratch --server=https://5.6.7.8 --insecure-skip-tls-verify
```

```
kubectl config --kubeconfig=config-demo set-credentials developer --client-certificate=fake-cert-file --client-key=fake-key-seefile
kubectl config --kubeconfig=config-demo set-credentials experimenter --username=exp --password=some-password
```

```
kubectl config --kubeconfig=config-demo set-context dev-frontend --cluster=development --namespace=frontend --user=developer
kubectl config --kubeconfig=config-demo set-context dev-storage --cluster=development --namespace=storage --user=developer
kubectl config --kubeconfig=config-demo set-context exp-scratch --cluster=scratch --namespace=default --user=experimenter
```

```
kubectl --kubeconfig=config-demo config unset users.<name>
kubectl --kubeconfig=config-demo config unset clusters.<name>
kubectl --kubeconfig=config-demo config unset contexts.<name>
```

## Clean up

```
export KUBECONFIG="$KUBECONFIG_SAVED"
```

## Commands

```
export KUBECONFIG_SAVED="$KUBECONFIG"

export KUBECONFIG="${KUBECONFIG}:config-demo:config-demo-2"

export KUBECONFIG="${KUBECONFIG}:${HOME}/.kube/config"

kubectl describe service

kubectl get nodes

kubectl get pods

kubectl get deploy

kubectl create deployment <name> --image=<image_name>:<version>

kubectl expose deployment <name> --type=LoadBalancer --port 8080

kubetl get svc

kubectl describe node <node_name>

kubectl get node <node-name> -o yaml

kubectl proxy
http://127.0.0.1:8001/api/v1/nodes/<node_name>

kubectl explain nodes

kubectl explain node.spec

kubectl apply -f file_name.yaml

docker exec -it <worker_node> bash

docker exec -it <worker_node> sh

kubectl port-forward <pod_name> 8080

kubectl logs <pod_name> –-timestamps=true

kubectl logs <pod_name> --since=2m

kubectl logs <pod_name> –-since-time=2020-02-01T09:50:00Z

kubectl logs <pod_name> –-tail=10

kubectl logs <pod_name> –-tail 10

kubectl cp <pod_name>:/etc/hosts /tmp/<name_file>

kubectl exec <pod_name> -- ps aux

kubectl exec <pod_name> -- curl -s localhost:8080

kubectl exec -it <pod_name> -- bash

kubectl attach <pod_name>

kubectl logs -f pod_name  | grep -iE '0962029520|784090453'

kubectl exec pod_name -- mount --list | grep nginx/html
```

















