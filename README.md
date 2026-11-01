# Ask Ads · Claude Plugins

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

Маркетплейс плагинов Claude для рекламных кабинетов: **Яндекс Директ, VK Ads, Яндекс Метрика, Вебмастер и Вордстат** прямо в диалоге с Claude. От команды [Ask Ads](https://askads.ru) — чат-аналитика и сторожа рекламных кабинетов.

## Быстрый старт

**Claude Code** — две команды, каждая отдельным сообщением:

```
/plugin marketplace add askads/claude-plugins
```

```
/plugin install yandex-direct@askads
```

**Claude Desktop** — без команд, через интерфейс: Settings → Plugins → **Add from repository** → `askads/claude-plugins` → выбрать плагины.

**Claude в браузере (claude.ai)** — плагины не нужны, тот же коннектор добавляется напрямую: Settings → Connectors → **Add custom connector** → `https://mcp.askads.cloud/mcp`.

Первое обращение к Директу предложит войти через Яндекс (OAuth) — и можно спрашивать: «какие кампании тратят бюджет без конверсий?», «сколько денег на счёте?», или запустить готовый аудит: `/yandex-direct:audit`.

**Claude пишет, что не видит тулы Директа?** Коннектор ещё не авторизован:

- Claude Code: `/mcp` → выбрать `yandex-direct` → Authenticate (вход через Яндекс);
- Desktop / браузер: Settings → Connectors → коннектор Ask Ads → Connect, затем открыть новый чат.

## Плагины

| Плагин | Что даёт | Что нужно |
| --- | --- | --- |
| **`yandex-direct`** | Директ **без установки и токенов**: удалённый read-only коннектор Ask Ads (кампании, статистика, баланс и овердрафт, конверсии Метрики, данные Вебмастера) + скилл и команда `/yandex-direct:audit` | Только аккаунт Яндекса — вход по OAuth |
| `yandex-direct-api` | Полный Yandex Direct API v5 через локальный [mcp-yandex-direct](https://github.com/askads/mcp-yandex-direct): статистика, фразы, ставки, бюджеты — включая write-инструменты | Node.js 20+; вход через Яндекс прямо в чате |
| `vk-ads` | VK Реклама через [mcp-vk-ads](https://github.com/askads/mcp-vk-ads): кампании, баннеры, статистика, бюджеты | Node.js 20+; в чате — `client_id` и `client_secret` приложения VK Ads API |
| `yandex-metrica` | Яндекс Метрика через [mcp-yandex-metrica](https://github.com/askads/mcp-yandex-metrica): счётчики, цели, отчёты по трафику и конверсиям | Node.js 20+; вход через Яндекс прямо в чате |
| `yandex-webmaster` | Яндекс Вебмастер через [mcp-yandex-webmaster](https://github.com/askads/mcp-yandex-webmaster): диагностика сайта, индексация, поисковые запросы, sitemap, внешние ссылки, переобход | Node.js 20+; вход через Яндекс прямо в чате |
| `yandex-wordstat` | Поисковый спрос Вордстата через [mcp-yandex-wordstat](https://github.com/askads/mcp-yandex-wordstat): частотность, похожие запросы, сезонность и география | Node.js 20+, API-ключ и ID каталога Yandex Cloud (спросит при включении) |

Директ, Метрика, Вебмастер и VK Ads подключаются прямо в диалоге: скажите «подключи Яндекс Директ» (или Метрику, Вебмастер, ВК Рекламу), и сервер проведёт через вход. Поле токена при включении плагина можно оставить пустым — оно нужно только для CI и кабинетов клиентов агентства в VK Ads. Вордстату нужны API-ключ и ID каталога Yandex Cloud: их плагин спросит при включении. Всё, что введено при включении, хранится в системном keychain — руками править конфиги не нужно.

## Флагман: `yandex-direct`

Использует удалённый MCP-эндпоинт `https://mcp.askads.cloud/mcp`:

- **Read-only по построению** — сервер физически не может ничего изменить в кабинете;
- OAuth 2.1 + PKCE через ваш аккаунт Яндекса; пароль Ask Ads не видит, доступ отзывается в любой момент (в настройках Яндекса или отключением плагина);
- токен Яндекса хранится зашифрованным на сервере и **никогда не передаётся ассистенту**;
- ничего не нужно устанавливать — подходит и тем, у кого нет Node.js.

Подробнее — на [mcp.askads.cloud](https://mcp.askads.cloud) ([privacy](https://mcp.askads.cloud/privacy-policy) · [terms](https://mcp.askads.cloud/user-agreement)).

## English

Claude plugin marketplace for Russian ad platforms — **Yandex Direct, VK Ads, Yandex Metrica, Webmaster and Wordstat**. The flagship `yandex-direct` plugin is a zero-setup, strictly read-only remote connector (OAuth with your Yandex account, nothing to install). The other plugins run the open-source MCP servers locally; Direct, Metrica, Webmaster and VK Ads connect right in the chat, so the token field on enable can stay empty (it is for CI only). Wordstat asks for a Yandex Cloud API key and folder ID on enable.

In Claude Code, run the two commands below one at a time; in Claude Desktop use Settings → Plugins → **Add from repository** → `askads/claude-plugins`; on claude.ai add the connector directly: Settings → Connectors → **Add custom connector** → `https://mcp.askads.cloud/mcp`.

```
/plugin marketplace add askads/claude-plugins
```

```
/plugin install yandex-direct@askads
```

Main product — [askads.ru](https://askads.ru), a chat analyst and watchdog for ad accounts.

## Лицензия

[MIT](./LICENSE) · Поддержка: [support@askads.ru](mailto:support@askads.ru) · Telegram [@gistrec](https://t.me/gistrec)
