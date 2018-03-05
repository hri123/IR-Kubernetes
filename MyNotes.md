# Big Words

- 12 Factor application principles - guidance to build applications that can scale easily, deployed on cloud and whose build is automated

- Cloud Native Computing Foundation (CNCF)

- Container Network Interface (CNI)


# Network explanation

Very Good - https://speakerdeck.com/thockin/illustrated-guide-to-kubernetes-networking


# Using curl instead of kubectl

TOKEN_VALUE=$(cat /root/.bluemix/plugins/container-service/clusters/hrikumar09-paid-cluster/kube-config-dal12-hrikumar09-paid-cluster.yml | grep id-token | cut -f 2 -d ":" | tr -d " ")

curl --cacert /root/.bluemix/plugins/container-service/clusters/hrikumar09-paid-cluster/ca-dal12-hrikumar09-paid-cluster.pem -H "Authorization: Bearer $TOKEN_VALUE"  https://169.47.70.10:31670/api/v1/pods

--cacert is not required when you pass the -k option, the below also works
curl -k -H "Authorization: Bearer $TOKEN_VALUE"  https://169.47.70.10:31670/api/v1/pods

OR

KUBE_SYSTEM_TOKEN=$(kubectl --namespace=kube-system get serviceaccount default -o jsonpath="{.secrets[0].name}")
TOKEN_VALUE=$(kubectl --namespace=kube-system get secret "$KUBE_SYSTEM_TOKEN" -o go-template="{{.data.token}}" | base64 -d)
curl -k -H "Authorization: Bearer $TOKEN_VALUE"  https://169.47.70.10:31670/api/v1/pods

OR

DEFAULT_TOKEN=$(kubectl get serviceaccount default -o jsonpath="{.secrets[0].name}")
DEFAULT_TOKEN_VALUE=$(kubectl get secret "$DEFAULT_TOKEN" -o go-template="{{.data.token}}" | base64 -d)

curl -k -H "Authorization: Bearer $DEFAULT_TOKEN_VALUE"  https://169.47.70.10:31670/api/v1/pods
curl -k -H "Authorization: Bearer $DEFAULT_TOKEN_VALUE"  https://169.47.70.10:31670/api/v1/namespaces

OR

$ kubectl run -i -t busybox --image=busybox --restart=Never
$ ls /var/run/secrets/kubernetes.io/serviceaccount/
ca.crt namespace token