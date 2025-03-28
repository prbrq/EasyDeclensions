Cyriller - бесплатная программа склонения по падежам
========

Кириллер, это полностью открытый проект на c#, который является бесплатной альтернативой Морферу. 
На данные момент программа умеет:

* Склонять личные имена, фамилии, отчества без использования словаря.
* Склонять все существительные русского языка, включая имена с использованием словаря.
* Склонять существительные, которых нет в словаре по ближайшему совпадению.
* Склонять прилагательные русского языка с использованием словаря.
* Склонять прилагательные, которых нет в словаре по ближайшему совпадению.
* Писать числа прописью во всех падежах.
* Писать деньги прописью, в рублях, долларах, евро, юанях.
* Писать числа прописью вместе с единицей измерения.

## Как собрать

- Сборка всех проектов работает на Windows, под [Visual Studio](https://visualstudio.microsoft.com/downloads/) версии 2017 и 2019.
- Сборка `Cyriller` и `Cyriller.Samples` так же работает и запускается под Linux ([Ubuntu](https://ubuntu.com/download) 18.04 LTS), после установки .NET Core 3.0.

### Проект `Cyriller`

#### Зависит от:

- `Cyriller.Model`.
- `Cyriller.Rule`.
- `Cyriller.Zipper`, только на стадии компиляции.

#### Target Frameworks

- `netstandard2.0`.
- `net45`.

То есть, может быть использован в проектах под управлением:

- [.NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) версии 4.5 и выше.
- [.NET Core](https://dotnet.microsoft.com/download) версии 2.0 и выше.

*Больше о `netstandard` - [.NET Standard](https://docs.microsoft.com/en-us/dotnet/standard/net-standard).*

#### Сборка

Вначале необходимо собрать проект `Cyriller.Zipper`, он используется на стадии компиляции для упаковки словарей.
Проект `Cyriller.Zipper` требует .NET Core 3.0 последней версии. На момент написания это `3.0.100-preview7-012821`.

#### Сборка при помощи dotnet CLI

```
git clone https://github.com/miyconst/Cyriller
cd Cyriller
cd Cyriller.Zipper
dotnet build
cd ..
cd Cyriller.Samples
dotnet build
dotnet run
```

### Проект `Cyriller.Samples`

.NET Core 3.0 Console Application с примерами использования Cyriller.

### Проект `Cyriller.Zipper`

Служебное .NET Core 3.0 Console Application для упаковки словарей Cyriller на стадии компиляции.

## Как пользоваться

### Личные имена

Для склонения личных имен, можно использовать класс `CyrNoun`, в данном случае идет поиск имени в словаре.
Как пользоваться данным классом - смотри раздел [Существительное](#существительное).

Так же можно использовать класс `CyrName`, который склоняет личные имена по алгоритму, и не пользуется словарем.

- Смотри пример использования класса `CyrName` в методе [`/Cyriller.Samples/Program.cs/NameSamples`](Cyriller.Samples/Program.cs#L119).
- Старый [gist](https://gist.github.com/miyconst/47b108c868a934ac182fd2a3dd999e67) с примером склонения личных имен и фамилий.
- Класс `CyrName`, так же доступен для Java Script в упрощенном варианте - [`CyrName.js`](CyrName.js).

### Существительное

Склонение существительных выполняется при помощи класса `CyrNounCollection`.

При создании коллекции, весь словарь существительных склоняется и загружается в память.
Для этого требуется около 200 MB памяти и 2 секунд одного ядра процессора.

Коллекция `CyrNounCollection` имеет следующие варианты поиска слова:

- Поиск существительного по точному совпадению с автоматическим определением рода, падежа и числа.
- Поиск существительного по неточному совпадению с автоматическим определением рода, падежа и числа.
- Поиск существительного по точному совпадению с указанием рода, падежа и числа.
- Поиск существительного по неточному совпадению с указанием рода, падежа и числа.

Смотри пример использования классов `CyrNounCollection` и `CyrNoun` в методе [`/Cyriller.Samples/Program.cs/NounSamples`](Cyriller.Samples/Program.cs#L48).

### Прилагательное

Склонение прилагательных выполняется при помощи класса `CyrAdjectiveCollection`.

При создании коллекции, весь словарь прилагательных склоняется и загружается в память,
Для этого требуется около 250 MB памяти и 2 секунд одного ядра процессора.

Коллекция `CyrAdjectiveCollection` имеет следующие варианты поиска слова:

- Поиск прилагательного по точному совпадению с автоматическим определением рода, падежа, числа и одушевленности.
- Поиск прилагательного по неточному совпадению с автоматическим определением рода, падежа, числа и одушевленности.
- Поиск прилагательного по точному совпадению с указанием рода, падежа, числа и одушевленности.
- Поиск прилагательного по неточному совпадению с указанием рода, падежа, числа и одушевленности.

Смотри пример использования классов `CyrAdjectiveCollection` и `CyrAdjective` в методе [`/Cyriller.Samples/Program.cs/AdjectiveSamples`](Cyriller.Samples/Program.cs#L75).

### Фраза

При помощи класса `CyrPhrase` можно склонять словосочетания из существительных и прилагательных.

Смотри пример использования класса `CyrPhrase` в методе [`/Cyriller.Samples/Program.cs/PhraseSamples`](Cyriller.Samples/Program.cs#L102).

*Заметка: класс `CyrPhrase`, не умеет правильно склонять должности, так как должность это не словосочетание, где необходимо склонять все слова.*

### Число

Класс `CyrNumber`, отвечающий за склонение чисел стоит немного особняком и не нуждается в коллекциях.
Тем не менее, `CyrNumber` может склонять не просто числа, а количество чего-то. 
Это что-то может выражаться при помощи `CyrNoun`, которые в свою очередь можно получить из `CyrNounCollection`.

Основные возможности класса:

- Склоняет число прописью в указанном роде и одушевленности.
- Склоняет денежную сумму в указанной валюте, смотри класс `CyrNumber.Currency`.
- Склоняет количество указанных единиц, смотри класс `CyrNumber.Item`.
- Склоняет число прописью в указнный падеж, род и одушевленность.
- Склоняет денежную сумму в указанный падеж и валюту.
- Склоняет количество указанных единиц в указанный падеж.
- Выбирает правильный вариант слова в зависимости от указанного числа (1 год, 2 года, 5 лет).

Смотри пример использования классов `CyrNumber`, `CyrNumber.Currency` и `CyrNumber.Item` в методе [/Cyriller.Samples/Program.cs/NumberSamples](Cyriller.Samples/Program.cs#L169).

### Результат склонения

Класс `CyrResult` используется для хранения результатов склонения и содержит всевозможные свойства и методы для удобства доступа.

Список наиболее часто используемых свойств:

| Название        | Синоним        | Тип      | Описание                          |
|-----------------|----------------|----------|-----------------------------------|
| `Nominative`    | `Именительный` | `string` | Именительный, Кто? Что? (есть)    |
| `Genitive`      | `Родительный`  | `string` | Родительный, Кого? Чего? (нет)    |
| `Dative`        | `Дательный`    | `string` | Дательный, Кому? Чему? (дам)      |
| `Accusative`    | `Винительный`  | `string` | Винительный, Кого? Что? (вижу)    |
| `Instrumental`  | `Творительный` | `string` | Творительный, Кем? Чем? (горжусь) |
| `Prepositional` | `Предложный`   | `string` | Предложный, О ком? О чем? (думаю) |

Список наиболее часто используемых методов:

| Название        | Параметры                       | Возвращает                      | Описание                                                       |
|-----------------|---------------------------------|---------------------------------|----------------------------------------------------------------|
| `Get`           | `Cyriller.Model.CasesEnum Case` | `string`                        | Возвращает результат склонения в указанном падеже.             |
| `ToList`        |                                 | `List<string>`                  | Возвращает результат склонения по всем падежам в виде списка.  |
| `ToArray`       |                                 | `string[]`                      | Возвращает результат склонения по всем падежам в виде массива. |
| `ToDictionary`  |                                 | `Dictionary<CasesEnum, string>` | Возвращает результат склонения по всем падежам в виде словаря. |

### Исключения

Коллекции `CyrNounCollection` и `CyrAdjectiveCollection` выбрасывают `CyrWordNotFoundException` исключение если слово не найдено.

## Демо

Cyriller можно протестировать онлайн - [http://cyriller.1gb.ru/](http://cyriller.1gb.ru/).

## NuGet пакет

https://www.nuget.org/packages/Miyconst.Cyriller
