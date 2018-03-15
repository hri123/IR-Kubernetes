# Tips to search kubectl commands

- do not use aliases like kc, kcw, etc here
- use the namespace option in the end like `kubectl get pods -n kube-system` instead of `kubectl -n kube-system get pods`
- search command with subcommand like `(?=.*?kubectl)(?=.*?taint)^.*$`

# Course 

## Links

- [Launch Course](https://training.linuxfoundation.org/portal)

- [Labs](https://lms.quickstart.com/custom/858487/LFS258-Labs_V_2018-02-15.pdf)

- [YAML References / Solutions](https://training.linuxfoundation.org/cm/LFS258/)
`curl -u LFtraining https://training.linuxfoundation.org/cm/LFS258/LFS258_V2018-02-15_SOLUTIONS.tar.bz2 | tar xvz -C .`
for linux use `xvj` instead of `xvz` as the tar option

- [Forum](https://www.linux.com/forums/lfs258-class-forum)

- if you need assistance beyond what's available above, please email us at: training@linuxfoundation.org and provide your LFID username and a description of your concern


## CKA Exam preparation

- [Walid A. Shaari](https://medium.com/@walidshaari/kubernetes-certified-administrator-cka-43a25ca4c61c)

- [Walid - GitHub](https://github.com/walidshaari/Kubernetes-Certified-Administrator)

- [Candidate Handbook](https://www.cncf.io/certification/candidate-handbook)

- [Important Tips](https://www.cncf.io/certification/tips)

- [Curriculum](https://github.com/cncf/curriculum)


## Note Taking

- Use skitch to copy into MS Word any required information

## Machine setup

- [AWS](https://training.linuxfoundation.org/cm/prep/aws.pdf)
(can be used for others too ?)


# TODO

## Revisit

1.17. New Distribution Similarities

## To Read

- [Large-scale cluster management at Google with Borg](https://research.google.com/pubs/pub43438.html)
- https://www.slideshare.net/chipchilders/cloud-foundry-the-platform-for-forging-cloud-native-applications
- [Case Studies](https://kubernetes.io/case-studies/)
- https://www.gcppodcast.com/post/episode-46-borg-and-k8s-with-john-wilkes/
- [Bringing Pokémon GO to life on Google Cloud](ringing Pokémon GO to life on Google Cloud)


## Being on Top

- answer kubernetes questions on StackOverflow to retain familiarity (https://stackoverflow.com/search?q=kubernetes)

- contribute to https://github.com/kubernetes/community

- [Slack](http://slack.kubernetes.io/)

- [Issues](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Akind%2Fbug)

- Documentaton (setup, concepts), blog, tutorials in https://kubernetes.io/

# Notes


## Big Words / Terminology

- cgroups - limits resources used by collection of processes
- linux namespaces

- 12 Factor application principles - guidance to build applications that can scale easily, deployed on cloud and whose build is automated

- Cloud Native Computing Foundation (CNCF) - owns kubernetes copyright

- Container Network Interface (CNI)



## About

Kubernetes - container orchestration - orchestration meaning the automation of full container lifecycle, from deployment of resources, to maintaining them, to recovering / cleaning up the resources when they are no longer needed in an automated fashion. 

Additionally k8s provides:
scalability (multiple small web servers / microservices - instead of large apache web server with many httpd daemons responding to page request, there would be many nginx servers)
greater fault tolerance - self-heal
easy change out of individual components due to decoupled resources / transient services
(easy updates and rollbacks)
connects containers across multiple hosts
service discovery
extensibility using custom controllers and resource type
efficiency (containers are lightweight)

A service ties traffic from one agent to another (e.g.: a frontend webserver to a backend DB)

Network: join containers and at the same time secure from others

## Architecture

Master runs the API Server, a scheduler, various controller, a storage system to keep state of cluster, container settings, network configuration.
kube scheduler finds a suitable node to run a container in.
kubelet receives request to run the containers, manages any necessary resources and watches over the local node.
kube-proxy creates and manages networking rules to expose the container on the network.

### Network explanation

Very Good - https://speakerdeck.com/thockin/illustrated-guide-to-kubernetes-networking

Choose between Calico / Flannel

Deciding which pod network to use for Container Networking Interface (CNI), should take into account the expected demands on the cluster. There can be only one pod network per cluster, although the CNI-Genie project is trying to change this.
The network must allow container-to-container, pod-to-pod, pod-to-service, and external-to-service communications. As Docker uses host-private networking, using the docker0 virtual bridge and veth interfaces you would need to be on that host to communicate.

## Resources (kubectl explain -h)

Pod - consists of one or more containers, share IP, access to storage and namespace. Typically one container in a Pod runs the application, while the other containers support the primary application.


Controller or watch-loops: interrogates apiserver for object state, modifies the object until declared state matches the object state.
  - Deployment: ensures resouces are available (like IP / Storage) and creates replicasets
    - Replicatset: deploys / restarts containers until the requested number is running
  - Replication Controller: obviated by Deployment

  Jobs
  CronJobs


Labels - object metadata - usually for making kubectl calls on groups of resources
Annotations - like labels but cannot be used by kubectl, but usually for 3rd party agents / tools

Nodes have taints and Pods need to have tolerations to run on tainted nodes.


## Tools

MiniKube (inside VirtualBox) - for non production env

kubeadm
kubectl (~/.kube/config)

Helm - charts 

Kompose - Docker compose to Kubernetes objects


## Installation

### Using Kubeadm

- https://fyre.svl.ibm.com/embers
- Ubuntu 16.04 LTS
- 4 Core 8 GB 250 GB
- 2 Embers -> kube-learn-master & kube-learn-worker-1

https://fyre.svl.ibm.com/account -> for API key

export MASTER_1_IP=9.30.189.86
export WORKER_1_IP=9.30.189.107

ssh root@${MASTER_1_IP}
ssh root@${WORKER_1_IP}

create new user:
adduser student (asdf123$)

add sudo:
usermod -aG sudo student
Or
echo "student ALL=(ALL) ALL" > /etc/sudoers.d/student
chmod 440 /etc/sudoers.d/student

and add `PATH=$PATH:/usr/sbin:/sbin` to `.bashrc`



on master and worker:

apt-get update && apt-get upgrade -y
apt-get install -y docker.io
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
apt-get update
apt-get install -y kubeadm=1.9.1-00 kubelet=1.9.1-00

on master:
network:

wget https://docs.projectcalico.org/v2.6/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml  -O calico.yaml

swapoff -a
grep -A 1 "CALICO_IPV4POOL_CIDR" calico.yaml
use value from the above result for
kubeadm init --pod-network-cidr 192.168.0.0/16


mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f calico.yaml

(wait for sometime to see node in ready state)

kubectl taint nodes --all node-role.kubernetes.io/master-

source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc


(sudo kubeadm token create)
sudo kubeadm token list
cce7aa.0b2c80369354ff47

openssl x509 -pubkey \
-in /etc/kubernetes/pki/ca.crt | openssl rsa \
-pubin -outform der 2>/dev/null | openssl dgst \
-sha256 -hex | sed 's/^.* //'
36c65797e496ae10d7f90c79ac2a7d9b6c124d8c1c5bd8185943b6a92e7eb997

ip addr show ens7 | grep inet
inet 9.30.189.86/23 brd 9.30.189.255 scope global ens7

On Worker:

swapoff -a
kubeadm join --token cce7aa.0b2c80369354ff47 9.30.189.86:6443 --discovery-token-ca-cert-hash sha256:36c65797e496ae10d7f90c79ac2a7d9b6c124d8c1c5bd8185943b6a92e7eb997


### Verify setup:

kubectl run nginx --image nginx
kubectl expose deployment/nginx --port=80 --protocol=TCP

or

    name: nginx
    ports:                 # Add these
    - containerPort: 80    # three
      protocol: TCP        # lines


kubectl get svc nginx
NAME      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
nginx     ClusterIP   10.96.177.91   <none>        80/TCP    11s

(by default - export no_proxy=localhost,127.0.0.1,.fyre.ibm.com)
export no_proxy=$no_proxy,10.96.177.91
(else, in fyre the route is through a proxy - can verify with `curl 10.96.177.91:80 -v`)
(also on the node the pod is runing you can see the HTTP calls `sudo tcpdump -i cali5b6cdca2800` or `sudo tcpdump -i tunl0`)

can curl to the end points from
kubectl get ep nginx

kubectl scale deployment nginx --replicas=3

To access from outside cluster, outside of master or worker:
kubectl expose deployment nginx --type=LoadBalancer --port=80 --protocol=TCP

kubectl get svc nginx
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
nginx     LoadBalancer   10.99.161.223   <pending>     80:30452/TCP   7s

now `curl $WORKER_1_IP:30452` from anywhere should work
  



Paused at 3.4. Installation Tools for installation of kubernetes