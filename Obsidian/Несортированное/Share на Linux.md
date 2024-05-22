Устанавливаем самбу :

```
sudo apt install samba smbclient
```

Создаем файл конфигурации , пока пустой , настроим позже : 

```
sudo touch /etc/samba/smb.conf
```

Задаем параметр без пароля , должно быть имя такое же как и текущий пользователь :

```
sudo smbpasswd -a -n $USER
```

Создаем папку share : 

```
mkdir imgproject
```

Открываем папку с конфигом : 

```
sudo nano /etc/samba/smb.conf
```

