---
title: Konwencje autoryzacji stron razor w ASP.NET Core
author: guardrex
description: Informacje o sposobie kontrolowania dostępu do stron z konwencjami autoryzacji użytkowników, które użytkownicy anonimowego dostępu do stron lub folderów stron.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: cd1fa7957ca50db0de71f71234f84d3fbc631f45
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341746"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="3524a-103">Konwencje autoryzacji stron razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3524a-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="3524a-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3524a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3524a-105">Jednym ze sposobów kontroli dostępu w aplikacji stron Razor ma używać konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="3524a-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="3524a-106">Konwencje te pozwalają na autoryzowanie użytkowników i anonimowe użytkownicy mogą uzyskiwać dostęp do poszczególnych stron lub folderów stron.</span><span class="sxs-lookup"><span data-stu-id="3524a-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="3524a-107">Konwencje opisanych w tym temacie automatycznie zastosować [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="3524a-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="3524a-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3524a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3524a-109">Przykładowe zastosowania aplikacji [uwierzytelniania plików Cookie bez ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="3524a-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="3524a-110">Konto użytkownika dla użytkownika hipotetyczny, Maria Rodriguez jest zapisane na stałe w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3524a-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="3524a-111">Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszelkich hasło do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3524a-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="3524a-112">Użytkownik jest uwierzytelniany w `AuthenticateUser` metody w *Pages/Account/Login.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3524a-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="3524a-113">W przykładzie rzeczywistych użytkownik będzie uwierzytelniony w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3524a-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="3524a-114">Aby korzystać z ASP.NET Core Identity, postępuj zgodnie ze wskazówkami w [wprowadzenie do tożsamości na platformy ASP.NET Core](xref:security/authentication/identity) tematu.</span><span class="sxs-lookup"><span data-stu-id="3524a-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="3524a-115">Koncepcje i przykłady zamieszczone w tym temacie stosowane jednakowo do aplikacji, które używają ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="3524a-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="3524a-116">Wymaga autoryzacji do dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="3524a-116">Require authorization to access a page</span></span>

<span data-ttu-id="3524a-117">Użyj [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="3524a-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="3524a-118">Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko kreskami ukośnymi.</span><span class="sxs-lookup"><span data-stu-id="3524a-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="3524a-119">[Przeciążenia AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, gdy konieczne jest określenie zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="3524a-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="3524a-120">`AuthorizeFilter` Może odnosić się do strony klasy modelu z `[Authorize]` atrybutu filtru.</span><span class="sxs-lookup"><span data-stu-id="3524a-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="3524a-121">Aby uzyskać więcej informacji, zobacz [atrybutu filtru autoryzacji](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="3524a-121">For more information, see [Authorize filter attribute](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="3524a-122">Wymaga zezwolenia na dostęp do folderu stron</span><span class="sxs-lookup"><span data-stu-id="3524a-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="3524a-123">Użyj [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="3524a-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="3524a-124">Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor.</span><span class="sxs-lookup"><span data-stu-id="3524a-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="3524a-125">[Przeciążenia AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, gdy konieczne jest określenie zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="3524a-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="3524a-126">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="3524a-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="3524a-127">Użyj [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="3524a-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="3524a-128">Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko kreskami ukośnymi.</span><span class="sxs-lookup"><span data-stu-id="3524a-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="3524a-129">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="3524a-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="3524a-130">Użyj [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="3524a-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="3524a-131">Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor.</span><span class="sxs-lookup"><span data-stu-id="3524a-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="3524a-132">Uwaga na łączenie uprawnień i dostępu anonimowego</span><span class="sxs-lookup"><span data-stu-id="3524a-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="3524a-133">Jest on idealnie prawidłowy, aby określić, że folder stron wymaga autoryzacji i określić, że strony w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="3524a-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="3524a-134">Odwrotnie, jednak nie jest spełniony.</span><span class="sxs-lookup"><span data-stu-id="3524a-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="3524a-135">Nie można zadeklarować folderu stron dla dostępu anonimowego i określ strony na potrzeby autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="3524a-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="3524a-136">Wymaganie autoryzacji na stronie prywatnych nie będą działać, ponieważ podczas zarówno `AllowAnonymousFilter` i `AuthorizeFilter` filtry są stosowane do strony, `AllowAnonymousFilter` wins i kontrolujących dostęp.</span><span class="sxs-lookup"><span data-stu-id="3524a-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3524a-137">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3524a-137">Additional resources</span></span>

* [<span data-ttu-id="3524a-138">Razor strony trasy i strony modelu dostawców niestandardowych</span><span class="sxs-lookup"><span data-stu-id="3524a-138">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="3524a-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) — klasa</span><span class="sxs-lookup"><span data-stu-id="3524a-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
