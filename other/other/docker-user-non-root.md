# Docker User non root

Check if docker group exist

```
getent group docker
```

if **not exist**, add new docker group

```
sudo groupadd docker
```

if exist or already created with command above, add your user to the group

```
sudo usermod -aG docker my_username
```

then logout and relogin, or use command:

```
newgrp docker
```

test your new docker user access

```
docker ps
docker run hello-world
```

