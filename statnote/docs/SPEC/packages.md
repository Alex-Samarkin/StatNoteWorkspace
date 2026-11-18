# Пакеты и менеджер

Статус: DRAFT 0.1. Схема манифеста — врата F1. Финализируется в M13.

## Виды пакетов

`library` — исходники C# в `src/`, тесты в `tests/`.
`meta` — только зависимости и exports, кода нет.

## Манифест StatNote.toml

```toml
[package]
name = "statnote.org/med/survival"
uuid = "b7e2c1d4-0000-4000-8000-000000000001"
version = "1.4.2"
kind = "library"
sdk = "1.0"
license = "MIT"
description = { ru = "Анализ выживаемости", en = "Survival analysis" }

[dependencies]
"statnote.org/core/stats" = "1.2"
```

Мультипакет:

```toml
[package]
name = "statnote.org/med/bundle"
uuid = "b7e2c1d4-0000-4000-8000-000000000002"
version = "2026.11"
kind = "meta"

[dependencies]
"statnote.org/med/survival" = "1.4"
"statnote.org/med/pk" = "0.9"

[exports]
namespaces = ["StatNote.Med.Survival", "StatNote.Med.PK"]
```

## Поля манифеста

| Поле | Обязательное | Правило |
|---|---|---|
| name | да | модульный путь, строчные буквы, без пробелов |
| uuid | да для library | стабильный идентификатор, не меняется при переименовании |
| version | да | SemVer |
| kind | да | library или meta |
| sdk | да для library | мажорная версия контрактов |
| license | да | SPDX-идентификатор |
| dependencies | нет | путь = минимальная версия |

## Идентичность и мажорные версии

Путь плюс UUID. Мажорная версия 2 и выше — отдельный путь `.../pkg/v2`.
Это допускает сосуществование несовместимых версий в одном графе.

## Разрешение версий MVS

Для каждого модуля выбирается максимальная из минимально требуемых версий,
заявленных всеми участниками графа. Никакого поиска новейшей совместимой.
Результат детерминирован и не зависит от порядка обхода.

## Замок statnote.lock

TOML со списком: путь, uuid, точная версия, контентный хеш дерева исходников,
источник. Файл коммитится в git. Restore обязан воспроизводить его побитово.

## Реестр

Git-репозиторий метаданных: `Registry.toml`, каталоги по первой букве пути,
`Package.toml` с uuid и адресом репозитория, `Versions.toml` с версиями,
хешами и зависимостями. Регистрация через рецензируемый запрос.

Proxy-кэш: `GET /<module>/@v/list`, `/<module>/@v/<ver>.toml`, `/<module>/@v/<ver>.zip`.
Журнал контрольных сумм проверяется при каждом restore.

## Подключение в документе

```
[Преамбула]
{
    @packages { "statnote.org/med/bundle" = "2026.11"; }
    using Medical from package("statnote.org/med/bundle");
}
```

## Команды CLI

`statnote pkg init | add | restore | build | test | update | why | graph | vendor | publish | audit`

## Кэш и изоляция

Сборка Roslyn кэшируется по ключу: хеш исходников, версия SDK, TFM, RID.
Загрузка в коллектируемый AssemblyLoadContext; контракты SDK в общем контексте.

## Доверие

Подписи реестра, журнал сумм, `pkg audit` перед публикацией, yank вместо удаления.
См. `SECURITY_AND_TRUST.md`.
