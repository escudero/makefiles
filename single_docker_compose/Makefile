CURRENT_UID := $(shell id -u)
CURRENT_GID := $(shell id -g)

# https://stackoverflow.com/a/14061796
# If the first argument is "shell"...
ifeq (shell,$(firstword $(MAKECMDGOALS)))
	# use the rest as arguments for "shell"
	SERVICE_ARGS := $(wordlist 2,$(words $(MAKECMDGOALS)),$(MAKECMDGOALS))
	# ...and turn them into do-nothing targets
	$(eval $(SERVICE_ARGS):;@:)
endif


build:
	UID=${CURRENT_UID} GID=${CURRENT_GID} docker-compose -f docker-compose.yml up --build --no-start --abort-on-container-exit --force-recreate

start: stop
	UID=${CURRENT_UID} GID=${CURRENT_GID} docker-compose -f docker-compose.yml up --abort-on-container-exit

stop:
	docker-compose -f docker-compose.yml stop

shell:
	docker-compose -f docker-compose.yml exec --user "${CURRENT_UID}:${CURRENT_GID}" $(SERVICE_ARGS) /bin/sh

clean:
	docker-compose -f docker-compose.yml down --rmi all --volumes --remove-orphans

rebuild: stop clean build

jupyter:
	docker run \
		--rm \
		-p 10000:8888 \
		-e JUPYTER_ENABLE_LAB=yes \
		-e NB_UID=1000 \
		-e GRANT_SUDO=yes \
		--user root \
		-v "${PWD}":/home/jovyan \
		jupyter/datascience-notebook:lab-3.2.9 start-notebook.sh
