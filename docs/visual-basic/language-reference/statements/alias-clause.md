---
title: Предложение Alias
ms.date: 07/20/2015
f1_keywords:
- vb.Alias
helpviewer_keywords:
- Alias keyword [Visual Basic]
ms.assetid: 58c06b11-465d-4d87-906a-73200a3d7f19
ms.openlocfilehash: a7f67224c5d26816bdc1974dc4aa82d796c41284
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349150"
---
# <a name="alias-clause-visual-basic"></a>Предложение Alias (Visual Basic)
Указывает, что у внешней процедуры есть другое имя в своей библиотеке DLL.  
  
## <a name="remarks"></a>Примечания  
 В этом контексте можно использовать ключевое слово `Alias`:  
  
 [Оператор Declare](../../../visual-basic/language-reference/statements/declare-statement.md)  
  
 В следующем примере ключевое слово `Alias` используется для предоставления имени функции в advapi32. dll, `GetUserNameA`, которая `getUserName` используется вместо в этом примере. Функция `getUserName` вызывается в под`getUser`, которая отображает имя текущего пользователя.  
  
 [!code-vb[VbVbalrStatements#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>См. также:

- [Ключевые слова](../../../visual-basic/language-reference/keywords/index.md)
