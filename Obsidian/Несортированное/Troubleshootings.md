Если отвалилась почта , попробовать перезапустить службу :
```
service postfix restart
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

Удалить поды со статусом Error : 

```
for p in $(kubectl get pods --all-namespaces | grep Error | awk '{print $2}'); do kubectl delete pod $p --grace-period=0 --force;done
```

Время от времени нужно делать ребилд кластера и обновлять по новой! 


