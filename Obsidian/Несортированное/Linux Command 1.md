export NEW_VARIABLE='test' 
Создаем переменную для NEW_VARIABLE , будет называться test

source ~/.bashrc  сохраняет файл с переменными и перезагружает его

Переменные для текущего пользователя : 
```
echo "export VAR="value"" >> ~/.bashrc && source ~/.bashrc
```

Для Login оболочек :
```
echo "export VAR="value"" >> ~/.bash_profile && source ~/.bash_profile
```


Установка переменных для всех пользователей Линукс
```
echo "export VAR="value"" >> /etc/environment && source /etc/environment
```

Чтобы удалить переменную , есть команда unset или set -n

```
unset TEST_V
```

