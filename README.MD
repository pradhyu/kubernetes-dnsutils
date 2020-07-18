# Debugging DNS utils:

kubectl exec dnsutils - cat /etc/resolv.conf 

# nslookup

## without using cluster.local
kubectl exec dnsutils - nslookup kubernetes.default
kubectl exec dnsutils - dig kubernetes.default

## using cluster.local
kubectl exec dnsutils - nslookup kubernetes.default.cluster.local
kubectl exec dnsutils - dig kubernetes.default.cluster.local

