# Grafana-Prometheus-VictoriaMetrics
## Описание
<br>ПО: VirtualBox, Docker + Docker Compose, Grafana, Prometheus; ОС: OracleLinux.
<br>Задача:
<br>![380424642-6ccebb43-249b-4d0a-b1e7-5bb8dab805e1](https://github.com/user-attachments/assets/5e4c8f1d-8e96-485a-b57b-6ff2a452693a)

## Начало
<br>Установка гостевых дополнений
(за подробностями обращаемся к файлу [инструкция для VBoxGA.pdf](https://github.com/user-attachments/files/17509721/VBoxGA.pdf))
<br>Результат:
<br>![изображение](https://github.com/user-attachments/assets/9d431a2a-5a04-4539-998c-8c8ddb3276fd)

<br>Устанавлием утилиту wget, используя менеджер пакетов yum
```
sudo yum install wget
```
<br>![изображение](https://github.com/user-attachments/assets/7918e885-0e49-4201-bc81-db2ac0dcd19f)

<br>Скачиваем файл с указанного репозитория и сохраняем его в директорию `/etc/yum.repos.d/`. Этот файл имеет информацию о репозиториях, откуда выгружаются пакеты Docker.
```
sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
```
<br>![изображение](https://github.com/user-attachments/assets/2478c223-bd0a-410f-97bb-b3456d23ce58)

<br>Устанавлием непосредтсвенно пакеты Docker (сам Docker Engine, Docker CLI и Containerd).
+ `docker-ce` — это основной пакет Docker Community Edition.
+ `docker-ce-cli` — это командная строка Docker, которая позволяет взаимодействовать с Docker через терминал.
+ `containerd.io` — это контейнерный демон, который управляет жизненным циклом контейнеров.

```
sudo yum install docker-ce docker-ce-cli containerd.io
```
<br>![изображение](https://github.com/user-attachments/assets/6b0c24a5-4061-4e0e-ad77-49bd4cc1ab08)

<br>Подтверждаем загрузку и установку пакетов
<br>![изображение](https://github.com/user-attachments/assets/4683d0e9-c1a2-41f5-a72c-d079f16884e3)

<br>Автозагрузка Docker
```
sudo systemctl enable docker --now
```
<br>![изображение](https://github.com/user-attachments/assets/a4dac1b1-708d-4106-b8e6-c221d39aba70)

<br>Установка curl
```
sudo yum install curl
```
<br>![изображение](https://github.com/user-attachments/assets/96d9f341-9709-4b39-be32-5ab429260d68)

<br>Обращаемся к репозиторию, чтобы получить последнюю версию Docker Compose.
<br>Обратите внимание(!), что команду нужно вводить без sudo, потому что она только получает информацию о версии и не требует прав суперпользователя для выполнения.
```
COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
```

<br>Загружаем исполняемый файл Docker Compose соответствующей версии для вашей системы.
```
sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
```
<br>![изображение](https://github.com/user-attachments/assets/a53258dd-9434-4970-9e6c-71b3668ae02c)

<br>Изменяем прва доступа для файла (права на выполнение как исполняемого файла).
```
sudo chmod +x /usr/bin/docker-compose
```

<br>Показывает версию Docker Compose.
```
docker-compose --version
```
<br>![изображение](https://github.com/user-attachments/assets/78c2e664-d287-4a3a-b1fe-36dccc83ecab)
## Git+Grafana
Эти конфиги есть в репозитории на всякий пожарный.
<br>Установка гита + клонирование репозитория
```
git clone https://github.com/skl256/grafana_stack_for_docker.git
```
<br>![изображение](https://github.com/user-attachments/assets/5e0b4c97-ac01-4bdf-b299-58230e8716fe)

<br>Переходим в папку `grafana_stack_for_docker`
```
cd grafana_stack_for_docker
```
<br>![изображение](https://github.com/user-attachments/assets/f119876b-fbe9-4848-b6d7-c9e00028aafc)

<br>Создаем директоррию `config`
```
sudo mkdir -p /mnt/common_volume/swarm/grafana/config
```

<br>Создаем несколько диреткорий для хранения конфигурационных файлов и данных от Grafana.
```
sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}
```

<br>Меняет владельца на того, кто выполнил команду (для управления).
```
sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}
```

<br>Если файла нет - создает, если есть изменяет временные метки доступа до текущего времени, не изменяя содержимое.
```
touch /mnt/common_volume/grafana/grafana-config/grafana.ini
```

<br>Копируем **все файлы** из диреткории `config` в директорию `/mnt/common_volume/swarm/grafana/config/`.
```
cp config/* /mnt/common_volume/swarm/grafana/config/
```

<br>Переименовываем файл `grafana.yaml` в `docker-compose.yaml`. (можно проверить через ls)
```
mv grafana.yaml docker-compose.yaml
```
<br>![изображение](https://github.com/user-attachments/assets/a42f5b6b-5b2c-498f-b682-f8a0638ed24b)

<br>Поднимаем Docker Compose.
```
sudo docker compose up -d
```
<br>![изображение](https://github.com/user-attachments/assets/0760cbb0-9eee-400b-84a5-60ffe6b9b0ae)
## Работа в Grafana
<br>Находится на `localhost:3000`
<br>Данные для авторизации:
<br>Username: admin
<br>Password: admin
<br>![изображение](https://github.com/user-attachments/assets/ba7cd873-0bcc-4c0f-88ab-49ea53f5140a)

<br>Тут для того чтобы брать откуда-то данные вставляем экспортер.
```
cd grafana_stack_for_docker
```
```
sudo vi docker-compose.yaml
```


```node-exporter:
    image: prom/node-exporter
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    container_name: exporter
    hostname: exporter
    command:
      - --path.procfs=/host/proc
      - --path.sysfs=/host/sys
      - --collector.filesystem.ignored-mount-points
      - ^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/)
    ports:
      - 9100:9100
    restart: unless-stopped
    environment:
      TZ: "Europe/Moscow"
    networks:
      - default
```
<br>А затем в `/mnt/common_volume/swarm/grafana/config/prometheus.yaml` меняем targets:(тут айпишник) на **exporter:9100**.
<br>ПОСЛЕ ВСЕГО ЭТОГО НАДО ПЕРЕСОБРАТЬ ДОКЕР КОМПОЗ!!!
```
sudo docker-compose stop
```
![изображение](https://github.com/user-attachments/assets/db17bbe6-fda5-4c7a-9675-ef4a296a8d07)
```
sudo docker compose up -d
```
<br>![изображение](https://github.com/user-attachments/assets/a41f29f4-e6f1-4325-8b42-d72de9c101e6)

<br>Для того чтобы проверить что все работает можно пройтись по:
<br>Grafana
```
localhost:3000
```
<br>Prometheus
```
localhost:9090
```
<br>Node Exporter
```
localhost:9100
```
<br>Жмем кнопку Create Dashboards
<br>![изображение](https://github.com/user-attachments/assets/a13d0856-b118-4160-adc2-41dccb31cdab)
<br>Жмем кнопку +Add visualization, а после "Configure a new data source"
<br>![изображение](https://github.com/user-attachments/assets/9f10b873-9780-4dcd-86da-d0fd667cd2e3)
<br>Выбираем Prometheus
<br>![изображение](https://github.com/user-attachments/assets/b53af16b-17d3-4f9d-9019-5bce4f354299)
<br>Connection
```
http://prometheus:9090
```
<br>В Authentication выбираем **Basic authentication** и задаем логин и пароль:
<br>User: admin
<br>Password:admin
<br>![изображение](https://github.com/user-attachments/assets/89f31a43-7644-4e5a-b6a9-c96c7a81f390)

<br>Потом жмем на плюсик справа сверху и выбираем **Import Dashboard**. Вводим 1860, жмем Load.
<br>![изображение](https://github.com/user-attachments/assets/8c049256-504c-4324-a389-b8655d906118)

<br>Выбираем Prometheus
<br>![изображение](https://github.com/user-attachments/assets/1fcfaa5f-288a-4ed2-a0a4-f7003a13f717)

<br>Итог
<br>![изображение](https://github.com/user-attachments/assets/aa683996-01bf-49dd-9859-5002791255d7)

