---
title: Prosta autoryzacja w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób używać atrybutu Autoryzuj, aby ograniczyć dostęp do kontrolerów ASP.NET Core i akcji.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663584"
---
# <a name="simple-authorization-in-aspnet-core"></a>Prosta autoryzacja w ASP.NET Core

<a name="security-authorization-simple"></a>

Autoryzacja w MVC jest kontrolowana za pomocą atrybutu `AuthorizeAttribute` i jego różnych parametrów. Najprościej, stosując atrybut `AuthorizeAttribute` do kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji do dowolnego uwierzytelnionego użytkownika.

Na przykład poniższy kod ogranicza dostęp do `AccountController` do dowolnego uwierzytelnionego użytkownika.

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

Jeśli chcesz zastosować autoryzację do akcji, a nie kontrolera, zastosuj atrybut `AuthorizeAttribute` do samej akcji:

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

Teraz tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do funkcji `Logout`.

Można również użyć atrybutu `AllowAnonymous`, aby zezwolić na dostęp nieuwierzytelnionym użytkownikom do poszczególnych akcji. Na przykład:

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

Pozwoli to na `AccountController`tylko uwierzytelnionym użytkownikom, z wyjątkiem akcji `Login`, która jest dostępna dla wszystkich użytkowników, niezależnie od ich uwierzytelnionego lub nieuwierzytelnionego/anonimowego stanu.

> [!WARNING]
> `[AllowAnonymous]` pomija wszystkie instrukcje autoryzacji. W przypadku łączenia `[AllowAnonymous]` i dowolnych atrybutów `[Authorize]` atrybuty `[Authorize]` zostaną zignorowane. Na przykład jeśli stosujesz `[AllowAnonymous]` na poziomie kontrolera, wszystkie atrybuty `[Authorize]` na tym samym kontrolerze (lub w dowolnej akcji w niej) zostaną zignorowane.
