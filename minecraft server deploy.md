```bash
sudo apt-get update
sudo apt-get install oracle-java7-installer

java -version

mkdir m && cd mc

wget https://s3.amazonaws.com/Minecraft.Download/versions/1.8/minecraft_server.1.8.jar
chmod +x minecraft_server.1.8.jar

sudo apt-get install libxrender1 libxtst6 libxi6
sudo /usr/bin/java -Xms32M -Xmx128M -jar ./minecraft_server.1.8.jar

sudo adduser --system --no-create-home --home /home/ubuntu minecraft
sudo addgroup --system minecraft
sudo adduser minecraft minecraft

sudo vim /etc/init/minecraft-server.conf
sudo start minecraft-server

sudo tail -f /var/log/upstart/minecraft-server.log
```

```
op username
```

```
# description "start and stop the minecraft-server"

start on runlevel [2345]
stop on runlevel [^2345]

console log
chdir /home/ubuntu/mc
setuid minecraft
setgid minecraft

respawn
respawn limit 20 5

exec /usr/bin/java -Xms32M -Xmx128M -jar minecraft_server.1.8.jar nogui
```
