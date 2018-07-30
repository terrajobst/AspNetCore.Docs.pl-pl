---
title: Autoryzacja oparta na widok w programie ASP.NET Core MVC
author: rick-anderson
description: W tym dokumencie pokazano, jak wprowadzić i korzystanie z usługi autoryzacji w widoku platformy ASP.NET Core Razor.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342539"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autoryzacja oparta na widok w programie ASP.NET Core MVC

Deweloper chce często Pokazywanie, ukrywanie i inny sposób modyfikować interfejsu użytkownika na podstawie bieżącej tożsamości użytkownika. Aby uzyskać dostęp usługi autoryzacji w obrębie widoków MVC za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection). Aby wstawić usługi autoryzacji do widoku Razor, należy użyć `@inject` dyrektywy:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Jeśli chcesz, aby usługi autoryzacji w każdym widoku, należy umieścić `@inject` dyrektywy do *_ViewImports.cshtml* pliku *widoków* katalogu. Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).

Użyj usługi wprowadzonego kodu autoryzacji do wywołania `AuthorizeAsync` dokładnie tak samo jak zaznaczysz podczas [autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

W niektórych przypadkach zasób zostanie modelu widoku. Wywoływanie `AuthorizeAsync` dokładnie tak samo jak zaznaczysz podczas [autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

W poprzednim kodzie modelu jest przekazywany jako zasób, który powinno zająć oceny zasad pod uwagę.

> [!WARNING]
> Nie należy polegać na przełączanie widoczność elementów interfejsu użytkownika aplikacji, jak wyboru jedyny autoryzacji. Ukrywanie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego działania skojarzonego kontrolera. Rozważmy na przykład przycisk w poprzednim fragmencie kodu. Użytkownik może wywołać `Edit` metody akcji, jeśli użytkownik zna względną zasobu Adres URL jest */Document/Edit/1*. Z tego powodu `Edit` metody akcji, należy wykonać swoje własne sprawdzenie autoryzacji.
