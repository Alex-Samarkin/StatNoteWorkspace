# StatNoteWorkspace

Рабочее пространство проекта **StatNote** — языка статистических вычислений и среды
научного литерного программирования. Здесь же лежит вспомогательный Python-проект
`pythonstart00` с генераторами документации.

## Структура

```
StatNoteWorkspace/
├── .gitignore
├── StatNoteWorkspace.code-workspace   # настройки VS Code для workspace
└── PythonStart00/                     # Python-проект (uv) + документация StatNote
    ├── pyproject.toml                 # пакет pythonstart00 (Python >= 3.14, uv)
    ├── uv.lock
    ├── src/pythonstart00/             # исходники пакета
    ├── CreateDockWithPython.py        # генератор docs: statnote_docs/
    ├── CreateSecondOrderDoc.py        # генератор обвязки проекта: statnote/
    ├── statnote/                      # проект StatNote (C#/.NET) — документация
    └── statnote_docs/                 # концепция, ТЗ, ИИ-промпт, программа разработки
```

## Проекты

### Python — `pythonstart00`

Управляется через [uv](https://docs.astral.sh/uv/), требует Python 3.14+. Содержит
заглушку пакета и два скрипта-генератора, которые создают документацию StatNote:

- `CreateDockWithPython.py` — генерирует `statnote_docs/` (концепция, ТЗ, мастер-промпт,
  программа разработки).
- `CreateSecondOrderDoc.py` — генерирует проектную обвязку `statnote/` (README, контекстные
  артефакты, черновые спецификации, примеры `.smd`).

Установка и запуск:

```bash
cd PythonStart00
uv sync
uv run pythonstart00
```

### StatNote

Язык статистических вычислений и среда научного литерного программирования. Рабочее
название (см. `docs/OPEN_QUESTIONS.md`).

Документ `.smd` объединяет текст, формулы, расчёты и графики; один источник порождает
PDF, HTML и PPTX. Ключевые решения:

- транспиляция в C# на .NET 10 (LTS);
- переменные локальны по умолчанию;
- двухуровневые объекты (`.` — статика, `$` — динамика);
- broadcast с fusion (`f.(x)`, `x .+ y`);
- билингвальность ru/en;
- нативные пакеты на C# в исходных кодах.

**Статус:** F0 — предпроектная документация. Реализация не начата.

## Навигация по документации StatNote

| Документ | Назначение |
|---|---|
| `statnote_docs/01_concept.md` | Концепция (человеческое чтение) |
| `statnote_docs/02_specification.md` | Техническое задание |
| `statnote_docs/03_ai_prompt.md` | Мастер-промпт для ИИ-разработки |
| `statnote_docs/04_development_program.md` | Программа разработки и сохранение контекста |
| `statnote/docs/PROJECT_CONTEXT.md` | Текущее состояние проекта |
| `statnote/docs/DECISIONS.md` | Журнал архитектурных решений (ADR) |
| `statnote/docs/MODULE_STATUS.md` | Матрица модулей M0–M14 |
| `statnote/docs/SPEC/` | Черновые спецификации подсистем |

## Платформы

Windows 10/11 x64, Linux x64 (включая Manjaro), macOS — опционально.

## Лицензия

Не выбрана. См. `statnote/docs/LICENSE_POLICY.md` и открытый вопрос OQ-02.
