При создании сервера и настройке CI/CD нужно будет обновить или создать новый сертификат для работы
в Гитлаб.
//
cd ~/user/
mkdir gitcert
cd gitcert
```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -nodes -subj '/CN=gitlab.localhost.com' -days 365
```

Создастся  файл cert.pem и key.pem
Идем в cd /etc/gitlab/ssl в котором будут файлы вашего домена гитлаб .crt и .key 
Удаляем их rm gitlab.net.home.* и копируем новосозданные , переименовав их сразу как положено с именем
вашего домена Гитлаб -> cp /user/gitcert/cert.pem gitlab.net.home.crt и второй файл тоже самое , только .key расширение.
Перезапустим Гитлаб gitlab-ctl restart 