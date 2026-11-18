# Структура репозитория

```
statnote/
├── src/
│   ├── StatNote.Sdk/          публичные контракты, без зависимостей
│   ├── StatNote.Core/         AST, типы, семантика, словари
│   ├── StatNote.Parser/       ANTLR4, построение дерева
│   ├── StatNote.Transpiler/   дерево в C#
│   ├── StatNote.Compiler/     Roslyn, контекст, кэш ячеек
│   ├── StatNote.Runtime/      объекты, тензоры, таблицы, статистика
│   ├── StatNote.Package/      менеджер пакетов
│   ├── StatNote.Registry/     инструменты реестра
│   ├── StatNote.TestKit/      тестовый SDK для авторов пакетов
│   ├── StatNote.Export/       PDF, HTML, PPTX
│   ├── StatNote.UI/           Avalonia, пять вкладок
│   └── StatNote.Cli/          команды run, build, compile, pkg
├── tests/
│   ├── golden/                эталоны AST, C#, графов резолвера
│   ├── end_to_end/            сквозные документы
│   └── benchmarks/            аллокации и производительность
├── docs/                      документация и спецификации
├── examples/                  учебные документы .smd
├── packages/                  локальные пакеты рабочей области
├── vendor/                    офлайн-копии зависимостей
└── statnote.lock
```

Реестр живёт в отдельном git-репозитории и не входит в этот.

## Правила направления зависимостей

- `Sdk` не зависит ни от чего, кроме базовой библиотеки.
- `Core` зависит только от `Sdk`.
- `Parser` зависит от `Core`.
- `Transpiler` зависит от `Core` и `Sdk`.
- `Compiler` зависит от `Transpiler`, `Runtime`, `Package`.
- `Runtime` зависит от `Sdk` и `Core`.
- `Export` и `UI` зависят от `Compiler`, но не друг от друга.
- `Cli` собирает всё; на него не зависит никто.

Циклы между проектами запрещены и проверяются заданием CI.

## Правило размещения

Обращение к внешним средам допускается только в `Runtime` и `Package`.
`Core`, `Parser`, `Transpiler` остаются чистыми и тестируемыми без R и Python.
