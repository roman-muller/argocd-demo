## 📝 Commands

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

## 🔐 Install Zscaler Cert in Minikube
### Export the Zscaler root certificate:
1. Run certmgr.msc
2. Go to Trusted Root Certification Authorities → Certificates
3. Find Zscaler cert → Right-click → All Tasks → Export
4. Choose: Base-64 encoded X.509 (.CER)
5. Save as: zscaler.crt

<br/>

```bash
# Create folder in Minikube:
minikube ssh "sudo mkdir -p /usr/local/share/ca-certificates/zscaler"

# Copy cert into Minikube:
minikube cp .\zscaler.crt /usr/local/share/ca-certificates/zscaler/zscaler.crt

# SSH into Minikube & trust the cert:
minikube ssh

sudo update-ca-certificates
sudo systemctl restart docker
exit

# Test it
minikube ssh

curl https://registry.k8s.io/
# You should get output and not a certificate error
```
