---
title: -optionstrict
ms.date: 07/20/2015
f1_keywords:
- -optionstrict
helpviewer_keywords:
- -optionstrict compiler option [Visual Basic]
- optionstrict compiler option [Visual Basic]
- /optionstrict compiler option [Visual Basic]
ms.assetid: c7b10086-0fa4-49db-b3c8-4ae0db5957da
ms.openlocfilehash: 469e22aef9d746fc55e04ba884d17d60d8baa85a
ms.sourcegitcommit: 1f12db2d852d05bed8c53845f0b5a57a762979c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "72583073"
---
# <a name="-optionstrict"></a>-optionstrict

Обеспечивает строгую семантику типов для ограничения неявных преобразований типов.

## <a name="syntax"></a>Синтаксис

```console
-optionstrict[+ | -]
-optionstrict[:custom]
```

## <a name="arguments"></a>Аргументы

`+` &#124; `-`  
Необязательный. Параметр `-optionstrict+` позволяет ограничивать неявное преобразование типов. Значение по умолчанию для этого параметра — `-optionstrict-`. Параметр `-optionstrict+` совпадает с `-optionstrict`. Для разрешительной семантики типов можно использовать и то, и другое.

`custom`  
Обязательный. Предупреждать, если не учитывается превышена семантика языка.

## <a name="remarks"></a>Заметки

Если `-optionstrict+` действует, только расширяющие преобразования типов могут быть сделаны неявно. Неявные сужающие преобразования типов, такие как назначение объекта типа `Decimal` объекту целочисленного типа, выводятся как ошибки.

Чтобы создать предупреждения для неявных сужающих преобразований типов, используйте `-optionstrict:custom`. Используйте `-nowarn:numberlist`, чтобы пропускать определенные предупреждения и `-warnaserror:numberlist` обрабатывать определенные предупреждения как ошибки.

### <a name="to-set--optionstrict-in-the-visual-studio-ide"></a>Установка параметра-оптионстрикт в интегрированной среде разработки Visual Studio

1. Выберите проект в **Обозревателе решений**. В меню **проект** выберите пункт **Свойства.**

2. Откройте вкладку **Компиляция**.

3. Измените значение в поле **Option** ЗНАЧ.

### <a name="to-set--optionstrict-programmatically"></a>Установка параметра-оптионстрикт программными средствами

См. [оператор Option Case](../../../visual-basic/language-reference/statements/option-strict-statement.md).

## <a name="example"></a>Пример

Следующий код компилирует `Test.vb` с помощью строгой семантики типов.

```console
vbc -optionstrict+ test.vb
```

## <a name="see-also"></a>См. также

- [Компилятор Visual Basic с интерфейсом командной строки](../../../visual-basic/reference/command-line-compiler/index.md)
- [-optioncompare](../../../visual-basic/reference/command-line-compiler/optioncompare.md)
- [-optionexplicit](../../../visual-basic/reference/command-line-compiler/optionexplicit.md)
- [-optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)
- [-nowarn](../../../visual-basic/reference/command-line-compiler/nowarn.md)
- [-warnaserror (Visual Basic)](../../../visual-basic/reference/command-line-compiler/warnaserror.md)
- [Примеры командных строк компиляции](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Оператор Option Strict](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [Страница "Параметры Visual Basic по умолчанию", папка "Проекты", диалоговое окно "Параметры"](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
