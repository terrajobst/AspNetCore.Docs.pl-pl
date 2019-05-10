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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="182be-103">Autoryzacja prosta w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="182be-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="182be-104">Autoryzacja w MVC jest kontrolowany za pośrednictwem `AuthorizeAttribute` atrybutu i jej różnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="182be-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="182be-105">W najprostszym stosowanie `AuthorizeAttribute` atrybutu kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji dla każdego uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="182be-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="182be-106">Na przykład, poniższy kod ogranicza dostęp do `AccountController` dla każdego uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="182be-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="182be-107">Jeśli ma być stosowana autoryzacja akcję, a nie w kontrolerze, zastosuj `AuthorizeAttribute` atrybutu do samej akcji:</span><span class="sxs-lookup"><span data-stu-id="182be-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="182be-108">Teraz tylko uwierzytelnieni użytkownicy będą mogli `Logout` funkcji.</span><span class="sxs-lookup"><span data-stu-id="182be-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="182be-109">Można również użyć `AllowAnonymous` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do poszczególnych czynności.</span><span class="sxs-lookup"><span data-stu-id="182be-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="182be-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="182be-110">For example:</span></span>

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

<span data-ttu-id="182be-111">Pozwoliłoby to tylko uwierzytelnionym użytkownikom `AccountController`, z wyjątkiem `Login` akcji, który jest dostępny dla wszystkich, bez względu na ich stan uwierzytelnionego lub nieuwierzytelnionym / anonimowe.</span><span class="sxs-lookup"><span data-stu-id="182be-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="182be-112">`[AllowAnonymous]` Pomija wszystkie instrukcje autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="182be-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="182be-113">Jeśli możesz połączyć `[AllowAnonymous]` oraz `[Authorize]` atrybutu `[Authorize]` atrybuty są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="182be-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="182be-114">Na przykład jeśli zastosujesz `[AllowAnonymous]` na poziomie kontrolera wszelkie `[Authorize]` atrybutów na tego samego kontrolera (lub dowolnych akcji w obrębie jej) jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="182be-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
