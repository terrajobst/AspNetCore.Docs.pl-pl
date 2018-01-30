---
title: Proste autoryzacji
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak użyć atrybutu autoryzacji, aby ograniczyć dostęp do platformy ASP.NET Core kontrolerów i akcji."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 3299a8fcbd8d8e089d8d7f95e46551c102bcc054
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="simple-authorization"></a>Proste autoryzacji

<a name="security-authorization-simple"></a>

Autoryzacja w MVC steruje się za pomocą `AuthorizeAttribute` atrybutu i jego parametrów. W najprostszym stosowania `AuthorizeAttribute` atrybutu kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji, uwierzytelnionym użytkownikom.

Na przykład następujący kod ogranicza dostęp do `AccountController` na dowolny użytkownik uwierzytelniony.

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Jeśli chcesz zastosować autoryzacji akcję, a nie kontrolera, zastosuj `AuthorizeAttribute` atrybutu samej akcji:

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

Obecnie tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do `Logout` funkcji.

Można również użyć `AllowAnonymousAttribute` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do wykonywania poszczególnych czynności. Na przykład:

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Dzięki temu tylko uwierzytelnieni użytkownicy będą mogli `AccountController`, z wyjątkiem `Login` akcji, która jest dostępna przez wszystkich użytkowników, niezależnie od statusu uwierzytelnionego lub nieuwierzytelnione / anonimowy.

>[!WARNING]
> `[AllowAnonymous]`Pomija wszystkie instrukcje autoryzacji. W przypadku zastosowania łączenie `[AllowAnonymous]` oraz wszelkie `[Authorize]` atrybutu, a następnie atrybuty autoryzacji zawsze będą ignorowane. Na przykład w przypadku zastosowania `[AllowAnonymous]` na kontrolerze poziomu dowolnej `[Authorize]` atrybuty na tym samym kontrolerze lub na dowolnych akcji w obrębie jej zostaną zignorowane.
