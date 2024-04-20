

```
sudo mkdir -p /srv/gitlab
```


```
export GITLAB_HOME=/srv/gitlab
```



```
sudo docker run --detach \
  --hostname gitlab.localhost.com \
  --env GITLAB_OMNIBUS_CONFIG="external_url 'http://gitlab.localhost.com:8929'; gitlab_rails['gitlab_shell_ssh_port'] = 2424" \
  --publish 8929:8929 --publish 2424:2424 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ce
```

```
docker run -d -p 2222:22 --name gitlab gitlab/gitlab-ce
```

```
sudo docker run --detach \
  --hostname gitlab.example.com \
  --env GITLAB_OMNIBUS_CONFIG="external_url 'http://gitlab.localhost.com'" \
  --publish 198.51.100.1:443:443 \
  --publish 198.51.100.1:80:80 \
  --publish 198.51.100.1:22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ce
```
