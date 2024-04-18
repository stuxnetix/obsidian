docker run "ununtu" or alpine , mysql как пример - создает контейнер , docker run ubuntu:20.04 (любой тег , версию после знака : , скачает и запустит)
Параметр после запуска ls - лист директорий в образе (docker run ubuntu ls)
docker run -d (detouch mode , background , фоновый режим) ubuntu sleep 900 (работать будет 900 секунд)
docker images - посмотреть все доступные образы для запуска
docker start "ubuntu" or any - запускает контейнер
docker rm "container id - удалить по id буквам a8e or a8e b32 sd3 например" или по Names
docker ps -a - показать все процессы образов
docker pause ID or Name процесса - процесс на паузу , unpause - отключить
docker kill ID ora Name - убить процесс 
docker run --rm -d ubuntu sleep 900 - процесс само удалиться после тайминга или после kill
docker run --rm -d nginx - сервер для просмотра портов (в том случае если не знаешь , что да как в портах)
docker inspect ID or Names - показать инфу о контейнере , что да как (масскил)
docker stats ID or Names - инфа по ресурсам , сколько потребляет процесс
docker run --name My name ununtu echo "Hello World" - дать свое имя процессу и вывести Hello World
docker logs ID or names - логи контейнера , например nginx 
docker logs -f ID - в лайв режиме логи смотреть
docker exec -it ID or name /bin/bash or mkdir Test , rm Test , cat or nano Test.txt запустить команду в контейнере или выполнить команду , файл в образе
Удалится все после остановки контейнера , не везде разрешается что либо запускать.
docker rmi ubuntu - удалить 
docker system prune -a --volumes - удалить все контейнеры разом (сначала нужно все остановить перед удалением)
docker stop IDs - остановить все контейнеры 
docker run -p80:80 nginx - порт пробрасывается к nginx , можно открыть порт на внешку , дубликат -p 8080:80 nginx , юзер подключается к 8080 , который перебрасывает на 80 порт (проброс)
Если будет создать еще один контейнер дубликат , будет ошибка , нужно будет удалить вручную - статус Created
docker run "-e(environment -переменная )" --name DB-mysql -e MYSQL_ROOT_PASSWORD="password" -d mysql - база данных Mysql , создать с паролем и базой , после подсоединится и дать команду env , что бы осмотреться , там же можно
задать команду mysql -p , чтобы подключится к БД (ввести пароль)
docker run -v /opt/mysql_data:/var/lib/mysq mysql - монтировать физ данные БД в докер
docker run -v /opt/app_conf:/etc/app/configs app - монтировать настройки приложений в докер
docker run -v /var/lib/mysql mysql - монтировать по хешу в докер БД 
docker run -v /etc/app/configs app - монтировать по хешу в докер конф приложения
docker run -v mysql_data:/var/lib/mysql mysql - именные сохранения в докер БД
docker run -v app_conf(любое имя):/etc/app/configs app именные сохранения конф приложений
docker volume ls - посмотреть volume (-v or volume , это монтирование чего либо)
создать в папке opt -> mkdir data /-> vi index.htm -> docker run nginx --name web1 -p 80:80 -v /opt/data/index.htmp:/nginx..etc (смонтирует 1 часть во вторую)
cd var/lib/docker/volumes - папка с докер контейнерами , содержимым , если монтировать по хешу , будет хеш папка , если по имени , то именная , там же можно изменять что либо
docker volume create "names" создать volume с именем
docker volume rm name (удалить volume)
Volumes - это папки которые позволяют сохранять настройки и любые файлы приложения , для монтирования и т.п при обновлении , обязательно создавать именные
Тип сети Bridge (по умолчанию 172.17.0.0/16 имеют доступ к сети (-p 80:80 это Bridge) , Host (--network=host , serverIP 10.10.10.15:80 or --network=none для блока внешки и локально , только зайти с админа
docker network create --drive bridge NAME - создать свою сеть Докер DNS в Bridge сегменте , docker run --net NAME nginx - nginx в сети Name 
docker network --drive host NAME -drive None - контейнеры не общаются между собой
macvlan & ipvlan (1) свитч тема , контейнеры свои мак адреса имеют и IP , (2) все то же самое , но мак адреса те же что и мак сервера , вся разница. 
docker network ls - типы сетей в докере
driver host - больше 1 сети нельзя создавать!
Driver null - больше 1 сети нельзя создавать!
docker network inspect Name - посмотреть о сети инфу
docker network create -d bridge --subnet 192.168.10.0/24 --gateway 192.168.10.1 NAME - своя подсеть со своим диапазоном , 
docker network rm NAME NAME удалить сеть или сети
docker run -it --name NAME netshoot /bin/bash - сразу запустить программу в интерактивном режиме с консолью
Netshoot -> docker inspect ID or Name - инфа о контейнере сеть
docker run -name nginx --net MyNET - запустить контейнер в сети имя , ping по имени в одной сети тоже возможен ping nginx2 or ping mysql
docker network connect MyNET01 Nginx , подключить контейнер к сети MyNet01 , будет пинговаться
docker network disconnect ID сети nginx , отключиться от сети и контейнера
docker network create -d macvlan --subnet 192.168.100.0/24 --gateway 192.168.100.1 --ip-range 192.168.100.99/32 -o parent=ens18 NAME01 - cоздать сеть для локалки с определенной сетевой картой и определенным диапазоном сети.
MacVlan может быть только 1 для 1 интерфейса
docker run --rm -it --name con2 --ip 10.10.10.205 --net MyWlan Netshoot - запустить контейнер с определенным IP
