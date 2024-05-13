
Установка Prometheus в K8S

Добавляем репозиторий разработчика после установки : 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
```
helm repo update
```

Просмотреть доступные пакеты : 
```
helm search repo prometheus-community
```

Нужен пакет : **prometheus-community/kube-prometheus-stack**


Редактируем конфиг Alertmanager : 

```
 nano helm/kube-prometheus-stack/values.yml
```

Там можно настроить виды оповещений , например через Телеграмм или на почту: 

```
alertmanager:
  config:
	global:
  	telegram_api_url: "https://api.telegram.org"
	route:
  	receiver: telegram-alert
  	group_by: ['alertname', 'cluster', 'service']
  	repeat_interval: 1h
  	routes:
    	- match:
        	severity: critical
      	receiver: telegram-alert
      	continue: true
	receivers:
  	- name: telegram-alert
    	telegram_configs:
      	- chat_id: <ChatID>
        	bot_token: <BotToken>
        	api_url: "https://api.telegram.org"
        	send_resolved: true
        	parse_mode: html
        	template: |-
         	{{ define "telegram.default" }}
         	{{ range .Alerts }}
         	{{ if eq .Status "firing"}}&#x1F525<b>{{ .Status | toUpper }}</b>&#x1F525{{ else }}&#x2705<b>{{ .Status | toUpper }}</b>&#x2705{{ end }}
         	<b>{{ .Labels.alertname }}</b>
         	{{- if .Labels.severity }}
         	<b>Severity:</b> {{ .Labels.severity }}
         	{{- end }}
         	{{- if .Labels.cluster }}
         	<b>Cluster:</b> {{ .Labels.cluster }}
         	{{- end }}
         	{{- if .Labels.service }}
         	<b>Service:</b> {{ .Labels.service }}
         	{{- end }}
         	{{- if .Labels.instance}}
         	<b>Instance:</b> {{ .Labels.instance }}
         	{{- end }}
         	<b>Description:</b> {{ .Annotations.description }}
         	{{- end }}

```

----

Создаем в кластере namespace monitoring : 
```
kubectl create namespace monitoring
```

Устанавливаем туда Prometheus : 

```
helm install kube-prom-stack prometheus-community/kube-prometheus-stack -n monitoring -f values.yaml
```

Проверить статус после установки : 

```
helm status <app-name>
```

Проверить , что все поды работают : 
```
kubectl --namespace monitoring get pods -l "release=kube-prom-stack"
```

Идем в настройки Ingress , для того что бы дать Grafana доступ извне : 

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: kube-state-ingress
  namespace: monitoring
  labels:
	name: kube-state-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: grafana.selectel.site
	http:
  	paths:
  	- pathType: Prefix
    	path: "/"
    	backend:
      	service:
        	name: kube-prom-stack-grafana
        	port:
          	number: 80
```

После редактирования , применяем манифест :
```
kubectl apply -f ingress.yaml
```

После применения манифеста , можно зайти в Grafana , через браузер по доменному имени или IP : 

```
https://grafana.office.ru
```

----

Для того, чтобы использовать доменные имена, вы должны настроить DNS-записи для указанных доменных имен, чтобы они указывали на IP-адрес вашего кластера Kubernetes.

Пробросить порт на локальном компе с графаны на порт 8080 : 

```
pod=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=prometheus" -o jsonpath="{.items[0].metadata.name}") && kubectl port-forward ${pod} 8080:9090 -n monitoring
```

Можно открывать теперь графану по localhost:8080

Для Alertmanagers : 

```
pod=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=alertmanager" -o jsonpath="{.items[0].metadata.name}") && kubectl port-forward ${pod} 8080:9093 -n monitoring
```

