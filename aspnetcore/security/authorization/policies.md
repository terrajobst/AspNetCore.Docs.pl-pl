---
title: Na podstawie zasad autoryzacji w ASP.NET Core
author: rick-anderson
description: Informacje o sposobie tworzenia i używania programów obsługi zasady autoryzacji dotyczące wymuszania wymagań autoryzacji w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072864"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Na podstawie zasad autoryzacji w ASP.NET Core

Poniżej obejmuje [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) użyć wymaganie, obsługi wymagania i wstępnie skonfigurowanych zasad. Te bloki konstrukcyjne obsługi wyrażenie ocen autoryzacji w kodzie. Wynik jest strukturą bardziej rozbudowane, wielokrotnego użytku, testować autoryzacji.

Zasady autoryzacji składa się z co najmniej jednego wymagania. Jest on zarejestrowany w ramach konfiguracji usługi autoryzacji, w `Startup.ConfigureServices` metody:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

W powyższym przykładzie tworzona jest zasada "AtLeast21". Składa się z jednego wymaganie&mdash;z minimalnym wieku jest ona podawana jako parametr do wymagań.

Zasady są stosowane przy użyciu `[Authorize]` atrybutu o nazwie zasad. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Wymagania

Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika. W zasadach "AtLeast21" to wymaganie jest pojedynczy parametr&mdash;minimalnym wieku. Implementuje wymagane [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), która jest interfejsem znacznika puste. Sparametryzowane minimalny wymagany wiek można realizowane w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Wymagania nie musi być danych lub właściwości.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Programy obsługi autoryzacji

Program obsługi autoryzacji jest odpowiedzialny za obliczania właściwości zapotrzebowania. Program obsługi autoryzacji ocenia wymagania względem podanego [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) ustalenie, czy dostęp jest dozwolony.

Wymóg może mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers). Program obsługi może dziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymagane do obsługi. Alternatywnie program obsługi może wdrożyć [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) obsłużyć więcej niż jeden typ wymaganie.

### <a name="use-a-handler-for-one-requirement"></a>Użyj programu obsługi dla jednego wymagania

<a name="security-authorization-handler-example"></a>

Poniżej przedstawiono przykład relacją, w którym program obsługi minimalnego wieku korzysta z jednego wymaganie:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Poprzedni kod określa, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez znanego i zaufanego wystawcy. Autoryzacji nie może wystąpić w przypadku braku oświadczenia, w którym to przypadku jest zwracany o zakończeniu zadania. Gdy występuje oświadczenia są obliczane wieku użytkownika. Jeśli użytkownika spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacji uważa się pomyślnie. Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako jedyny parametr.

### <a name="use-a-handler-for-multiple-requirements"></a>Użyj wiele wymagań dotyczących obsługi

Poniżej przedstawiono przykład relacji jeden do wielu, w którym program obsługi uprawnienia wykorzystuje trzy wymagania:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Poprzedni kod przechodzi przez [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;nie zawiera wymagania właściwość oznaczona jako powiodło się. Jeśli użytkownik ma uprawnienie do odczytu, użytkownik musi być właścicielem lub sponsor, dostępu do żądanego zasobu. Jeśli użytkownik ma Edytuj lub usuń uprawnienie, musi być właścicielem dostępu do żądanego zasobu. Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako jedyny parametr.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Rejestracja programu obsługi

Programy obsługi są zarejestrowane w kolekcji usługi podczas konfigurowania. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Każdy program obsługi jest dodawany do kolekcji usług przez wywoływanie `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Co powinna zwracać obsługi?

Należy pamiętać, że `Handle` metody w [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości. Jaki jest stan powodzenia lub niepowodzenia wskazanych?

* Program obsługi oznacza Powodzenie przez wywołanie metody `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, która została pomyślnie zweryfikowana.

* Program obsługi nie musi obsługiwać awarie ogólnie rzecz biorąc, zgodnie z innych programów obsługi na ten sam wymóg może się powieść.

* Aby zagwarantować awarii, nawet w przypadku innych programów obsługi wymagań powiedzie się, należy wywołać `context.Fail`.

Wartość `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) właściwości (dostępne w ASP.NET Core 1.1 lub nowszej) short-circuits wykonywania programów obsługi, gdy `context.Fail` jest wywoływana. `InvokeHandlersAfterFailure` Domyślnie `true`, w którym to przypadku wszystkie wywołania. Dzięki temu wymagania powodować efekty uboczne, takich jak rejestrowanie, które zawsze mają miejsce nawet wtedy, gdy `context.Fail` została wywołana w innego programu obsługi.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Dlaczego może być wielu obsług dla wymagania?

W przypadkach, w którym ma oceny na **lub** podstawę, wdrożenie wielu obsług dla pojedynczego wymagania. Na przykład firma Microsoft ma drzwi, które mogły być otwierane tylko w przypadku kart klucza. Jeśli karta klucza pozostanie w domu, recepcjonista drukuje naklejce tymczasowego i otwiera drzwi. W tym scenariuszu należy wymaganiem pojedynczego *BuildingEntry*, ale wielu obsług, każda z nich badanie pojedynczego wymaganie.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Upewnij się, że oba programy obsługi są [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Jeśli albo obsługi zakończy się powodzeniem, gdy zasada oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.

## <a name="using-a-func-to-fulfill-a-policy"></a>Przy użyciu func do zrealizowania zasady

Mogą wystąpić sytuacje, w których spełnienie jest proste express w kodzie zasady. Można podać `Func<AuthorizationHandlerContext, bool>` przy konfigurowaniu zasad z `RequireAssertion` konstruktora zasad.

Na przykład poprzedniej `BadgeEntryHandler` można ponownie zapisać w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Uzyskiwanie dostępu do kontekstu żądania MVC w obsłudze

`HandleRequirementAsync` Metoda implementuje obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi. Struktury, np. MVC lub Jabbr są także dodać dowolny obiekt, aby `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.

Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości. Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko else zapewniane przez MVC i stron Razor.

Korzystanie z `Resource` właściwość jest tylko dla struktury. Korzystając z informacji w `Resource` właściwości ogranicza zasad autoryzacji do określonej struktury. Należy rzutować `Resource` za pomocą właściwości `as` — słowo kluczowe, a następnie potwierdź rzutowanie ma powiedzie się, aby upewnić się, kod nie awarii z `InvalidCastException` uruchomienia na innych platformach:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
