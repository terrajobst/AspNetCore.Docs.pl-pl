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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Razor Pages Konwencji autoryzacji w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania. Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron. Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie). Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core. Aby użyć tożsamości ASP.NET Core, postępuj zgodnie ze <xref:security/authentication/identity>wskazówkami w temacie.

## <a name="require-authorization-to-access-a-page"></a>Wymagaj autoryzacji dostępu do strony

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Można zastosować do klasy modelu strony `[Authorize]` z atrybutem Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymagaj autoryzacji dostępu do folderu stron

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Wymagaj autoryzacji dostępu do strony obszaru

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru. Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Wymagaj autoryzacji dostępu do folderu obszarów

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru. Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

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

Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem. Gdy obie <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> są stosowane <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony, ma pierwszeństwo i kontroluje dostęp.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Jednym ze sposobów kontroli dostępu w aplikacji Razor Pages jest użycie konwencji autoryzacji podczas uruchamiania. Konwencje te umożliwiają Autoryzowanie użytkowników i Zezwalanie użytkownikom anonimowym na dostęp do poszczególnych stron lub folderów stron. Konwencje opisane w tym temacie automatycznie stosują [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja używa [uwierzytelniania plików cookie bez tożsamości ASP.NET Core](xref:security/authentication/cookie). Koncepcje i Przykłady przedstawione w tym temacie dotyczą również aplikacji korzystających z tożsamości ASP.NET Core. Aby użyć tożsamości ASP.NET Core, postępuj zgodnie ze <xref:security/authentication/identity>wskazówkami w temacie.

## <a name="require-authorization-to-access-a-page"></a>Wymagaj autoryzacji dostępu do strony

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Można zastosować do klasy modelu strony `[Authorize]` z atrybutem Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Aby uzyskać więcej informacji, zobacz temat [Autoryzuj atrybut Filter](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymagaj autoryzacji dostępu do folderu stron

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną Razor Pages głównym.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Wymagaj autoryzacji dostępu do strony obszaru

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do strony obszaru w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Nazwa strony jest ścieżką pliku bez rozszerzenia względem katalogu głównego stron określonego obszaru. Na przykład nazwa strony dla *obszaru plik/tożsamość/strony/Zarządzanie/accounts. cshtml* jest */Manage/accounts*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Wymagaj autoryzacji dostępu do folderu obszarów

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> do wszystkich obszarów w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Ścieżka folderu to ścieżka folderu względem katalogu głównego stron określonego obszaru. Na przykład ścieżka folderu dla plików w obszarze *obszary/tożsamość/strony/Zarządzanie/* to */Manage*.

Aby określić [zasady autoryzacji](xref:security/authorization/policies), użyj [przeciążenia AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżką aparatu widoku, która jest ścieżką względną do Razor Pages, bez rozszerzenia i zawierającą tylko ukośniki.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj Konwencji przez <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> , aby dodać <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do wszystkich stron w folderze w określonej ścieżce: <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*>

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

Wymaganie autoryzacji na stronie prywatnej kończy się niepowodzeniem. Gdy obie <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> i <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> są stosowane <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> do strony, ma pierwszeństwo i kontroluje dostęp.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
