---
title: Proste autoryzacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak użyć atrybutu autoryzacji, aby ograniczyć dostęp do platformy ASP.NET Core kontrolerów i akcji.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="5c4cd-103">Proste autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c4cd-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="5c4cd-104">Autoryzacja w MVC steruje się za pomocą `AuthorizeAttribute` atrybutu i jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="5c4cd-105">W najprostszym stosowania `AuthorizeAttribute` atrybutu kontrolera lub akcji ogranicza dostęp do kontrolera lub akcji, uwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="5c4cd-106">Na przykład następujący kod ogranicza dostęp do `AccountController` na dowolny użytkownik uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="5c4cd-107">Jeśli chcesz zastosować autoryzacji akcję, a nie kontrolera, zastosuj `AuthorizeAttribute` atrybutu samej akcji:</span><span class="sxs-lookup"><span data-stu-id="5c4cd-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="5c4cd-108">Obecnie tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do `Logout` funkcji.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="5c4cd-109">Można również użyć `AllowAnonymous` atrybutu, aby zezwolić na dostęp przez nieuwierzytelnionych użytkowników do wykonywania poszczególnych czynności.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="5c4cd-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5c4cd-110">For example:</span></span>

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

<span data-ttu-id="5c4cd-111">Dzięki temu tylko uwierzytelnieni użytkownicy będą mogli `AccountController`, z wyjątkiem `Login` akcji, która jest dostępna przez wszystkich użytkowników, niezależnie od statusu uwierzytelnionego lub nieuwierzytelnione / anonimowy.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="5c4cd-112">`[AllowAnonymous]` Pomija wszystkie instrukcje autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="5c4cd-113">W przypadku zastosowania łączenie `[AllowAnonymous]` oraz wszelkie `[Authorize]` atrybutu, a następnie atrybuty autoryzacji zawsze będą ignorowane.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="5c4cd-114">Na przykład w przypadku zastosowania `[AllowAnonymous]` na kontrolerze poziomu dowolnej `[Authorize]` atrybuty na tym samym kontrolerze lub na dowolnych akcji w obrębie jej zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="5c4cd-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
