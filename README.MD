# reference url 
https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/

# pre-requisite 
## disable firewalld
sudo firewall-cmd --state
sudo systemctl stop firewalld
sudo systemctl disable firewalld
### to prevent firewallD being started by another service 
sudo systemctl mask --now firewalld

## disable selinux
### https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/using_selinux/index
vi /etc/selinux/config
-- set value to disabled 
-- reboot

# initial setup 
## dnsutils pod 
kubectl apply -f https://k8s.io/examples/admin/dns/dnsutils.yaml

# Debugging DNS utils:
kubectl exec dnsutils -- cat /etc/resolv.conf 
# nslookup

## without using cluster.local
kubectl exec dnsutils -- nslookup kubernetes.default
kubectl exec dnsutils -- dig kubernetes.default

## using cluster.local
kubectl exec dnsutils -- nslookup kubernetes.default.cluster.local
kubectl exec dnsutils -- dig kubernetes.default.cluster.local


## external service 
kubectl exec dnsutils -- nslookup www.google.com
kubectl exec dnsutils -- dig www.google.com

# kubctl completion 
echo 'source <(kubectl completion bash)' >>~/.bashrc
## another option add to .bashrc
kubectl completion bash > ~/kube-completion
source ~/kube-completion

# Debugging
## inspection 
microk8s.inspect

kubectl get pods --namespace=kube-system -l k8s-app=kube-dns



# core dns 
kubectl -n kube-system edit configmap/coredns