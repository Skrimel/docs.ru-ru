---
title: Для различных версий платформ
description: Практические рекомендации для создания кросс платформенные библиотеки .NET.
author: jamesnk
ms.author: mairaw
ms.date: 10/02/2018
ms.openlocfilehash: 72fa891d5b1054af485a98d89b4efb11d6b0018b
ms.sourcegitcommit: e42d09e5966dd9fd02847d3e7eeb4ec0877069f8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2018
ms.locfileid: "49374908"
---
# <a name="cross-platform-targeting"></a>Для различных версий платформ

Современные .NET поддерживает несколько операционных систем и устройств. Очень важно для библиотек .NET с открытым кодом для поддержки как многие разработчики максимально ли они создают на веб-сайте ASP.NET, размещенного в Azure или .NET игры в Unity.

## <a name="net-standard"></a>.NET Standard

.NET standard — это лучший способ добавления Поддержка разных платформ в библиотеку .NET. [.NET standard](../net-standard.md) — это спецификация API-интерфейсы .NET, которые доступны во всех реализациях .NET. Применение .NET Standard позволяет создавать библиотеки, которые являются ограниченными использовать API-интерфейсы, которые находятся в данной версии .NET Standard, это означает, что его можно использовать на всех платформах, которые реализуют эту версию .NET Standard.

![.NET standard](./media/cross-platform-targeting/platforms-netstandard.png ".NET Standard")

Применение .NET Standard, а также успешно компиляцию проекта, не гарантирует, что библиотека будет успешно выполняться на всех платформах:

1. API для конкретных платформ не удастся на других платформах. Например <xref:Microsoft.Win32.Registry?displayProperty=nameWithType> будет успешно выполнено на Windows и исключение <xref:System.PlatformNotSupportedException> при использовании в любой операционной системе.
2. API-интерфейсы могут вести себя по-разному. Например отражение API-интерфейсы имеют разные характеристики производительности при компиляции ahead-of-time использования приложением на iOS и универсальной платформы Windows.

> [!TIP]
> Команде разработчиков .NET [предлагает анализатор Roslyn](../analyzers/api-analyzer.md) помогут вам оценить возможные причины этой проблемы.

**СДЕЛАТЬ ✔️** начинаются с включая `netstandard2.0` целевой объект.

> Наиболее универсальные библиотеки не нужен API вне .NET Standard 2.0. .NET standard 2.0 поддерживается на всех современных платформах и является рекомендуемым способом обеспечить поддержку нескольких платформ с помощью одного целевого объекта.

**ИЗБЕГАЙТЕ ❌** включая `netstandard1.x` целевой объект.

> .NET standard 1.x распространяется в виде комплексного набора пакетов NuGet, который создает диаграмму зависимостей большой пакет и приводит к разработчикам, загрузка много пакетов при сборке. Современные платформы .NET, включая .NET Framework 4.6.1, UWP и Xamarin, поддержка .NET Standard 2.0. Следует выбирать только .NET Standard 1.x, если необходимо специально предназначенных для более старых платформ.

**СДЕЛАТЬ ✔️** включают `netstandard2.0` целевой, если требуется `netstandard1.x` целевой объект.

> Все платформы, которые поддерживают .NET Standard 2.0 будет использовать `netstandard2.0` ориентированы и использовать преимущества меньшего графика пакета во время более старых платформ по-прежнему будет работать и откат к использованию `netstandard1.x` целевой объект.

**❌ НЕ** включают целевой объект .NET Standard, если библиотека зависит от модели приложений для конкретных платформ.

> Например библиотеки набора средств элементов управления универсальной платформы Windows зависит от модель приложения, которая доступна только для универсальной платформы Windows. Модели определенного приложения API-интерфейсы будет недоступен в .NET Standard.

## <a name="multi-targeting"></a>Настройка для различных версий

Иногда необходимо получить доступ к API-интерфейсам конкретной платформы из ваших библиотек. Лучший способ вызова API конкретной платформы с помощью многоплатформенного нацеливания, который выполнит сборку проекта для многих [целевых платформ .NET](../frameworks.md) , а не только для одного.

