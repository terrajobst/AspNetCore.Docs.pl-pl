---
title: Autoryzacja prosta w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ograniczyć dostęp do platformy ASP.NET Core kontrolerów i akcji za pomocą atrybutu autoryzacji.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903130"
---
# <a name="simple-authorization-in-aspnet-core"></a>Autoryzacja prosta w programie ASP.NET Core

<a name="security-authorization-simple"></a>

Autoryzacja w MVC jest kontrolowany za pośrednictwem `AuthorizeAttribute` atrybutu i jej różnych parametrów. W najprostszym stosowanie `AuthorizeAttribute` atrybutu kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji dla każdego uwierzytelnionego użytkownika.

Na przykład, poniższy kod ogranicza dostęp do `AccountController` dla każdego uwierzytelnionego użytkownika.

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

Jeśli ma być stosowana autoryzacja akcję, a nie w kontrolerze, zastosuj `AuthorizeAttribute` atrybutu do samej akcji:

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

Teraz tylko uwierzytelnieni użytkownicy będą mogli `Logout` funkcji.

Można również użyć `AllowAnonymous` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do poszczególnych czynności. Na przykład:

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

Pozwoliłoby to tylko uwierzytelnionym użytkownikom `AccountController`, z wyjątkiem `Login` akcji, który jest dostępny dla wszystkich, bez względu na ich stan uwierzytelnionego lub nieuwierzytelnionym / anonimowe.

> [!WARNING]
> `[AllowAnonymous]` Pomija wszystkie instrukcje autoryzacji. Jeśli możesz połączyć `[AllowAnonymous]` oraz `[Authorize]` atrybutu `[Authorize]` atrybuty są ignorowane. Na przykład jeśli zastosujesz `[AllowAnonymous]` na poziomie kontrolera wszelkie `[Authorize]` atrybutów na tego samego kontrolera (lub dowolnych akcji w obrębie jej) jest ignorowana.
