---
title: Autoryzacja oparta na oświadczeniach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać sprawdzanie oświadczenia dotyczące autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086151"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autoryzacja oparta na oświadczeniach w programie ASP.NET Core

<a name="security-authorization-claims-based"></a>

Po utworzeniu tożsamości może być przypisany co najmniej jeden oświadczeń wystawione przez zaufany. Oświadczenie to pary nazwa-wartość reprezentującą jakie podmiot, nie jakie podmiotu można zrobić. Na przykład masz prawa jazdy, wystawiony przez urząd lokalny opracowuje licencji. Twoje prawa jazdy znajdują się informacje o dacie urodzenia. W tym przypadku wyniesie Nazwa oświadczenia `DateOfBirth`, wartość oświadczenia będą informacje o dacie urodzenia, na przykład `8th June 1970` i wystawca, którego będzie kierowania organu licencji. Autoryzacji oświadczeń, w najprostszym sprawdza wartość oświadczenia i umożliwia dostęp do zasobu na podstawie tej wartości. Na przykład jeśli chcesz, aby uzyskać dostęp do club nocy proces autoryzacji mogą być:

Ze specjalistą ds. zabezpieczeń drzwi biblioteki używane do oceny wartość Twojej daty urodzenia oświadczeń i czy ufają one wystawcy (opracowuje urzędu licencji) przed udzieleniem im dostępu.

Tożsamość może zawierać wielu oświadczeń z wieloma wartościami i może zawierać wielu oświadczeń tego samego typu.

## <a name="adding-claims-checks"></a>Dodawanie oświadczenia kontroli

Oświadczenia deklaratywne sprawdzanie autoryzacji na podstawie — Deweloper umieszcza je w ramach ich kodować, kontrolera lub akcji w kontrolerze, określając oświadczenia, które musi posiadać bieżącego użytkownika i opcjonalnie wartość oświadczenia musi posiadać dostęp do żądany zasób. Oświadczenia, że wymagania są oparte na zasadach, deweloper musi kompilacji i zarejestrowania zasadę wyrażania wymagań oświadczeń.

Najprostszy typ oświadczenia zasad szuka obecności oświadczenia i nie sprawdza wartość.

Najpierw należy do kompilacji i zarejestrowania zasad. Ma to miejsce w ramach konfiguracji usługi autoryzacji, które normalnie bierze udział w `ConfigureServices()` w swojej *Startup.cs* pliku.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

W tym przypadku `EmployeeOnly` zasad sprawdza obecność `EmployeeNumber` oświadczenia na bieżącej tożsamości.

Następnie zastosować zasady za pomocą `Policy` właściwość `AuthorizeAttribute` atrybutu, aby określić nazwę zasad;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Atrybut można stosować do całego kontrolera, w tym wystąpieniu tylko do tożsamości dopasowania zasad będą miały dostęp do dowolnej akcji w kontrolerze.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Jeśli masz kontrolera, który jest chroniony przez `AuthorizeAttribute` atrybutu, ale chcesz zezwolić na dostęp anonimowy do określonej akcji, które można zastosować `AllowAnonymousAttribute` atrybutu.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

Większość oświadczenia pochodzą z wartością. Podczas tworzenia zasad, można określić listę dozwolonych wartości. Poniższy przykład tylko powiedzie się dla pracowników, których liczba pracowników wyniosła 1, 2, 3, 4 lub 5.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

### <a name="add-a-generic-claim-check"></a>Dodaj sprawdzanie oświadczenia ogólny

Jeśli wartość oświadczenia nie jest pojedynczą wartość lub transformacji jest wymagany, użyj [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Aby uzyskać więcej informacji, zobacz [spełniają zasady za pomocą func](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Wielokrotne obliczenie zasad

Jeśli wiele zasad są stosowane do kontrolera lub akcji, wszystkie zasady muszą minąć, zanim można udzielić dostępu. Na przykład:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

W powyższym przykładzie dowolnej tożsamości, która spełnia `EmployeeOnly` zasady mogą uzyskiwać dostęp do `Payslip` akcji, jak te zasady są wymuszane na kontrolerze. Jednak aby można było wywołać `UpdateSalary` akcji, które należy spełnić tożsamości *zarówno* `EmployeeOnly` zasad i `HumanResources` zasad.

Jeśli chcesz bardziej złożone zasady, takie jak biorąc datę urodzenia oświadczenia, obliczenia wieku z niego, a następnie sprawdzając wiek to 21 lub starszy, a następnie należy napisać [zasady niestandardowe programy obsługi](xref:security/authorization/policies).
