---
title: Autoryzacji opartej na widoku w programie ASP.NET MVC Core
author: rick-anderson
description: "Ten dokument przedstawia sposób wstrzyknąć i korzystać z usługi autoryzacji wewnątrz widoku Razor aplikacji ASP.NET Core."
keywords: Platformy ASP.NET Core autoryzacji, IAuthorizationService, Razor autoryzacji
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="4adf2-104">Autoryzacji na podstawie widoku</span><span class="sxs-lookup"><span data-stu-id="4adf2-104">View-based authorization</span></span>

<span data-ttu-id="4adf2-105">Deweloper chce często Pokaż, Ukryj lub modyfikację interfejsu użytkownika na podstawie bieżącej tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4adf2-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="4adf2-106">Można uzyskać dostępu do usługi autoryzacji w obrębie widoków MVC za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4adf2-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="4adf2-107">Aby wstawić usługi autoryzacji do widoku Razor, użyj `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="4adf2-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="4adf2-108">Jeśli ma usługi autoryzacji w każdym widoku `@inject` dyrektywy do *_ViewImports.cshtml* pliku *widoków* katalogu.</span><span class="sxs-lookup"><span data-stu-id="4adf2-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="4adf2-109">Aby uzyskać więcej informacji, zobacz [iniekcji zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4adf2-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="4adf2-110">Korzystania z usługi wprowadzony autoryzacji do wywołania `AuthorizeAsync` w taki sam sposób czy sprawdzania podczas [autoryzacji na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="4adf2-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4adf2-111">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4adf2-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4adf2-112">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4adf2-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="4adf2-113">W niektórych przypadkach zasób zostanie model widoku.</span><span class="sxs-lookup"><span data-stu-id="4adf2-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="4adf2-114">Wywołanie `AuthorizeAsync` w taki sam sposób czy sprawdzania podczas [autoryzacji na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="4adf2-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4adf2-115">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4adf2-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4adf2-116">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4adf2-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="4adf2-117">W powyższym kodzie modelu jest przekazywany jako zasób, które należy wykonać oceny zasad pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="4adf2-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="4adf2-118">Nie należy polegać na przełączanie widoczność elementów interfejsu użytkownika aplikacji, jak wyboru wyłącznie autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4adf2-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="4adf2-119">Ukrywanie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego działania skojarzonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4adf2-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="4adf2-120">Rozważmy na przykład przycisk w poprzednim fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="4adf2-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="4adf2-121">Użytkownik może wywołać `Edit` metody akcji, jeśli użytkownik zna względną zasobów adres URL jest */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="4adf2-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="4adf2-122">Z tego powodu `Edit` metody akcji należy sprawdzić jego własnej autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4adf2-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
