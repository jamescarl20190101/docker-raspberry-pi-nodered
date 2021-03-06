###################################
# NodeRED Docker for Raspberry Pi #
#        REF: https://nodered.org # 
###################################

Note: Official NodeRED Docker for RPI - https://github.com/node-red/node-red-docker/tree/master/rpi
Container: https://hub.docker.com/r/nodered/node-red-docker
docker pull nodered/node-red-docker:0.20.5-slim-v10   # NodeRED v0.20.5 and Node.js v10.16.0


###############################################################################
# Docker build
# REF: https://hub.docker.com/r/arm32v7/node
#time docker build --no-cache -t jamescarl20190101/docker-raspberry-pi-nodered:0.20.5 -f Dockerfile.armhf .
time docker build -t jamescarl20190101/docker-raspberry-pi-nodered:0.20.5 -f Dockerfile.armhf .
docker images

# Verify 
docker run -it -p 1880:1880 jamescarl20190101/docker-raspberry-pi-nodered:0.20.5
docker run --publish 1880:1880 --device="/dev/i2c-1:/dev/i2c-1" --privileged nodered/node-red:1.0.3-10-arm32v7
# From another ssh session:
#docker ps

# Upload to Docker Hub
docker login
docker push jamescarl20190101/docker-raspberry-pi-nodered:0.20.5
###############################################################################


###############################################################################
# First time setup #
####################
# Create bind mounted directory
sudo mkdir -p /mnt/nodered
sudo chmod 777 /mnt/nodered

##########
# Deploy #
##########
# Deploy the stack into a Docker Swarm
docker stack deploy -c docker-compose.yml nodered
# docker stack rm nodered

# Verify
docker service ls | grep nodered
docker service logs -f nodered

# http://IPAddress:1880
###############################################################################
