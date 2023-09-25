<h1 align="center">Kittygram</h1>


<h2 align="center">Описание проекта</h2>
Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Это полностью рабочий проект, который состоит из бэкенд-приложения на Django и фронтенд-приложения на React.

Проект позволяет регистрироваться, добавлять любимых питомцев, указывать их основной цвет, выбирать и добавлять достижения своих питомцев.

<br>

<h2 align="center">Возможности</h2>
В проекте реализовано: <br>
- Фронтенд на React, бэкенд на Django 3.2.3<br>
- Система регистрации и аутентификации пользователей<br>
- Добавление котиков  выбором достижений и расцветки<br>
- Доступна админ-панель для работы с ресурсами через сайт<br>

<h2 align="center">Инструкция по запуску</h2>

### **Необходимая версия Python 3.9.13**
1. Клонируйте проект
```
git@github.com:shkiperzero/api_yamdb.git
```
2. Создайте виртуальное окружение: 
``` 
python -m venv venv
```
для Linux
```
python3 -m venv venv
```
Если пользуетесь Windows и первый вариант не сработал попробуйте 
```
py -m venv venv
```
3. Активируйте виртуальное окружение 
Если у Вас Windows
```
. venv/Scripts/activate
```
Если у Вас Linux
```
. venv/bin/activate
```
4. Установите все необходимые библиотеки командой 
```
pip install -r requirements.txt
```
<br>

<h3 align="center">Настройка фронтенд</h3>
<br>

1. Находясь в директории с фронтенд-приложением, установите зависимости для него
```
npm i
```
2. Из директории с фронтенд-приложением выполните команду:
```
npm run build
```
3. Скопируйте статику фронтенд-приложения в директорию по умолчанию
```
sudo cp -r путь_к_директории_с_фронтенд-приложением/build/. /var/www/kittygram/
```
<br>

<h3 align="center">Настройка бэкенд</h3>
<br>
<p>! ! ! До разворачивания проекта необходимо в корне проекта создать файл .env и указать в нём переменные окружения.<br>
Черновик можно посмотреть в файле .env_example</p>
<br>
1. Создайте файл конфигурации юнита systemd для Gunicorn
```
sudo nano /etc/systemd/system/gunicorn_kittygram.service
```

Конфигурация юнита:
```
[Unit]
Description=kittygramm_gunicorn daemon
After=network.target

[Service]
User=yc-user
WorkingDirectory=/home/yc-user/infra_sprint1/backend/
ExecStart=/home/yc-user/infra_sprint1/venv/bin/gunicorn --bind 0.0.0.0:8080 kittygram_backend.wsgi

[Install]
WantedBy=multi-user.target

```
2. Запустите gunicorn командой 
```
sudo systemctl start gunicorn_kittygram.service
```
3. Добавьте сервис для запуска при старте системы
```
sudo systemctl enable gunicorn_kittygram.service
```
4. Установите Nginx
```
sudo apt install nginx -y
```
5. Запустите Nginx 
```
sudo systemctl start nginx
```
6. Обновите настройки Nginx
```
sudo nano /etc/nginx/sites-enabled/default
```

Настройки:
```
server {
    server_name your-ip your-site.com;

    location /admin/ {
        proxy_pass http://127.0.0.1:8080;
        client_max_body_size 20M;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:8080;
        client_max_body_size 20M;
    }

    location /media/ {
        alias /var/www/kittygram/media/;

    }

    location / {
        root /var/www/kittygram;
        index index.html index.htm;
        try_files $uri /index.html;
    }

}

```
7. Проверьте корректность настроек
```
nginx -t
```
И перезагрузите nginx
```
sudo systemctl reload nginx
```

8. Установите certbot и получите ssl сертификат
```
# Установка пакетного менеджера snap.
sudo apt install snapd
# Установка и обновление зависимостей для пакетного менеджера snap.
sudo snap install core; sudo snap refresh core
# Установка пакета certbot.
sudo snap install --classic certbot
# Обеспечение доступа к пакету для пользователя с правами администратора.
sudo ln -s /snap/bin/certbot /usr/bin/certbot
# Получение сертификата
sudo certbot --nginx
```
Перезагрузите nginx
```
sudo systemctl reload nginx
```

9. Установите все необходимые библиотеки командой 
```
pip install -r requirements.txt
```


<h2 align="center">Использованные технологии</h2>
<p>Проект реализован на языке Python версии 3.9.13</p>
<p>Фреймворк разработки - Django версии 3.2.16</p>
<p>Api реализовано на Django Rest Framework версии 3.12.4</p>

<h2 align="center">Об авторе</h2>
Автор - студент Яндекс.Практикум 25 когорты Константин Упоров. Python backend-разработчик.<br>
email для связи - orbikadm@gmail.com