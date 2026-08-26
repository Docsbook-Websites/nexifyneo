---
title: "Аккаунты в API NN Agent: подключение и сессии авторизации"
description: "Операции NN Agent для подключения аккаунтов мессенджеров по API: сессия авторизации, ввод кода и облачного пароля, список и удаление аккаунтов."
---

Аккаунт — подключённый аккаунт мессенджера, от имени которого NN Agent пишет и читает. Подключение идёт через сессию авторизации: вы открываете сессию, мессенджер присылает код, вы передаёте код в сессию.

Все запросы требуют bearer-токен, см. [Авторизацию](./authentication.md).

## Сессии авторизации

<!-- widget:api -->

## POST /v2/accounts/auth-sessions

Открыть сессию авторизации по номеру телефона. Возвращает идентификатор сессии, к которому дальше привязываются код и пароль. Тело запроса — JSON.

## POST /v2/accounts/auth-sessions/qr

Открыть сессию авторизации по QR-коду — альтернатива коду из сообщения. Тело запроса — JSON.

## GET /v2/accounts/auth-sessions/{auth_id}

Состояние сессии: ждёт код, ждёт пароль, завершена, отклонена. Опрашивайте эту операцию между шагами подключения.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии авторизации |

## POST /v2/accounts/auth-sessions/{auth_id}/code

Передать код подтверждения, который мессенджер прислал на номер. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии авторизации |

## POST /v2/accounts/auth-sessions/{auth_id}/password

Передать облачный пароль, если у аккаунта включена двухфакторная защита. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии авторизации |

## POST /v2/accounts/auth-sessions/{auth_id}/resend-code

Запросить повторную отправку кода подтверждения.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии авторизации |

## DELETE /v2/accounts/auth-sessions/{auth_id}

Прервать незавершённую сессию авторизации.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии авторизации |

<!-- /widget -->

Пошаговый разбор этой последовательности — [Подключить аккаунт по API](../guides/connect-account.md).

## Аккаунты

<!-- widget:api -->

## GET /v2/accounts

Список подключённых аккаунтов клиента. Клиент определяется по токену.

## GET /v2/accounts/{account_id}

Карточка одного аккаунта.

| Field | Type | Required | Description |
|---|---|---|---|
| `account_id` | string | yes | UUID аккаунта |

## POST /v2/accounts/{account_id}/update

Изменить параметры аккаунта. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `account_id` | string | yes | UUID аккаунта |

## DELETE /v2/accounts/{account_id}

Отключить аккаунт от NN Agent.

| Field | Type | Required | Description |
|---|---|---|---|
| `account_id` | string | yes | UUID аккаунта |

<!-- /widget -->

## Привязка аккаунтов к кампании

Аккаунты сами по себе ничего не рассылают — они привязываются к кампании. Операции привязки и отвязки описаны на странице [Кампании и контакты](./campaigns.md).

## Дальше

- [Подключить аккаунт по API](../guides/connect-account.md) — порядок вызовов целиком.
- [Диалоги](./dialogs.md) — что читать и отправлять после подключения.
- [Мульти-аккаунтинг](../features/multi-accounting.md) — та же механика в кабинете.
