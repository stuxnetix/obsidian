Удалить сам Докер и его производные : 
```
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```
Контейнеры и образы :

```
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
