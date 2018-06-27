---
title: Proste autoryzacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak użyć atrybutu autoryzacji, aby ograniczyć dostęp do platformy ASP.NET Core kontrolerów i akcji.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961126"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="81d03-103">Proste autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81d03-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="81d03-104">Autoryzacja w MVC steruje się za pomocą `AuthorizeAttribute` atrybutu i jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="81d03-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="81d03-105">W najprostszym stosowania `AuthorizeAttribute` atrybutu kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji, uwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="81d03-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="81d03-106">Na przykład następujący kod ogranicza dostęp do `AccountController` na dowolny użytkownik uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="81d03-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="81d03-107">Jeśli chcesz zastosować autoryzacji akcję, a nie kontrolera, zastosuj `AuthorizeAttribute` atrybutu samej akcji:</span><span class="sxs-lookup"><span data-stu-id="81d03-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="81d03-108">Obecnie tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do `Logout` funkcji.</span><span class="sxs-lookup"><span data-stu-id="81d03-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="81d03-109">Można również użyć `AllowAnonymous` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do wykonywania poszczególnych czynności.</span><span class="sxs-lookup"><span data-stu-id="81d03-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="81d03-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="81d03-110">For example:</span></span>

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

<span data-ttu-id="81d03-111">Dzięki temu tylko uwierzytelnieni użytkownicy będą mogli `AccountController`, z wyjątkiem `Login` akcji, która jest dostępna przez wszystkich użytkowników, niezależnie od statusu uwierzytelnionego lub nieuwierzytelnione / anonimowy.</span><span class="sxs-lookup"><span data-stu-id="81d03-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="81d03-112">`[AllowAnonymous]` Pomija wszystkie instrukcje autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="81d03-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="81d03-113">Jeśli znajdzie się `[AllowAnonymous]` oraz wszelkie `[Authorize]` atrybutu `[Authorize]` atrybuty są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="81d03-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="81d03-114">Na przykład w przypadku zastosowania `[AllowAnonymous]` na poziomie kontrolera żadnych `[Authorize]` atrybutów na tym samym kontrolerze (lub dowolnych akcji w obrębie jej) jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="81d03-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
