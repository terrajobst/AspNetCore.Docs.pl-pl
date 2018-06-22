---
title: Proste autoryzacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak użyć atrybutu autoryzacji, aby ograniczyć dostęp do platformy ASP.NET Core kontrolerów i akcji.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272068"
---
# <a name="simple-authorization-in-aspnet-core"></a>Proste autoryzacji w ASP.NET Core

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
> `[AllowAnonymous]` Pomija wszystkie instrukcje autoryzacji. W przypadku zastosowania łączenie `[AllowAnonymous]` oraz wszelkie `[Authorize]` atrybutu, a następnie atrybuty autoryzacji zawsze będą ignorowane. Na przykład w przypadku zastosowania `[AllowAnonymous]` na kontrolerze poziomu dowolnej `[Authorize]` atrybuty na tym samym kontrolerze lub na dowolnych akcji w obrębie jej zostaną zignorowane.
