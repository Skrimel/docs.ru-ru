---
ms.openlocfilehash: c8f44ae1a500ed240dbff7d9a2c1479af368b7f1
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394197"
---
### <a name="identity-default-bootstrap-version-of-ui-changed"></a>Удостоверение. Изменение версии Bootstrap пользовательского интерфейса по умолчанию

Начиная с ASP.NET Core 3.0, пользовательский интерфейс удостоверений по умолчанию использует Bootstrap версии 4.

#### <a name="version-introduced"></a>Представленная версия

3.0

#### <a name="old-behavior"></a>Старое поведение

Вызов метода `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();` был таким же, как и `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap3);`.

#### <a name="new-behavior"></a>Новое поведение

Вызов метода `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();` такой же, как и `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap4);`.

#### <a name="reason-for-change"></a>Причина изменения

Выпуск Bootstrap версии 4 соответствует временному интервалу для ASP.NET Core 3.0.

#### <a name="recommended-action"></a>Рекомендуемое действие

Это изменение затронет вас, если вы используете пользовательский интерфейс удостоверений по умолчанию и добавили его в `Startup.ConfigureServices`, как показано в следующем примере:

```csharp
services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();
```

Выполните одно из указанных ниже действий.

- перенесите приложение для использования Bootstrap 4, следуя [инструкциям по переносу](https://getbootstrap.com/docs/4.0/migration);
- обновите `Startup.ConfigureServices` для принудительного использования Bootstrap 3. Например:

    ```csharp
    services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap3);
    ```

#### <a name="category"></a>Категория

ASP.NET Core

#### <a name="affected-apis"></a>Затронутые API

Нет

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
