---
title: Proste autoryzacji
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak użyć atrybutu autoryzacji, aby ograniczyć dostęp do platformy ASP.NET Core kontrolerów i akcji."
keywords: Platformy ASP.NET Core autoryzacji, klasy AuthorizeAttribute
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="4f3e2-104">Proste autoryzacji</span><span class="sxs-lookup"><span data-stu-id="4f3e2-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="4f3e2-105">Autoryzacja w MVC steruje się za pomocą `AuthorizeAttribute` atrybutu i jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="4f3e2-106">W najprostszym stosowania `AuthorizeAttribute` atrybutu kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji, uwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="4f3e2-107">Na przykład następujący kod ogranicza dostęp do `AccountController` na dowolny użytkownik uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="4f3e2-108">Jeśli chcesz zastosować autoryzacji akcję, a nie kontrolera, zastosuj `AuthorizeAttribute` atrybutu samej akcji:</span><span class="sxs-lookup"><span data-stu-id="4f3e2-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="4f3e2-109">Obecnie tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do `Logout` funkcji.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="4f3e2-110">Można również użyć `AllowAnonymousAttribute` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do wykonywania poszczególnych czynności.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="4f3e2-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4f3e2-111">For example:</span></span>

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

<span data-ttu-id="4f3e2-112">Dzięki temu tylko uwierzytelnieni użytkownicy będą mogli `AccountController`, z wyjątkiem `Login` akcji, która jest dostępna przez wszystkich użytkowników, niezależnie od statusu uwierzytelnionego lub nieuwierzytelnione / anonimowy.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="4f3e2-113">`[AllowAnonymous]`Pomija wszystkie instrukcje autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="4f3e2-114">W przypadku zastosowania łączenie `[AllowAnonymous]` oraz wszelkie `[Authorize]` atrybutu, a następnie atrybuty autoryzacji zawsze będą ignorowane.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="4f3e2-115">Na przykład w przypadku zastosowania `[AllowAnonymous]` na kontrolerze poziomu dowolnej `[Authorize]` atrybuty na tym samym kontrolerze lub na dowolnych akcji w obrębie jej zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="4f3e2-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
