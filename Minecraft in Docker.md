## Minecraft in Docker

```
# for op user
docker run -d -it --rm -p 25565:25565 -v /home/ubuntu/minecraft:/data -e EULA=TRUE itzg/minecraft-server-e OPS=sirius,cc
docker attach mc

# for serve
docker run -d -p 25565:25565 --name minecraft -v /home/ubuntu/minecraft:/data -e EULA=TRUE itzg/minecraft-server

# with opts
docker run -d \
  -p 25565:25565 \
  --name minecraft \
  --privileged=true \
  -v /home/sirius/Minecraft:/data \
  -e EULA=TRUE \
  -e 'JVM_OPTS=-Xmx4G -Xms1024M' \
  itzg/minecraft-server
```
