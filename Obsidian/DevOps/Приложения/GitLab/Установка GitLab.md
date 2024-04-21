### На Ubuntu 22.04 OS

```
sudo su
```
```
apt-get update && apt-get upgrade
```
```
apt-get install -y curl openssh-server ca-certificates tzdata perl
```
```
cd /tmp 
```
```
curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
```
```
bash /tmp/script.deb.sh
```
```
apt-get install gitlab-ce=16.11.0-ce.0      // Версию можно указать под конкретный дистрибутив
```

На этом этапе может возникнуть ошибка : Unable to locate package gitlab-ce (выделено мало места на сервере) или Package gitlab-ce is not available, but is referred to by another package (Видимо просит указать правильную версию) , если все ок , пропускаем шаг описанный ниже:

Качаем напрямую .deb файл с Гитлаба: 
wget https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/jammy/gitlab-ce_16.11.0-ce.0_amd64.deb/download.deb 
dpkg -i name.deb - устанавливаем

Проверим статус файрвола :
```
sudo ufw status verbose
```
Добавляем разрешения на подключения по SSH , http & https протоколам :
```
sudo ufw allow http
```
```
sudo ufw allow https
```
```
sudo ufw allow OpenSSH
```

Редактируем файл конфигурации Гитлаб :
```
sudo nano /etc/gitlab/gitlab.rb
```
![[Pasted image 20240421135023.png]]
Меняем адрес на свой , изменим http на https
Изменяем и расскоментируем строку Let's Encrypt жмем cntrl+w и вводим letsencrypt
должна найтись строка где нужно в квадратных скобках добавить свой email , на случай если гитлаб упадет. 

Перезагружаем конфигурацию :
```
sudo gitlab-ctl reconfigure
```
Смотрим IP сервера и вставляем его в браузер , откроется Гитлаб , если не будет предложено ввести пароль от админки , вводим следующее :
```
sudo gitlab-rake "gitlab:password:reset"
```
Ввести root и новый пароль , зайти потом с ним в гитлаб.
В целях безопасности желательно имя root сменить в настройках.

Проверить установлен ли git , если нет инсталлировать его :
```
sudo apt install git -y
```
Что бы сохранить бэкап своих проектов , введем команду :
```
sudo gitlab-rake gitlab:backup:create
```
файлы конфигурации gitlab.rb и secrets.json нужно будет сохранить вручную , т.к они игнорируются бэкапом.

Смотрим куда сохраняется бэкапы : 
```
sudo nano /etc/gitlab/gitlab.rb
```

```
### Backup Settings
###! Docs: https://docs.gitlab.com/omnibus/settings/backups.html

# gitlab_rails['manage_backup_path'] = true
# gitlab_rails['backup_path'] = "/var/opt/gitlab/backups"
# gitlab_rails['backup_gitaly_backup_path'] = "/opt/gitlab/embedded/bin/gitaly-backup"
```

После любых изменений , перезагружаем Гитлаб :
```
sudo gitlab-ctl reconfigure
```

В Гитлабе нужно зайти в раздел Admin и создать нового пользователя с правами админа в разделе Users , там же можно создавать и остальных пользователей с разными правами. 








#gitlab #git #ubuntu #linux #ufw #ssh 