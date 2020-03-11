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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Razor Pages Konwencji autoryzacji w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania. Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron. Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie). Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core. Aby użyć ASP.NET Core Identity, postępuj zgodnie ze wskazówkami w <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Wymagaj autoryzacji dostępu do strony

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> można zastosować do klasy modelu strony z atrybutem filtru `[Authorize]`. Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymagaj autoryzacji dostępu do folderu stron

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Wymagaj autoryzacji dostępu do strony obszaru

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru. Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Wymagaj autoryzacji dostępu do folderu obszarów

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru. Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Uwaga na temat łączenia autoryzowanych i anonimowych dostępu

Należy określić, że folder stron wymaga autoryzacji, a następnie określić, że strona w tym folderze zezwala na dostęp anonimowy:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Odwrócenie jest jednak nieprawidłowe. Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić stronę w tym folderze, która wymaga autoryzacji:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem. Gdy na stronie są stosowane zarówno <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, jak i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ma pierwszeństwo i kontroluje dostęp.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania. Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron. Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie). Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core. Aby użyć ASP.NET Core Identity, postępuj zgodnie ze wskazówkami w <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Wymagaj autoryzacji dostępu do strony

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> można zastosować do klasy modelu strony z atrybutem filtru `[Authorize]`. Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymagaj autoryzacji dostępu do folderu stron

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Wymagaj autoryzacji dostępu do strony obszaru

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru. Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Wymagaj autoryzacji dostępu do folderu obszarów

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru. Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj Konwencji <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> za pośrednictwem <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Uwaga na temat łączenia autoryzowanych i anonimowych dostępu

Prawidłowe jest określenie, że folder stron, które wymagają autoryzacji, a nie określa, że strona w tym folderze zezwala na dostęp anonimowy:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Odwrócenie jest jednak nieprawidłowe. Nie można zadeklarować folderu stron dla dostępu anonimowego, a następnie określić stronę w tym folderze, która wymaga autoryzacji:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem. Gdy na stronie są stosowane zarówno <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, jak i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ma pierwszeństwo i kontroluje dostęp.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
