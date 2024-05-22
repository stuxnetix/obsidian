Если был забыт пароль на базе Postgres от пользователя postgres 

Останавливаем БД : 
```
sudo systemctl stop postgresql
```

В настройках файла pg_hba.conf нужно дать параметр trust 

```
local all postgres trust  
host all postgres 0.0.0.0/0 trust  
host all all 127.0.0.0/8 trust
```

Перезагружаем конфигурации и запускаем postgresql : 

```
systemctl daemon-reload
```

```
sudo systemctl start postgresql
```

Можно входить в psql и настраиваем пароль , либо через запрос в БД : 

```
ALTER ROLE postgres WITH PASSWORD '123';
```

либо через :

```
psql \password
```

