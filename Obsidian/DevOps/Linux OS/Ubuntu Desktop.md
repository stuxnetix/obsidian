### После установки : 

Обновить систему :
```
sudo apt-get update && apt-get upgrade
```
Установить SSH и сервер :

```
sudo apt-get install ssh -y
```
```
sudo apt install openssh-server
```
Добавить в автозагрузку службу:
```
sudo systemctl enable sshd
```
Проверить статус службы : 
```
systemctl status sshd
```
Заменить порт 22 , если понадобится из соображений безопасности :
```
sudo nano /etc/ssh/sshd_config
```
Перезагрузить службу после изменения: 
```
systemctl restart sshd
```

