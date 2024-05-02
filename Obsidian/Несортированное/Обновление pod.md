
	Хорошо. Работу с этим проектом мы отложим на потом. Сейчас мне нужна твоя помощь в обновлении pod в kubernetes. В моем кластере несколько pod завершаются с ошибкой. Я обновила код приложения, теперь мне нужны обновить эти pod


В Kubernetes, вы можете обновить pods, выполнив обновление соответствующего Deployment, это приведет к перезапуску всех связанных pods. Вот иллюстративный шаг.



Обновите image в Deployment . Это можно сделать используя команду kubectl set image:

~~~bash

kubectl set image deployment/<your-deployment-name> <container-name>=<new-image:tag>
~~~

your-deployment-name - имя Deployment, который вы хотите обновить.

container-name - имя контейнера в pod-ах, которое вы хотите обновить.

<new-image:tag> - новый образ Docker, который вы хотите использовать.


Если вы обновляете образ в Docker registry без изменения тегов, вам, возможно, придется выполнить принудительное обновление с помощью следующей команды:

~~~bash
kubectl rollout restart deployment/<your-deployment-name>
~~~

Проверьте статус обновления с помощью команды:

~~~bash
kubectl rollout status deployment/<your-deployment-name>
~~~

Выполнив эти действия, Kubernetes начнет обновлять ваш Deployment, заменяя старые pods новыми по одному или по несколько за раз, в зависимости от вашей стратегии обновления.


# Мой новый код для этих pod ешё не собран в образ. Как мне это сделать


Чтобы собрать новый Docker образ из вашего исходного кода, воспользуйтесь следующими шагами:



Перейдите в каталог, где хранится ваш Dockerfile.



Соберите образ с использованием следующей команды:

~~~bash
docker build -t your_username/your_image_name:tag .
~~~

your_username - это имя вашего Docker Hub пользователя.

your_image_name - это имя вашего образа.

tag это тег вашего образа, обычно используется latest для последнего обновления.



После успешной сборки проверьте образ, используя команду docker images. Образ должен быть в списке.



Затем загрузите образ в Docker Hub или в ваш приватный регистр с помощью следующей команды:


~~~bash
docker push your_username/your_image_name:tag
~~~
Теперь ваш образ доступен для использования на любой машине с Docker или в любом кластере Kubernetes.


Запомните, важно, чтобы Dockerfile был настроен правильно для ваших целей.



```bash
kubectl get deployments
```




~~~bash
kubectl set image deployment/my-deployment container1=new-image1:version container2=new-image2:version
~~~






~~~bash
kubectl set image deployment/proxy-chrome chrome-proxy=sheppard92/chrome-proxy:latest proxy-chrome-agent=sheppard92/chrome-agent:latest
~~~


```bash

cd integration/

 git checkout

git branch

git pull origin main

sheppard@NORMANDY:~/integration$ git pull origin main
Warning: Permanently added 'gitlab.afo.spb.ru' (ED25519) to the list of known hosts.
Из gitlab.afo.spb.ru:data-mining/integration
 * branch            main       -> FETCH_HEAD
Уже актуально.


git pull origin dev

sheppard@NORMANDY:~/integration$ git pull origin dev
Warning: Permanently added 'gitlab.afo.spb.ru' (ED25519) to the list of known hosts.
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 16 (delta 6), reused 16 (delta 6), pack-reused 0
Распаковка объектов: 100% (16/16), 2.69 КиБ | 550.00 КиБ/с, готово.
Из gitlab.afo.spb.ru:data-mining/integration
 * branch            dev        -> FETCH_HEAD
 * [новая ветка]     dev        -> origin/dev
подсказка: You have divergent branches and need to specify how to reconcile them.
подсказка: You can do so by running one of the following commands sometime before
подсказка: your next pull:
подсказка: 
подсказка:   git config pull.rebase false  # merge (the default strategy)
подсказка:   git config pull.rebase true   # rebase
подсказка:   git config pull.ff only       # fast-forward only
подсказка: 
подсказка: You can replace "git config" with "git config --global" to set a default
подсказка: preference for all repositories. You can also pass --rebase, --no-rebase,
подсказка: or --ff-only on the command line to override the configured default per
подсказка: invocation.


git config pull.rebase false

sheppard@NORMANDY:~/integration$ git config pull.rebase false
sheppard@NORMANDY:~/integration$ git pull origin dev
Warning: Permanently added 'gitlab.afo.spb.ru' (ED25519) to the list of known hosts.
Из gitlab.afo.spb.ru:data-mining/integration
 * branch            dev        -> FETCH_HEAD
Merge made by the 'ort' strategy.
 pom.xml                                            |  6 +-
 .../komus/routes/KomusRouteProductsFull.java       |  4 +-
 .../routes/MinorSuppliersRouteProductsBatch.java   | 90 ++++++++++++----------
 .../routes/MinorSuppliersRouteProductsSingle.java  | 78 +++++++++++--------
 4 files changed, 103 insertions(+), 75 deletions(-)



git push

sheppard@NORMANDY:~/integration$ git push
Warning: Permanently added 'gitlab.afo.spb.ru' (ED25519) to the list of known hosts.
Перечисление объектов: 10, готово.
Подсчет объектов: 100% (10/10), готово.
При сжатии изменений используется до 16 потоков
Сжатие объектов: 100% (4/4), готово.
Запись объектов: 100% (4/4), 469 байтов | 469.00 КиБ/с, готово.
Всего 4 (изменений 2), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
To gitlab.afo.spb.ru:data-mining/integration.git
   9dbcec9..fa9f76a  main -> main


```


