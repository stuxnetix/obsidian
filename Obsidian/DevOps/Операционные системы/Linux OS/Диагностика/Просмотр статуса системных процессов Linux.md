Просмотреть все статусы служб в каком они состоянии : 

```
systemctl list-units
```

Просмотр определенной службы , ее статус : 

```
systemctl status postgresql
```

Перезапуск службы :

```
systemctl restart postgresql
```

Перезапуск конфигураций определенной службы :

```
systemctl reload postgresql
```


Просмотр зависимостей службы или приложения : 

```
systemctl list-dependencies postgresql
```

Проверка свойств модуля : 

```
systemctl show postgresql
```

Все о процессе : 

```
sudo systemctl status 'postgresql*'
```

