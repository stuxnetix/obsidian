Для этого существует базовая утилита **dcdiag** 

Что бы быстро проверить состояние конкретного контроллера домена запустите команду в Powershell :

```
dcdiag /s:DC01
```

Команда отработает базовые тесты по домену и вернет либо False (провал) , либо Passed (проверено) 

----
### Типовые тесты :  

Advertising – проверяет роли и сервисы, опубликованные на DC

FRSEvent – проверяет наличие ошибок в службе репликации файлов (ошибки репликации SYSVOL)

FSMOCheck – проверяет, что DC может подключиться к KDC, PDC, серверу глобального каталога

MachineAccount — проверяет корректность регистрации учетной записи DC в AD, корректность доверительных отношения с доменом

NetLogons – проверка наличие прав на выполнение репликации;

Replications – проверка статуса репликации между контроллерами домена и наличие ошибок;
KnowsOfRoleHolders – проверяет доступность контроллеров домена с ролями FSMO

Services – проверяет, запущены ли на контроллере домена необходимые службы

Systemlog – проверяет наличие ошибок в журналах DC

----

#### Можно отдельно запустить дополнительные проверки домена :

**Topology** – проверяет, что KCC сгенерировал полную топологию для всех DC

**CheckSecurityError**

**CutoffServers** – находит DC, который не получает репликацию из-за того, что партнёр недоступен

**DNS** – доступны 6 проверок службы DNS **(/DnsBasic, /DnsForwarders,** **/DnsDelegation,** **/DnsDymanicUpdate,** **/DnsRecordRegistration,** **/DnsResolveExtName)**

**OutboundSecureChannels**

**VerifyReplicas** – проверяет корректность репликации разделов приложения

**VerifyEnterpriseReferences**

------

Что бы как пример , проверить корректность работы DNS на всех доменах , используем команду :

```
dcdiag.exe /s:DC01 /test:dns /e /v
```

Выйдет таблица с результатами , если будет Fail на каком то пункте в таблице , его можно отдельно проверить.
Тут ошибка была в тесте Forwarders как пример , проверяем ее :

```
dcdiag.exe /s:DC01 /test:dns /DnsForwarders /v
```

Можно провести расширенную диагностику и результаты вывести в лог файл :

```
dcdiag /s:DC01 /v >> c:\ps\dc01_dcdiag_test.log
```

Следующая команда позволяет вывести информацию только о пройденных тестах :

```
Dcdiag /s:DC01 | select-string -pattern '\. (.*) \b(passed|failed)\b test (.*)'
```

Что бы получить состояние всех контроллеров домена , запускаем :

```
dcdiag.exe /s:winitpro.ru /a
```

Если вывести только ошибки запускаем :

```
dcdiag.exe /s:dc01 /q
```

Можно попробовать исправить ошибки с помощью команды :

```
dcdiag.exe /s:dc01 /fix
```

-----

## Проверка ошибок репликации между контроллерами домена Active Directory

Для проверки репликации используется команда **repadmin

Базовая команда проверки статуса репликации :

```
repadmin /replsum
```

 В идеальном случае значение **largest delta** не должно превышать 1 час (зависит от топологии и настроек частоты межсайтовых репликаций), а количество ошибок = 0

Проверка всех DC в домене :

```
repadmin /replsum *
```

Проверка межсайтовой репликации : 

```
repadmin /showism
```

Просмотр топологии и найденных ошибок :

```
repadmin /showrepl
```

Данная команда проверит DC и вернет время последней успешной репликации для каждого раздела каталога (_last attempt xxxx was successful_)

----

Для вывода расширенной информации :

```
repadmin /showrepl *
```

----

Для запуска репликации паролей с обычного контроллера домена на контроллер домена на чтение (RODC) используется ключ **/rodcpwdrepl** 

```
repadmin /rodcpwdrepl
```

----
Опция /replicate позволяет запустить немедленную репликацию указанного раздела каталога на определенный DC.

```
repadmin /replicate
```

----
Для запуска синхронизации указанного DC со всеми партнерами по репликации, используйте команду : 

```
replmon /syncall <nameDC>
```

Просмотр очереди репликации : 

```
repadmin /queue
```

В идеальном случае , очередь должна быть пуста! 

----

Проверить время создания последней версии бэкапа контроллера текущего домена :

```
Repadmin /showbackup *
```

Еще как вариант проверить репликацию и вывести в отдельный журнал :

**Powershell**
```
Get-ADReplicationPartnerMetadata -Target * -Partition * | Select-Object Server,Partition,Partner,ConsecutiveReplicationFailures,LastReplicationSuccess,LastRepicationResult | Out-GridView
```

----

 #### С помощью команды Get-Service можно проверить состояние типовых служб:

- Active Directory Domain Services (ntds)

- Active Directory Web Services (adws) – именно к этой службе подключаются все командлеты из модуля AD PowerShell

- DNS (dnscache и dns)

- Kerberos Key Distribution Center (kdc)

- Windows Time Service (w32time)

- NetLogon (netlogon)

```
Get-Service -name ntds,adws,dns,dnscache,kdc,w32time,netlogon -ComputerName dc03
```

----

#ActiveDirectory #ACDD #Powershell #Windows #Server #CMD #DevOps #Admin
