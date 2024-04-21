ZFS - является 128-битной файловой системой. Максимальный размер пула составляет 16 экзабайт (18 млн терабайт). 

1. ZFS является self-healing системой. ZFS использует 256-битный контрольные суммы для проверки данных.
2. Позволяет добавлять пулы или же наращивать емкость пула без остановки системы или же приложений
3. Очень гибкая организация RAID'ов.

Основная ОС для этой файловой системы является [[Solaris OC]] (Солярис) 

##### Немного терминологии

  

- _checksum_ — Контрольная сумма. 256-разрядный хеш-код данных в блоке файловой системы.
- _clone_ — Клон. Файловая система, исходное содержимое которой идентично содержимому снапшота.
- _snapshot_ — Снимок. Образ файловой системы или тома в определенный момент времени, доступный только для чтения.
- _dataset_ — Набор данных. Общее название следующих объектов ZFS: клонов, файловых систем, снимков или томов. Каждый набор данных идентифицируется по уникальному имени в пространстве имен ZFS. Наборы данных определяются с помощью следующего формата: пул/путь[ @снимок]
- _pool_ — Пул. Логическая группа устройств, описывающая размещение и физические характеристики доступного пространства для хранения данных. Из пула берется пространство для наборов данных.
- _volume_ — Том. Набор данных, используемый для эмулирования физического устройства. Например, можно создать том ZFS в качестве устройства подкачки.


В Solaris работа с ZFS идет в основном через 2 команды. Это zpool и zfs.  
  
zpool — работа с пулами. Их создание, изменение, удаление и т.д.  
  
zfs — работа с самой файловой системой.  
  
Итак, из чего же можно создать пул? Да из чего угодно. От файлов под другой фс, до дисков в дисковом массиве.  
  

###### Примеры создания пула MyPool

  
### Создание пула из файлов /zfs1/disk01 и /zfs1/disk02 созданных командой mkfile

## Создание пула используя обычный слайс  
`zpool create MyPool c1t0d0s0`  
  
###  Создание пула c зеркалированием и использованием spare дисков.  
`zpool create MyPool mirror c1t0d0 c2t0d0 mirror c1t0d1 c2t0d1`  
`zpool create MyPool mirror c1t0d0 c2t0d0 spare c3t0d0`  
  
### Список ваших пулов и их статус можно посмотреть с помощью команда  
`# zpool list   NAME SIZE USED AVAIL CAP HEALTH ALTROOT   MyPool 191M 94K 191M 0% ONLINE -`  
  
`zpool status -v   pool: MyPool   state: ONLINE   scrub: none requested   config:      NAME STATE READ WRITE CKSUM   MyPool ONLINE 0 0 0   /disk1 ONLINE 0 0 0   /disk2 ONLINE 0 0 0`  
  
### Статусы могут быть следующие  

- DEGRADED — Один или несколько устройства верхнего уровня находятся в нерабочем состоянии, т.к. были отключены. Но дальнейшее функционирование возможно.
- FAULTED — Один или несколько устройства верхнего уровня находятся в состоянии FAULT. Дальнейшее функционирование не возможно.
- OFFLINE — Пул был отключен командой «zpool offline».
- ONLINE — Пул находится в состоянии ONLINE и нормально функционирует.
- REMOVED — Устройство (Пул) было физически удалено при работающей системе.
- UNAVAIL — Устройство (Пул) недоступно.

  
  

#### Изменение пула MyPool

  
  
Добавление и удаление элементов (дисков) из пула, не использующего зеркалирование осуществляется с помощью команд:  
`zpool add MyPool /zfs1/disk3`  
`zpool remove MyPool /disk3`  
  
Добавление и удаление элементов (дисков) из пула, c зеркалированиеv осуществляется с помощью команд:  
`zpool attach MyPool /disk1 /disk3`  
`zpool detach MyPool /disk1 /disk3`  
  
Ввод элемента в состояние offline с невозможностью чтения/записи до ввода в состояние online (используя ключ -t можно ввести элемент в состояние offline только до ребута)  
`zpool offline MyPool /disk1`  
  
Как это отобразится на Пуле в целом:  
`zpool status -v   pool: myzfs   state: DEGRADED   status: One or more devices has been taken offline by the administrator.   Sufficient replicas exist for the pool to continue functioning   in a degraded state.   action: Online the device using 'zpool online' or replace the device   with 'zpool replace'.   scrub: resilver completed with 0 errors on Tue Sep 11 13:39:25 2007   config:      NAME STATE READ WRITE CKSUM   myzfs DEGRADED 0 0 0   mirror DEGRADED 0 0 0   /disk1 OFFLINE 0 0 0   /disk2 ONLINE 0 0 0`  
  
И обратно online  
`zpool online MyPool /disk1`  
  
Замена одного элемента пула на другой  
`zpool replace MyPool /disk1 /disk3`  
  

###### Создание и работа с файловой системой

  
  
Создаем файловую систему  
`zfs create MyPool/systemname`  
  
опцией -о мы можем выбрать точку монтирования, отличную от /MyPool/systemname  
  
Резервируем место под свою файловую систему  
`zfs set reservation=20m MyPool/systemname`  
  
Устанавливаем квоту для вашей файловой системы, которую вы не сможете превысить  
`zfs set quota=20m MyPool/systemname`  
  
Включаем  компрессию.  
`zfs set compression=on MyPool/systemname`  
  
Расшариваем  нашу файловую систему.  
`zfs set sharenfs=on MyPool/systemname` в NFS  
`zfs set sharesmb=on MyPool/systemname` в SMB  
  
Снимаем снапшот  
`zfs snapshot MyPool/systemname@hostname`  
  
Так как снапшот — это просто снимок системы в данный момент, то иногда стоит создавать клона.  
`zfs clone MyPool/systemname@hostnameyPool/systemname2`  
  
Удаляем файловую систему. Если существуют файловые системы-наследники, надо использовать ключ -f  
`zfs destroy MyPool/systemname`
