

Переименовать компьютер : 

```
Rename-Computer -NewName spb-dc02
```

----
Задать статический IP адрес :

```
Get-NetAdapter  
New-NetIPAddress -IPAddress 192.168.13.14 -DefaultGateway 192.168.13.1 -PrefixLength 24 -InterfaceIndex 6  
Set-DNSClientServerAddress -InterfaceIndex 6 -ServerAddresses ("127.0.0.1","192.168.10.14")
```

----
Дать серверу роль ACDD : 

```
Install-WindowsFeature AD-Domain-Services –IncludeManagementTools -Verbose
```

Проверить , что все работает : 

```
Get-WindowsFeature -Name *AD-Domain
```

----

Узнать где находится GC (Глобальный каталог) в ACDD : 

```
dsquery server –isgc_
```


Тут ищем серверы в домене contoso как пример:
```
dsquery server –domain contoso.com –isgc
```

Поиск серверов по всему лесу :

```
dsquery server –forest –isgc
```

Поиск по сайту Default : 

```
dsquery server –site Default-First-Site-Name
```

Получить информацию о новом DC : 

```
Get-ADDomainController -Identity "SPB-DC02"
```

Проверить состояние репликации между DC : 

```
repadmin /showrepl *  
repadmin /replsummary
```

Выполнить полную репликацию : 

```
repadmin /syncall
```

