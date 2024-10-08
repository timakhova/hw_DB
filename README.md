# Домашнее задание по лекции "Базы данных" Тимахова Наталья

# Задание 1

Сотрудники (

    идентификатор, первичный ключ, serial,
    ФИО сотрудника, varchar(100),
    оклад, numeric(10, 2),
    должность, varchar(100),
    дата найма, date,
    идентификатор подразделения, внешний ключ, integer )

Подразделения (

    идентификатор, первичный ключ, serial,
    наименование структурного подразделения, varchar(150),
    тип подразделения, varchar(50),
    адрес филиала, varchar(200) )

Проекты (

    идентификатор, первичный ключ, serial,
    название проекта, varchar(100) )

Назначения на проект (

    идентификатор, первичный ключ, serial,
    идентификатор сотрудника, внешний ключ, integer,
    идентификатор проекта, внешний ключ, integer,
    дата назначения, date )

Филиалы (

    идентификатор, первичный ключ, serial,
    адрес филиала, varchar(200),
    город, varchar(50),
    регион, varchar(50) )

Должности (

    идентификатор, первичный ключ, serial,
    наименование должности, varchar(100) )

Оклад сотрудников (

    идентификатор, первичный ключ, serial,
    идентификатор сотрудника, внешний ключ, integer,
    оклад, numeric(10, 2),
    дата начала действия оклада, date )

# Задание 2

Шаг 1: Применение 1НФ (Первая нормальная форма)

Требование: Таблица должна быть плоской, а все ячейки содержать одно атомарное значение. В нашем случае эта форма уже выполнена, так как каждое поле содержит одно значение.

Шаг 2: Применение 2НФ (Вторая нормальная форма)

Требование: Таблица должна быть в 1НФ, а все неключевые атрибуты должны полностью зависеть от первичного ключа. Необходимо избавиться от частичных зависимостей.
Проблемы:

    ФИО сотрудника → Оклад, Должность, Дата найма.
    Оклад, должность и дата найма сотрудника зависят только от ФИО сотрудника, что указывает на частичные зависимости.

    ФИО сотрудника → Тип подразделения, Структурное подразделение.
    Эти поля также зависят от ФИО сотрудника, что приводит к дублированию данных, если несколько сотрудников работают в одном подразделении.

    ФИО сотрудника → Проект.
    Сотрудник может быть назначен на разные проекты, но повторение данных о сотруднике в таблице увеличивает вероятность ошибок и избыточности.

Решение для 2НФ: Разделить таблицу на несколько взаимосвязанных таблиц, чтобы устранить частичные зависимости.

Шаг 3: Применение 3НФ (Третья нормальная форма)

Требование: Таблица должна быть во 2НФ, и неключевые атрибуты не должны зависеть друг от друга, а только от первичного ключа.
Проблемы:

    Тип подразделения → Структурное подразделение.
    Тип подразделения полностью зависит от структурного подразделения, а не от ФИО сотрудника.

    Структурное подразделение → Адрес филиала.
    Адрес филиала зависит от структурного подразделения, а не от сотрудника.

Решение для 3НФ: Устранить транзитивные зависимости, создав отдельные таблицы для подразделений, типов подразделений и филиалов.
Функциональные зависимости в исходной таблице:

    ФИО сотрудника → Оклад, Должность, Дата найма
    ФИО сотрудника → Структурное подразделение, Тип подразделения
    ФИО сотрудника → Адрес филиала
    ФИО сотрудника → Проект
    Структурное подразделение → Адрес филиала
    Структурное подразделение → Тип подразделения

Предложение по нормализации:

    Таблица "Сотрудники":
        Идентификатор сотрудника (PK),
        ФИО,
        Должность (FK),
        Дата найма.

    Таблица "Должности":
        Идентификатор должности (PK),
        Наименование должности.

    Таблица "Оклады":
        Идентификатор записи (PK),
        Идентификатор сотрудника (FK),
        Оклад,
        Дата действия оклада.

    Таблица "Подразделения":
        Идентификатор подразделения (PK),
        Наименование подразделения,
        Тип подразделения (FK),
        Идентификатор филиала (FK).

    Таблица "Типы подразделений":
        Идентификатор типа (PK),
        Наименование типа подразделения.

    Таблица "Филиалы":
        Идентификатор филиала (PK),
        Адрес,
        Город,
        Регион.

    Таблица "Назначения на проект":
        Идентификатор назначения (PK),
        Идентификатор сотрудника (FK),
        Идентификатор проекта (FK),
        Дата назначения.

    Таблица "Проекты":
        Идентификатор проекта (PK),
        Наименование проекта.
