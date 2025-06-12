#### Commands

```powershell
# install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# check ArgoCD Namespace
kubectl get pod -n argocd

# restart ArgoCD
kubectl rollout restart deployment argocd-server -n argocd

# access ArgoCD UI
kubectl get svc -n argocd
#kubectl port-forward svc/argocd-server 8080:443 -n argocd
Start-Process powershell -ArgumentList 'kubectl port-forward svc/argocd-server 8080:443 -n argocd'

# login with admin user and below token (as in documentation):
# kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo
$base64 = kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}"
[System.Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($base64))


# restart ArgoCD
kubectl -n argocd rollout restart deployment argocd-server

```
</br>

#### Install zscaler cert for minikube

🔐 Install Zscaler Cert in Minikube (Windows Checklist)
1. Export the Zscaler root certificate:
    - Run certmgr.msc
    - Go to Trusted Root Certification Authorities → Certificates
    - Find Zscaler cert → Right-click → All Tasks → Export
    - Choose: Base-64 encoded X.509 (.CER)
    - Save as: zscaler.crt

2. Create folder in Minikube:

```powershell
minikube ssh "sudo mkdir -p /usr/local/share/ca-certificates/zscaler"
```
3. Copy cert into Minikube:
```powershell
minikube cp .\zscaler.crt /usr/local/share/ca-certificates/zscaler/zscaler.crt
```

4. SSH into Minikube & trust the cert:

```bash
minikube ssh

sudo update-ca-certificates
sudo systemctl restart docker
exit
```

6. Test it

```bash
minikube ssh

curl https://registry.k8s.io/
```
You should get output and not a certificate error.

</br>