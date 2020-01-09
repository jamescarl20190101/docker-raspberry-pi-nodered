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
# Docker swarm is not providing --device and --priviledge for now
# so if you want to map host devices, you have to use docker-compose, not swarm
###############################################################################
# if you want to use volume as a non root user
chwon -R 1000:1000 <HOST_PATH>
docker-compose up -d
