---
title: "Как подключить аккаунт мессенджера к NN Agent по API"
description: "Порядок вызовов NN Agent для подключения аккаунта: открыть сессию авторизации, дождаться кода, передать код и облачный пароль при двухфакторной защите."
---

Подключение аккаунта к NN Agent по API идёт через сессию авторизации: вы открываете сессию, мессенджер присылает код на номер, вы передаёте код обратно в сессию. При включённой двухфакторной защите добавляется ещё один шаг — облачный пароль.

Перед началом получите bearer-токен, см. [Авторизацию](../api/authentication.md).

<!-- widget:stepper -->

### Откройте сессию авторизации

```bash
curl -X POST https://api.nexifyneo.com/v2/accounts/auth-sessions \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d 'YOUR_JSON_BODY'
```

В ответе вернётся идентификатор сессии в формате UUID. Все следующие шаги адресуются им, а не идентификатором аккаунта: аккаунта пока не существует.

Альтернатива для входа по QR-коду вместо кода из сообщения — `POST /v2/accounts/auth-sessions/qr`.

### Дождитесь, пока мессенджер пришлёт код

Опрашивайте состояние сессии, пока она не перейдёт в ожидание кода:

```bash
curl https://api.nexifyneo.com/v2/accounts/auth-sessions/AUTH_ID \
  -H "Authorization: Bearer YOUR_API_TOKEN"
```

Если код не пришёл, запросите повторную отправку: `POST /v2/accounts/auth-sessions/{auth_id}/resend-code`.

### Передайте код подтверждения

```bash
curl -X POST https://api.nexifyneo.com/v2/accounts/auth-sessions/AUTH_ID/code \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d 'YOUR_JSON_BODY'
```

После этого снова проверьте состояние сессии: она либо завершится успехом, либо запросит облачный пароль.

### Передайте облачный пароль, если включена двухфакторная защита

```bash
curl -X POST https://api.nexifyneo.com/v2/accounts/auth-sessions/AUTH_ID/password \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d 'YOUR_JSON_BODY'
```

Этот шаг нужен только аккаунтам с двухфакторной защитой. Без неё сессия завершается сразу после кода.

### Убедитесь, что аккаунт появился

```bash
curl https://api.nexifyneo.com/v2/accounts \
  -H "Authorization: Bearer YOUR_API_TOKEN"
```

Новый аккаунт присутствует в списке со своим UUID. Этот UUID дальше используется для [диалогов](../api/dialogs.md) и для привязки к кампании.

<!-- /widget -->

## Если что-то пошло не так

- Незавершённую сессию можно прервать: `DELETE /v2/accounts/auth-sessions/{auth_id}`.
- Ответ `401` означает проблему с токеном, а не с аккаунтом — см. [Авторизацию](../api/authentication.md).
- Обрыв на проверке TLS-сертификата не связан ни с токеном, ни с сессией: `api.nexifyneo.com` отдаёт самоподписанный сертификат, см. [Известные ограничения](../api/overview.md).

## Какие аккаунты подключать

Для исходящих подключайте отдельные аккаунты, а не личные. Решение о блокировке принимает площадка, и при потере одного аккаунта переписка и контакты остаются в NN Agent — вы продолжите с другого. Разбор в [частых вопросах](../faq.md).

## Дальше

- [Запустить первую кампанию по API](./first-campaign.md) — что делать с подключённым аккаунтом.
- [Аккаунты в API](../api/accounts.md) — полный перечень операций.
