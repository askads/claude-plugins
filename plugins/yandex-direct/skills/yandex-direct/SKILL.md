---
name: yandex-direct
description: Working with Yandex Direct ad accounts through the Ask Ads MCP tools — campaigns, performance statistics, budgets, account balance and overdraft, Yandex Metrica conversions. Use when the user asks about their Yandex Direct advertising, spend, clicks, CPC/CPA, keywords, balance or conversion goals.
---

# Яндекс Директ через Ask Ads MCP

Коннектор **строго read-only**: ни один инструмент ничего не меняет в кабинете.
Если пользователь просит что-то изменить (ставки, бюджеты, статусы) — объясни,
что коннектор только читает, и предложи конкретные шаги для ручного изменения
в интерфейсе Директа.

## С чего начинать

1. `get_account_info` — логин, **валюта кабинета**, тип (агентство/рекламодатель).
2. `list_campaigns` — карта кампаний: Id, Name, Type, State (ON/OFF/SUSPENDED…), Status.

Дальше отвечай на вопрос пользователя, фильтруя по `campaignIds`, — не выгружай
весь аккаунт без необходимости.

## Статистика

`get_statistics` — отчёты Reports API:

- `reportType`: чаще всего `CAMPAIGN_PERFORMANCE_REPORT`; по объявлениям — `AD_PERFORMANCE_REPORT`, по фразам — `CRITERIA_PERFORMANCE_REPORT`.
- `dateRangeType`: `LAST_7_DAYS`, `LAST_30_DAYS`, `THIS_MONTH` или `CUSTOM_DATE` + `dateFrom`/`dateTo` (YYYY-MM-DD).
- `fieldNames`: запрашивай только нужное, например `["CampaignId", "CampaignName", "Impressions", "Clicks", "Cost", "Conversions", "CostPerConversion"]`.
- Сузь выборку через `campaignIds`, когда речь об конкретных кампаниях.

**Деньги** в отчётах и списках сервер уже отдаёт **в валюте кабинета** (микроединицы
сконвертированы). Валюту бери из `get_account_info` — не предполагай, что это ₽.

## Баланс и «показы остановлены»

- `get_balance` — баланс единого счёта (Live v4): `Amount` строкой в валюте кабинета,
  **отрицательное значение = задолженность**.
- Остаток овердрафта: `get_account_info` с `fieldNames: ["OverdraftSumAvailable"]` —
  значение в **микроединицах**, дели на 1 000 000.
- «Тратить нечем» (показы встают) только когда `max(баланс, 0) + остаток овердрафта ≤ 0`.
  Минус на балансе при живом овердрафте — показы **идут** (кабинет тратит в кредит);
  не пугай пользователя преждевременно.

## Конверсии из Метрики (если доступны тулы `metrika_*`)

- `metrika_list_counters` — найди счётчик по домену сайта пользователя.
  **counterId — это id счётчика Метрики, не id кампании Директа.**
- `metrika_list_goals` — цели счётчика; `metrika_get_statistics` — визиты/конверсии по целям.
- Если тулов с префиксом `metrika_` нет — бандл Метрики недоступен; конверсии бери
  из полей Директа (`Conversions`, `CostPerConversion` в отчёте).

## Прочие инструменты

Структура: `list_ad_groups`, `list_ads`, `list_keywords`, `get_bid_modifiers`.
Расширения объявлений: `get_sitelinks`, `get_callouts`, `get_vcards`, `get_ad_images`,
`get_ad_videos`, `get_creatives`. Справочники: `get_regions` (id гео), `get_dictionaries`
(валюты, ставки НДС и т.п.). Квота API: `get_quota` (остаток Units за сутки).

## Правила ответов

- Числа сводить в компактные таблицы; проценты и динамику считать самому.
- Выводы («слив бюджета», «фраза не работает») формулировать как рекомендации —
  решение принимает пользователь.
- При ошибке доступа предложи переподключить коннектор (Sign in with Yandex);
  доступ можно отозвать в настройках аккаунта Яндекса.
