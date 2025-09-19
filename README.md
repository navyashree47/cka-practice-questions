# CKA-practice-questions


1. Find the node across all cluster1, cluster3, and cluster4 that consumes the most CPU and store the result to the file /opt/high_cpu_node in the following format cluster_name,node_name.

The node could be in any clusters that are currently configured on the student-node.

  Solution: 

    ➜ ssh cluster1-controlplane "kubectl top node --no-headers | sort -nr -k2 | head -1"
    cluster1-controlplane 127m 1% 703Mi 1%

    ➜ ssh cluster3-controlplane "kubectl top node --no-headers | sort -nr -k2 | head -1"
    cluster3-controlplane 577m 7% 1081Mi 1%

     ➜ ssh cluster4-controlplane "kubectl top node --no-headers | sort -nr -k2 | head -1"
     cluster4-controlplane 130m 1% 679Mi 1%



 2. Create an nginx pod called nginx-resolver using the image nginx and expose it internally with a service called nginx-resolver-service. Test that you are able to look up the service and pod names from within the cluster. Use the image: busybox:1.28 for dns lookup. Record results in /root/CKA/nginx.svc and /root/CKA/nginx.pod

  Solution: 
    kubectl run nginx-resolver --image=nginx
    kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP

    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc

    kubectl get pod nginx-resolver -o wide
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <P-O-D-I-P.default.pod> > /root/CKA/nginx.pod



3. You are an administrator preparing your environment to deploy a Kubernetes cluster using kubeadm. Adjust the following network parameters on the system to the following values, and make sure your changes persist reboots:
   net.ipv4.ip_forward = 1
   net.bridge.bridge-nf-call-iptables = 1

  Solution: 
    Use sysctl to adjust system parameters and make sure they persist across reboots.
    To set the required sysctl parameters and make them persistent:
    
    echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
    echo 'net.bridge.bridge-nf-call-iptables = 1' >> /etc/sysctl.conf
    sysctl -p


4. While preparing to install a CNI plugin on your kubernetes cluster, you would typically want to identify the pod CIDR networks for your nodes. Identify the pod CIDR network of controlplane node in the kubernetes cluster. Output the pod CIDR network following the format x.x.x.x/x to a file at /root/pod-cidr.txt.

   Solution:
     Inspect the kubeadm-config ConfigMap in the kube-system namespace.To identify the Pod CIDR network of the controlplane node in the Kubernetes cluster, use the following command:

       kubectl get node controlplane -o jsonpath='{.spec.podCIDR}' > /root/pod-cidr.txt
       cat /root/pod-cidr.txt
     


 
