# api_yamdb
# API - приложение YaMDb

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title). 
Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором.

В каждой категории есть произведения: книги, фильмы или музыка. 

Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок автоматически высчитывается средняя оценка произведения.

# Техническое описание проекта YaMDb

## Алгоритм регистрации пользователей

Пользователь отправляет запрос с параметром email на /auth/email/.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email .
Пользователь отправляет запрос с параметрами email и confirmation_code на /auth/token/, в ответе на запрос ему приходит token (JWT-токен).
При желании пользователь отправляет PATCH-запрос на /users/me/ и заполняет поля в своём профайле (описание полей — в документации).

## Пользовательские роли

Аноним — может просматривать описания произведений, читать отзывы и комментарии.
Аутентифицированный пользователь — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.
Модератор — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
Администратор — полные права на управление проектом и всем его содержимым. Может создавать и удалять категории и произведения. Может назначать роли пользователям.
Администратор Django — те же права, что и у роли Администратор.

## Ресурсы API YaMDb

AUTH: аутентификация.
USERS: пользователи.
TITLES: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
CATEGORIES: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
GENRES: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
REVIEWS: отзывы на произведения. Отзыв привязан к определённому произведению.
COMMENTS: комментарии к отзывам. Комментарий привязан к определённому отзыву

Подробная информация о ресурсах: /redoc

### AUTH:
- Получение JWT-токена в обмен на email и confirmati
- Отправление confirmation_code на переданный email.

### USERS:
- Получить список всех пользователей. Права доступа: Администратор
- Создание пользователя. Права доступа: Администратор
- Получить пользователя по username. Права доступа: Администратор
- Изменить данные пользователя по username. доступа: Администратор
- Удалить пользователя по username. Права доступа: Администратор
- Получить данные своей учетной записи Права доступа: Любой авторизованный пользователь
- Изменить данные своей учетной записи Права доступа: Любой авторизованный пользователь

### TITLES:
- Получить список всех объектов Права доступа: Доступно без токена
- Создать произведение для отзывов. Права доступа: Администратор
- Информация об объекте Права доступа: Доступно без токенао 
- Обновить информацию об объекте Права доступа: Администратор
- Удалить произведение. Права доступа: Администратор


### CATEGORIES:
- Получить список всех категорий Права доступа: Доступно без токена
- Создать категорию. Права доступа: Администратор
- Удалить категорию. Права доступа: Администратор

### GENRES:
- Получить список всех жанров Права доступа: Доступно без токена
- Создать жанр. Права доступа: Администратор
- Удалить жанр. Права доступа: Администратор

### REVIEWS:
- Получить список всех отзывов. Права доступа: Доступно без токена
- Создать новый отзыв. Пользователь может оставить только один отзыв на один объект. Права доступа: Аутентифицированные пользователи
- Получить отзыв по id. Права доступа: Доступно без токена
- Частично обновить отзыв по id. Права доступа: Автор отзыва, модератор или администратор
- Удалить отзыв по id Права доступа: Автор отзыва, модератор или администратор

### COMMENTS:
- Получить список всех комментариев к отзыву по id. Права доступа: Доступно без токена
- Создать новый комментарий для отзыва. Права доступа: Аутентифицированные пользователи
- Получить комментарий для отзыва по id. Права доступа: Доступно без токена
- Частично обновить комментарий к отзыву по id. Права доступа: Автор комментария, модератор или администратор
- Удалить комментарий к отзыву по id. Права доступа: Автор комментария, модератор или администратор


