---
title: Razor Pages Konwencji autoryzacji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak kontrolować dostęp do stron z konwencjami, które autoryzują użytkowników i umożliwiają anonimowym użytkownikom dostęp do stron lub folderów stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994033"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="c2328-103">Razor Pages Konwencji autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2328-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="c2328-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c2328-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c2328-105">Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c2328-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="c2328-106">Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron.</span><span class="sxs-lookup"><span data-stu-id="c2328-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="c2328-107">Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="c2328-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="c2328-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2328-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c2328-109">Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="c2328-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="c2328-110">Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2328-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="c2328-111">Aby użyć tożsamości ASP.NET Core, postępuj zgodnie ze <xref:security/authentication/identity>wskazówkami w temacie.</span><span class="sxs-lookup"><span data-stu-id="c2328-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="c2328-112">Wymagaj autoryzacji dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="c2328-112">Require authorization to access a page</span></span>

<span data-ttu-id="c2328-113">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*></span><span class="sxs-lookup"><span data-stu-id="c2328-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="c2328-114">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="c2328-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="c2328-115">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="c2328-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="c2328-116">Można zastosować do klasy modelu strony `[Authorize]` z atrybutem Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="c2328-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="c2328-117">Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="c2328-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="c2328-118">Wymagaj autoryzacji dostępu do folderu stron</span><span class="sxs-lookup"><span data-stu-id="c2328-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="c2328-119">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*></span><span class="sxs-lookup"><span data-stu-id="c2328-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="c2328-120">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="c2328-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="c2328-121">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="c2328-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="c2328-122">Wymagaj autoryzacji dostępu do strony obszaru</span><span class="sxs-lookup"><span data-stu-id="c2328-122">Require authorization to access an area page</span></span>

<span data-ttu-id="c2328-123">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*></span><span class="sxs-lookup"><span data-stu-id="c2328-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="c2328-124">Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="c2328-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="c2328-125">Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="c2328-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="c2328-126">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="c2328-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="c2328-127">Wymagaj autoryzacji dostępu do folderu obszarów</span><span class="sxs-lookup"><span data-stu-id="c2328-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="c2328-128">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*></span><span class="sxs-lookup"><span data-stu-id="c2328-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="c2328-129">Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="c2328-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="c2328-130">Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.</span><span class="sxs-lookup"><span data-stu-id="c2328-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="c2328-131">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="c2328-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="c2328-132">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="c2328-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="c2328-133">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*></span><span class="sxs-lookup"><span data-stu-id="c2328-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="c2328-134">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="c2328-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="c2328-135">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="c2328-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="c2328-136">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*></span><span class="sxs-lookup"><span data-stu-id="c2328-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="c2328-137">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="c2328-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="c2328-138">Uwaga na temat łączenia autoryzowanych i anonimowych dostępu</span><span class="sxs-lookup"><span data-stu-id="c2328-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="c2328-139">Prawidłowe jest określenie, że folder stron, które wymagają autoryzacji, a nie określa, że strona w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="c2328-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="c2328-140">Odwrócenie jest jednak nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="c2328-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="c2328-141">Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić stronę w tym folderze, która wymaga autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="c2328-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="c2328-142">Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="c2328-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="c2328-143">Gdy obie <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> są stosowane <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony, ma pierwszeństwo i kontroluje dostęp.</span><span class="sxs-lookup"><span data-stu-id="c2328-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2328-144">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c2328-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c2328-145">Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="c2328-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="c2328-146">Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron.</span><span class="sxs-lookup"><span data-stu-id="c2328-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="c2328-147">Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="c2328-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="c2328-148">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2328-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c2328-149">Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="c2328-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="c2328-150">Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2328-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="c2328-151">Aby użyć tożsamości ASP.NET Core, postępuj zgodnie ze <xref:security/authentication/identity>wskazówkami w temacie.</span><span class="sxs-lookup"><span data-stu-id="c2328-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="c2328-152">Wymagaj autoryzacji dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="c2328-152">Require authorization to access a page</span></span>

<span data-ttu-id="c2328-153">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*></span><span class="sxs-lookup"><span data-stu-id="c2328-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="c2328-154">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="c2328-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="c2328-155">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="c2328-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="c2328-156">Można zastosować do klasy modelu strony `[Authorize]` z atrybutem Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="c2328-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="c2328-157">Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="c2328-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="c2328-158">Wymagaj autoryzacji dostępu do folderu stron</span><span class="sxs-lookup"><span data-stu-id="c2328-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="c2328-159">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*></span><span class="sxs-lookup"><span data-stu-id="c2328-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="c2328-160">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="c2328-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="c2328-161">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="c2328-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="c2328-162">Wymagaj autoryzacji dostępu do strony obszaru</span><span class="sxs-lookup"><span data-stu-id="c2328-162">Require authorization to access an area page</span></span>

<span data-ttu-id="c2328-163">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*></span><span class="sxs-lookup"><span data-stu-id="c2328-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="c2328-164">Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="c2328-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="c2328-165">Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="c2328-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="c2328-166">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="c2328-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="c2328-167">Wymagaj autoryzacji dostępu do folderu obszarów</span><span class="sxs-lookup"><span data-stu-id="c2328-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="c2328-168">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*></span><span class="sxs-lookup"><span data-stu-id="c2328-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="c2328-169">Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="c2328-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="c2328-170">Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.</span><span class="sxs-lookup"><span data-stu-id="c2328-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="c2328-171">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="c2328-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="c2328-172">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="c2328-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="c2328-173">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*></span><span class="sxs-lookup"><span data-stu-id="c2328-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="c2328-174">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="c2328-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="c2328-175">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="c2328-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="c2328-176">Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*></span><span class="sxs-lookup"><span data-stu-id="c2328-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="c2328-177">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="c2328-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="c2328-178">Uwaga na temat łączenia autoryzowanych i anonimowych dostępu</span><span class="sxs-lookup"><span data-stu-id="c2328-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="c2328-179">Prawidłowe jest określenie, że folder stron, które wymagają autoryzacji, a nie określa, że strona w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="c2328-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="c2328-180">Odwrócenie jest jednak nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="c2328-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="c2328-181">Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić stronę w tym folderze, która wymaga autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="c2328-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="c2328-182">Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="c2328-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="c2328-183">Gdy obie <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> są stosowane <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony, ma pierwszeństwo i kontroluje dostęp.</span><span class="sxs-lookup"><span data-stu-id="c2328-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2328-184">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c2328-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
