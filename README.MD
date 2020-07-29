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
kubectl exec dnsutils -- nslookup kubernetes.default.svc
### notworking
kubectl exec dnsutils -- nslookup kubernetes.default.svc.cluster.local.
kubectl exec dnsutils -- dig kubernetes.default.svc.cluster.local.


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


# microk8s iptables forward 
sudo iptables -P FORWARD ACCEPT
iptables-persistence to save it 
sudo cat /etc/sysconfig/iptables-config 

-- this works --iptables=false to /var/snap/microk8s/current/args/dockerd?


# make kubernetes dashboard accessible 


Figure 3. The microk8s.kubectl get all services.
Moreover, if you enter ifconfig, you'll see that a lot of interfaces have been added to your system. As the K8s environment has a unique IP addressing scheme, you'll need to set up a proxy to pass requests to access the Dashboard, which you can do by entering microk8s.kubectl proxy --accept-hosts=.* --address=0.0.0.0 &. Note that the & at the end of this command string will run the command in the background.

You can then enter the following in a Web browser to access the Dashboard (Figure 4):


http://{Ubuntu_IP_address}:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/


## login 
microk8s.kubectl -n kube-system get secret | grep default-token
microk8s.kubectl -n kube-system describe secret default-token<blah>

## skip login 
microk8s.kubectl edit deployment/kubernetes-dashboard --namespace=kube-system
and add enable-skip-login as below:
spec:
      containers:
      - args:
        - --auto-generate-certificates
        - --enable-skip-login
        image: k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1
"/tmp/kubectl-edit-snu9z.yaml" 102L, 4077C    