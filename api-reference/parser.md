---
title: "Парсер в API NN Agent: задачи сбора, проверки и воркеры"
description: "Операции парсера NN Agent: создание и выгрузка задач сбора, проверка сущностей, системные аккаунты для парсинга и состояние воркеров."
---

# Парсер: задачи сбора и воркеры

Парсер собирает данные, из которых потом получается база контактов кампании. Он опубликован только в версии `v1` — в `v2` этих операций нет.

Работа делится на три части: задачи сбора, системные аккаунты, от имени которых сбор идёт, и воркеры, которые его выполняют.

## Задачи сбора

<!-- widget:api -->

## GET /v1/parser/tasks

Список задач сбора.

## POST /v1/parser/tasks

Создать задачу сбора. Тело запроса — JSON.

## GET /v1/parser/tasks/{task_id}

Состояние задачи сбора.

| Field | Type | Required | Description |
|---|---|---|---|
| `task_id` | string | yes | UUID задачи |

## POST /v1/parser/tasks/{task_id}/cancel

Отменить выполняющуюся задачу.

| Field | Type | Required | Description |
|---|---|---|---|
| `task_id` | string | yes | UUID задачи |

## GET /v1/parser/tasks/{task_id}/export

Выгрузить результат задачи.

| Field | Type | Required | Description |
|---|---|---|---|
| `task_id` | string | yes | UUID задачи |

## DELETE /v1/parser/tasks/{task_id}

Удалить задачу вместе с её результатом.

| Field | Type | Required | Description |
|---|---|---|---|
| `task_id` | string | yes | UUID задачи |

## POST /v1/parser/entity-checks

Создать проверку сущности. Тело запроса — JSON.

<!-- /widget -->

## Системные аккаунты

Системные аккаунты — отдельные аккаунты, от имени которых работает парсер. Они подключаются собственной сессией авторизации, устроенной так же, как подключение обычного аккаунта: код, при необходимости облачный пароль.

<!-- widget:api -->

## GET /v1/parser/system-accounts

Список системных аккаунтов парсера.

## GET /v1/parser/system-accounts/{account_id}

Карточка системного аккаунта.

| Field | Type | Required | Description |
|---|---|---|---|
| `account_id` | string | yes | UUID системного аккаунта |

## DELETE /v1/parser/system-accounts/{account_id}

Отключить системный аккаунт.

| Field | Type | Required | Description |
|---|---|---|---|
| `account_id` | string | yes | UUID системного аккаунта |

## POST /v1/parser/system-accounts/auth-sessions

Открыть сессию авторизации системного аккаунта. Тело запроса — JSON.

## GET /v1/parser/system-accounts/auth-sessions/{auth_id}

Состояние сессии авторизации.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии |

## POST /v1/parser/system-accounts/auth-sessions/{auth_id}/code

Передать код подтверждения. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии |

## POST /v1/parser/system-accounts/auth-sessions/{auth_id}/password

Передать облачный пароль при двухфакторной защите. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии |

## DELETE /v1/parser/system-accounts/auth-sessions/{auth_id}

Прервать незавершённую сессию.

| Field | Type | Required | Description |
|---|---|---|---|
| `auth_id` | string | yes | UUID сессии |

<!-- /widget -->

## Воркеры

<!-- widget:api -->

## GET /v1/parser/workers

Список воркеров парсера и их состояние.

## GET /v1/parser/workers/{node_name}

Состояние одного воркера.

| Field | Type | Required | Description |
|---|---|---|---|
| `node_name` | string | yes | Имя узла воркера |

<!-- /widget -->

## Дальше

- [Кампании и контакты](./campaigns.md) — куда попадает собранная база.
- [Справочник v1](./v1-reference.md) — остальные операции первой версии.
