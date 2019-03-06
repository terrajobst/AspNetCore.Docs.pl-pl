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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Konwencje autoryzacja stron razor, w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Jednym ze sposobów w celu kontroli dostępu w aplikacji stron Razor jest używać konwencji autoryzacji podczas uruchamiania. Konwencje te umożliwiają autoryzować użytkowników i zezwolić anonimowym użytkownikom dostępu do poszczególnych stron lub foldery stron. Stosuje się konwencje opisane w tym temacie automatycznie [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) celu kontroli dostępu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Ta aplikacja używa przykładowych [uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity](xref:security/authentication/cookie). Pojęcia i przykłady przedstawione w tym temacie stosuje się jednakowo do aplikacji, które używają tożsamości platformy ASP.NET Core. Aby korzystać z tożsamości platformy ASP.NET Core, postępuj zgodnie ze wskazówkami w <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Wymaga autoryzacji do dostępu do strony

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Mogą być stosowane do klasy modelu strony z `[Authorize]` atrybutu filtru. Aby uzyskać więcej informacji, zobacz [atrybutu filtru autoryzacji](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymaga zezwolenia na dostęp do folderu stron

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Wymaga zezwolenia na dostęp do strony obszaru

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> na stronie obszaru w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Nazwa strony jest ścieżkę do pliku bez rozszerzenia względem katalogu głównego stron dla określonego obszaru. Na przykład nazwa strony dla pliku *Areas/Identity/Pages/Manage/Accounts.cshtml* jest */Zarządzanie/kont*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Wymaga zezwolenia na dostęp do folderu obszarów

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów, w folderze w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Ścieżka folderu jest ścieżka do folderu względem katalogu głównego stron dla określonego obszaru. Na przykład ścieżka folderu plików w obszarze *obszarów/tożsamość/stron/zarządzanie/* jest *i zarządzanie nim*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko ukośników.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Konwencji za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Określona ścieżka jest ścieżka aparat widoku, która jest ścieżką względną głównego stron Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Uwaga dotycząca łączenie uprawnień i dostępu anonimowego

Jest on prawidłowy, aby określić, że folder stron, które wymagają autoryzacji i niż określać, czy strony, w tym folderze zezwala na dostęp anonimowy:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Jednakże, odwrotna jest nieprawidłowa. Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić strony, w tym folderze, który wymaga autoryzacji:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Wymaganie autoryzacji na stronie prywatnych nie powiedzie się. Gdy zarówno <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> są stosowane do strony, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ma pierwszeństwo i kontrolujących dostęp.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
