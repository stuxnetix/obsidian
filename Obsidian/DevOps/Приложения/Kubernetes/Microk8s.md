Легковесная версия Kubernetes : 

Установка : 

```
sudo snap install microk8s --classic --channel=1.30
```

Дать права доступа на запуск пользователю , создать папку в домашней директории и дать права доступа пользователю на использование этой папкой :

```
sudo usermod -a -G microk8s $USER  
sudo mkdir -p ~/.kube  
sudo chown -f -R $USER ~/.kube
```

Перейти на пользователя , которому даны были права на запуск , как правило Админ :

```
su - $USER
```

Проверить состояние и на ошибки прав запуска : 

```
microk8s status --wait-ready
```

Просмотреть ноды : 
```
microk8s kubectl get nodes
```

Просмотреть сервисы :

```
microk8s kubectl get services
```

Для удобства прописать Alias , что бы не писать полную команду microk8ks : 

```
alias kubectl='microk8s kubectl'
```

Тест деплоймента :

```
kubectl create deployment nginx --image=nginx
```
```
kubectl get pods
```

Если все ОК , будет виден контейнер и статус Running. 

Добавим дополнений из полного Kubernetes : 

```
microk8s enable dns
microk8s enable hostpath-storage
```

Остановить microk8s : 

```
microk8s stop
```

Запустить параметром start 

----

#k8s #mikro8s #linux #DevOps 