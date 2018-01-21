---
title: "Autoryzacji opartej na oświadczeniach"
author: rick-anderson
description: "Tym dokumencie opisano sposób dodawania oświadczeń sprawdzeń autoryzacji w aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: dd8f42684f9e58b9329602aa9b70d2c0ab950892
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="claims-based-authorization"></a>Autoryzacji opartej na oświadczeniach

<a name="security-authorization-claims-based"></a>

Po utworzeniu tożsamości może być przypisana co najmniej jednego oświadczenia wystawione przez zaufany. Oświadczenie para reprezentujący jakie podmiot jest wartość nazwy, nie jakie tematu może. Na przykład masz prawa jazdy, wystawiony przez urząd lokalny licencji jazdy. W sterowniku licencji znajdują się datę urodzenia. W takim przypadku nazwa oświadczenia jest `DateOfBirth`, wartość oświadczenia będą datę urodzenia, na przykład `8th June 1970` i Wystawca byłaby pobudzenie urzędu licencji. Autoryzacji oświadczeń, w najprostszym sprawdza wartość oświadczenia i zezwala na dostęp do zasobu na podstawie tej wartości. Na przykład, jeśli chcesz uzyskać dostęp do klub nocy proces autoryzacji. może to być:

Drzwi urzędnika oceni wartość daty urodzenia oświadczeń i określa, czy ufają wystawcy (pobudzenie urzędu licencji) zanim zostanie przyznany dostęp.

Tożsamość może zawierać wielu oświadczeń z wieloma wartościami i może zawierać wielu oświadczenia tego samego typu.

## <a name="adding-claims-checks"></a>Dodawanie oświadczenia kontroli

Oświadczenia deklaratywne sprawdzeń autoryzacji na podstawie — dewelopera umieszcza je ich kodem, kontrolera lub akcji w obrębie kontrolera, określając oświadczenia, które bieżący użytkownik musi posiadać i opcjonalnie wartość oświadczenia musi posiadać dostępu żądany zasób. Oświadczeń wymagania są oparte na zasadach, deweloper musi kompilacji i zarejestruj zasad, przedstawiając wymagania oświadczeń.

Najprostszy typ oświadczenia zasad szuka obecności oświadczenia, nie sprawdzając wartość.

Należy najpierw skompilować i zarejestrować zasad. Ma to miejsce w ramach konfiguracji usługi autoryzacji, które zwykle bierze udział w `ConfigureServices()` w Twojej *Startup.cs* pliku.

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

W takim przypadku `EmployeeOnly` zasad sprawdza obecność `EmployeeNumber` oświadczeń w bieżącej tożsamości.

Następnie Zastosuj zasad przy użyciu opcji `Policy` właściwość `AuthorizeAttribute` atrybutu, aby określić nazwę zasady;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Atrybut można stosować do całego kontrolera, w tym wystąpieniu tylko tożsamości pasujących zasady będą miały dostęp do dowolnej akcji na kontrolerze.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Jeśli masz kontroler, która jest chroniona przez `AuthorizeAttribute` atrybutu, ale chcesz zezwolić na dostęp anonimowy do określonej akcji, należy zastosować `AllowAnonymousAttribute` atrybutu.

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

Większość oświadczenia mają wartość. Można określić listę dozwolonych wartości podczas tworzenia zasad. Poniższy przykład tylko powiedzie się dla pracowników, których liczba pracowników była 1, 2, 3, 4 lub 5.

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

## <a name="multiple-policy-evaluation"></a>Wiele ocena zasad

Jeśli wiele zasad można zastosować do kontrolera lub akcji, wszystkie zasady muszą upłynąć, zanim zostanie przyznany dostęp. Na przykład:

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

W powyższym przykładzie wszystkie tożsamości, która spełnia `EmployeeOnly` zasady mogą uzyskiwać dostęp do `Payslip` akcji, jak te zasady są wymuszane na kontrolerze. Jednak aby można było wywołać `UpdateSalary` akcji, które należy spełnić tożsamość *zarówno* `EmployeeOnly` zasad i `HumanResources` zasad.

Jeśli chcesz bardziej złożone zasady, takie jak pobieranie daty urodzenia oświadczenia, obliczanie wieku z niego, a następnie sprawdzanie wiek to 21 lub starsze, a następnie należy napisać [zasady niestandardowe programy obsługi](policies.md).
