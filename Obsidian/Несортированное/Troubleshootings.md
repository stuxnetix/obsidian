Если отвалилась почта , попробовать перезапустить службу :
```
service postfix restart
```

Обновить chrome на поде , если ругается на ошибку версий : 
```
apt-get update
apt-get --only-upgrade install google-chrome-stable
```

```
kubectl -n proxy-chrome get pods -l app=proxy-chrome-admin-api -o name | xargs -I{} kubectl -n proxy-chrome exec {} -- cat alexAdminApi.log >> alex-admin-api_pods.logs
```

Удалить все поды со статусом Terminating : 
```
for p in $(kubectl get pods --all-namespaces | grep Terminating | awk '{print $2}'); do kubectl delete pod $p --grace-period=0 --force;done
```

Удалить поды со статусом Unknown : 
```
for p in $(kubectl get pods --all-namespaces | grep ContainerStatusUnknown | awk '{print $2}'); do kubectl delete pod $p --grace-period=0 --force;done
```

Удалить поды под статусом Error : 

```
for p in $(kubectl get pods --all-namespaces | grep CrashLoopBackOff | awk '{print $2}'); do kubectl delete pod $p --grace-period=0 --force;done
```

Время от времени нужно делать ребилд кластера и обновлять по новой! 


