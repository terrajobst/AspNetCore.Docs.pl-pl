---
title: Autoryzacja oparta na zasobach w ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zaimplementować autoryzację opartą na zasobach w aplikacji ASP.NET Core, gdy atrybut Autoryzuj nie wystarcza.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 835592521c714e270595e1448ae6e0aed1707b77
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590005"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autoryzacja oparta na zasobach w ASP.NET Core

Strategia autoryzacji zależy od zasobów, do których uzyskuje się dostęp. Rozważ dokument, który ma właściwość Author. Tylko autor może zaktualizować dokument. W związku z tym dokument musi zostać pobrany z magazynu danych, zanim będzie można przeprowadzić ocenę autoryzacji.

Obliczanie atrybutu występuje przed powiązaniem danych i przed wykonaniem procedury obsługi stron lub akcji ładującej dokument. Z tych powodów niewystarczająca jest autoryzacja deklaratywna z atrybutem `[Authorize]`. Zamiast tego można wywołać niestandardową metodę autoryzacji &mdash;a stylu znanej jako samodzielna *autoryzacja*.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).

[Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data) zawiera przykładową aplikację, która korzysta z autoryzacji opartej na zasobach.

## <a name="use-imperative-authorization"></a>Używanie bezwzględnej autoryzacji

Autoryzacja jest zaimplementowana jako usługa [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) i jest zarejestrowana w kolekcji usług w ramach klasy `Startup`. Usługa jest udostępniana za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection) do obsługi stron lub akcji.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` ma dwa przeciążania metody `AuthorizeAsync`: jeden z nich akceptuje zasób i nazwę zasad, a pozostałe zaakceptują zasób i listę wymagań do obliczenia.

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

W poniższym przykładzie zasób do zabezpieczenia jest ładowany do niestandardowego obiektu `Document`. Zostanie wywołane Przeciążenie `AuthorizeAsync`, aby określić, czy bieżący użytkownik może edytować podany dokument. Niestandardowe zasady autoryzacji "EditPolicy" są uwzględniane w decyzji. Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji, zobacz [niestandardową autoryzację opartą na zasadach](xref:security/authorization/policies) .

> [!NOTE]
> W poniższych przykładach kodu założono, że uwierzytelnianie zostało uruchomione i ustawiono właściwość `User`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Napisz procedurę obsługi opartą na zasobach

Pisanie procedury obsługi autoryzacji opartej na zasobach nie jest znacznie inne niż [pisanie procedury obsługi zwykłego wymagania](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Utwórz niestandardową klasę wymagania i zaimplementuj klasę obsługi wymagań. Aby uzyskać więcej informacji na temat tworzenia klasy wymagań, zobacz [wymagania](xref:security/authorization/policies#requirements).

Klasa obsługi określa typ wymagania i zasobu. Na przykład program obsługi wykorzystujący `SameAuthorRequirement` i zasób `Document`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

W poprzednim przykładzie Załóżmy, że `SameAuthorRequirement` jest szczególnym przypadkiem bardziej generycznej `SpecificAuthorRequirement` klasy. Klasa `SpecificAuthorRequirement` (niepokazywana) zawiera właściwość `Name` reprezentującą nazwę autora. Właściwość `Name` może zostać ustawiona na bieżącego użytkownika.

Zarejestrowanie wymagania i procedury obsługi w `Startup.ConfigureServices`:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Wymagania operacyjne

Jeśli podejmujesz decyzje w oparciu o wyniki operacji CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), użyj klasy pomocnika [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) . Ta klasa umożliwia pisanie pojedynczej procedury obsługi zamiast pojedynczej klasy dla każdego typu operacji. Aby go użyć, Podaj nazwy niektórych operacji:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Procedura obsługi jest implementowana w następujący sposób przy użyciu wymagania `OperationAuthorizationRequirement` i zasobu `Document`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

Poprzednia procedura obsługi sprawdza poprawność operacji przy użyciu zasobu, tożsamości użytkownika i właściwości `Name` wymaganie.

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a>Wyzwania i Zabroń przy użyciu obsługi zasobów operacyjnych

W tej sekcji pokazano, jak są przetwarzane wyniki akcji wezwanie i zabraniające działania oraz jak wyzwania i zabraniają się.

Aby wywołać procedurę obsługi zasobów operacyjnych, określ operację podczas wywoływania `AuthorizeAsync` w obsłudze stron lub akcji. Poniższy przykład określa, czy uwierzytelniony użytkownik może wyświetlić podany dokument.

> [!NOTE]
> W poniższych przykładach kodu założono, że uwierzytelnianie zostało uruchomione i ustawiono właściwość `User`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Jeśli autoryzacja powiedzie się, zostanie zwrócona Strona do wyświetlania dokumentu. Jeśli autoryzacja nie powiedzie się, ale użytkownik zostanie uwierzytelniony, zwrócenie `ForbidResult` informuje wszystkie oprogramowanie pośredniczące uwierzytelniania, które nie powiodło się. Podczas uwierzytelniania należy zwrócić `ChallengeResult`. W przypadku klientów interakcyjnej przeglądarki może być konieczne przekierowanie użytkownika do strony logowania.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Jeśli autoryzacja powiedzie się, zostanie zwrócony widok dla dokumentu. Jeśli autoryzacja nie powiedzie się, zwraca `ChallengeResult` informuje inne oprogramowanie pośredniczące uwierzytelniania, że autoryzacja nie powiodła się, a oprogramowanie pośredniczące może pobrać odpowiednią odpowiedź. Odpowiednia odpowiedź może zwrócić kod stanu 401 lub 403. W przypadku klientów interakcyjnych przeglądarki może to oznaczać przekierowanie użytkownika do strony logowania.

::: moniker-end
