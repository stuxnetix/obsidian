Перезапустить K8S : 

```
sudo systemctl restart kubelet
```

Проверить статус после перезагрузки : 

```
sudo systemctl status kubelet
```

Если не помогло и все равно есть ошибки , перезапускаем Docker :

```
sudo systemctl restart docker
```

----
Чтобы грамотно перезагрузить Worker Node в К8S нужно сначала вывести систему за кордон , убедится что K8S в этот момент не запускает новые поды и только потом перезагрузить ОС :

```
kubectl cordon Node_Name
```

```
sudo reboot
```

После перезагрузки вывести систему из кордона : 

```
kubectl uncordon Node_Name
```

----

Если речь идет о перезапуске подов , имеет смысл установить кол-во реплик на 0 , а после этого увеличить до исходного состояния : 

```
kubectl scale deployments/nginx-server --replicas=0
```

Или более грамотный вариант , каждый под будет завершаться по очереди и сразу перезапускаться  : 

```
kubectl rollout restart deployment [deployment_name] -n [namespace]
```

----

