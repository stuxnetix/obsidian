Решение проблем с выходом в интернет и ошибкой : 
```
temporary failure in name resolution 
```

Можно настроить статику или оставить DHCP в файле : 

/etc/netplan/00-  or 50- yaml 

Необязательно.

----

Решение : 

```
sudo rm /etc/resolv.conf
```

```
sudo touch /etc/resolv.conf
```

```
sudo nano /etc/resolv.conf
```

Записываем туда строку : 

```
nameserver 1.1.1.1
```

cntrl+s и выходим , перезапускаем службу :

```
sudo systemctl restart systemd-resolved.service
```

Проверяем : 

```
ping google.com
```

Если работает , то хорошо , если нет , как вариант перезагрузить систему. 

