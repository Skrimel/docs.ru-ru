---
title: Соображения относительно версии и обновления для разработчиков на C#
description: Добавление в библиотеку новых компонентов языка может повлиять на код, который использует эту библиотеку.
ms.date: 09/19/2018
ms.openlocfilehash: 56685422e2c73dcca25acbdccb3a77a8de9df775
ms.sourcegitcommit: fb78d8abbdb87144a3872cf154930157090dd933
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47199935"
---
# <a name="version-and-update-considerations-for-c-developers"></a>Соображения относительно версии и обновления для разработчиков на C#

Совместимость очень важна при добавлении новых функций языка C#. В большинстве случаев существующий код сможет компилироваться в новой версии компилятора безо всяких проблем.

Особое внимание требуется в тех случаях, когда в библиотеку добавляются новые возможности языка. Например, если вы создаете новую библиотеку с функциями из последней версии, но ее должны использовать приложения, созданные в предыдущих версиях компилятора. Или же вы обновляете существующую библиотеку, но многие ваши пользователи пока используют старые версии. Принимая решения о внедрении новых функций, необходимо учесть два аспекта совместимости: на уровне исходного кода и на уровне двоичных файлов.

## <a name="binary-compatible-changes"></a>Изменения, совместимые на уровне двоичных файлов

Изменения библиотеки считаются **совместимыми на уровне двоичных файлов**, если новую версию библиотеки можно использовать без повторной сборки приложении и других библиотек, которые ее используют. В этом сценарии не нужно перестраивать зависимые сборки и (или) вносить изменения в исходный код. Измерения, совместимые на уровне двоичных файлов, всегда совместимы и на уровне исходного кода.

## <a name="source-compatible-changes"></a>Изменения, совместимые на уровне исходного кода

Изменения библиотеки будут **совместимыми на уровне исходного кода**, если для приложений и библиотек, которые используют новую библиотеку, не потребуется изменять исходный код, но для правильной работы нужно заново скомпилировать их с новой версией.

## <a name="incompatible-changes"></a>Несовместимые изменения

Если изменение не является совместимым ни **на уровне исходного кода**, ни **на уровне двоичных файлов**, для зависимых приложений и библиотек потребуется изменить исходный код и заново скомпилировать их.

## <a name="evaluate-your-library"></a>Оценка существующих библиотек

Эти концепции совместимости влияют на компоненты библиотеки с атрибутами public и protected, но не на ее внутреннюю реализацию. Любые новые возможности внутри библиотеки всегда **совместимы на уровне двоичных файлов**.  

К **совместимым на уровне двоичных файлов** изменениям относится любой новый синтаксис, который создает такой же скомпилированный код любых открытых (public) объявлений, как и со старым синтаксисом. Например, замена метода на член, воплощающий выражение, будет **совместимым на уровне двоичных файлов изменением**.

Исходный код:

```csharp
public double CalculateSquare(double value)
{
    return value * value;
}
```

Новый код:

```csharp
public double CalculateSquare(double value) => value * value;
```

Изменения, **совместимые на уровне исходного кода**, создают новый синтаксис, который изменяет скомпилированный код одного или нескольких открытых (public) членов таким образом, который сохраняет его совместимость со всеми существующими вызывающими объектами. Например, если подпись метода ранее передавала параметр по значению, а теперь содержит `in` с параметром по ссылке, такое изменение будет совместимым на уровне исходного кода, но не на уровне двоичных файлов:

Исходный код:

```csharp
public double CalculateSquare(double value) => value * value;
```

Новый код:

```csharp
public double CalculateSquare(in double value) => value * value;
```

Во всех статьях [о новых возможностях](index.md) всегда указано, является ли новая функция для открытых объявлений совместимый на уровне исходного кода или на уровне двоичных файлов.