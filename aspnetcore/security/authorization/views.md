---
title: Autoryzacja oparta na widok w programie ASP.NET Core MVC
author: rick-anderson
description: W tym dokumencie pokazano, jak wprowadzić i korzystanie z usługi autoryzacji w widoku platformy ASP.NET Core Razor.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898651"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="8e774-103">Autoryzacja oparta na widok w programie ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="8e774-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="8e774-104">Deweloper chce często Pokazywanie, ukrywanie i inny sposób modyfikować interfejsu użytkownika na podstawie bieżącej tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e774-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="8e774-105">Aby uzyskać dostęp usługi autoryzacji w obrębie widoków MVC za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8e774-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8e774-106">Aby wstawić usługi autoryzacji do widoku Razor, należy użyć `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="8e774-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="8e774-107">Jeśli chcesz, aby usługi autoryzacji w każdym widoku, należy umieścić `@inject` dyrektywy do *_ViewImports.cshtml* pliku *widoków* katalogu.</span><span class="sxs-lookup"><span data-stu-id="8e774-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="8e774-108">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8e774-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="8e774-109">Użyj usługi wprowadzonego kodu autoryzacji do wywołania `AuthorizeAsync` dokładnie tak samo jak zaznaczysz podczas [autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="8e774-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8e774-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8e774-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8e774-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8e774-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="8e774-112">W niektórych przypadkach zasób zostanie modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="8e774-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="8e774-113">Wywoływanie `AuthorizeAsync` dokładnie tak samo jak zaznaczysz podczas [autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="8e774-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8e774-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8e774-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8e774-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8e774-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="8e774-116">W poprzednim kodzie modelu jest przekazywany jako zasób, który powinno zająć oceny zasad pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="8e774-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="8e774-117">Nie należy polegać na przełączanie widoczność elementów interfejsu użytkownika aplikacji, jak wyboru jedyny autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8e774-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="8e774-118">Ukrywanie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego działania skojarzonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8e774-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="8e774-119">Rozważmy na przykład przycisk w poprzednim fragmencie kodu.</span><span class="sxs-lookup"><span data-stu-id="8e774-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="8e774-120">Użytkownik może wywołać `Edit` metody akcji, jeśli użytkownik zna względną zasobu Adres URL jest */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="8e774-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="8e774-121">Z tego powodu `Edit` metody akcji, należy wykonać swoje własne sprawdzenie autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8e774-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
