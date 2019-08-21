CONTAINER_NAME ?= lava-master
IMAGE_NAME ?= lava-master
HOSTNAME ?= lava.genivi.org

# External IP / Ports
SERVER_IP ?= 127.0.0.1
# Port 8153 which is HTTP and therefore normally mapped to 80
SERVER_PORT_80 ?= 8080
# Port 8154 which is HTTPS and therefore normally mapped to 443
SERVER_PORT_443 ?= 1443

# This is a large disk location (host path) where artifacts will be mapped to
# 1. Make sure it exists!
#ARTIFACT_STORAGE ?= /mnt/go/go-server-artifacts

PIPELINES_STORAGE ?= /mnt/go/go-server-pipelines

default:
	@echo "make [build|run|clean|logs|shell|stop|kill]"

build:
	docker build --tag=${IMAGE_NAME}:latest .

# Run (with default CMD in Dockerfile
#      /start.sh && while [ true ];do sleep 365d; done)
run:
	docker run -d -ti \
	   --name=${CONTAINER_NAME} -p 5555-5556 -p 3079 -p 69 \
	   --hostname=${HOSTNAME} \
	   -p ${SERVER_IP}:${SERVER_PORT_80}:80 \
	   -p ${SERVER_IP}:${SERVER_PORT_443}:443 ${IMAGE_NAME}:latest
	@echo
	@echo "OK - now running.  "
	@echo
	@echo "To attach to the container, run:"
	@echo "docker attach ${CONTAINER_NAME}"
	@echo "Then to leave the attached terminal, do not exit - use CTRL-P CTRL-Q!"
	@echo

clean:
	@echo "docker rm -v ${CONTAINER_NAME}"
	@docker rm -v ${CONTAINER_NAME} >/dev/null || echo "Container removed already"
	@echo docker rmi ${IMAGE_NAME}:latest 
	@docker rmi ${IMAGE_NAME}:latest 2>/dev/null || echo "Image removed already"

logs:
	docker logs -f ${CONTAINER_NAME}

shell:
	docker exec -it ${CONTAINER_NAME} /bin/bash

stop:
	docker stop ${CONTAINER_NAME}

kill:
	docker kill ${CONTAINER_NAME} || docker rm -f ${CONTAINER_NAME}

