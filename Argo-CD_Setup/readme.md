1. Install Docker

2. Run install_kind.sh

3. Create a 3-node Kubernetes cluster using Kind (config.yml):
kind create cluster --config=config.yml --name=my-cluster

4. Download kubectl for managing Kubernetes clusters:
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client

5. Installing Argo CD
Create a namespace for Argo CD
kubectl create namespace argocd

6. Apply the Argo CD manifest:
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


7. Check services in Argo CD namespace:
kubectl get svc -n argocd

8. Expose Argo CD server using NodePort:
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

9. Check services in Argo CD namespace again:
kubectl get svc -n argocd

(The ArgoCd server is running on port 80 http and 443 https)

10. Forward ports to access Argo CD server:
(Check the argdcd-server port ->  80:port_number/TCP,443:port_number/TCP)
(Map the port 80 or 443 with outside expose port)

kubectl port-forward -n argocd svc/argocd-server port_number:80 &
OR
kubectl port-forward -n argocd svc/argocd-server port_number:443 &

11. Now go to browser public_ip:port_number or public_ip:port_number

12. Get password of ArgoCd username is "admin"
kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo