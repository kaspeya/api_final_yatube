# Документация к API проекта Yatube (v1)

# Как запустить проект:
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
| Эндпоинты              | Что делает            | 
| -------------          | -------------         |
| 'GET'  /api/v1/posts/  | Получить список всех публикаций.
При указании параметров limit и offset выдача должна работать с пагинацией.  |
| 'POST' /api/v1/posts/  | Создание публикации  |
| 'GET'  /api/v1/groups/ | Получить список всех групп.|

paths:
  /api/v1/posts/:
    get:
      operationId: Получение публикаций
      description: >-
        Получить список всех публикаций. При указании параметров limit и offset
        выдача должна работать с пагинацией.
      parameters:
        - name: limit
          required: false
          in: query
          description: Количество публикаций на страницу
          schema:
            type: integer
        - name: offset
          required: false
          in: query
          description: Номер страницы после которой начинать выдачу
          schema:
            type: integer
      responses:
        '200':
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/GetPost'
              examples:
                Ответ с пагинацией:
                  value:
                    count: 123
                    next: http://api.example.org/accounts/?offset=400&limit=100
                    previous: http://api.example.org/accounts/?offset=200&limit=100
                    results:
                      -
                        id: 0
                        author: string
                        text: string
                        pub_date: 2021-10-14T20:41:29.648Z
                        image: string
                        group: 0            
          description: Удачное выполнение запроса без пагинации
      tags:
        - api
    post:
      operationId: Создание публикации
      description: >-
        Добавление новой публикации в коллекцию публикаций. Анонимные запросы
        запрещены.
      parameters: []
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Post'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Post'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Post'
      responses:
        '201':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Post'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    text:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
      tags:
        - api
  '/api/v1/posts/{id}/':
    get:
      operationId: Получение публикации
      description: Получение публикации по id.
      parameters:
        - name: id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Post'
          description: Удачное выполнение запроса
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: Попытка запроса несуществующей публикации
      tags:
        - api
    put:
      operationId: Обновление публикации
      description: >-
        Обновление публикации по id. Обновить публикацию может только автор
        публикации. Анонимные запросы запрещены.
      parameters:
        - name: id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Post'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Post'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Post'
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Post'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    text:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '403':
          content:
            application/json:
              examples:
                '403':
                  value:
                    detail: У вас недостаточно прав для выполнения данного действия.
          description: Попытка изменения чужого контента
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: Попытка изменения несуществующей публикации
      tags:
        - api
    patch:
      operationId: Частичное обновление публикации
      description: >-
        Частичное обновление публикации по id. Обновить публикацию может только
        автор публикации. Анонимные запросы запрещены.
      parameters:
        - name: id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Post'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Post'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Post'
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Post'
          description: Удачное выполнение запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '403':
          content:
            application/json:
              examples:
                '403':
                  value:
                    detail: У вас недостаточно прав для выполнения данного действия.
          description: Попытка изменения чужого контента
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: Попытка изменения несуществующей публикации
      tags:
        - api
    delete:
      operationId: Удаление публикации
      description: >-
        Удаление публикации по id. Удалить публикацию может только автор
        публикации. Анонимные запросы запрещены.
      parameters:
        - name: id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
      responses:
        '204':
          description: Удачное выполнение запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '403':
          content:
            application/json:
              examples:
                '403':
                  value:
                    detail: У вас недостаточно прав для выполнения данного действия.
          description: Попытка изменения чужого контента
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: Попытка удаления несуществующей публикации
      tags:
        - api
  '/api/v1/posts/{post_id}/comments/':
    get:
      operationId: Получение комментариев
      description: Получение всех комментариев к публикации.
      parameters:
        - name: post_id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
      responses:
        '200':
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Comment'
          description: Удачное выполнение запроса
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    {
                    "detail": "Страница не найдена."
                    }
          description: Получение списка комментариев к несуществующей публикации
      tags:
        - api
    post:
      operationId: Добавление комментария
      description: Добавление нового комментария к публикации. Анонимные запросы запрещены.
      parameters:
        - name: post_id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Comment'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Comment'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Comment'
      responses:
        '201':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Comment'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    text:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: Попытка добавить комментарий к несуществующей публикации
      tags:
        - api
  '/api/v1/posts/{post_id}/comments/{id}/':
    get:
      operationId: Получение комментария
      description: Получение комментария к публикации по id.
      parameters:
        - name: post_id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
        - name: id
          in: path
          required: true
          description: id комментария
          schema:
            type: integer
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Comment'
          description: Удачное выполнение запроса
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: >-
            Попытка запросить несуществующий комментарий или к несуществующей
            публикации
      tags:
        - api
    put:
      operationId: Обновление комментария
      description: >-
        Обновление комментария к публикации по id. Обновить комментарий может
        только автор комментария. Анонимные запросы запрещены.
      parameters:
        - name: post_id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
        - name: id
          in: path
          required: true
          description: id комментария
          schema:
            type: integer
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Comment'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Comment'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Comment'
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Comment'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    text:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '403':
          content:
            application/json:
              examples:
                '403':
                  value:
                    detail: У вас недостаточно прав для выполнения данного действия.
          description: Попытка изменения чужого контента
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: >-
            Попытка изменить несуществующий комментарий или к несуществующей
            публикации
      tags:
        - api
    patch:
      operationId: Частичное обновление комментария
      description: >-
        Частичное обновление комментария к публикации по id. Обновить
        комментарий может только автор комментария. Анонимные запросы запрещены.
      parameters:
        - name: post_id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
        - name: id
          in: path
          required: true
          description: id комментария
          schema:
            type: integer
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Comment'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Comment'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Comment'
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Comment'
          description: Удачное выполнение запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '403':
          content:
            application/json:
              examples:
                '403':
                  value:
                    detail: У вас недостаточно прав для выполнения данного действия.
          description: Попытка изменения чужого контента
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: >-
            Попытка изменить несуществующий комментарий или к несуществующей
            публикации
      tags:
        - api
    delete:
      operationId: Удаление комментария
      description: >-
        Удаление комментария к публикации по id. Обновить комментарий может
        только автор комментария. Анонимные запросы запрещены.
      parameters:
        - name: post_id
          in: path
          required: true
          description: id публикации
          schema:
            type: integer
        - name: id
          in: path
          required: true
          description: id комментария
          schema:
            type: integer
      responses:
        '204':
          description: Удачное выполнение запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
        '403':
          content:
            application/json:
              examples:
                '403':
                  value:
                    detail: У вас недостаточно прав для выполнения данного действия.
          description: Попытка изменения чужого контента
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: >-
            Попытка удалить несуществующий комментарий или к несуществующей
            публикации
      tags:
        - api
  /api/v1/groups/:
    get:
      operationId: Список сообществ
      description: Получение списка доступных сообществ.
      parameters: []
      responses:
        '200':
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Group'
          description: Удачное выполнение запроса
      tags:
        - api
  '/api/v1/groups/{id}/':
    get:
      operationId: Информация о сообществе
      description: Получение информации о сообществе по id.
      parameters:
        - name: id
          in: path
          required: true
          description: id сообщества
          schema:
            type: integer
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Group'
          description: Удачное выполнение запроса
        '404':
          content:
            application/json:
              examples:
                '404':
                  value:
                    detail: Страница не найдена.
          description: Попытка запроса несуществующего сообщества
      tags:
        - api
  /api/v1/follow/:
    get:
      operationId: Подписки
      description: >-
        Возвращает все подписки пользователя, сделавшего запрос. Анонимные запросы запрещены.
      parameters:
        - name: search
          required: false
          in: query
          description: >-
            Возможен поиск по подпискам по параметру search
          schema:
            type: string
      responses:
        '200':
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Follow'
          description: Удачное выполнение запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
      tags:
        - api
    post:
      operationId: Подписка
      description: >-
        Подписка пользователя от имени которого сделан запрос на пользователя
        переданного в теле запроса. Анонимные запросы запрещены.
      parameters: []
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Follow'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/Follow'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/Follow'
      responses:
        '201':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Follow'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    following:
                      - Обязательное поле.
                Пользователь не существует:
                  value:
                    following:
                      - Объект с username=... не существует.
                Подписка на себя:
                  value:
                    following:
                      - Нельзя подписаться на самого себя!
          description: >-
            Отсутствует обязательное поле в теле запроса или оно не
            соответствует требованиям
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Учетные данные не были предоставлены.
          description: Запрос от имени анонимного пользователя
      tags:
        - api
  /api/v1/jwt/create/:
    post:
      operationId: Получить JWT-токен
      description: Получение JWT-токена.
      parameters: []
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TokenObtainPair'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/TokenObtainPair'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/TokenObtainPair'
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Token'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    username:
                      - Обязательное поле.
                    password:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: No active account found with the given credentials
          description: Переданная учетная запись не существует
      tags:
        - api
  /api/v1/jwt/refresh/:
    post:
      operationId: Обновить JWT-токен
      description: Обновление JWT-токена.
      parameters: []
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TokenRefresh'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/TokenRefresh'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/TokenRefresh'
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TokenRefresh2'
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    refresh:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Token is invalid or expired
                    code: token_not_valid
          description: Передан невалидный токен
      tags:
        - api
  /api/v1/jwt/verify/:
    post:
      operationId: Проверить JWT-токен
      description: Проверка JWT-токена.
      parameters: []
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TokenVerify'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/TokenVerify'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/TokenVerify'
      responses:
        '200':
          description: Удачное выполнение запроса
        '400':
          content:
            application/json:
              examples:
                '400':
                  value:
                    token:
                      - Обязательное поле.
          description: Отсутствует обязательное поле в теле запроса
        '401':
          content:
            application/json:
              examples:
                '401':
                  value:
                    detail: Token is invalid or expired
                    code: token_not_valid
          description: Передан невалидный токен
      tags:
        - api
components:
  schemas:
    Post:
      type: object
      properties:
        id:
          type: integer
          title: id публикации
          readOnly: true
        author:
          type: string
          title: username пользователя
          readOnly: true
        text:
          type: string
          title: текст публикации
        pub_date:
          type: string
          format: date-time
          readOnly: true
        image:
          type: string
          format: binary
          nullable: true
        group:
          type: integer
          title: id сообщества
          nullable: true
      required:
        - text
    GetPost:
      type: object
      properties:
        id:
          type: integer
          title: id публикации
          readOnly: true
        author:
          type: string
          title: username пользователя
          readOnly: true
        text:
          type: string
          title: текст публикации
        pub_date:
          type: string
          format: date-time
          readOnly: true
        image:
          type: string
          format: binary
          nullable: true
        group:
          type: integer
          title: id сообщества
          nullable: true
    Comment:
      type: object
      properties:
        id:
          type: integer
          title: id комментария
          readOnly: true
        author:
          type: string
          title: username пользователя
          readOnly: true
        text:
          type: string
          title: текст комментария
        created:
          type: string
          format: date-time
          readOnly: true
        post:
          type: integer
          title: id публикации
          readOnly: true
      required:
        - text
    Group:
      type: object
      properties:
        id:
          type: integer
          readOnly: true
        title:
          type: string
          maxLength: 200
        slug:
          type: string
          maxLength: 50
          pattern: '^[-a-zA-Z0-9_]+$'
        description:
          type: string
      required:
        - title
        - slug
        - description
    Follow:
      type: object
      properties:
        user:
          type: string
          title: username владелец токена
          readOnly: true
        following:
          type: string
          title: username
      required:
        - following
    TokenObtainPair:
      type: object
      properties:
        username:
          type: string
        password:
          type: string
          writeOnly: true
      required:
        - username
        - password
    Token:
      title: Токен
      type: object
      properties:
        refresh:
          type: string
          title: refresh токен
        access:
          type: string
          title: access токен
    TokenRefresh:
      type: object
      properties:
        refresh:
          type: string
        access:
          type: string
          readOnly: true
      required:
        - refresh
    TokenVerify:
      type: object
      properties:
        token:
          type: string
      required:
        - token
    TokenRefresh2:
      type: object
      properties:
        access:
          type: string
