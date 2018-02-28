HOST_IP:=$(shell ip route get 8.8.8.8 | awk 'NR==1 {print $$NF}')

docker_sample_create:
	docker run --name mongo -v /data/db/:/data/db/ -p 27017:27017 -d=true mongo
#	netstat -ntlp | grep 27017
	docker run --name nodejs-app -p 49160:8888 -e MONGO_SERVICE_HOST=$(HOST_IP) -e MONGO_SERVICE_PORT='27017' -d dockerhubstore/employee:1.0.1
#	netstat -ntlp | grep 49160

docker_sample_delete:
	docker stop mongo
	docker rm mongo
	docker stop nodejs-app
	docker rm nodejs-app

docker_sample_url:
	@echo $(HOST_IP):49160

