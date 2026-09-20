---
title: "Кампании и контакты в API NN Agent: сборка рассылки"
description: "Операции NN Agent для кампаний по API: создание и обновление кампании, загрузка и выгрузка контактов, пул сообщений и привязка аккаунтов."
---

# Кампании и контакты в API

Кампания собирается из трёх частей: контакты, пул вариантов первого сообщения и привязанные аккаунты. Ниже операции версии `v2`; идентификатор клиента подставляется по токену.

## Кампании

<!-- widget:api -->

## GET /v2/campaigns

Список кампаний клиента.

## POST /v2/campaigns

Создать кампанию. Тело запроса — JSON.

## GET /v2/campaigns/{campaign_id}

Карточка кампании.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## POST /v2/campaigns/{campaign_id}/update

Изменить параметры кампании. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## DELETE /v2/campaigns/{campaign_id}

Удалить кампанию вместе с её контактами и пулом сообщений.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

<!-- /widget -->

## Аккаунты кампании

<!-- widget:api -->

## POST /v2/campaigns/{campaign_id}/accounts

Привязать аккаунты к кампании: с них пойдут исходящие. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## DELETE /v2/campaigns/{campaign_id}/accounts

Отвязать аккаунты от кампании. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

<!-- /widget -->

## Контакты

Контакты кампании загружаются файлом и выгружаются в том же формате. Параметр `format` принимает только `csv` или `xlsx`, по умолчанию `csv`.

<!-- widget:api -->

## POST /v2/campaigns/{campaign_id}/contacts/upload

Загрузить базу контактов файлом. Тело запроса — `multipart/form-data` с полем `file`.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |
| `format` | string | no | `csv` или `xlsx`, по умолчанию `csv` |

## GET /v2/campaigns/{campaign_id}/contacts

Список контактов кампании.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## GET /v2/campaigns/{campaign_id}/contacts/stats

Сводка по контактам кампании.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## GET /v2/campaigns/{campaign_id}/contacts/export

Выгрузить контакты кампании файлом.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |
| `format` | string | no | `csv` или `xlsx`, по умолчанию `csv` |

## DELETE /v2/campaigns/{campaign_id}/contacts

Очистить базу контактов кампании.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

<!-- /widget -->

## Пул сообщений

Пул хранит варианты первичного сообщения. NN Agent берёт из него текст для очередного контакта, поэтому одинаковый текст не уходит всей базе подряд.

<!-- widget:api -->

## GET /v2/campaigns/{campaign_id}/message-pool

Список вариантов первого сообщения кампании.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## POST /v2/campaigns/{campaign_id}/message-pool

Добавить вариант сообщения в пул. Тело запроса — JSON.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |

## DELETE /v2/campaigns/{campaign_id}/message-pool/{message_id}

Удалить один вариант из пула.

| Field | Type | Required | Description |
|---|---|---|---|
| `campaign_id` | string | yes | UUID кампании |
| `message_id` | string | yes | UUID варианта сообщения |

<!-- /widget -->

## Дальше

- [Запустить первую кампанию по API](../guides/first-campaign.md) — порядок вызовов целиком.
- [Диалоги](./dialogs.md) — что происходит после отправки первого сообщения.
- [AI Outreach](../features/ai-outreach.md) — та же механика в кабинете.
