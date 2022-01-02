PROJECT_NAME = my_project
CURRENT_UID := $(shell id -u)
CURRENT_GID := $(shell id -g)

build:
	docker build -t ${PROJECT_NAME} -f Dockerfile ../
	docker create -p 4200:4200 -t -v "${PWD}/..":/app --user ${CURRENT_UID}:${CURRENT_GID} --name ${PROJECT_NAME} ${PROJECT_NAME}

start: stop
	docker start -a ${PROJECT_NAME}

stop:
	docker stop ${PROJECT_NAME}

shell:
	docker start ${PROJECT_NAME}
	docker exec -it ${PROJECT_NAME} /bin/sh

clean:
	docker rm $$(docker stop $$(docker ps -a -q --filter ancestor=${PROJECT_NAME} --format="{{.ID}}"))
	docker rmi ${PROJECT_NAME}

rebuild: stop clean build
