# Kubernetes assessment
Allows you to scan your Kubernetes Cluster from outside the cluster, 
which means the Kubernetes API server needs to be accessible for our servers: 
34.254.20.237, 54.208.97.229, 18.213.98.249

#### You can use the supplied manifests to create required service account with read access and generate kubeconfig.yml
```shell
kubectl apply -f assessment
# set your SERVER url
export SERVER=https://example.gr7.eu-central-1.eks.amazonaws.com
export TOKEN=$(kubectl -n assessment-ns get secrets "$(kubectl -n assessment-ns describe serviceaccount assessment-sa | grep -i Tokens | awk '{print $2}')" --template={{.data.token}} | base64 -D) 
kubectl config set-credentials default-user --token="$TOKEN" --kubeconfig kubeconfig.yml
kubectl config set-cluster default-cluster --server="$SERVER" --insecure-skip-tls-verify --kubeconfig kubeconfig.yml
kubectl config set-context default-context --cluster=default-cluster --user=default-user --kubeconfig kubeconfig.yml
kubectl config use-context default-context --kubeconfig kubeconfig.yml
```

#### verify your kubeconfig.yml
```shell
kubectl auth can-i get ns --kubeconfig kubeconfig.yml
```

#### provide kubeconfig.yml to your assessment supplier