Чтобы защитить потребителей от необходимости создания для отдельных платформ, следует стремиться имеют в .NET стандартный поток вывода, а также один или несколько выходов зависящие от платформы. С помощью версий все сборки, упаковываются внутри одного пакета NuGet. Потребители могут затем ссылаться на тот же пакет и NuGet выбирает соответствующую реализацию. Библиотеки .NET Standard выступает в качестве резервной библиотека, которая является использовать везде, за исключением случаев, где ваш пакет NuGet предлагает реализацию зависящие от платформы. Для различных версий позволяет использовать условную компиляцию кода и вызов API конкретной платформы.

![Пакет NuGet с несколькими сборками](./media/cross-platform-targeting/nuget-package-multiple-assemblies.png "пакета NuGet с помощью нескольких сборок")

**Рассмотрите ВОЗМОЖНОСТЬ ✔️** предназначенных для реализаций .NET, в дополнение к .NET Standard.

> Предназначенных для реализаций .NET позволяет вызывать API конкретных платформ, которые находятся вне .NET Standard.
>
> При этом не удалять поддержка .NET Standard. Вместо этого исключение от реализации и предоставляют возможности API-интерфейсы. Таким образом, библиотеку можно использовать всякий раз и поддерживает подготовки среды выполнения функций.

**ИЗБЕГАЙТЕ ❌** с помощью различных версий с .NET Standard, если исходный код является одинаковым для всех целевых объектов.

> .NET Standard сборки будут автоматически использоваться с NuGet. Для различных версий отдельных реализаций .NET увеличивает `*.nupkg` размер, не получая никаких преимуществ.

**✔️ Попробуйте** добавления целевого объекта для `net461` при вы предлагаете `netstandard2.0` целевой объект. 

> С помощью .NET Standard 2.0 с .NET Framework имеет некоторые проблемы, устраненные в .NET Framework 4.7.2. Можно повысить производительность разработчиков, которые по-прежнему используют .NET Framework 4.6.1 — 4.7.1, предложив им двоичный файл, предназначенную для .NET Framework 4.6.1.

**СДЕЛАТЬ ✔️** распространять библиотеки с помощью пакета NuGet.

> NuGet будет выберите целевой объект рекомендации для разработчиков и экранировать их нужно выбрать соответствующую реализацию.

**СДЕЛАТЬ ✔️** использовать файл проекта `TargetFrameworks` свойства, если для различных версий.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <!-- This project will output netstandard2.0 and net461 assemblies -->
    <TargetFrameworks>netstandard2.0;net461</TargetFrameworks>
  </PropertyGroup>
</Project>
```

**Рассмотрите ВОЗМОЖНОСТЬ ✔️** с помощью [MSBuild.Sdk.Extras](https://github.com/onovotny/MSBuildSdkExtras) при многоплатформенного нацеливания для универсальной платформы Windows и Xamarin, так как это значительно упрощает файле проекта.

## <a name="older-targets"></a>Более старые целевых объектов

Платформа .NET поддерживает предназначенных для версий платформы .NET Framework, длинные за пределы поддержки, а также платформы, которые обычно больше не используются. Пока значение в обеспечении работы библиотеки на как максимальное число целевых объектов для максимально того, чтобы обойти отсутствие интерфейсов API можно добавить значительные накладные расходы. Мы считаем определенных платформ стоят больше не планируется использовать, учитывая их обработки рекламных кампаний и ограничения.

**❌ НЕ** включать целевой переносимой библиотеки классов (PCL). Например, `portable-net45+win8+wpa81+wp8`.

> .NET standard — это современный способ поддержки кросс платформенные библиотеки .NET и заменяет PCL.

**❌ НЕ** включить целевые объекты для платформ .NET, которые больше не поддерживаются. Например, `SL4`, `WP`.

>[!div class="step-by-step"]
[Назад](./get-started.md)
[Вперед](./strong-naming.md)