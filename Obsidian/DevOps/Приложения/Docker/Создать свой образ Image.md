Создание Image и загрузка в Docker Hub

As Example: 
1. Создаем приложение (example PHP)
2. Создаем для него Dockerfile (Apache + PHP + App)
3. Создаем Docker Image из нашего Dockerfile
4. Создаем Repository на Docker Hub
5. Загружаем наш образ в наш репозиторий на Докер хаб
6. Тестируем на образ на Докер хаб

Под Ubuntu как пример:

Файл php создаем
Пишем файл Dockerfile 
Создаем образ Docker build -t Mybuild
Зарегистрироваться на Докер Хабе и создать репозиторий
docker tag mybuild:latest nameDockerHub/nameRepository:latest - дать тэг образу (latest - версия)
docker login -> задать логин и пароль для репозитория 

docker push nameDockerhub/nameRepository:latest - загрузить образ в репозиторий Докер хаб

Если что то пошло не так после запуска docker run -d -rm -p 8080:80 nameDockerHub/nameRepository
В браузере набрать http://ip_server:8080 (error) 
docker ps - смотрим ID 
docker exec -it ID /bin/bash
tail -30 /var/log/httpd/error_log  

// Все вышеперечисленное в том случае если писали код вручную и забыли точку или запятую 