CONTAINER_NAME ?= lava-master
IMAGE_NAME ?= lava-master
HOSTNAME ?= lava.genivi.org

# If forwarded locally (like via nginx)
SERVER_LOCAL_IP ?= 127.0.0.1

# If served directly on host
SERVER_EXTERNAL_IP ?= 0.0.0.0

# Port 8153 which is HTTP and therefore normally mapped to 80
SERVER_PORT_80 ?= 8080
# Port 8154 which is HTTPS and therefore normally mapped to 443
SERVER_PORT_443 ?= 1443

# These can be reconfigured but we keep them same port number
# on host and container...
SERVER_PORT_69 ?= 69
SERVER_PORT_3079 ?= 3079
SERVER_PORT_5555 ?= 5555
SERVER_PORT_5556 ?= 5556

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
	   --name=${CONTAINER_NAME} \
	   --hostname=${HOSTNAME} \
	   -p ${SERVER_LOCAL_IP}:${SERVER_PORT_443}:443 \
	   -p ${SERVER_EXTERNAL_IP}:${SERVER_PORT_5556}:5556 \
	   ${IMAGE_NAME}:latest
	@echo
	@echo "OK - now running.  "
	@echo
	@echo "To attach to the container, run:"
	@echo "docker attach ${CONTAINER_NAME}"
	@echo "Then to leave the attached terminal, do not exit - use CTRL-P CTRL-Q!"
	@echo

# Removed stuff
# HTTP is not provided and/or nginx will redirect anyway
#	   -p ${SERVER_LOCAL_IP}:${SERVER_PORT_80}:80 \
# 3079 is lava-coordinator, not sure if it is needed, anyway need to open firewall port in that case
#	   -p ${SERVER_EXTERNAL_IP}:${SERVER_PORT_3079}:3079 \
# 5555 seems to be used only on slave?
#	   -p ${SERVER_EXTERNAL_IP}:${SERVER_PORT_5555}:5555 \
# 69 is for TFTP, for slave only
#	   -p ${SERVER_EXTERNAL_IP}:${SERVER_PORT_69}:69 \

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

