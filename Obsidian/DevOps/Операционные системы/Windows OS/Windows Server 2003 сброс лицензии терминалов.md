**Решение проблемы:**

1. Загрузть ПК в безопасном режиме;
    1. Удалить файл **TLSLic.edb** в директории **C:\windows\system32\lsserver\**.

При удалении файла **TLSLic.edb** временные лицензии выдаются заново.


Fix2:  создать .sh скрипт и вписать туда:

```
rem скрипт сброса триального периода на терминальном сервере на 90 дней.  
net stop TermServLicensing  
xcopy %windir%"\system32\LServer" %windir%"\system32\LServer.old" /i /h /y  
rmdir %windir%"\system32\LServer" /s /q  
MD %windir%"\system32\LServer"  
reg export HKLM\SoftWare\MicroSoft\MSLicensing TServerOlg.reg  
reg delete HKLM\SoftWare\MicroSoft\MSLicensing /f  
net start TermServLicensing  
pause
```

Если не пускает , то при загрузке пытаемся дать команду в консоли mstsc /admin

Если сообщение продолжает появляться, на клиентской машине удаляем из реестра ветку HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSLicensing и перезагружаемся

Если это не помогает , экспортируем ветку MSLicensing с компа у которого все хорошо.

Попробовать изменить тип выдаваемой лицензии с устройств , на пользователя и наоборот. 

Еще вариант сброса лицензии: 
```
net stop termservlicensing 
del %SystemRoot%\system32\LServer\. /Q 
net start termservlicensing
```




#терминал #server #windows #server2003
