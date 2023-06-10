#  Kittygram
## _Социальная сеть для обмена фотографиями любимых питомцев_

![badge](https://github.com/Glebchik57/kittygram_final/actions/workflows/main.yml/badge.svg)

## Технологии

 - Python 3.9
 - Django==3.2.3
 - Docker
 - Nginx
 - gunicorn
 - PostgreSQL
 - GitHub Actions

## Как запустить проект на локальный компьютер:
Клонировать репозиторий и перейти в него в командной строке:
```
git@github.com:Glebchik57/kittygram_final.git
```
Подключиться к удалённому серверу:
```
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
```
Установить Docker на сервер:
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```
Скопировать на сервер файл docker-compose.production.yml:
```
 scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/taski/docker-compose.production.yml
 _или сделать это вручную_
```
Создать файл .env:
```
sudo touch .env
```
В файле .env прописать следующие: 
```
SECRET_KEY
POSTGRES_USER
POSTGRES_PASSWORD
POSTGRES_DB
DB_HOST=db
DB_PORT=5432
```
Запустить Docker Compose в режиме демона:
```
sudo docker compose -f docker-compose.production.yml up -d
```
Выполнить миграции, соберите статические файлы бэкенда:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /static/static/
```
Откоректировать конфиг nginx на сервере:
```
sudo nano /etc/nginx/sites-enabled/default
```
Должно быть так:
```
 location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:8000;
    }
```
Поздравляю! теперь у вас есть сайт, где вы можете делиться с миром фотографиями своих котов.

## Автор:
_Севостьянов Глеб_