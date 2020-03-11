---
title: Autoryzacja na podstawie widoku w ASP.NET Core MVC
author: rick-anderson
description: W tym dokumencie pokazano, jak wstrzyknąć i wykorzystać usługę autoryzacji wewnątrz widoku ASP.NET Core Razor.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663598"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="05aff-103">Autoryzacja na podstawie widoku w ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="05aff-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="05aff-104">Deweloper często chce pokazać, ukryć lub zmodyfikować inny interfejs użytkownika w oparciu o bieżącą tożsamość użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05aff-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="05aff-105">Dostęp do usługi autoryzacji można uzyskać w widokach MVC za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05aff-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="05aff-106">Aby wstrzyknąć usługę autoryzacji do widoku Razor, użyj dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="05aff-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="05aff-107">Jeśli chcesz, aby usługa autoryzacji była w każdym widoku, umieść dyrektywę `@inject` w pliku *_ViewImports. cshtml* w katalogu *widoki* .</span><span class="sxs-lookup"><span data-stu-id="05aff-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="05aff-108">Aby uzyskać więcej informacji, zobacz [iniekcja zależności w widokach](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05aff-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="05aff-109">Użyj wstrzykiwanej usługi autoryzacji do wyzwolenia `AuthorizeAsync` w taki sam sposób, jak podczas [autoryzacji opartej na zasobach](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="05aff-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

<span data-ttu-id="05aff-110">W niektórych przypadkach zasób będzie modelem widoku.</span><span class="sxs-lookup"><span data-stu-id="05aff-110">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="05aff-111">Wywołaj `AuthorizeAsync` w taki sam sposób, jak podczas [autoryzacji opartej na zasobach](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="05aff-111">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

<span data-ttu-id="05aff-112">W poprzednim kodzie model jest przenoszona jako zasób, a Ocena zasad powinna być uwzględniana.</span><span class="sxs-lookup"><span data-stu-id="05aff-112">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="05aff-113">Nie należy polegać na przełączaniu widoczności elementów interfejsu użytkownika aplikacji jako pojedynczej kontroli autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="05aff-113">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="05aff-114">Ukrycie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego skojarzonej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="05aff-114">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="05aff-115">Rozważmy na przykład przycisk w powyższym fragmencie kodu.</span><span class="sxs-lookup"><span data-stu-id="05aff-115">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="05aff-116">Użytkownik może wywołać metodę akcji `Edit`, jeśli wie, że adres URL zasobu względnego to */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="05aff-116">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="05aff-117">Z tego powodu metoda działania `Edit` powinna wykonywać własne sprawdzanie autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="05aff-117">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
