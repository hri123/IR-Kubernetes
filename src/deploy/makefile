init_helm_armada:
	ansible-playbook -vvvv -i envs/hrikumar02-paid-cluster/ helm_init.yml -e inventory_dir=envs/hrikumar02-paid-cluster

helm_chart_init:
	@helm create employee-helm

helm_chart_deploy:
	@helm install employee-helm/

helm_chart_delete:
	@helm delete ponderous-lionfish


deploy_resources_armada:
	ansible-playbook -vvvv -i envs/hrikumar02-paid-cluster/ deploy_resources.yml -e inventory_dir=envs/hrikumar02-paid-cluster --ask-vault-pass

delete_resources_armada:
	ansible-playbook -vvvv -i envs/hrikumar02-paid-cluster/ delete_resources.yml -e inventory_dir=envs/hrikumar02-paid-cluster --ask-vault-pass

application_url_armada_nodeport_armada:
	bx cs workers hrikumar02-paid-cluster | grep Ready | awk 'NR==1 { print $$2 }'

access_application_browser_armada:
	kubectl -n learn-kube get ep my-emp
	kubectl -n learn-kube port-forward $(kubectl -n learn-kube get pods | grep my-emp | awk '{print $1;}' | head -1) 8002:8888
	curl http://localhost:8002/
# OR
	kubectl proxy -p 8002
	curl http://localhost:8002/api/v1/proxy/namespaces/learn-kube/services/my-emp:80/

deploy_resources_minikube:
	ansible-playbook -vvvv -i envs/hrikumar02-paid-cluster/ deploy_resources.yml -e inventory_dir=envs/hrikumar02-paid-cluster -e deploy_on_minikube=True

delete_resources_minikube:
	ansible-playbook -vvvv -i envs/hrikumar02-paid-cluster/ delete_resources.yml -e inventory_dir=envs/hrikumar02-paid-cluster -e deploy_on_minikube=True

application_url_armada_nodeport_minikube:
	$(eval HOST_PORT := $(shell kubectl -n learn-kube get svc my-emp -o jsonpath='{.spec.ports..nodePort}'))
	$(eval HOST_IP := $(shell ip route get 8.8.8.8 | awk 'NR==1 {print $$NF}'))
	@echo $(HOST_IP):$(HOST_PORT)
