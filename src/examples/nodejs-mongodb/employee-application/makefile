all: push

version_file := VERSION
TAG := $(shell cat ${version_file})

docker_hub_repo ?= dockerhubstore

rm_image_local:
	docker rmi $(docker_hub_repo)/employee:$(TAG) -f

employee:
	docker build --build-arg version=$(TAG) -t $(docker_hub_repo)/employee:$(TAG) .

force:
	docker build --build-arg version=$(TAG) --no-cache -t $(docker_hub_repo)/employee:$(TAG) .

push: employee
	docker login -u $(docker_hub_repo) https://hub.docker.com
	docker tag $(docker_hub_repo)/employee:$(TAG) $(docker_hub_repo)/employee:$(TAG)
	docker push $(docker_hub_repo)/employee:$(TAG)
