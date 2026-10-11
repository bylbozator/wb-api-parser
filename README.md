<div align="center">

# WB API Parser

**Парсер выгрузок Wildberries-подобного API в MySQL**

[![PHP](https://img.shields.io/badge/PHP-Laravel-FF2D20?logo=laravel&logoColor=white)](https://laravel.com/)
[![MySQL](https://img.shields.io/badge/MySQL-Aiven-4479A1?logo=mysql&logoColor=white)](https://www.aiven.io/)
[![Streaming](https://img.shields.io/badge/CLI-artisan-339933?logo=php&logoColor=white)](https://laravel.com/docs/artisan)

Загрузка данных о продажах, заказах, остатках и доходах из API маркетплейса в
реляционную базу данных. Реализовано на **Laravel** с обязательным SSL-подключением
к MySQL.

</div>

---

## Возможности

- Выгрузка четырёх сущностей из API: **sales**, **orders**, **stocks**, **incomes**.
- Персистентное хранение в MySQL (Aiven) с SSL-подключением.
- Консольная команда `app:fetch-api-data` с фильтром по отдельной сущности.
- Гибкая конфигурация через `.env` (хост, порт, база, пользователь, API key).
- Миграции для всех таблиц (`0001_01_01_*` — пользователи, кэш, очередь; `2026_06_30_*` — предметная модель).

## Модель данных

| Таблица   | Назначение                         |
|-----------|------------------------------------|
| `sales`   | Продажи                        |
| `orders`  | Заказы                          |
| `stocks`  | Остатки складов                  |
| `incomes` | Доходы / поступления             |

## Установка и запуск

```bash
composer install

cp .env.example .env
php artisan key:generate

php artisan migrate

# Загрузка всех сущностей
php artisan app:fetch-api-data

# Или одной конкретной
php artisan app:fetch-api-data sales
php artisan app:fetch-api-data orders
php artisan app:fetch-api-data stocks
php artisan app:fetch-api-data incomes
```

### Переменные окружения

```env
DB_HOST=mysql-1fdee782-d805.h.aivencloud.com
DB_PORT=24405
DB_DATABASE=defaultdb
DB_USERNAME=avnadmin
DB_PASSWORD=ваш_пароль
DB_SSLMODE=required

API_BASE_URL=http://example.com/api
API_KEY=ваш_ключ
```

## Структура

```
app/Console/Commands/FetchApiData.php   — консольная команда загрузки данных
app/Models/User.php                     — модель пользователя
database/migrations/                    — схема БД
routes/console.php                      — регистрация команды
```

## Зачем это

Парсер используется для регулярной синхронизации торговых данных маркетплейса
во внутреннюю аналитическую базу: таблицы остатков и продаж — исходники для
отчётов о товарных позициях и финансовых показателях.