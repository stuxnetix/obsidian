Просмотреть размер логов журнала : 
```
journalctl --disk-usage
```

Если размер большой , чистим : 
```
sudo journalctl --vacuum-time=3d
```

