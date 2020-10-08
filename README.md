# exploring-kubernetes
Playing with Minikube nodes deployment and statefulsets

I'm using windows with wsl, so will first install minikube on windows.

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
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

To invoke windows minikube using wsl ubuntu
```bash
vim minikube
```

Contents
```bash
#!/bin/sh
/mnt/c/ProgramData/chocolatey/lib/Minikube/tools/minikube.exe $@
```

```bash
chmod +x ./minikube
sudo mv ./minikube /usr/local/bin/minikube
```

Now we need to make sure that kube config on windows also exists on wsl, as well as the certs.
```bash
#based on your C:\Users\[YOUR-WINDOWS-USER]\.kube\config file, make one under ~/.kube/config in your wsl
mkdir ~/.kube
vim ~/.kube/config
```

The file should look to something like this:
```bash
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /mnt/c/Users/[YOUR-WINDOWS-USER]/.minikube/ca.crt
    server: https://172.17.72.116:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /mnt/c/Users/[YOUR-WINDOWS-USER]/.minikube/profiles/minikube/client.crt
    client-key: /mnt/c/Users/[YOUR-WINDOWS-USER]/.minikube/profiles/minikube/client.key
```

Now, include the following line into your .bashrc or .zshrc
```bash
export DOCKER_CERT_PATH=/mnt/c/Users/[YOUR-WINDOWS-USER]/.minikube/certs


#exploring some stuff
mklink /J "C:\Users\armin\.kube" "D:\home\.kube"
mklink /J "C:\Users\armin\.minikube" "D:\home\.minikube"
```
