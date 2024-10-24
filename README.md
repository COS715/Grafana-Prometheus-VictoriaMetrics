# Grafana-Prometheus-VictoriaMetrics
## Описание
ПО: VirtualBox, Docker + Docker Compose, Grafana, Prometheus; ОС: OracleLinux.
Итог: ?
## Начало
Установка гостевых дополнений
(за подробностями обращаемся к файлу [инструкция для VBoxGA.pdf](https://github.com/user-attachments/files/17509721/VBoxGA.pdf))

результат:

![изображение](https://github.com/user-attachments/assets/9d431a2a-5a04-4539-998c-8c8ddb3276fd)

Устанавлием утилиту wget, используя менеджер пакетов yum
`sudo yum install wget`
Скачиваем файл с указанного репозитория и сохраняет его в директорию /etc/yum.repos.d/
`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`
`sudo yum install docker-ce docker-ce-cli containerd.io`
`sudo systemctl enable docker --now`
`sudo yum install curl`
`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`
`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`
`chmod +x /usr/bin/docker-compose`
`docker-compose --version`
