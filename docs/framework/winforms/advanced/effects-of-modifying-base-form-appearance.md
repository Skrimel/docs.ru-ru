---
title: Влияние изменения внешнего вида базовой формы
ms.date: 03/30/2017
helpviewer_keywords:
- parent forms [Windows Forms]
- inherited forms [Windows Forms], modifications to base form
- Windows Forms, base form appearance
- base forms
- inheritance [Windows Forms], forms
ms.assetid: 1c3f2b29-a05c-4c6f-aa1a-4e66b94f343a
ms.openlocfilehash: b24bd2db564c7acb0a748c47095ee0777ea1eb88
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69955147"
---
# <a name="effects-of-modifying-a-base-forms-appearance"></a>Влияние изменения внешнего вида базовой формы

В процессе разработки приложения часто может потребоваться изменить внешний вид базовой формы, от которой наследуются другие формы проекта (или других проектов).

Во время разработки изменения внешнего вида базовой формы (будь то значения свойств либо добавление и удаление элементов управления) отражаются на наследуемых формах при построении проекта, содержащего базовую форму. Просто сохранить изменения в базовой форме будет недостаточно. Для сборки проекта выберите в меню **Сборка** пункт **Собрать**.

Изменения, внесенные в базовую форму во время выполнения, не влияют на наследуемые формы, экземпляры которых уже созданы.

## <a name="see-also"></a>См. также

- [base](../../../csharp/language-reference/keywords/base.md)
- [Практическое руководство. Наследовать Windows Forms](how-to-inherit-windows-forms.md)
- [Визуальное наследование в Windows Forms](windows-forms-visual-inheritance.md)
