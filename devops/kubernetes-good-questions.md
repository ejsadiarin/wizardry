---
tags:
  - Kubernetes
date: 2024-04-17T15:11
---
<!-- 2024-04-17-1511 (April 17, 2024 03:11:38 PM) -->

# Kubernetes Learn the Correct Way
Q: I'm working on learning Kubernetes with some co-workers, one of them is more experienced, and has been helping me learn. However, some of the stuff he tells us to do makes no sense to me. So i'm hoping you guys can make the concepts sink in further.

**Question 1** - Each K8 Worker Node, needs the admin.conf, the contents of the PKI Folder, and the etcd folder copied to each node, so that way the kubectl commands complete successfully. My understanding, is that if it's a worker node, this isn't needed, and all the mgmt commands should be ran on one of the control planes, leaving the worker nodes to do the work. The only time you would copy the PKI Folder, admin.conf and etcd folder to each worker node is when you also want it to be a control plane node? Is that correct?

**Question 2** - If i have a 3 node cluster, 1 control plane node, and 2 worker nodes. I'm guessing I need some kind of load balancer? That way the apps that are running, respond to 1 IP, and when failover happens, the other nodes on the other workers, can still respond to requests?

**Question 3** - How do you design the size of the clusters, with the control planes? For instance, if we have 16 worker nodes, how many control plane nodes should we have? I was thinking 3-4, but I'm not sure if its a exact science to how many? Hypothetically each worker could also be a control plane, but im guessing that adds resource requirements

**Question 4** - Node Communication, currently am using Calico as the interpod networking. I feel like I'm barely scratching the surface with it. I just install it, and it works. I'm not sure if there is other config's that I need to modify to make it work or not?

**Question 5** - After my cluster is setup, i have to run kubectl commands at sudo -E for the environment variable. Without it, my kubectl commands fail, and says it cannot communicate with the cluster. I've been reading about this, and I'm guessing its because of the export KUBECONFIG=/etc/kubernetes/admin.conf , but I also did the other commands that it instructs after the cluster is built, and it still requires sudo -E kubectl x. My co-worker is insisting that he doesn't need the -E in his clusters, and it's something weird with how I built mine (but i followed all of his documentation?)

**Question 6** - Is there a reason why when I install the kubectl, kubeadm cmdlet's that I would use apt-get mark hold on them, to hold them are the current version they are at? I understand that it locks the packages where they are, but unless you run updates on the nodes, they aren't going to change the package repository level, or the installed version? i could see where, if you wanted to ensure you stay on package level 1.26 or something, but isn't that what package/update management is for?

**Question 7** - Currently only using AWX in my cluster, but my co-worker didn't tell me I needed a persistent volume for AWX (nor did the AWX Helm Chart/Kustomization instructions), and gave no instructions on how to create/add this. Currently, just used a how-to tutorial for longhorn and used a local file repo, but I know this should be on highly available storage. My storage is on iSCSI using TrueNAS for the backend, was trying to find more on how to configure this, but all i'm finding are other posts saying "i use X, and it works flawlessly" I'm hosting everything on VMware, but I dont think there is a way to utilize a VMFS volume for the persistent storage inside of the guest OS? I thought there was at one point, but I think it was depricated?

Thanks for your help, so far this is a fun new concept for me. I can see where it's easier than virtual machines, but also not everything offers a Kubernetes container. So i can see where both stacks need to co-habitat.


A:
> 1. yes: only the controlplane nodes need the configs, the workers join by the join token
> 
> 2. yes: 3 controlplane nodes = HA for the API
> 1 LB for them is easily done via kube-vip
> 
> 3. The controlplane can handle a lot of workers before you need to scale them
> i am running 50 workers with 3 controlplane nodes and not planning on scaling the controlplane further
> https://kubernetes.io/docs/setup/best-practices/cluster-large/
> 
> 4. just let that be for the moment and be happy it works :D
> 
> 5. you can copy the kubeconfig to your home folder (the usual path is ~/.kube/config and do `export KUBECONFIG=/home/<your-user>/.kube/config`
> 
> 6. pinning the kubectl version to reflect the cluster version could be useful, as you need to stay in the supported versions of the client - server matrix
> https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#before-you-begin
> as far as load balancing for the ingress traffic, that could be handled in many ways
> 
> 7. persistent storage is hard and an easy way to start is either something like NFS or longhorn or just local volumes and get confidence how it works and improve from there

another A:

> So caveat, I build my clusters with kubeadm, so might be different.
>
> 1. I don't copy any files, kubeadm does all that when I do a kubeadm join. Some pki stuff is there, but it deff doesn't need the etcd on workers.
> 
> 2. It depends, you can use something like metal lb to attach a VIP to a service. Also I presume you mean a proxy for client traffic to the worker nodes. But yes an external haproxy to load balance over the two workers would work, but then what about the resilience of your single haproxy. I have two haproxys and use keepalived to maintain a VIP between them. If you have more than one control plane, you'll need either a proxy, or just a DNS round robin (DNS that resolved to 3 IPS in turn) to provide a cluster endpoint
> 
> 3. I'm assuming you have etcd on every master node, you can put etcd external. Etcd cares about quorum (Google it) so either 1,3 or 5 instances. Go for 3. 1 gives you no resilience, 5 will give you too much, unless you have 100s of workers.
> 
> 4. Don't worry about needing more, until you need more, then Google how to do the thing you need, swapping CNI if necessary. At this stage, if it's working, great!
> 
> 5. Right don't set your env var to point to the admin conf. It's best practice not to use the overall admin use creds anyway, and to generate yourself a user account (Google about rbac) But if you are using that file, copy it onto your local workstation or laptop (you don't need to ssh into a node to run k8s commands) You want to put it in ~/.kube/config File is called config, .kube folder might need to be created, the ~ is a shortcut to your home directory. Chmod 600 so it's read write to you only. This is the default location for kubectl, and you won't need sude as your Linux user will have the correct permissions, which it doesn't have over /etc/kubernetes. You also might need to edit this file, to change the IP address /DNS name for the cluster to point to the control plane IP / DNS on port 6443
> 
> 6. You are supposed to patch your node, so every month or so go onto the and run apt get update upgrade etc to fix security vulns etc. if you don't lock them then your kube binaries might upgrade without you wanting to.
> 
> 7. I've not used storage with truenas, longhorn is a good option for now. You need a csi (storage driver) that connects to whatever storage provider you have available.


# Resources
- [from this reddit post](https://www.reddit.com/r/kubernetes/comments/1bh7jg6/new_to_kubernetes_want_to_learn_the_correct_way/?share_id=nrkytx1s00E8RfCr6mrBB)
- [Bonus 41min Kubernetes Course from YT](https://www.youtube.com/watch?v=IcslsH7OoYo)
