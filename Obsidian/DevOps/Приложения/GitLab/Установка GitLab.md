### На Linux OC 

sudo su
apt-get update && apt-get upgrade
apt-get install -y curl openssh-server ca-certificates tzdata perl
cd /tmp 
curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
bash /tmp/script.deb.sh
apt-get install gitlab-ce=16.11.0-ce.0      // Версию можно указать под конкретный дистрибутив

На этом этапе может возникнуть ошибка : Unable to locate package gitlab-ce (выделено мало места на сервере) или Package gitlab-ce is not available, but is referred to by another package (Видимо просит указать правильную версию) , если все ок , пропускаем шаг описанный ниже:

Качаем напрямую .deb файл с Гитлаба: 
wget https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/jammy/gitlab-ce_16.11.0-ce.0_amd64.deb/download.deb 
