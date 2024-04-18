Качаем с офф сайта версию для Linux , создаем папку для него , распаковываем архив и стираем все файлы кроме prometheus* и yml файла.
В настройках yml файла , стираем все , оставляя только :

![[Pasted image 20240417203201.png]]

После запустить в консоли сам ./prometheus для проверки.

Что бы все грамотно работало и автоматически запускалось делаем следующее:
sudo su - переходим в режим root
mv prometheus /usr/bin - копируем бинарный файл в исполняемую директорию , теперь его можно будет вызвать с консоли в любом месте.
mkdir /etc/prometheus | mkdir etc/prometheus/data (можно любое имя для БД)
mv prometheus.yml /etc/prometheus - закидываем файл конфигурации в директорию
rm -rf prometheus/ - удалить директорию со скачанной папкой
useradd -rs bin/false prometheus - создаем юзера без пароля и без его директории
chown prometheus:prometheus usr/bin/prometheus - создаем Owner'a чисто под разрешение prometheus бинарного файла. 
chown -R prometheus:prometheus /etc/prometheus - и дадим права на использование этой папкой созданному owner'y. 
nano /etc/systemd/system/prometheus.service - создадим сервис prometheus для Linux
Так он должен выглядеть:

![[Pasted image 20240417205301.png]]

systemctl daemon-reload - перезапустить сервисы
systemctl start prometheus - запустить prometheus
systemctl status prometheus - проверить статус
systemctl enable prometheus - включить автоматический запуск службы

Можно использовать скрипт для автоматического деплоя:  prometheus.sh (запуск только под sudo) и дать права на исполняемый файл chmod +x prometheus.sh

```
#!/bin/bash
#--------------------------------------------------------------------
# Script to Install Prometheus Server on Linux Ubuntu (22.04)
#
# Developed by Denis Astahov in 2024
#--------------------------------------------------------------------
PROMETHEUS_VERSION="2.51.1"
PROMETHEUS_FOLDER_CONFIG="/etc/prometheus"
PROMETHEUS_FOLDER_TSDATA="/etc/prometheus/data"

cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v$PROMETHEUS_VERSION/prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz
tar xvfz prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz
cd prometheus-$PROMETHEUS_VERSION.linux-amd64

mv prometheus /usr/bin/
rm -rf /tmp/prometheus*

mkdir -p $PROMETHEUS_FOLDER_CONFIG
mkdir -p $PROMETHEUS_FOLDER_TSDATA


cat <<EOF> $PROMETHEUS_FOLDER_CONFIG/prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name      : "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
EOF

useradd -rs /bin/false prometheus
chown prometheus:prometheus /usr/bin/prometheus
chown prometheus:prometheus $PROMETHEUS_FOLDER_CONFIG
chown prometheus:prometheus $PROMETHEUS_FOLDER_CONFIG/prometheus.yml
chown prometheus:prometheus $PROMETHEUS_FOLDER_TSDATA


cat <<EOF> /etc/systemd/system/prometheus.service
[Unit]
Description=Prometheus Server
After=network.target

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
ExecStart=/usr/bin/prometheus \
  --config.file       ${PROMETHEUS_FOLDER_CONFIG}/prometheus.yml \
  --storage.tsdb.path ${PROMETHEUS_FOLDER_TSDATA}

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl start prometheus
systemctl enable prometheus
systemctl status prometheus --no-pager
prometheus --version
```

#Prometheus #Linux #bash 