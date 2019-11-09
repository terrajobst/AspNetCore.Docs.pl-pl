---
title: Autoryzacja na podstawie widoku w ASP.NET Core MVC
author: rick-anderson
description: W tym dokumencie pokazano, jak wstrzyknąć i wykorzystać usługę autoryzacji wewnątrz widoku ASP.NET Core Razor.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896978"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autoryzacja na podstawie widoku w ASP.NET Core MVC

Deweloper często chce pokazać, ukryć lub zmodyfikować inny interfejs użytkownika w oparciu o bieżącą tożsamość użytkownika. Dostęp do usługi autoryzacji można uzyskać w widokach MVC za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection). Aby wstrzyknąć usługę autoryzacji do widoku Razor, użyj dyrektywy `@inject`:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Jeśli chcesz, aby usługa autoryzacji była w każdym widoku, umieść dyrektywę `@inject` w pliku *_ViewImports. cshtml* w katalogu *widoki* . Aby uzyskać więcej informacji, zobacz [iniekcja zależności w widokach](xref:mvc/views/dependency-injection).

Użyj wstrzykiwanej usługi autoryzacji do wyzwolenia `AuthorizeAsync` w taki sam sposób, jak podczas [autoryzacji opartej na zasobach](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

W niektórych przypadkach zasób będzie modelem widoku. Wywołaj `AuthorizeAsync` w taki sam sposób, jak podczas [autoryzacji opartej na zasobach](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

W poprzednim kodzie model jest przenoszona jako zasób, a Ocena zasad powinna być uwzględniana.

> [!WARNING]
> Nie należy polegać na przełączaniu widoczności elementów interfejsu użytkownika aplikacji jako pojedynczej kontroli autoryzacji. Ukrycie elementu interfejsu użytkownika może nie całkowicie uniemożliwić dostęp do jego skojarzonej akcji kontrolera. Rozważmy na przykład przycisk w powyższym fragmencie kodu. Użytkownik może wywołać metodę akcji `Edit`, jeśli wie, że adres URL zasobu względnego to */Document/Edit/1*. Z tego powodu metoda działania `Edit` powinna wykonywać własne sprawdzanie autoryzacji.
