# Тестовое задание для фронт разработчика

1. **Цель:** определить уровень разработчика; показать какие задачи предстоить решать на проекте
2. **Продолжительность:** для выполнения задания потребуется до 12 часов
3. **Инструменты:** React, Twitter Bootstrap 4, Axios, Redux (optional)
4. **Результат:** ссылка на публичный git репозиторий с исходным кодом проекта; ссылка на рабочее приложение (Heroku, Google Cloud, Digital Ocean, AWS EC2, K8S, Firebase, etc.)

## Описание приложения

Приложение должно позволять пользователям загружать изображения на сервер, отображать и редактировать загруженные изображения. Серверную часть выполняет postman mock сервер по адресу: https://8a4bdb22-38b7-46c7-9944-809575dff15b.mock.pstmn.io Сервер для хранения ассетов - Cloudinary. Все ендпоинты ниже по тексту.

## Описание UI/UX

В верхней части страницы находится дроп-зона с текстом “Для начала загрузки перетащите файлы или нажмите сюда". При перетаскивании файла изображения на зону, рамка должна подсвечиваться зеленым цветом. При перетаскивании файла неверного формата - подсвечиваться красным цветом; текст в зоне должен измениться на “Этот тип файлов не поддерживается”.

После успешного перетаскивания, для каждого файла должна быть отрисована карточка изображения с серым полупрозрачным оверлеем и прогрессом загрузки изображения. После успешной загрузки изображения на сервер ассетов Cloudinary, карточка переходит в **режим редактирования**. При неудачной загрузке, в карточке под картинкой должно быть отрисовано сообщение об ошибке и кнопка для повторной загрузки изображения в данной карточке.

В режиме редактирования отображаются изображение и 2 дропдауна: *ивент* и *отметка* (данные получаются с mock сервера) и кнопка “Сохраить”. Для новых изображений дропдаун *отметки* задисейблен до тех пор пока не выбран *ивент*. После выбора ивента, в дропдауне *отметки* появляются опции соответсвующие выбранному ивенту. Кнопка “Сохранить” задисейблена до тех пор, пока не выбраны значения в обеих дропдаунах. При нажатии на кнопку “Сохраить” все инпуты дисейблятся и на mock сервер отправляется URL загруженной картинки (в ответе сервера Cloudinary это будет *secure_url*) и ID выбранной отметки, после чего карточка переходит в **режим просмотра**. При неудачном сохранении, в карточке под картинкой должно быть отрисовано сообщение об ошибке и кнопка для повторного сохранения изображения в данной карточке.

В режиме просмотра отображаются изображение, название ивента и отметки. При наведении курсора на карточку, отображается кнопка “Изменить” поверх изображения в правом верхнем углу. При нажатии на кнопку “Изменить”, карточка переходит в режим редактирования.

При перезагрузке страницы, все несохраненные изображения/карточки теряются. Никаких механизмов сохранения данных на стороне клиентов реализовывать в рамках тестового задания не стоит (с учетом того, что мок сервер всегда возвращает только 1 сохраненную карточку).

## Технические замечания

### Ресайзинг и сжатие изображений

На стороне клиента (до отправки на сервер ассетов) изображения должны сжиматься в JPEG с качеством 0.7 и размером не превышающим 1024px по большей из сторон с сохранения пропорций. Предпочтительно не использовать сторонних библиотек для этой задачи.

### Отправка изображений на сервер ассетов

Сжатые и уменьшенные изображения должны загружаться на сервер Cloudinary:

* **URL: ** https://api.cloudinary.com/v1_1/dldnai0ij/image/upload
* **Method:** POST
* **Preset:** jo9jidf8
* **Tags:** take-home-test

### Сохранение карточек изображений на mock сервере

Для имитации сохранения карточки изображения, необходимо сделать запрос на mock сервер Postman’a.

* **URL: ** https://8a4bdb22-38b7-46c7-9944-809575dff15b.mock.pstmn.io/api/waypoint-captures
* **Method:** POST
* **JSON Payload:** `{“waypointId": <ID>, “imageUrl”: <URL>}`

**waypointId:** - ID *отметки* выбранной в дропдауне
**imageUrl:** - URL изображения на сервере Cloudinary (аттрибут secure_url в отверете Cloudinary при загрузке ассета)

### Изменение карточек изображения на mock сервере

Для имитации изменения карточки изображения, необходимо сделать запрос на mock сервер Postman’a.

* **URL: ** https://8a4bdb22-38b7-46c7-9944-809575dff15b.mock.pstmn.io/api/waypoint-captures
* **Method:** PUT
* **JSON Payload:** `{“waypointId": <ID>}`

### Загрузка карточек изображений с mock сервера

Для имитации загрузки карточек изображений, необходимо сделать запрос на mock сервер Postman’a

* **URL: ** https://8a4bdb22-38b7-46c7-9944-809575dff15b.mock.pstmn.io/api/waypoint-captures/my
* **Method:** GET

В ответе всегда возвращается 1 статическая карточка.

### Загрузка списка ивентов и отметок с mock сервера

Для имитации загрузки списка ивентов и отметок, необходимо сделать запрос на mock сервер Postman’a

* **URL: ** https://8a4bdb22-38b7-46c7-9944-809575dff15b.mock.pstmn.io/api/events/my
* **Method:** GET

В ответе всегда возвращаются 3 ивента с отметками. **!!!** Для отображения на UI используйте только ивенты со статусом “active” **!!!**
