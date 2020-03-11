---
title: Razor Pages Konwencji autoryzacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować dostęp do stron z konwencjami, które autoryzują użytkowników i umożliwiają anonimowym użytkownikom dostęp do stron lub folderów stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 00fc487c6ac802f213bcf83994ecc2b1a1468589
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662058"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="4a907-103">Razor Pages Konwencji autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a907-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4a907-104">Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="4a907-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="4a907-105">Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron.</span><span class="sxs-lookup"><span data-stu-id="4a907-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="4a907-106">Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="4a907-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="4a907-107">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a907-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4a907-108">Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="4a907-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="4a907-109">Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a907-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="4a907-110">Aby użyć ASP.NET Core Identity, postępuj zgodnie ze wskazówkami w <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="4a907-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="4a907-111">Wymagaj autoryzacji dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="4a907-111">Require authorization to access a page</span></span>

<span data-ttu-id="4a907-112">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="4a907-113">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="4a907-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="4a907-114">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="4a907-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="4a907-115"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> można zastosować do klasy modelu strony z atrybutem filtru `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="4a907-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="4a907-116">Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="4a907-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="4a907-117">Wymagaj autoryzacji dostępu do folderu stron</span><span class="sxs-lookup"><span data-stu-id="4a907-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="4a907-118">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="4a907-119">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="4a907-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="4a907-120">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="4a907-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="4a907-121">Wymagaj autoryzacji dostępu do strony obszaru</span><span class="sxs-lookup"><span data-stu-id="4a907-121">Require authorization to access an area page</span></span>

<span data-ttu-id="4a907-122">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="4a907-123">Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="4a907-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="4a907-124">Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="4a907-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="4a907-125">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="4a907-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="4a907-126">Wymagaj autoryzacji dostępu do folderu obszarów</span><span class="sxs-lookup"><span data-stu-id="4a907-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="4a907-127">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="4a907-128">Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="4a907-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="4a907-129">Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.</span><span class="sxs-lookup"><span data-stu-id="4a907-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="4a907-130">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="4a907-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="4a907-131">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="4a907-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="4a907-132">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="4a907-133">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="4a907-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="4a907-134">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="4a907-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="4a907-135">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="4a907-136">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="4a907-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="4a907-137">Uwaga na temat łączenia autoryzowanych i anonimowych dostępu</span><span class="sxs-lookup"><span data-stu-id="4a907-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="4a907-138">Należy określić, że folder stron wymaga autoryzacji, a następnie określić, że strona w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="4a907-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="4a907-139">Odwrócenie jest jednak nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="4a907-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="4a907-140">Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić stronę w tym folderze, która wymaga autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="4a907-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="4a907-141">Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4a907-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="4a907-142">Gdy na stronie są stosowane zarówno <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, jak i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ma pierwszeństwo i kontroluje dostęp.</span><span class="sxs-lookup"><span data-stu-id="4a907-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a907-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4a907-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4a907-144">Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="4a907-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="4a907-145">Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron.</span><span class="sxs-lookup"><span data-stu-id="4a907-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="4a907-146">Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="4a907-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="4a907-147">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a907-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4a907-148">Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="4a907-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="4a907-149">Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a907-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="4a907-150">Aby użyć ASP.NET Core Identity, postępuj zgodnie ze wskazówkami w <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="4a907-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="4a907-151">Wymagaj autoryzacji dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="4a907-151">Require authorization to access a page</span></span>

<span data-ttu-id="4a907-152">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="4a907-153">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="4a907-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="4a907-154">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="4a907-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="4a907-155"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> można zastosować do klasy modelu strony z atrybutem filtru `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="4a907-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="4a907-156">Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="4a907-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="4a907-157">Wymagaj autoryzacji dostępu do folderu stron</span><span class="sxs-lookup"><span data-stu-id="4a907-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="4a907-158">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="4a907-159">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="4a907-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="4a907-160">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="4a907-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="4a907-161">Wymagaj autoryzacji dostępu do strony obszaru</span><span class="sxs-lookup"><span data-stu-id="4a907-161">Require authorization to access an area page</span></span>

<span data-ttu-id="4a907-162">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="4a907-163">Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="4a907-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="4a907-164">Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="4a907-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="4a907-165">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="4a907-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="4a907-166">Wymagaj autoryzacji dostępu do folderu obszarów</span><span class="sxs-lookup"><span data-stu-id="4a907-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="4a907-167">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="4a907-168">Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="4a907-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="4a907-169">Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.</span><span class="sxs-lookup"><span data-stu-id="4a907-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="4a907-170">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="4a907-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="4a907-171">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="4a907-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="4a907-172">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="4a907-173">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.</span><span class="sxs-lookup"><span data-stu-id="4a907-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="4a907-174">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="4a907-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="4a907-175">Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="4a907-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="4a907-176">Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.</span><span class="sxs-lookup"><span data-stu-id="4a907-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="4a907-177">Uwaga na temat łączenia autoryzowanych i anonimowych dostępu</span><span class="sxs-lookup"><span data-stu-id="4a907-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="4a907-178">Prawidłowe jest określenie, że folder stron, które wymagają autoryzacji, a nie określa, że strona w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="4a907-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="4a907-179">Odwrócenie jest jednak nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="4a907-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="4a907-180">Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić stronę w tym folderze, która wymaga autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="4a907-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="4a907-181">Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4a907-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="4a907-182">Gdy na stronie są stosowane zarówno <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, jak i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ma pierwszeństwo i kontroluje dostęp.</span><span class="sxs-lookup"><span data-stu-id="4a907-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a907-183">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4a907-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
