# Grafana-Prometheus-VictoriaMetrics
## Описание
ПО: VirtualBox, Docker + Docker Compose, Grafana, Prometheus; ОС: OracleLinux.
Итог: ?
## Начало
<br>Установка гостевых дополнений
(за подробностями обращаемся к файлу [инструкция для VBoxGA.pdf](https://github.com/user-attachments/files/17509721/VBoxGA.pdf))
<br>Результат:
<br>![изображение](https://github.com/user-attachments/assets/9d431a2a-5a04-4539-998c-8c8ddb3276fd)
<br>Устанавлием утилиту wget, используя менеджер пакетов yum
<br>`sudo yum install wget`
<br>![изображение](https://github.com/user-attachments/assets/7918e885-0e49-4201-bc81-db2ac0dcd19f)
<br>Скачиваем файл с указанного репозитория и сохраняем его в директорию `/etc/yum.repos.d/`. Этот файл имеет информацию о репозиториях, откуда выгружаются пакеты Docker.
<br>`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`
<br>![изображение](https://github.com/user-attachments/assets/2478c223-bd0a-410f-97bb-b3456d23ce58)
<br>Устанавлием непосредтсвенно пакеты Docker (сам Docker Engine, Docker CLI и Containerd).
- `docker-ce` — это основной пакет Docker Community Edition.
- `docker-ce-cli` — это командная строка Docker, которая позволяет взаимодействовать с Docker через терминал.
- `containerd.io` — это контейнерный демон, который управляет жизненным циклом контейнеров.
<br>`sudo yum install docker-ce docker-ce-cli containerd.io`
<br>![изображение](https://github.com/user-attachments/assets/6b0c24a5-4061-4e0e-ad77-49bd4cc1ab08)
Подтверждаем загрузку и установку пакетов
<br>![изображение](https://github.com/user-attachments/assets/4683d0e9-c1a2-41f5-a72c-d079f16884e3)
<br>Автозагрузка Docker
<br>`sudo systemctl enable docker --now`
<br>![изображение](https://github.com/user-attachments/assets/a4dac1b1-708d-4106-b8e6-c221d39aba70)
<br>Установка curl
<br>`sudo yum install curl`
<br>![изображение](https://github.com/user-attachments/assets/96d9f341-9709-4b39-be32-5ab429260d68)
<br>Обращаемся к репозиторию, чтобы получить последнюю версию Docker Compose.
<br>Обратите внимание(!), что команду нужно вводить без sudo, потому что она только получает информацию о версии и не требует прав суперпользователя для выполнения.
<br>`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`
<br>Загружаем исполняемый файл Docker Compose соответствующей версии для вашей системы.
<br>`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`
<br>![изображение](https://github.com/user-attachments/assets/a53258dd-9434-4970-9e6c-71b3668ae02c)
<br>Изменяем прва доступа для файла (права на выполнение как исполняемого файла).
<br>`chmod +x /usr/bin/docker-compose`
<br>Показывает версию Docker Compose.
<br>`docker-compose --version`
<br>![изображение](https://github.com/user-attachments/assets/78c2e664-d287-4a3a-b1fe-36dccc83ecab)

