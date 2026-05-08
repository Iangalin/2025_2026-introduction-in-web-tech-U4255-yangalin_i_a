# Лабораторная работа №1

## "Основы работы с Docker"
### Описание
Это первая лабораторная работа по изучению основ контейнеризации с использованием Docker. В ходе работы вы изучите базовые концепции Docker, научитесь создавать образы, запускать контейнеры и управлять ими.

### Цель работы
Научиться работать с Docker: устанавливать Docker, создавать Dockerfile, собирать образы, запускать контейнеры и управлять ими.

### Правила по оформлению
Правила по оформлению отчета по лабораторной работе вы можете изучить по [ссылке](../reportdesign.md)

### Ход работы

#### Обычная лабораторная работа

**Изучение основ Docker:**

1. **Установка Docker:**
      - Установить Docker Desktop (для Windows/Mac) или Docker Engine (для Linux)
      - Проверить установку командой `docker --version`
      - Запустить тестовый контейнер: `docker run hello-world`
      - Изучить базовые команды: `docker images`, `docker ps`, `docker ps -a`

2. **Работа с готовыми образами:**
      - Скачать образ Ubuntu: `docker pull ubuntu:latest`
      - Запустить интерактивный контейнер: `docker run -it ubuntu bash`
      - Внутри контейнера установить пакет (например, curl): `apt update && apt install -y curl`
      - Проверить установку: `curl --version`
      - Выйти из контейнера: `exit`

3. **Запуск веб-сервера:**
      - Запустить контейнер с nginx: `docker run -d -p 8080:80 --name web-server nginx:alpine`
      - Проверить работу в браузере: `http://localhost:8080`
      - Посмотреть логи контейнера: `docker logs web-server`
      - Подключиться к контейнеру: `docker exec -it web-server sh`

4. **Управление контейнерами:**
      - Посмотреть запущенные контейнеры: `docker ps`
      - Посмотреть все контейнеры: `docker ps -a`
      - Остановить контейнер: `docker stop web-server`
      - Запустить остановленный контейнер: `docker start web-server`
      - Удалить контейнер: `docker rm web-server`
      - Удалить образ: `docker rmi nginx:alpine`

5. **Работа с томами (volumes):**
      - Создать том: `docker volume create my-volume`
      - Запустить контейнер с томом: `docker run -it --name volume-test -d -v my-volume:/data ubuntu bash`
      - Подключиться к контейнеру: `docker exec -it volume-test bash`
      - Создать файл в томе: `echo "Hello from volume" > /data/test.txt`
      - Удалить контейнер и создать новый с тем же томом
      - Проверить, что файл сохранился

#### Лабораторная работа со звездочкой

**Создание Dockerfile по заданным условиям:**

**Задача:** Создать Dockerfile, который должен выполнять следующие условия:

**1. Создание файлов проекта:**

   - **app.py** - создать файл со следующим содержимым:

         from flask import Flask
         app = Flask(__name__)
         
         @app.route('/')
         def hello():
            return "Hello from Docker!"
         
         if __name__ == '__main__':
            app.run(host='0.0.0.0', port=5000)

   - **requirements.txt** - создать файл со следующим содержимым:

         Flask==2.0.1


**2. Создание Dockerfile:**

   Создать Dockerfile со следующими условиями:
   
   1. Использовать базовый образ `python:3.9-slim`
   2. Установить рабочую директорию `/app`
   3. Установить системные пакеты: `curl` и `vim`
   4. Установить Python пакеты из файла `requirements.txt`
   5. Скопировать файл `app.py` в контейнер
   6. Создать пользователя `appuser` с UID 1000
   7. Переключиться на пользователя `appuser`
   8. Открыть порт 5000
   9. Установить переменную окружения `FLASK_ENV=production`
   10. Запустить приложение командой `python app.py`

**3. Проверка работы:**

   - Собрать образ: `docker build -t my-flask-app .`
   - Запустить контейнер: `docker run -d -p 5000:5000 --name flask-container my-flask-app`
   - Проверить работу: `curl http://localhost:5000`

**Результат:** Вы должны уметь создавать Dockerfile по заданным техническим требованиям.

### Результаты лабораторной работы
В результате данной работы у вас должно быть:

- Установленный и настроенный Docker
- Понимание основных команд Docker
- Опыт работы с готовыми образами
- Умение запускать и управлять контейнерами
- Отчет с описанием выполненных задач и скриншотами

### Полезные ссылки

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Commands Reference](https://docs.docker.com/engine/reference/commandline/docker/)
- [Docker Images](https://docs.docker.com/engine/reference/commandline/images/)
- [Docker Containers](https://docs.docker.com/engine/reference/commandline/container/)
