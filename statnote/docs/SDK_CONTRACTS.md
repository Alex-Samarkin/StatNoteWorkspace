# SDK_CONTRACTS

Статус: DRAFT 0.1. Финализируется в M12. Изменение — только через ADR BREAKING.

## Назначение

Публичные контракты для авторов пакетов на C#. Проект `StatNote.Sdk` не имеет
зависимостей, кроме базовой библиотеки, и публикуется отдельно.

## Атрибуты

```csharp
[AttributeUsage(AttributeTargets.Class)]
public sealed class StatModuleAttribute : Attribute
{
    public string Name { get; init; } = string.Empty;
    public string RuName { get; init; } = string.Empty;
}

[AttributeUsage(AttributeTargets.Method)]
public sealed class StatFunctionAttribute : Attribute
{
    public string Name { get; init; } = string.Empty;
    public string RuName { get; init; } = string.Empty;
}

[AttributeUsage(AttributeTargets.Method)]
public sealed class StatPureAttribute : Attribute { }

[AttributeUsage(AttributeTargets.Method | AttributeTargets.Class)]
public sealed class StatDocAttribute : Attribute
{
    public string Ru { get; init; } = string.Empty;
    public string En { get; init; } = string.Empty;
}
```

Дополнительно: `StatTypeAttribute`, `StatCellAttributeAttribute`.

## Интерфейсы

```csharp
public interface IStatModule
{
    void Register(IModuleRegistry registry);
}

public interface IModuleRegistry
{
    void RegisterFunction(string name, string ruName, Delegate implementation, bool isPure);
    void RegisterType(string name, string ruName, Type type);
    void RegisterCellAttribute(string name, string ruName, Type attributeType);
}
```

## Политика пропущенных значений

```csharp
public enum MissingPolicy
{
    Propagate,
    Omit,
    Fail
}
```

Значение по умолчанию — открытый вопрос OQ-06.

## Правила для авторов

- Сигнатуры используют типы SDK: тензоры, спаны, таблицы, факторы, пропуски.
- Функция без `StatPure` не участвует в сплавлении и служит барьером.
- Каждая функция несёт `StatDoc` на обоих языках.
- Каждый пакет содержит тесты на `StatNote.TestKit`.
- Мажорная версия `sdk` в манифесте обязана совпадать с версией контрактов.

## TestKit

Черновой API: `FunctionAssert.Golden`, `MissingAssert.Propagates`,
`BroadcastAssert.ShapeCompatible`, `PurityAssert.HasNoSideEffects`.
