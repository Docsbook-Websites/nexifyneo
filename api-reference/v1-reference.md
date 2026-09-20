---
title: "Справочник v1 API NN Agent: полный перечень операций"
description: "Все операции первой версии API NN Agent с путями, параметрами и назначением, а также подсказка, чем их заменить в актуальной версии v2."
---

# Справочник v1: полный перечень операций

Версия `v1` осталась от предыдущего поколения API NN Agent. Она работает, но для новой интеграции берите [версию v2](./overview.md): там идентификатор клиента подставляется по токену, а действие выражено HTTP-методом, а не путём.

Ниже перечислены все 43 операции `v1` вне парсера. Парсер разобран отдельно на странице [Парсер](./parser.md).

Столбец «Тело» показывает, что операция ожидает в теле запроса: `JSON`, `файл` для загрузки `multipart/form-data` или прочерк, если тело не требуется. Состав полей JSON опубликованная спецификация не описывает.

## Клиент и лицензии

| Метод | Путь | Назначение | Параметры пути и запроса | Тело |
|---|---|---|---|---|
| `POST` | `/v1/client_create` | Создать клиента | — | JSON |
| `GET` | `/v1/client_id/{tg_id}` | Идентификатор клиента по Telegram ID | `tg_id` | — |
| `POST` | `/v1/client_id/resolve` | Определить идентификатор клиента | — | JSON |
| `GET` | `/v1/client_info/{client_id}` | Данные клиента | `client_id` | — |
| `POST` | `/v1/change_password` | Сменить пароль | — | JSON |
| `GET` | `/v1/licenses/{client_id}` | Лицензии клиента | `client_id` | — |
| `POST` | `/v1/activate_code` | Активировать код | — | JSON |
| `POST` | `/v1/bot/client_token/reissue` | Перевыпустить токен клиента (внутренняя операция бота) | — | JSON |

## Кампании

| Метод | Путь | Назначение | Параметры пути и запроса | Тело |
|---|---|---|---|---|
| `POST` | `/v1/create_campaign` | Создать кампанию | — | JSON |
| `GET` | `/v1/campaign_list/{client_id}` | Список кампаний клиента | `client_id` | — |
| `GET` | `/v1/campaign_info/{client_id}/{campaign_id}` | Карточка кампании | `client_id`, `campaign_id` | — |
| `DELETE` | `/v1/campaign` | Удалить кампанию | — | JSON |
| `POST` | `/v1/update_campaign` | Обновить кампанию | — | JSON |
| `POST` | `/v1/campaigns/{campaign_id}/message_pool` | Добавить вариант в пул сообщений кампании | `campaign_id`, `client_id` | JSON |
| `GET` | `/v1/campaigns/{campaign_id}/message_pool` | Пул сообщений кампании | `campaign_id` | — |
| `DELETE` | `/v1/campaigns/{campaign_id}/message_pool/{message_id}` | Удалить вариант из пула кампании | `campaign_id`, `message_id` | — |

## Контакты

| Метод | Путь | Назначение | Параметры пути и запроса | Тело |
|---|---|---|---|---|
| `POST` | `/v1/contacts/upload/csv` | Загрузить контакты из CSV | `client_id`, `campaign_id` | файл |
| `POST` | `/v1/contacts/upload/excel` | Загрузить контакты из Excel | `client_id`, `campaign_id` | файл |
| `DELETE` | `/v1/contacts/clear` | Очистить контакты | `client_id`, `campaign_id` | — |
| `GET` | `/v1/contacts` | Список контактов | — | — |
| `GET` | `/v1/contacts/short-stats` | Сводка по контактам | — | — |
| `GET` | `/v1/contacts/export` | Выгрузить контакты в CSV | — | — |
| `GET` | `/v1/contacts/export/excel` | Выгрузить контакты в Excel | — | — |

## Пул сообщений клиента

| Метод | Путь | Назначение | Параметры пути и запроса | Тело |
|---|---|---|---|---|
| `POST` | `/v1/message-pool/upload` | Загрузить пул сообщений клиента | `client_id`, `campaign_id` | JSON |
| `GET` | `/v1/message-pool` | Пул сообщений клиента | `client_id`, `campaign_id` | — |
| `DELETE` | `/v1/message-pool/clear` | Очистить пул сообщений клиента | `client_id`, `campaign_id` | — |

## Аккаунты и сессии авторизации

| Метод | Путь | Назначение | Параметры пути и запроса | Тело |
|---|---|---|---|---|
| `POST` | `/v1/accounts/auth-sessions` | Открыть сессию авторизации | — | JSON |
| `POST` | `/v1/accounts/auth-sessions/qr` | Открыть сессию авторизации по QR | — | JSON |
| `POST` | `/v1/accounts/auth-sessions/{auth_id}/code` | Передать код подтверждения | `auth_id` | JSON |
| `POST` | `/v1/accounts/auth-sessions/{auth_id}/password` | Передать облачный пароль | `auth_id` | JSON |
| `POST` | `/v1/accounts/auth-sessions/{auth_id}/resend-code` | Повторно отправить код | `auth_id` | — |
| `GET` | `/v1/accounts/auth-sessions/{auth_id}` | Состояние сессии авторизации | `auth_id` | — |
| `DELETE` | `/v1/accounts/auth-sessions/{auth_id}` | Прервать сессию авторизации | `auth_id` | — |
| `POST` | `/v1/update_account` | Обновить аккаунт | — | JSON |
| `GET` | `/v1/clients/{client_id}/accounts` | Аккаунты клиента | `client_id` | — |
| `POST` | `/v1/clients/{client_id}/campaigns/{campaign_id}/accounts/attach` | Привязать аккаунты к кампании | `client_id`, `campaign_id` | JSON |
| `POST` | `/v1/clients/{client_id}/campaigns/{campaign_id}/accounts/detach` | Отвязать аккаунты от кампании | `client_id`, `campaign_id` | JSON |
| `DELETE` | `/v1/clients/{client_id}/accounts/{account_id}` | Отключить аккаунт | `client_id`, `account_id` | — |
| `GET` | `/v1/accounts/{account_id}` | Карточка аккаунта | `account_id` | — |

## Диалоги

| Метод | Путь | Назначение | Параметры пути и запроса | Тело |
|---|---|---|---|---|
| `GET` | `/v1/accounts/{account_id}/dialogs` | Список диалогов | `account_id` | — |
| `GET` | `/v1/accounts/{account_id}/dialogs/unread` | Непрочитанные диалоги | `account_id` | — |
| `GET` | `/v1/accounts/{account_id}/dialogs/{peer_user_id}` | Сообщения диалога | `account_id`, `peer_user_id` | — |
| `POST` | `/v1/accounts/{account_id}/dialogs/send` | Отправить сообщение в диалог | `account_id` | JSON |

## Дальше

- [Обзор API](./overview.md) — таблица соответствия `v1` и `v2`.
- [Парсер](./parser.md) — операции сбора, доступные только в `v1`.
- [Ошибки](./errors.md) — коды ответов и `request_id`.
