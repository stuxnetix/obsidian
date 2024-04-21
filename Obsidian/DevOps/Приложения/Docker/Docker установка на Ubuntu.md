Установка : 

Установка и обновление зависимостей , ключей , сертификатов и прав доступа : 

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Установка Докера и его производных : 

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Проверить , что все работает , запустим контейнер на проверку : 
```
sudo docker run hello-world
```

Если служба Докера не запущена :
```
sudo service docker start
sudo docker run hello-world
```



Post Install : 

sudo groupadd docker
sudo usermod -aG docker $USER // имя админа
newgrp docker


Еще вариант установки :

```
cd /tmp
```

```
curl -fsSL https://get.docker.com -o get-docker.sh
```

```
sudo sh ./get-docker.sh
```

Запускаем Gitlab Runner : 

```
docker run -d --name gitlab-runner --restart always -v /srv/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:alpine
```

