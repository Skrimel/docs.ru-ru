---
title: Метод <methodname> несколько раз определен с одинаковыми подписями
ms.date: 07/20/2015
f1_keywords:
- vbc30269
- bc30269
helpviewer_keywords:
- BC30269
ms.assetid: 39489621-6617-4e5c-9b24-c2faf8273891
ms.openlocfilehash: b884220053bbcec633c964a41892bc8df42f41c7
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64602443"
---
# <a name="methodname-has-multiple-definitions-with-identical-signatures"></a>"\<имя_метода >" имеет несколько определений с одинаковыми сигнатурами
Объект `Function` или `Sub` в объявлении процедуры используются одинаковые процедуры имя и список аргументов, как и в предыдущем объявлении. Одной из возможных причин является попытка перегрузить исходный текст процедуры. Перегруженные процедуры должны иметь разные списки аргументов.  
  
 **Идентификатор ошибки:** BC30269  
  
## <a name="to-correct-this-error"></a>Исправление ошибки  
  
- Измените имя процедуры или список аргументов, или удалите повторяющееся объявление.  
  
## <a name="see-also"></a>См. также

- [Ссылки на объявленные элементы](../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Вопросы, связанные с перегрузкой процедур](../../../visual-basic/programming-guide/language-features/procedures/considerations-in-overloading-procedures.md)
