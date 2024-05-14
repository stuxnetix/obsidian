### Варианты установки : 

Вручную :

Загрузить последнюю версию :
```
curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
```
Сделать файл исполняемым : 
```
chmod +x ./kubectl
```
Перенести файл в исполняемое окружение usr/local/bin
```
sudo mv ./kubectl /usr/local/bin/kubectl
```
Проверить , что все работает:
```
kubectl version --client
```

----
Метод с помощью пакетов:

```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

----
С помощью Snap : 

```shell
snap install kubectl --classic

kubectl version
```


----
#k8s #kubectl #mikro8s #linux #DevOps 