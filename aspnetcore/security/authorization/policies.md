---
title: Autoryzacja niestandardowa oparte na zasadach w ASP.NET Core
author: rick-anderson
description: "Informacje o sposobie tworzenia i używania programów obsługi zasad autoryzacji niestandardowej dla wymuszania wymagań autoryzacji w aplikacji platformy ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: c249985a6266483d47f447ac4a232546ed2b2708
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="custom-policy-based-authorization"></a>Niestandardowe autoryzacji opartych na zasadach

Poniżej obejmuje [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) użyć wymaganie, obsługi wymagania i wstępnie skonfigurowanych zasad. Te bloki konstrukcyjne obsługi wyrażenie ocen autoryzacji w kodzie. Wynik jest strukturą bardziej rozbudowane, wielokrotnego użytku, testować autoryzacji.

Zasady autoryzacji składa się z co najmniej jednego wymagania. Jest on zarejestrowany w ramach konfiguracji usługi autoryzacji, w `ConfigureServices` metody `Startup` klasy:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

W powyższym przykładzie tworzona jest zasada "AtLeast21". Ma on jeden wymaganie, że o minimalnym wieku, które są udostępniane jako parametr wymogiem.

Zasady są stosowane przy użyciu `[Authorize]` atrybutu o nazwie zasad. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Wymagania

Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika. W zasadach "AtLeast21" to wymaganie jest pojedynczy parametr&mdash;minimalnym wieku. Implementuje wymagane `IAuthorizationRequirement`, która jest interfejsem znacznika puste. Sparametryzowane minimalny wymagany wiek można realizowane w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Wymagania nie musi być danych lub właściwości.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Programy obsługi autoryzacji

Program obsługi autoryzacji jest odpowiedzialny za obliczania właściwości zapotrzebowania. Program obsługi autoryzacji ocenia wymagania względem podanego `AuthorizationHandlerContext` ustalenie, czy dostęp jest dozwolony. Wymóg może mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers). Programy obsługi dziedziczą `AuthorizationHandler<T>`, gdzie `T` jest wymagane do obsługi.

<a name="security-authorization-handler-example"></a>

Program obsługi minimalnego wieku może wyglądać następująco:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Poprzedni kod określa, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez znanego i zaufanego wystawcy. Autoryzacji nie może wystąpić w przypadku braku oświadczenia, w którym to przypadku jest zwracany o zakończeniu zadania. Gdy występuje oświadczenia są obliczane wieku użytkownika. Jeśli użytkownika spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacji uważa się pomyślnie. Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako parametr.

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

Niezależnie od wywołana wewnątrz obsługi sieci obsługi wszystkie wymagane będzie wywoływany, gdy zasady wymaga to wymaganie. Dzięki temu wymagania dotyczące efekty uboczne, takich jak rejestrowanie, które będą zawsze miały miejsce nawet wtedy, gdy `context.Fail()` została wywołana w innego programu obsługi.

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
