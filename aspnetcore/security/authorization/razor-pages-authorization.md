---
title: Konwencje autoryzacja stron razor, w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak kontrolować dostęp do stron z konwencjami, które autoryzować użytkowników i zezwolić anonimowym użytkownikom dostępu do strony lub foldery stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345512"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="01cd9-103">Konwencje autoryzacja stron razor, w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01cd9-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="01cd9-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="01cd9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="01cd9-105">Jednym ze sposobów w celu kontroli dostępu w aplikacji stron Razor jest używać konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="01cd9-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="01cd9-106">Konwencje te umożliwiają autoryzować użytkowników i zezwolić anonimowym użytkownikom dostępu do poszczególnych stron lub foldery stron.</span><span class="sxs-lookup"><span data-stu-id="01cd9-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="01cd9-107">Stosuje się konwencje opisane w tym temacie automatycznie [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) celu kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="01cd9-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="01cd9-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01cd9-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="01cd9-109">Ta aplikacja używa przykładowych [uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="01cd9-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="01cd9-110">Pojęcia i przykłady przedstawione w tym temacie stosuje się jednakowo do aplikacji, które używają tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01cd9-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="01cd9-111">Aby korzystać z tożsamości platformy ASP.NET Core, postępuj zgodnie ze wskazówkami w <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="01cd9-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="01cd9-112">Wymaga autoryzacji do dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="01cd9-112">Require authorization to access a page</span></span>

<span data-ttu-id="01cd9-113">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="01cd9-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="01cd9-114">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.</span><span class="sxs-lookup"><span data-stu-id="01cd9-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="01cd9-115">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="01cd9-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="01cd9-116"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Mogą być stosowane do klasy modelu strony z `[Authorize]` atrybutu filtru.</span><span class="sxs-lookup"><span data-stu-id="01cd9-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="01cd9-117">Aby uzyskać więcej informacji, zobacz [atrybutu filtru autoryzacji](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="01cd9-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="01cd9-118">Wymaga zezwolenia na dostęp do folderu stron</span><span class="sxs-lookup"><span data-stu-id="01cd9-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="01cd9-119">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="01cd9-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="01cd9-120">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.</span><span class="sxs-lookup"><span data-stu-id="01cd9-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="01cd9-121">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="01cd9-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="01cd9-122">Wymaga zezwolenia na dostęp do strony obszaru</span><span class="sxs-lookup"><span data-stu-id="01cd9-122">Require authorization to access an area page</span></span>

<span data-ttu-id="01cd9-123">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> na stronie obszaru w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="01cd9-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="01cd9-124">Nazwa strony jest ścieżkę do pliku bez rozszerzenia względem katalogu głównego stron dla określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="01cd9-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="01cd9-125">Na przykład nazwa strony dla pliku *Areas/Identity/Pages/Manage/Accounts.cshtml* jest */Zarządzanie/kont*.</span><span class="sxs-lookup"><span data-stu-id="01cd9-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="01cd9-126">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="01cd9-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="01cd9-127">Wymaga zezwolenia na dostęp do folderu obszarów</span><span class="sxs-lookup"><span data-stu-id="01cd9-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="01cd9-128">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów, w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="01cd9-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="01cd9-129">Ścieżka folderu jest ścieżka do folderu względem katalogu głównego stron dla określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="01cd9-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="01cd9-130">Na przykład ścieżka folderu plików w obszarze *obszarów/tożsamość/stron/zarządzanie/* jest *i zarządzanie nim*.</span><span class="sxs-lookup"><span data-stu-id="01cd9-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="01cd9-131">Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="01cd9-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="01cd9-132">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="01cd9-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="01cd9-133">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="01cd9-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="01cd9-134">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.</span><span class="sxs-lookup"><span data-stu-id="01cd9-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="01cd9-135">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="01cd9-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="01cd9-136">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="01cd9-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="01cd9-137">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.</span><span class="sxs-lookup"><span data-stu-id="01cd9-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="01cd9-138">Uwaga dotycząca łączenie uprawnień i dostępu anonimowego</span><span class="sxs-lookup"><span data-stu-id="01cd9-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="01cd9-139">Jest on prawidłowy, aby określić, że folder stron, które wymagają autoryzacji i niż określać, czy strony, w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="01cd9-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="01cd9-140">Jednakże, odwrotna jest nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="01cd9-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="01cd9-141">Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić strony, w tym folderze, który wymaga autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="01cd9-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="01cd9-142">Wymaganie autoryzacji na stronie prywatnych nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="01cd9-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="01cd9-143">Gdy zarówno <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> są stosowane do strony, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ma pierwszeństwo i kontrolujących dostęp.</span><span class="sxs-lookup"><span data-stu-id="01cd9-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01cd9-144">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="01cd9-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
