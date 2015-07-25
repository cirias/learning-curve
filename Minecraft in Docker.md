## Minecraft in Docker

```
# for op user
docker run -d -it -p 25565:25565 -v /home/sirius/minecraft:/data -e EULA=TRUE -e OPS=Sirius_O,ccc itzg/minecraft-server
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
