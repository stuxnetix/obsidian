Если Inactice (отключен) , можно включить :
```
sudo ufw enable
```
Флаг disable отключит файрвол

Заблокировать IP или подсеть в конце IP добавить  /24
```
sudo ufw deny from 91.198.174.190
```
Блокировка по сетевому интерфейсу :
```
sudo ufw deny in on eth0 from 91.198.174.192
```
Флаг allow (разрешить) работает в обратном случае в вышеперечисленных командах.

Удалить ранее разрешенные или заблокированные правила файрвола:
```
sudo ufw delete allow from 91.198.174.192
```
Можно удалять по ID правилам :
```
sudo ufw status numbered
```
Посмотреть список доступных профилей приложений :
```
sudo ufw app list
```
Что бы включить профиль приложения в разрешенный доступ :
```
sudo ufw allow Name
```
Если вы хотите разрешить только HTTPS-запросы к вашему веб-серверу, вам нужно сначала включить наиболее ограничивающее правило, которым в данном случае будет «Nginx HTTPS», а затем отключить активное правило «Nginx Full»:

```
sudo ufw allow "Nginx HTTPS" 
sudo ufw delete allow "Nginx Full"
```
