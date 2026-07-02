# Ask Ads · Claude Plugins

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

Маркетплейс плагинов Claude для рекламных кабинетов: **Яндекс Директ, VK Ads и Яндекс
Метрика** прямо в диалоге с Claude. От команды [Ask Ads](https://askads.ru) — чат-аналитика
и сторожа рекламных кабинетов.

## Быстрый старт

В Claude Code (или Claude Desktop → вкладка плагинов):

```
/plugin marketplace add askads/claude-plugins
/plugin install yandex-direct@askads
```

Дальше Claude предложит войти через Яндекс (OAuth) — и можно спрашивать:
«какие кампании тратят бюджет без конверсий?», «сколько денег на счёте?»,
или запустить готовый аудит: `/yandex-direct:audit`.

## Плагины

| Плагин | Что даёт | Что нужно |
| --- | --- | --- |
| **`yandex-direct`** | Директ **без установки и токенов**: удалённый read-only коннектор Ask Ads (кампании, статистика, баланс и овердрафт, конверсии Метрики) + скилл и команда `/yandex-direct:audit` | Только аккаунт Яндекса — вход по OAuth |
| `yandex-direct-api` | Полный Yandex Direct API v5 со **своим** токеном через [mcp-yandex-direct](https://github.com/askads/mcp-yandex-direct): статистика, фразы, ставки, бюджеты — включая write-инструменты с подтверждением | Node.js 18+ и OAuth-токен Директа (спросит при включении) |
| `vk-ads` | VK Реклама через [mcp-vk-ads](https://github.com/askads/mcp-vk-ads): кампании, баннеры, статистика, бюджеты | Node.js 18+ и токен VK Ads API (спросит при включении) |
| `yandex-metrica` | Яндекс Метрика через [mcp-yandex-metrica](https://github.com/askads/mcp-yandex-metrica): счётчики, цели, отчёты по трафику и конверсиям | Node.js 18+ и OAuth-токен Метрики (спросит при включении) |

Токены local-плагинов запрашиваются диалогом при включении и хранятся в системном
keychain — руками править конфиги не нужно. Где взять токен — в README соответствующего
сервера (ссылки в таблице).

## Флагман: `yandex-direct`

Использует удалённый MCP-эндпоинт `https://mcp.askads.cloud/mcp`:

- **Read-only по построению** — сервер физически не может ничего изменить в кабинете;
- OAuth 2.1 + PKCE через ваш аккаунт Яндекса; пароль Ask Ads не видит, доступ отзывается
  в любой момент (в настройках Яндекса или отключением плагина);
- токен Яндекса хранится зашифрованным на сервере и **никогда не передаётся ассистенту**;
- ничего не нужно устанавливать — подходит и тем, у кого нет Node.js.

Подробнее — на [mcp.askads.cloud](https://mcp.askads.cloud)
([privacy](https://mcp.askads.cloud/privacy-policy) · [terms](https://mcp.askads.cloud/user-agreement)).

Пользуетесь Claude в браузере без плагинов? Тот же коннектор добавляется вручную:
Settings → Connectors → **Add custom connector** → `https://mcp.askads.cloud/mcp`.

## English

Claude plugin marketplace for Russian ad platforms — **Yandex Direct, VK Ads and Yandex
Metrica**. The flagship `yandex-direct` plugin is a zero-setup, strictly read-only remote
connector (OAuth with your Yandex account, nothing to install); the other plugins run the
open-source MCP servers locally with your own API tokens (prompted on enable, stored in
the system keychain).

```
/plugin marketplace add askads/claude-plugins
/plugin install yandex-direct@askads
```

Main product — [askads.ru](https://askads.ru), a chat analyst and watchdog for ad accounts.

## Лицензия

[MIT](./LICENSE) · Поддержка: [support@askads.ru](mailto:support@askads.ru) · Telegram [@gistrec](https://t.me/gistrec)
