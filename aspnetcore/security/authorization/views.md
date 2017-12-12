---
title: Autoryzacji opartej na widoku w programie ASP.NET MVC Core
author: rick-anderson
description: "Ten dokument przedstawia sposób wstrzyknąć i korzystać z usługi autoryzacji wewnątrz widoku Razor aplikacji ASP.NET Core."
keywords: Platformy ASP.NET Core autoryzacji, IAuthorizationService, Razor autoryzacji
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a>Autoryzacji na podstawie widoku

Deweloper chce często Pokaż, Ukryj lub modyfikację interfejsu użytkownika na podstawie bieżącej tożsamości użytkownika. Można uzyskać dostępu do usługi autoryzacji w obrębie widoków MVC za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Aby wstawić usługi autoryzacji do widoku Razor, użyj `@inject` dyrektywy:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Jeśli ma usługi autoryzacji w każdym widoku `@inject` dyrektywy do *_ViewImports.cshtml* pliku *widoków* katalogu. Aby uzyskać więcej informacji, zobacz [iniekcji zależności do widoków](xref:mvc/views/dependency-injection).

Korzystania z usługi wprowadzony autoryzacji do wywołania `AuthorizeAsync` w taki sam sposób czy sprawdzania podczas [autoryzacji na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

W niektórych przypadkach zasób zostanie model widoku. Wywołanie `AuthorizeAsync` w taki sam sposób czy sprawdzania podczas [autoryzacji na podstawie zasobów](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

W powyższym kodzie modelu jest przekazywany jako zasób, które należy wykonać oceny zasad pod uwagę.

> [!WARNING]
> Nie należy polegać na przełączanie widoczność elementów interfejsu użytkownika aplikacji, jak wyboru wyłącznie autoryzacji. Ukrywanie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego działania skojarzonego kontrolera. Rozważmy na przykład przycisk w poprzednim fragmentu kodu. Użytkownik może wywołać `Edit` metody akcji, jeśli użytkownik zna względną zasobów adres URL jest */Document/Edit/1*. Z tego powodu `Edit` metody akcji należy sprawdzić jego własnej autoryzacji.
