Empire Streets — RAGE:MP

Кодовая база спроектирована по принципам DDD/Hexagonal (Ports & Adapters) с четким разделением Domain/Application/Infrastructure/Presentation. Цель — обеспечить масштабируемость и читаемость.

Основные подсистемы

- Профиль/Регистрация
- Кастомизация персонажа
- Предметы/Инвентарь
- HUD/Меню игрока
- Голосовой чат (интеграция с RAGE:MP)
- Кланы/Рейтинг/Битвы/Захват территории
- Транспорт
- Бизнесы

Технологии

- Server: C#, netcoreapp3.1, EF Core (PostgreSQL), Redis (кэш/рейтинги)
- Client: TypeScript (RAGE:MP client bridge)
- UI: Vue 3 (TS), Pinia, адаптивная верстка (CEF)

Архитектура (слои)

- Domain: чистые сущности, value objects, доменные события.
- Application: use cases (CQRS), порты (интерфейсы), валидация, транзакции.
- Infrastructure: EF Core, PostgreSQL, Redis, репозитории, конфигурации, миграции.
- Presentation (RageServer): обработчики событий RAGE:MP, RemoteEvents, интеграция с Application.
- Client/UI: TS‑мост к серверу, CEF‑интерфейс на Vue 3 + Pinia.

Запуск окружения (БД/кэш)

1. Установите Docker Desktop.
2. Запустите: `docker compose up -d`
   - PostgreSQL: localhost:5432 (user: empire, pass: empire, db: empire)
   - Redis: localhost:6379

Сборка сервера

- Требуется netcoreapp3.1.
- В каталоге `server` создайте solution и проекты (если не используете уже добавленные .csproj):
  - `dotnet new sln -n EmpireStreets`
  - `dotnet sln add src/EmpireStreets.Domain/EmpireStreets.Domain.csproj`
  - `dotnet sln add src/EmpireStreets.Application/EmpireStreets.Application.csproj`
  - `dotnet sln add src/EmpireStreets.Infrastructure/EmpireStreets.Infrastructure.csproj`
  - `dotnet sln add src/EmpireStreets.RageServer/EmpireStreets.RageServer.csproj`

Пакеты NuGet

- Microsoft.EntityFrameworkCore
- Npgsql.EntityFrameworkCore.PostgreSQL
- StackExchange.Redis
- Microsoft.Extensions.Configuration.\*
- Microsoft.Extensions.DependencyInjection
- RAGE:MP C# API (GTANetworkAPI)

RAGE:MP интеграция

- Серверный модуль: `server/src/EmpireStreets.RageServer`.
- Клиентский мост: `client/` (TS) — вызывает RemoteEvents и управляет CEF.
- UI (CEF): `ui/` (Vue 3 + Pinia). Сборка UI попадает в client‑пакет.

Сборка фронта

- client: сборка TS → `client/dist/client_packages/...`
- ui: Vite build → копирование артефактов в `client/dist/client_packages/empire_streets/ui/`

Дальнейшие шаги

- Реализовать use cases по фичам (регистрация, инвентарь, кланы, бои, территории, бизнесы).
- Добавить миграции EF Core и сиды.
- Расширять UI по экранам: логин/регистрация, HUD, инвентарь, кланы, карта территорий, транспорт, бизнес‑панель.
