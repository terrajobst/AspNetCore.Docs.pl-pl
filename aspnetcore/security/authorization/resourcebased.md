---
title: Oparte na zasobach autoryzacji w ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zaimplementuj autoryzacji na podstawie zasobów w aplikacji platformy ASP.NET Core, gdy atrybut autoryzacji nie będą wystarczające.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 3be2d9b9aef18763fbdba78e044dd6b68ddcef0c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Oparte na zasobach autoryzacji w ASP.NET Core

Strategia autoryzacji zależy od zasobu, do której uzyskuje dostęp. Należy wziąć pod uwagę dokumentu, który ma właściwość autora. Tylko autor może zaktualizować dokumentu. W rezultacie dokument musi zostać pobrana z magazynu danych, zanim nastąpi ocena autoryzacji.

Atrybut oceny występuje przed wiązania z danymi i przed realizacją strony obsługi lub akcji, który jest ładowany dokument. Z tego względu deklaratywne autoryzacji z `[Authorize]` atrybutu nie będą wystarczające. Zamiast tego można wywołać metody autoryzacji niestandardowej&mdash;stylu znany jako imperatywnych autoryzacji.

Użyj [przykładowe aplikacje](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) aby poznać funkcje opisane w tym temacie.

[Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji](xref:security/authorization/secure-data) zawiera przykładową aplikację korzystającą autoryzacji na podstawie zasobów.

## <a name="use-imperative-authorization"></a>Użycie imperatywnych autoryzacji

Autoryzacja jest zaimplementowany jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) usługi i jest zarejestrowane w kolekcji usługi w ramach `Startup` klasy. Usługa ma zostać udostępnione za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) do obsługi strony lub akcji.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` zawiera dwa `AuthorizeAsync` przeciążenia metody: przyjmuje jeden zasób i nazwę zasady i innych akceptowanie zasobu oraz listę wymagań dotyczących oceny.

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

W poniższym przykładzie zasobów być zabezpieczone jest ładowany do niestandardowego `Document` obiektu. `AuthorizeAsync` Przeciążenia jest wywoływane w celu ustalenia, czy bieżący użytkownik może edytować dokument podana. Niestandardowe zasady autoryzacji "EditPolicy" jest brana pod uwagę w decyzji. Zobacz [autoryzacji na podstawie zasad niestandardowych](xref:security/authorization/policies) Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji.

> [!NOTE]
> Poniższy kod przykładów założono uwierzytelniania zostało uruchomione i zestaw `User` właściwości.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Pisanie programu obsługi opartych na zasobach

Pisanie programu obsługi dla autoryzacji na podstawie zasobu nie jest znacznie różni się od [pisanie programu obsługi wymagań zwykły](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Tworzenie klasy niestandardowej wymaganie i zaimplementować klasę programu obsługi wymagań. Klasa obsługi określa wymaganie i typu zasobu. Na przykład program obsługi przy użyciu `SameAuthorRequirement` wymaganie i `Document` zasobów wygląda następująco:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Zarejestruj wymagania oraz procedury obsługi w `Startup.ConfigureServices` metody:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Wymagań operacyjnych

Jeśli wprowadzasz decyzji, w oparciu o wyniki operacji CRUD (tworzenia, odczytu, aktualizacji, usuwania), użyj [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Klasa pomocy. Ta klasa umożliwia pisanie pojedynczego obsługi zamiast poszczególnych klas dla każdego typu operacji. Aby go użyć, należy podać niektóre nazwy operacji:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Program obsługi jest implementowane, za pomocą `OperationAuthorizationRequirement` wymaganie i `Document` zasobów:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Powyższej procedury obsługi weryfikuje operację, używając zasobu, tożsamość użytkownika i wymaganie `Name` właściwości.

Aby wywołać obsługi operacyjne zasobów, określ podczas wywoływania operacji `AuthorizeAsync` w obsługi strony lub akcji. Poniższy przykład określa, czy uwierzytelniony użytkownik będzie mógł wyświetlić dokument podana.

> [!NOTE]
> Poniższy kod przykładów założono uwierzytelniania zostało uruchomione i zestaw `User` właściwości.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Jeśli autoryzacji zakończy się powodzeniem, zwracany jest strona do wyświetlania dokumentu. Jeśli autoryzacji kończy się niepowodzeniem, ale użytkownik jest uwierzytelniony, zwracając `ForbidResult` informuje oprogramowanie pośredniczące uwierzytelniania, która Autoryzacja nie powiodła się. A `ChallengeResult` jest zwracany, gdy uwierzytelnianie odbywa się. Dla klientów w przeglądarkach interaktywne można spróbować przekieruje użytkownika do strony logowania.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Jeśli autoryzacji zakończy się pomyślnie, zostanie zwrócony widok dokumentu. W przypadku niepowodzenia autoryzacji zwracanie `ChallengeResult` informuje o wszystkich programów pośredniczących uwierzytelnienia Autoryzacja nie powiodła się, czy oprogramowanie pośredniczące może zająć właściwą odpowiedź. Właściwą odpowiedź może zwracać kod stanu 401 lub 403. Dla klientów w przeglądarkach interaktywne to może oznaczać przekierowanie użytkownika do strony logowania.

---
