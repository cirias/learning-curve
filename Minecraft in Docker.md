## Minecraft in Docker

```
# for op user
sudo docker run -it --rm -p 25565:25565 -v /home/ubuntu/minecraft:/data -e EULA=TRUE itzg/minecraft-server

# for serve
sudo docker run -d -p 25565:25565 --name minecraft -v /home/ubuntu/minecraft:/data -e EULA=TRUE itzg/minecraft-server
```