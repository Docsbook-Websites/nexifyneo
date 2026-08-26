---
title: "Как запустить первую кампанию NN Agent по API"
description: "Соберите кампанию NN Agent запросами: создайте кампанию, загрузите контакты файлом, наполните пул первых сообщений и привяжите аккаунты."
---

Кампания в NN Agent собирается из трёх частей: контакты, пул вариантов первого сообщения и привязанные аккаунты. Ниже порядок вызовов версии `v2`.

Перед началом подключите хотя бы один аккаунт — см. [Подключить аккаунт по API](./connect-account.md).

<!-- widget:stepper -->

### Создайте кампанию

```bash
curl -X POST https://api.nexifyneo.com/v2/campaigns \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d 'YOUR_JSON_BODY'
```

В ответе вернётся идентификатор кампании в формате UUID. Дальше он подставляется в путь всех остальных запросов.

### Загрузите базу контактов

Контакты загружаются файлом, а не построчно. Поддерживаются CSV и XLSX; формат указывается параметром запроса `format`, по умолчанию `csv`.

```bash
curl -X POST "https://api.nexifyneo.com/v2/campaigns/CAMPAIGN_ID/contacts/upload?format=csv" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -F "file=@contacts.csv"
```

Проверить, что база встала, можно сводкой: `GET /v2/campaigns/{campaign_id}/contacts/stats`.

### Наполните пул первых сообщений

```bash
curl -X POST https://api.nexifyneo.com/v2/campaigns/CAMPAIGN_ID/message-pool \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d 'YOUR_JSON_BODY'
```

Добавьте несколько вариантов, а не один. NN Agent берёт из пула текст для очередного контакта, и это единственный способ не отправить всей базе один и тот же текст символ в символ.

### Привяжите аккаунты

```bash
curl -X POST https://api.nexifyneo.com/v2/campaigns/CAMPAIGN_ID/accounts \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d 'YOUR_JSON_BODY'
```

С привязанных аккаунтов пойдут исходящие. Отвязка выполняется тем же адресом методом `DELETE`.

### Проверьте состав кампании

```bash
curl https://api.nexifyneo.com/v2/campaigns/CAMPAIGN_ID \
  -H "Authorization: Bearer YOUR_API_TOKEN"
```

Сверьте, что у кампании есть контакты, пул сообщений и хотя бы один аккаунт. Кампания без пула или без аккаунта отправлять не с чего и нечего.

<!-- /widget -->

## Как запускается отправка

Отдельной операции «запустить кампанию» опубликованная спецификация не объявляет. Запуск выполняется обновлением кампании (`POST /v2/campaigns/{campaign_id}/update`) либо из кабинета; какие именно поля меняет запуск, уточните у менеджеров в боте `@nn_official_bot`.

## Что дальше происходит с диалогами

После первого касания разговор ведёт AI по вашему сценарию — см. [Чат-флоу](../features/chat-flow.md). Переписку можно читать и продолжать вручную через [операции диалогов](../api/dialogs.md).

Выгрузить базу с результатами обратно: `GET /v2/campaigns/{campaign_id}/contacts/export?format=xlsx`.

## Дальше

- [Кампании и контакты в API](../api/campaigns.md) — полный перечень операций.
- [Диалоги](../api/dialogs.md) — чтение переписки и отправка сообщений.
- [AI Outreach](../features/ai-outreach.md) — как это выглядит в кабинете.
