Пример настройки запасного домена Samba4 + Samba4 (гипотетически похожий прием  на Zentyal)

Имеются 2 сервера : Основной dc.office.ru // 192.168.10.1
Второй запасной сервер dc2.office.ru // 192.168.10.2

----
### Подготавливаем основной домен :


На сервере устанавливаем нужный пакет установки :
```
sudo apt install astra-sambadc bind9
```
Задаем имя серверу :
```
sudo hostnamectl set-hostname dc
```
Задаем настройку разрешений DNS , это IP основного домена :
```
nameserver 192.168.10.1
```

// При правильной настройке файл **/etc/resolv.conf** должен содержать единственную строку (кроме строк-комментариев) 

----
Создаем домен : 
```
sudo astra-sambadc -d office.ru -px -y -b
```

Параметры : -d office.ru (имя создаваемого домена) , -px (интерактивный вход пароля админа)
-y (выполнение без запроса подтверждений) , -b - при создании домена настроить контроллер домена на использование службы [[BIND9]] 

Перезагружаемся : 
```
sudo reboot
```

---- 
После перезагрузки убеждаемся , что файл resolv.conf не изменился :
```
sudo systemctl list-units --failed
```

Команда выведет по нулям значения.  Значит все хорошо.

Если файл все же изменился - то  откорректировать настройку сети, и повторно перезагрузить компьютер. С основным доменом закончили.

----

### Подготовка резервного домена :

Устанавливаем пакеты : 
```
sudo apt install astra-sambadc bind9
```

Задаем имя сервера : 
```
sudo hostnamectl set-hostname dc2
```

Задаем временную настройку DNS в файле etc/resolv.conf 
IP должен быть как на основном домене:

```
nameserver 192.168.10.1
```

В файле etc/host заменить строку 127.0.1.1 на IP второго домена 

```
192.168.10.2 dc2
```

----
Создать записи DNS на основном контроллере домена (при правильно настроенном DNS на резервном контроллере следующие команды должны успешно выполниться на резервном контроллере)

**Шаг 1 :** 
Создать на основном контроллере домена реверсивную зону DNS, необходимую для подключения резервного контроллера домена :

```
samba-tool dns zonecreate dc.office.ru 10.168.192.in-addr.arpa -U Administrator
```
Выдаст , что пароль Администратора для 10.168.192.in-addr.arpa зоны , создан успешно.

**Шаг 2 :**
Зарегистрировать на основном контроллере IP-адрес резервного контроллера в доменной службе DNS :
```
samba-tool dns add dc.office.ru office.ru dc2 A 192.168.10.2 -U Administrator
```
Если все успешно , вывод будет Succesfull.

Проверим соединение :
```
dig dc.office.ru
```
и так же dc2
Вывод должен быть без ошибок.

----
Запретить запуск недоменных служб winbind, smbd, nmbd и krb5-kdc
```
sudo systemctl stop winbind smbd nmbd krb5-kdc  
sudo systemctl mask winbind smbd nmbd krb5-kdc
```

Разрешить запуск доменной службы samba-ad-dc :
```
sudo systemctl unmask samba-ad-dc  
sudo systemctl enable samba-ad-dc
```

Привести файл /etc/krb5.conf к виду :
```
`[libdefaults]`

    `default_realm = OFFICE.RU`

    `dns_lookup_realm = true`

    `dns_lookup_kdc = true`

`[realms]`

`OFFICE.LAN = {`

    `default_domain = office.ru`

`}`

`[domain_realm]`

    `dc = office.ru`
```

-----
Получить Kerberos-билет администратора домена : 
```
kinit Administrator
```
Удалить (переименовать) файл /etc/samba/smb.conf:
```
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.initial
```
----
Создать контролер домена:
```
sudo samba-tool domain join office.ru DC -U Administrator --realm=office.ru --dns-backend=BIND9_DLZ
```
Должен быть примерный вывод после этой команды :
```
Joined domain OFFICE (SID S-1-5-21-3226316390-2541821163-4149523140) as a DC
```
----
Настраиваем службу Bind9 для работы с DC :

Добавить файл настроек /var/lib/samba/bind-dns/named.conf в конфигурационный файл /etc/bind/named.conf:
```
echo 'include "/var/lib/samba/bind-dns/named.conf";' | sudo tee -a /etc/bind/named.conf
```
Дать права доступа :
```
sudo chown -R root:bind /var/lib/samba/bind-dns
```
Перезагрузить службу Bind9
```
sudo systemctl restart bind9
```
Задать **постоянную** настройку разрешения DNS. В файле /etc/resolv.conf теперь должен быть указан IP-адрес **резервного** контроллера:
```
nameserver 192.168.10.2
```
Перезагружаемся : 
```
sudo reboot
```
----
После перезагрузки проверяем настройки : 

Убедиться, что содержимое файла /etc/resolv.conf не изменилось. Если файл изменился - откорректировать настройку сети, и повторно перезагрузить компьютер.

```
sudo systemctl list-units --failed
```

Синхронизировать состояние контроллеров, выполнив репликацию данных 
С основного на резервный :

```
samba-tool drs replicate dc2.office.ru dc.office.ru dc=office,dc=ru -U Administrator
```
С резервного контроллера на основной:
```
samba-tool drs replicate dc.office.ru dc2.office.ru dc=office,dc=ru -U Administrator
```
Если все ок , будет сообщение Succesfull

----

Проверим работоспособность : 

```
sudo samba-tool user create testuser
```
На основном контроллере домена получить список пользователей, и убедиться, что учетная запись тестового пользователя появилась на основном контроллере:

```
sudo samba-tool user list
```

Должно сработать на выдачу пользователей. Если все ОК , работа выполнена!

-----
#samba #ACDD #Linux #DC #DevOps #Domain 