```bash


docker build -t sheppard92/integration:prod1.1 .

docker images

docker push sheppard92/integration:prod1.1

kubectl get deployment

kubectl set image deployment/integration integration=sheppard92/integration:prod1.1


sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new -p registry.afo.spb.ru/k.caesarenko/integration/sheppard92/integration:new   
"docker tag" requires exactly 2 arguments.
See 'docker tag --help'.

Usage:  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new registry.afo.spb.ru/k.caesarenko/integration/sheppard92/integration:new
sheppard@NORMANDY:~/integration$ docker push registry.afo.spb.ru/k.caesarenko/integration/sheppard92/integration:new
The push refers to repository [registry.afo.spb.ru/k.caesarenko/integration/sheppard92/integration]
2c836b38f209: Preparing 
19f1441b1598: Preparing 
b9c5f89efe24: Preparing 
48611df389be: Preparing 
34f7184834b2: Preparing 
5836ece05bfd: Waiting 
72e830a4dff5: Waiting 
denied: requested access to the resource is denied
sheppard@NORMANDY:~/integration$ docker login registry.afo.spb.ru
Authenticating with existing credentials...
Login Succeeded
sheppard@NORMANDY:~/integration$ docker push registry.afo.spb.ru/k.caesarenko/integration/sheppard92/integration:new
The push refers to repository [registry.afo.spb.ru/k.caesarenko/integration/sheppard92/integration]
2c836b38f209: Preparing 
19f1441b1598: Preparing 
b9c5f89efe24: Preparing 
48611df389be: Preparing 
34f7184834b2: Preparing 
5836ece05bfd: Waiting 
72e830a4dff5: Waiting 
denied: requested access to the resource is denied
sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new registry.afo.spb.ru/k.caesarenko/data_mining/integration/sheppard92/integration:newation/sheppard92/integration:n
sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new registry.afo.spb.ru/k.caesarenko/data_mining/integration/sheppard92/integration:new
sheppard@NORMANDY:~/integration$ docker push registry.afo.spb.ru/k.caesarenko/data_mining/integration/sheppard92/integration:new
The push refers to repository [registry.afo.spb.ru/k.caesarenko/data_mining/integration/sheppard92/integration]
2c836b38f209: Preparing 
19f1441b1598: Preparing 
b9c5f89efe24: Preparing 
48611df389be: Preparing 
34f7184834b2: Preparing 
5836ece05bfd: Waiting 
72e830a4dff5: Waiting 
denied: requested access to the resource is denied
sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new registry.afo.spb.ru/k.caesarenko/data-mining/integration/sheppard92/integration:new
sheppard@NORMANDY:~/integration$ docker push registry.afo.spb.ru/k.caesarenko/data-mining/integration/sheppa
rd92/integration:new
The push refers to repository [registry.afo.spb.ru/k.caesarenko/data-mining/integration/sheppard92/integration]
2c836b38f209: Preparing 
19f1441b1598: Preparing 
b9c5f89efe24: Preparing 
48611df389be: Preparing 
34f7184834b2: Preparing 
5836ece05bfd: Waiting 
72e830a4dff5: Waiting 
denied: requested access to the resource is denied
sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new registry.afo.spb.ru/data-mining/integration:sheppard92/integration:new
Error parsing reference: "registry.afo.spb.ru/data-mining/integration:sheppard92/integration:new" is not a valid repository/tag: invalid reference format
sheppard@NORMANDY:~/integration$ docker tag sheppard92/integration:new registry.afo.spb.ru/data-mining/integration:sheppard92-integration-new
sheppard@NORMANDY:~/integration$ docker push registry.afo.spb.ru/data-mining/integration:sheppard92-integration-new
The push refers to repository [registry.afo.spb.ru/data-mining/integration]
2c836b38f209: Pushed 
19f1441b1598: Pushed 
b9c5f89efe24: Pushed 
48611df389be: Pushed 
34f7184834b2: Layer already exists 
5836ece05bfd: Layer already exists 
72e830a4dff5: Layer already exists 
sheppard92-integration-new: digest: sha256:2e0872807fbac68812c51fc0dd635ba92d89f18a04b1415075c6db4c7d59e360 size: 1787
sheppard@NORMANDY:~/integration$ kubectl set image deployment/integration integration=registry.afo.spb.ru/data-mining/integration:sheppard92-integration-new
deployment.apps/integration image updated
sheppard@NORMANDY:~/integr
sheppard@NORMANDY:~/integr
sheppard@NORMANDY:~/integr
sheppard@NORMANDY:~/integration$ 





```


```bash

Удаление по статусу
kubectl get pods -A --no-headers | grep 'ContainerStatusUnknown' | awk '{print $2 " -n " $1}' | while read -r pod; do kubectl delete pod $pod --grace-period=0 --force; done
```

