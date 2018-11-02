---
title: Управление версиями и библиотеки .NET
description: Рекомендации по использованию системы управления версиями для библиотек .NET.
author: jamesnk
ms.author: mairaw
ms.date: 10/02/2018
ms.openlocfilehash: f95c8ade1f91af5c13184b839b327c9397c6fe5a
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2018
ms.locfileid: "50187862"
---
# <a name="versioning"></a>Управление версиями

Разработка библиотек программного обеспечения редко завершается версией 1.0. Хорошая библиотека постоянно развивается: появляются новые функции, исправляются ошибки и повышается производительность. Очень важно правильно выпускать новые версии библиотеки .NET, расширяя ее возможности и не допуская появления проблем для уже существующих пользователей.

## <a name="breaking-changes"></a>Критические изменения

См. сведения об [оценке критических изменений между версиями](./breaking-changes.md).

## <a name="version-numbers"></a>Номера версий

Для библиотеки .NET версию можно указать разными способами. Вот несколько наиболее важных версий:

### <a name="nuget-package-version"></a>Версия пакета NuGet

[Версия пакета NuGet](/nuget/reference/package-versioning) отображается на сайте NuGet.org в диспетчере пакетов Visual Studio NuGet. Она добавляется к исходному коду при использовании пакета. Именно версию пакета NuGet чаще всего будут видеть пользователи и именно ее они будут считать версией используемой библиотеки. Версия пакета NuGet используется только в пределах NuGet и никак не влияет на поведение во время выполнения.

```xml
<PackageVersion>1.0.0-alpha1</PackageVersion>
```

Идентификатор пакета NuGet в сочетании с версией пакета NuGet используется для идентификации пакета в NuGet. Например, `Newtonsoft.Json` + `11.0.2`. Пакет с суффиксом обозначает пакет предварительной версии, и для него используются специальные правила, благодаря которым он идеально подходит для тестирования. См. с сведения о [пакетах предварительной версии](./nuget.md#pre-release-packages).

Так как версия пакета NuGet встречается разработчикам чаще всего, мы рекомендуем обновлять ее с использованием [Семантического версионирования](https://semver.org/) (SemVer). SemVer указывает на значимость изменений, реализованных в очередном выпуске, помогая разработчикам правильно выбрать версию для использования. Например, переход от версии `1.0` к `2.0` указывает на наличие потенциальных критических изменений.

**✔️ ДОПУСТИМО.** Попробуйте использовать [SemVer 2.0.0](https://semver.org/) для управления версиями пакета NuGet.

**✔️ РЕКОМЕНДУЕТСЯ.** Используйте версию пакета NuGet в общедоступной документации, так как именно этот номер версии пользователи будут видеть чаще всего.

**✔️ РЕКОМЕНДУЕТСЯ.** Добавьте суффикс предварительной версии при выпуске нестабильной версии пакета.

> Чтобы использовать пакеты предварительных версий, пользователи должны явно согласиться с тем, что работа над пакетом еще не завершена.

### <a name="assembly-version"></a>Версия сборки

Версия сборки используется средой CLR во время выполнения для выбора загружаемой версии сборки. Выбор сборки через систему управления версиями применяется только к сборкам со строгими именами.

```xml
<AssemblyVersion>1.0.0.0</AssemblyVersion>
```

Среда CLR Windows для .NET Framework при загрузке сборки со строгими именами требует точного совпадения номера. Предположим, что `Libary1, Version=1.0.0.0` компилируется со ссылкой на `Newtonsoft.Json, Version=11.0.0.0`. Тогда .NET Framework будет загружать только версию `11.0.0.0`. Чтобы загрузить во время выполнения другую версию, необходимо добавить переадресацию привязок в файл конфигурации приложения .NET.

Использование строгого именования в сочетании с версией сборки позволяет организовать [строгую загрузку версий сборок](../../framework/app-domains/assembly-versioning.md). Хотя строгое именование библиотек имеет ряд преимуществ, оно часто вызывает исключения времени выполнения, если не удается найти точную версию сборки. Для устранения таких проблем требуется [переадресация привязок](../../framework/configure-apps/redirect-assembly-versions.md) в `app.config`/`web.config`. Загрузка сборок в .NET Core стала менее строгой, и среда CLR в .NET Core автоматически загружает во время выполнения сборки с более поздней версией.

**✔️ ДОПУСТИМО.** Вы можете указывать в AssemblyVersion только основной номер версии.

> Например, для библиотек версий 1.0 и 1.0.1 можно указать одинаковое значение AssemblyVersion `1.0.0.0`, а для библиотеки 2.0 изменить AssemblyVersion на `2.0.0.0`. Если версия сборки меняется редко, потребуется меньше переадресаций привязок.

**✔️ ДОПУСТИМО.** Вы можете синхронизировать основной номер версии AssemblyVersion и версию пакета NuGet.

> Номер AssemblyVersion включается в некоторые информационные сообщения для пользователей, например в составе имени сборки и имени типа сборки в сообщениях об исключениях. Сохранение связи между этими версиями позволит разработчикам лучше понимать, какую версию они используют.

**❌ НЕЛЬЗЯ.** Не используйте фиксированное значение AssemblyVersion.

> Конечно же, отсутствие изменений в AssemblyVersion позволит избежать переадресации привязок, запрещая устанавливать более одной версии сборки в глобальный кэш сборок. Кроме того, приложения со ссылками на такую сборку в глобальном кэше сборок не смогут работать, если другое приложение загрузит в глобальный кэш сборок новую версию сборки с критическими изменениями.

### <a name="assembly-file-version"></a>Версия файла сборки

Версия файла сборки используется для отображения версии файла в ОС Windows и никак не влияет на поведение во время выполнения. Настройка этой версии является необязательной. Она отображается в диалоговом окне "Свойства файла" в проводнике Windows:

```xml
<FileVersion>11.0.2.21924</FileVersion>
```

![Проводник Windows](./media/versioning/win-properties.png "Windows Explorer")

> [!NOTE]
> Если этот номер версии не соответствует формату `Major.Minor.Build.Revision`, появляется оповещение об ошибке. Его можно игнорировать.

**✔️ ДОПУСТИМО.** Вы можете использовать номер сборки непрерывной интеграции в качестве номера редакции AssemblyFileVersion.

> Например, если вы создаете версию проекта 1.0.0, а номер сборки непрерывной интеграции имеет значение 99, параметр AssemblyFileVersion получит значение 1.0.0.99.

### <a name="assembly-informational-version"></a>Информационная версия сборки

Информационная версия сборки используется для сохранения дополнительных сведений о версии и никак не влияет на поведение во время выполнения. Настройка этой версии является необязательной. Если вы используете SourceLink, это значение при сборке составляется из номера версии пакета NuGet и номера в системе управления версиями. Например, `1.0.0-beta1+204ff0a` включает хэш фиксации для исходного кода, из которого построена сборка. См. сведения об [использовании SourceLink](./sourcelink.md).

```xml
<AssemblyInformationalVersion>The quick brown fox jumped over the lazy dog.</AssemblyInformationalVersion>
```

**❌ НЕЖЕЛАТЕЛЬНО.** Не указывайте самостоятельно информационную версию сборки.

> Разрешите SourceLink автоматически создавать этот номер версии из метаданных NuGet и системы управления версиями.

>[!div class="step-by-step"]
[Назад](./publish-nuget-package.md)
[Вперед](./breaking-changes.md)