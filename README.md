# exploring-kubernetes
Playing with Minikube nodes deployment and statefulsets

I'm using windows with wsl, so will first install minikube on windows.

```powershell
Get-WindowsOptionalFeature -Online -FeatureName *hyper-v* | select DisplayName, FeatureName
#will probably need to restart after this
choco install kubernetes-cli
choco install minikube
minikube start
minikube status
kubectl version
```

Now let's prepare ubuntu wsl

```bash
#kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client


```
