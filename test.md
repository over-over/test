# Бизнес-задача

Реализовать функционал авторизации и регистрации в mobile web Utair, используя следующий стэк:
- CRA
- React.js
- Redux
- Redux-saga
- React-router
- Connected-react-router

## Задачи

1. Реализовать механизм получения и хранения токенов, используя методы /session/guest и /session/refresh
2. Реализовать механизм взаимодействия с API
3. Реализовать механизм авторизации и регистрации без дизайна,без валидации полей, без анимаций. Только получение данных от пользователя, переход по страницам и запросы к API. При успехе регистрации нужно предзаполнить форму логина, перейти на страницу sign in и запустить процесс авторизации. Если пользователь трижды вводит неправильный пин-код - очищать стейт приложения, переходить на страницу sign in.

Дополнительные задания (по желанию, не обязательно)

4. Реализовать snack-bar feature - показ уведомлений пользователю. По дефолту показывать 3000мс. Показывать ошибки API или предусмотренные в дизайне в снеках. Для ошибок API использовать поле msgUser
6. Сделать автоформатирование телефона типа +7 (999) 333-22-22. Провалидировать тектовые поля на этапе заполнения.
5. Сделать анимированный лоадер (см. дизайн)
6. Сверстать экраны (см. дизайн)
7. При успешном логине сделать запрос GET /account/profile и вывести в консоль

Внимание!
Зарегистрироваться со своим номером телефона вы сможете только 1 раз.

## Дизайн

https://www.figma.com/file/ifEf06dcm84dEc6sRGdB1D/Kode-test-task

## API

API Endpoint url: https://www.utair.ru/mobile/api/v8

```
Session Guest - получение токенов
POST /sessions/guest
type Body = {"appVersion":"Web","brandName":"Web","lang":"ru","model":"Web","osVersion":"Web","platform":"web","screenResolution":"Web","udid":"65eeaed8-ce5e-414f-9193-5246b913bdec"}

udid генерируется 1 раз на клиенте с помощью uuid и сохраняется на устройстве

пример ответа:
{"token": "75f7da80165bb8f93381ec4ad28a34ef6b80ea0aa7e9f3ce5d829fab5646eaad", "refresh_token": "c4fac2e41aef1e3bbeaa91628911f5563379640bce6a6757583f7c08a99c6f55"}
```

```
Session Refresh - обновление токена (использовать на ошибку "AuthTokenExpired")
POST /sessions/refresh
type Body = {
  refresh_token: string
}
пример ответа:
{"token": "75f7da80165bb8f93381ec4ad28a34ef6b80ea0aa7e9f3ce5d829fab5646eaad", "refresh_token": "c4fac2e41aef1e3bbeaa91628911f5563379640bce6a6757583f7c08a99c6f55"}
```

### Полученный токен использовать во всех остальных запросах, передавая его в Headers, например "Authorization: Token 75f7da80165bb8f96b81ec4ad28a34ef6b80ea0aa7e9f3ce5d829fab5646eaad"

```
Вход, повторная отправка кода подтверждения
POST /account/profile/login
type Body = { login: string }

login - номер телефона (например, '79995554433') или email (например, 'trainee@appkode.ru')

пример ответа:
{"attemptId": "a955002f-9166-4a3c-8d1e-c488c9d772b7", "channel": "phone"}
```

```
Регистрация
POST /account/profile
type Body = { login: string; confirmationGDPRDate: number}

login - номер телефона, например '79995554433'
confirmationGDPRDate - timestamp на момент отправки запроса

```

```
Подтверждение входа
POST /account/profile/login/confirm
type Body = {
  attemptId: string;
  code: string
}

attemptId - из результатов запроса login
code - введенный пользователем 4значный код (например, '1234')

пример ответа:
{"success": true, "id": "some id", "krrParams": {"krrAccessToken": "some access token", "krrRefreshToken": "some refresh tokne", "loginConfirmCookie": "some cookie"}}
```

```
Получение данных профиля
GET /account/profile

пример ответа:
{"profileData": {"address": {}, "block": {}, "cards": [], "categories": [], "channels": {"email": {"verified": true}, "phone": {"verified": true}, "sms": {"verified": true}}, "documents": [{"number": "2400111222", "type": "PS"}], "email": "email@gmail.com", "initials": {"original": {"name": "", "secondName": "", "surname": ""}}, "initialsIsEditable": true, "phone": "+79995554433", "security": {}, "status": {"cardNo": "1024444228"}, "gender": ""}, "bonusData": {"qualifying": 0, "household": 0, "level": null, "redemption": 0, "levelExpire": null}, "profileIsLimited": true}
```

## Результат

Результат разместить в ПРИВАТНОМ личном github, добавить в коллабораторы https://github.com/vsfront
