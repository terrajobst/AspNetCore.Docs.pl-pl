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
ms.openlocfilehash: 503ebc665efd460a85f49844ddc847eb12114308
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
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

Można również użyć `AllowAnonymous` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do wykonywania poszczególnych czynności. Na przykład:

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
