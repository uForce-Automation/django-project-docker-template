
# Django project template (Docker)
В данном репозитории содержится шаблон Django проекта, который можно запускать локально при помощи Docker. В ходе запуска создастся тестовая БД, которую можно использовать в процессе разработки. При переносе на сервер придётся создать собственную локальную базу, а также настроить демонов для запуска Celery.

**В проекте используется:**
- PostgreSQL
- CustomUser (на основе AbstractUser)
- Django Rest Framework (пока без JWT)

**В проект добавятся позже:**
- Celery и Celerybeat
- Работа с таймзонами
- DRF с JWT


**Порядок действий:**
1. Устанавливаем и запускаем Docker Desktop
2. Клонируем текущий репозиторий и загружаем к себе на компьютер
3. В терминале открываем директорию нашего проекта
4. Генерируем новый secret-key `python regenerate_secret_key.py`
5. Собираем контейнер командой `docker-compose build`
6. Запускаем наш контейнер командой `docker-compose up`
7. Открываем адрес localhost в браузере, проверяем, что открылась стартовая страница Django
8. Открываем тестовую страницу localhost/core/hello
9. Создаём новое окно терминала
10. Вводим в новом окне терминала команду `docker-compose exec web bash`. Команда откроет bash терминал linux
11. Командой `python manage.py makemigrations core` создаём в нашей БД таблицу под приложение core
11. Командой `python manage.py migrate` создаём нашу базу данных
12. Создаём юзера-администратора командой `python manage.py createsuperuser`
13. Открываем в браузере страницу localhost/admin и пробуем залогиниться
14. Поздравляю, ваш проект работает, можно его изменять под свои нужды


**Полезные команды:**
- `docker-compose build` - собирает наш контейнер. Подгружает python, все нужные зависимости, прописывает конфиги в системе контейнера
- `docker-compose build --no-cache` - собирает наш контейнер без использования кэшированных ранее этапов
- `docker-compose up` и `docker-compose down` - запуск и выключение контейнера
- `docker-compose up --detach` - позволяет запускать контейнер, не занимая терминал логами, в фоновом режиме
- `docker-compose exec web bash` - запуск bash терминала нашего проекта.
    - `python manage.py shell` - запуск django shell
    - `python manage.py makemigrations; python manage.py migrate` - для миграции БД
    - `python manage.py collectstatic --no-input --clear` - для сбора статики, чтобы админка выглядела по-человечески

**Дополнительные комментарии:**
- Пока что темплейт далёк от идеала. Будем дополнять постепенно.
    - Не хватает Celery, работы с таймзонами, Django Rest API.
    - Если видите ошибки, смело правьте!
    - Если хотите что-то дописать или улучшить, смело это делайте!
    - Dockerfile нужно переписать
- Иногда бывает, что возникает ошибка, в ходе которой `entrypoint.sh` не может найтись внутри контейнера. В таком случае просто закомментируйте строку с вызовом этого файла в конце `Dockerfile` и молитесь, чтобы БД поднялась быстро и без проблем (этот файл добавляет ожидание запуска БД)
- Для демонстрации работы Django Rest Framework в проект добавлена модель Company, для который подключен endpoint DRF http://localhost/api/companies
