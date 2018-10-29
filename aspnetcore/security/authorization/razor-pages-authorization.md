---
title: Konwencje autoryzacja stron razor, w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak kontrolować dostęp do stron z konwencjami, które autoryzować użytkowników i zezwolić anonimowym użytkownikom dostępu do strony lub foldery stron.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207267"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="76772-103">Konwencje autoryzacja stron razor, w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76772-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="76772-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="76772-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="76772-105">Jednym ze sposobów w celu kontroli dostępu w aplikacji stron Razor jest używać konwencji autoryzacji podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="76772-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="76772-106">Konwencje te umożliwiają autoryzować użytkowników i zezwolić anonimowym użytkownikom dostępu do poszczególnych stron lub foldery stron.</span><span class="sxs-lookup"><span data-stu-id="76772-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="76772-107">Stosuje się konwencje opisane w tym temacie automatycznie [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) celu kontroli dostępu.</span><span class="sxs-lookup"><span data-stu-id="76772-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="76772-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="76772-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="76772-109">Ta aplikacja używa przykładowych [uwierzytelniania plików Cookie bez użycia produktu ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="76772-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="76772-110">Konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest ustalona w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76772-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="76772-111">Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszystkie hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="76772-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="76772-112">Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="76772-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="76772-113">Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="76772-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="76772-114">Aby korzystać z tożsamości platformy ASP.NET Core, postępuj zgodnie ze wskazówkami w [wprowadzenie do tożsamości programu ASP.NET Core](xref:security/authentication/identity) tematu.</span><span class="sxs-lookup"><span data-stu-id="76772-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="76772-115">Pojęcia i przykłady przedstawione w tym temacie stosuje się jednakowo do aplikacji, które używają tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="76772-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="76772-116">Wymaga autoryzacji do dostępu do strony</span><span class="sxs-lookup"><span data-stu-id="76772-116">Require authorization to access a page</span></span>

<span data-ttu-id="76772-117">Użyj [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="76772-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="76772-118">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.</span><span class="sxs-lookup"><span data-stu-id="76772-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="76772-119">[Przeciążenia AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="76772-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="76772-120">`AuthorizeFilter` Mogą być stosowane do klasy modelu strony z `[Authorize]` atrybutu filtru.</span><span class="sxs-lookup"><span data-stu-id="76772-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="76772-121">Aby uzyskać więcej informacji, zobacz [atrybutu filtru autoryzacji](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="76772-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="76772-122">Wymaga zezwolenia na dostęp do folderu stron</span><span class="sxs-lookup"><span data-stu-id="76772-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="76772-123">Użyj [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="76772-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="76772-124">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.</span><span class="sxs-lookup"><span data-stu-id="76772-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="76772-125">[Przeciążenia AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="76772-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="76772-126">Wymaga zezwolenia na dostęp do strony obszaru</span><span class="sxs-lookup"><span data-stu-id="76772-126">Require authorization to access an area page</span></span>

<span data-ttu-id="76772-127">Użyj [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stronie obszaru w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="76772-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="76772-128">Nazwa strony jest ścieżkę do pliku bez rozszerzenia względem katalogu głównego stron dla określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="76772-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="76772-129">Na przykład nazwa strony dla pliku *Areas/Identity/Pages/Manage/Accounts.cshtml* jest */Zarządzanie/kont*.</span><span class="sxs-lookup"><span data-stu-id="76772-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="76772-130">[Przeciążenia AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="76772-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="76772-131">Wymaga zezwolenia na dostęp do folderu obszarów</span><span class="sxs-lookup"><span data-stu-id="76772-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="76772-132">Użyj [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich obszarów, w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="76772-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="76772-133">Ścieżka folderu jest ścieżka do folderu względem katalogu głównego stron dla określonego obszaru.</span><span class="sxs-lookup"><span data-stu-id="76772-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="76772-134">Na przykład ścieżka folderu plików w obszarze *obszarów/tożsamość/stron/zarządzanie/* jest *i zarządzanie nim*.</span><span class="sxs-lookup"><span data-stu-id="76772-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="76772-135">[Przeciążenia AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="76772-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="76772-136">Zezwalaj na dostęp anonimowy do strony</span><span class="sxs-lookup"><span data-stu-id="76772-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="76772-137">Użyj [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="76772-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="76772-138">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.</span><span class="sxs-lookup"><span data-stu-id="76772-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="76772-139">Zezwalaj na dostęp anonimowy do folderu stron</span><span class="sxs-lookup"><span data-stu-id="76772-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="76772-140">Użyj [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze w określonej ścieżce:</span><span class="sxs-lookup"><span data-stu-id="76772-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="76772-141">Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.</span><span class="sxs-lookup"><span data-stu-id="76772-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="76772-142">Uwaga dotycząca łączenie uprawnień i dostępu anonimowego</span><span class="sxs-lookup"><span data-stu-id="76772-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="76772-143">Jest on doskonale prawidłowy, aby określić, że folder stron wymaga autoryzacji i określić strony, w tym folderze zezwala na dostęp anonimowy:</span><span class="sxs-lookup"><span data-stu-id="76772-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="76772-144">Odwrotnej kolejności, jednak nie jest spełniony.</span><span class="sxs-lookup"><span data-stu-id="76772-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="76772-145">Nie można zadeklarować folderu stron dla dostępu anonimowego i określanie strony w celu autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="76772-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="76772-146">Wymaga autoryzacji na stronie prywatnych nie będą działać, ponieważ gdy zarówno `AllowAnonymousFilter` i `AuthorizeFilter` filtry są stosowane do strony, `AllowAnonymousFilter` wins i kontrolujących dostęp.</span><span class="sxs-lookup"><span data-stu-id="76772-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76772-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="76772-147">Additional resources</span></span>

* [<span data-ttu-id="76772-148">Razor strony trasy i strony modelu dostawcy niestandardowi</span><span class="sxs-lookup"><span data-stu-id="76772-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="76772-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) klasy</span><span class="sxs-lookup"><span data-stu-id="76772-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
