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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Konwencje autoryzacja stron razor, w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Jednym ze sposobów w celu kontroli dostępu w aplikacji stron Razor jest używać konwencji autoryzacji podczas uruchamiania. Konwencje te umożliwiają autoryzować użytkowników i zezwolić anonimowym użytkownikom dostępu do poszczególnych stron lub foldery stron. Stosuje się konwencje opisane w tym temacie automatycznie [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) celu kontroli dostępu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Ta aplikacja używa przykładowych [uwierzytelniania plików Cookie bez użycia produktu ASP.NET Core Identity](xref:security/authentication/cookie). Konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest ustalona w aplikacji. Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszystkie hasła do logowania użytkownika. Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku. Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych. Aby korzystać z tożsamości platformy ASP.NET Core, postępuj zgodnie ze wskazówkami w [wprowadzenie do tożsamości programu ASP.NET Core](xref:security/authentication/identity) tematu. Pojęcia i przykłady przedstawione w tym temacie stosuje się jednakowo do aplikacji, które używają tożsamości platformy ASP.NET Core.

## <a name="require-authorization-to-access-a-page"></a>Wymaga autoryzacji do dostępu do strony

Użyj [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.

[Przeciążenia AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> `AuthorizeFilter` Mogą być stosowane do klasy modelu strony z `[Authorize]` atrybutu filtru. Aby uzyskać więcej informacji, zobacz [atrybutu filtru autoryzacji](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymaga zezwolenia na dostęp do folderu stron

Użyj [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.

[Przeciążenia AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Wymaga zezwolenia na dostęp do strony obszaru

Użyj [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stronie obszaru w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Nazwa strony jest ścieżkę do pliku bez rozszerzenia względem katalogu głównego stron dla określonego obszaru. Na przykład nazwa strony dla pliku *Areas/Identity/Pages/Manage/Accounts.cshtml* jest */Zarządzanie/kont*.

[Przeciążenia AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Wymaga zezwolenia na dostęp do folderu obszarów

Użyj [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich obszarów, w folderze w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Ścieżka folderu jest ścieżka do folderu względem katalogu głównego stron dla określonego obszaru. Na przykład ścieżka folderu plików w obszarze *obszarów/tożsamość/stron/zarządzanie/* jest *i zarządzanie nim*.

[Przeciążenia AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) jest dostępna, jeśli chcesz określić zasady autoryzacji.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Uwaga dotycząca łączenie uprawnień i dostępu anonimowego

Jest on doskonale prawidłowy, aby określić, że folder stron wymaga autoryzacji i określić strony, w tym folderze zezwala na dostęp anonimowy:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Odwrotnej kolejności, jednak nie jest spełniony. Nie można zadeklarować folderu stron dla dostępu anonimowego i określanie strony w celu autoryzacji:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Wymaga autoryzacji na stronie prywatnych nie będą działać, ponieważ gdy zarówno `AllowAnonymousFilter` i `AuthorizeFilter` filtry są stosowane do strony, `AllowAnonymousFilter` wins i kontrolujących dostęp.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Razor strony trasy i strony modelu dostawcy niestandardowi](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) klasy
