---
title: Autoryzacja oparta na zasób w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zaimplementować autoryzacja na podstawie zasobów w aplikacji ASP.NET Core, gdy atrybut autoryzacji nie wystarczyć.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 2cb3844a38f7482c27fb471343109d51a516ea20
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206699"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autoryzacja oparta na zasób w programie ASP.NET Core

Strategia autoryzacji zależy od zasobu, do którego uzyskiwany jest dostęp. Należy wziąć pod uwagę dokumentu, który ma właściwość autora. Tylko autor może zaktualizować dokumentu. W związku z tym dokument musi zostać pobrany z magazynu danych, zanim nastąpi oceny autoryzacji.

Atrybut występuje przed powiązanie danych oraz przed wykonaniem tej procedury obsługi strony lub akcji, która ładuje dokumentu. Z tego względu deklaratywne autoryzacją za pomocą `[Authorize]` atrybutu nie wystarczyć. Zamiast tego należy wywołać metodę autoryzacja niestandardowa&mdash;stylu, znane jako imperatywne autoryzacji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

[Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data) zawiera przykładową aplikację, która używa autoryzacja na podstawie zasobów.

## <a name="use-imperative-authorization"></a>Użycie imperatywnych autoryzacji

Autoryzacja jest implementowany jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) usługi i nie jest zarejestrowany w kolekcji usługi w ramach `Startup` klasy. Usługa jest udostępniana za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) do obsługi strony lub akcji.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` ma dwa `AuthorizeAsync` przeciążenia metody: przyjmuje jeden zasób i nazwę zasady i innych akceptowanie zasobu i zawiera listę wymagań do oceny.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

W poniższym przykładzie zasobu, który ma zostać zabezpieczony jest ładowany do niestandardowego `Document` obiektu. `AuthorizeAsync` Przeciążenie jest wywoływane w celu ustalenia, czy bieżący użytkownik może edytować podany dokument. Niestandardowe zasady autoryzacji "EditPolicy" jest brane pod uwagę decyzji. Zobacz [niestandardowego na podstawie zasad autoryzacji](xref:security/authorization/policies) Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji.

> [!NOTE]
> Poniższy kod przykładów przyjęto założenie, uwierzytelnianie zostało uruchomione i ustaw `User` właściwości.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Pisanie programu obsługi opartego na zasobach

Pisanie programu obsługi, do autoryzacji opartej na zasób nie jest znacznie różni się od [pisanie programu obsługi wymagań zwykły](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Utwórz niestandardowe wymagania klasę i zaimplementować klasę programu obsługi wymagań. Klasy obsługi określa wymagań i typu zasobu. Na przykład program obsługi przy użyciu `SameAuthorRequirement` wymagań i `Document` zasobów wygląda następująco:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Zarejestruj wymagania oraz program obsługi w `Startup.ConfigureServices` metody:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Wymagania operacyjne

Jeśli wprowadzasz decyzji w oparciu o wyniki operacji CRUD (tworzenia, odczytu, Update, Delete), użyj [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Klasa pomocy. Ta klasa umożliwia pisanie pojedynczy program obsługi, a nie poszczególnych klas dla każdego typu operacji. Aby go użyć, należy podać nazwy niektórych operacji:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Program obsługi jest implementowana w następujący sposób, przy użyciu `OperationAuthorizationRequirement` wymagań i `Document` zasobów:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Poprzedni program obsługi sprawdza poprawność tej operacji z użyciem zasobu, tożsamość użytkownika i wymogów `Name` właściwości.

Aby wywołać procedurę obsługi operacyjnej zasobów, określać podczas wywoływania operacji `AuthorizeAsync` obsługi strony lub akcji. Poniższy przykład określa, czy uwierzytelniony użytkownik jest uprawniony do wyświetlenia podany dokument.

> [!NOTE]
> Poniższy kod przykładów przyjęto założenie, uwierzytelnianie zostało uruchomione i ustaw `User` właściwości.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Jeśli autoryzacja zakończy się powodzeniem, zwracany jest strona do wyświetlania dokumentu. Jeśli autoryzacji kończy się niepowodzeniem, ale użytkownik jest uwierzytelniony, zwracając `ForbidResult` informuje oprogramowanie pośredniczące uwierzytelniania, który Autoryzacja nie powiodła się. Element `ChallengeResult` jest zwracana, gdy można dokonać uwierzytelnienia. Dla klientów w przeglądarkach interaktywne jego może być przekierowanie użytkownika do strony logowania.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Jeśli autoryzacja zakończy się powodzeniem, zwracany jest widok dla dokumentu. W przypadku niepowodzenia autoryzacji zwracanie `ChallengeResult` dowolnego oprogramowania pośredniczącego uwierzytelniania informuje, że autoryzacja nie powiodła się, a oprogramowanie pośredniczące może potrwać właściwą odpowiedź. Właściwa odpowiedź może zwracać kod stanu 401 lub 403. Dla klientów w przeglądarkach interaktywne może oznaczać, że przekierowanie użytkownika do strony logowania.

---
