# Grafana-Prometheus-VictoriaMetrics
## Описание
ПО: VirtualBox, Docker + Docker Compose, Grafana, Prometheus; ОС: OracleLinux.
Итог: ?
## Start
Установка гостевых дополнений
(за подробности обращаемся к файлу

![изображение](https://github.com/user-attachments/assets/9d431a2a-5a04-4539-998c-8c8ddb3276fd)

`sudo yum install wget`

`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`
`sudo yum install docker-ce docker-ce-cli containerd.io`
`sudo systemctl enable docker --now`
`sudo yum install curl`
`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`
`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`
`chmod +x /usr/bin/docker-compose`
`docker-compose --version`
