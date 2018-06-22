---
title: Autoryzacji opartej na widoku w programie ASP.NET MVC Core
author: rick-anderson
description: Ten dokument przedstawia sposób wstrzyknąć i korzystać z usługi autoryzacji wewnątrz widoku Razor aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: f25bab61afc93ff14bfd9c36d95a6d2e54b06dfb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277826"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="0cb37-103">Autoryzacji opartej na widoku w programie ASP.NET MVC Core</span><span class="sxs-lookup"><span data-stu-id="0cb37-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="0cb37-104">Deweloper chce często Pokaż, Ukryj lub modyfikację interfejsu użytkownika na podstawie bieżącej tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0cb37-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="0cb37-105">Można uzyskać dostępu do usługi autoryzacji w obrębie widoków MVC za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0cb37-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="0cb37-106">Aby wstawić usługi autoryzacji do widoku Razor, użyj `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="0cb37-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="0cb37-107">Jeśli ma usługi autoryzacji w każdym widoku `@inject` dyrektywy do *_ViewImports.cshtml* pliku *widoków* katalogu.</span><span class="sxs-lookup"><span data-stu-id="0cb37-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="0cb37-108">Aby uzyskać więcej informacji, zobacz [iniekcji zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0cb37-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="0cb37-109">Korzystania z usługi wprowadzony autoryzacji do wywołania `AuthorizeAsync` w taki sam sposób czy sprawdzania podczas [autoryzacji na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="0cb37-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0cb37-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0cb37-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0cb37-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0cb37-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="0cb37-112">W niektórych przypadkach zasób zostanie model widoku.</span><span class="sxs-lookup"><span data-stu-id="0cb37-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="0cb37-113">Wywołanie `AuthorizeAsync` w taki sam sposób czy sprawdzania podczas [autoryzacji na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="0cb37-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0cb37-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0cb37-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0cb37-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0cb37-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="0cb37-116">W powyższym kodzie modelu jest przekazywany jako zasób, które należy wykonać oceny zasad pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="0cb37-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb37-117">Nie należy polegać na przełączanie widoczność elementów interfejsu użytkownika aplikacji, jak wyboru wyłącznie autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0cb37-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="0cb37-118">Ukrywanie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego działania skojarzonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0cb37-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="0cb37-119">Rozważmy na przykład przycisk w poprzednim fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="0cb37-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="0cb37-120">Użytkownik może wywołać `Edit` metody akcji, jeśli użytkownik zna względną zasobów adres URL jest */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="0cb37-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="0cb37-121">Z tego powodu `Edit` metody akcji należy sprawdzić jego własnej autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0cb37-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
