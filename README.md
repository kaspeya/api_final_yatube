# Документация к API проекта [Yatube](https://github.com/kaspeya/yamdb_final)

## Как запустить проект:
#### Клонировать репозиторий и перейти в него в командной строке:
```
git clone git@github.com:kaspeya/api_final_yatube.git
cd yatube_api
```
#### Cоздать и активировать виртуальное окружение:
```
python3 -m venv env
source env/bin/activate
```
#### Установить зависимости из файла requirements.txt:
```
python3 -m pip install --upgrade pip
pip install -r requirements.txt
```
#### Выполнить миграции:
```
python3 manage.py migrate
```
#### Запустить проект:
```
python3 manage.py runserver
```
# Примеры запросов:
| CRUD      | Эндпоинты | Что получаем | 
| --- | --- | --- |
| 'GET'     | /api/v1/posts/                          | Список всех публикаций.           |
| 'POST'    | /api/v1/posts/                          | Создание публикации.              |
| 'GET'     | /api/v1/posts/{post_id}/                | Получение публикации.             |
| 'PUT'     | /api/v1/posts/{post_id}/                | Обновление публикации.            |
| 'PATCH'   | /api/v1/posts/{post_id}/                | Частичное обновление публикации.  |
| 'DELETE'  | /api/v1/posts/{post_id}/                | Удаление публикации.              |
| 'GET'     | /api/v1/posts/{post_id}/comments/       | Получение комментариев.           |
| 'POST'    | /api/v1/posts/{post_id}/comments/       | Добавление комментарияв.          |
| 'GET'     | /api/v1/posts/{post_id}/comments/{id}   | Получение комментария.            |
| 'PUT'     | /api/v1/posts/{post_id}/comments/{id}   | Обновление комментария.           |
| 'PATCH'   | /api/v1/posts/{post_id}/comments/{id}   | Частичное обновление комментария. |
| 'DELETE'  | /api/v1/posts/{post_id}/comments/{id}   | Удаление комментария.             |
| 'GET'     | /api/v1/groups/                         | Список групп.                     |
| 'GET'     | /api/v1/groups/{id}                     | Информация о группе.              |
| 'GET'     | /api/v1/follow/                         | Подписки.                         |
| 'POST'    | /api/v1/groups/                         | Подписка.                         |
| 'POST'    | /api/v1/jwt/create/                     | Получить JWT-токен.               |
| 'POST'    | /api/v1/jwt/refresh/                    | Обновить JWT-токен.               |
| 'POST'    | /api/v1/jwt/verify/                     | Проверить JWT-токен.              |
 

## Документация в формате [ReDoc](http://127.0.0.1:8000/redoc/).
