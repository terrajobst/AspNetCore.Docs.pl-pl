---
title: ASP.NET Core Blazor iniekcji zależności
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: a39d913636afc55ac9d831de923ba7ae8db1216b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963077"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a>ASP.NET Core Blazor iniekcji zależności

Autor [Rainer Stropek](https://www.timecockpit.com)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection). Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu. Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.

DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji. Może to być przydatne w aplikacjach Blazor, aby:

* Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako *pojedyncze usługi.*
* Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań. Rozważmy na przykład interfejs `IDataAccess` w celu uzyskania dostępu do danych w aplikacji. Interfejs jest implementowany przez konkretną klasę `DataAccess` i zarejestrowana jako usługa w kontenerze usługi aplikacji. Gdy składnik używa elementu DI do otrzymania implementacji `IDataAccess`, składnik nie jest połączony z konkretnym typem. Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.

## <a name="default-services"></a>Usługi domyślne

Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.

| Usługa | Okres istnienia | Opis |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Pojedynczego | Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI. Należy zauważyć, że to wystąpienie `HttpClient` używa przeglądarki do obsługi ruchu HTTP w tle. [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) jest automatycznie ustawiany na podstawowy prefiks identyfikatora URI aplikacji. Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>. |
| `IJSRuntime` | Pojedynczego | Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>. |
| `NavigationManager` | Pojedynczego | Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji. Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers). |

Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli. W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.

## <a name="add-services-to-an-app"></a>Dodawanie usług do aplikacji

Po utworzeniu nowej aplikacji należy przejrzeć metodę `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Metoda `ConfigureServices` jest przenoszona do <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, który jest listą obiektów deskryptorów usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług. Poniższy przykład ilustruje koncepcję z interfejsem `IDataAccess` i jego konkretną implementacją `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.

| Okres istnienia | Opis |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor aplikacje webassembly nie mają obecnie koncepcji DI Scopes. usługi zarejestrowane `Scoped`zachowują się jak usługi `Singleton` Services. Jednak model hostingu serwera Blazor obsługuje okres istnienia `Scoped`. W aplikacjach Blazor Server Rejestracja usługi w zakresie została objęta zakresem *połączenia*. Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI tworzy *pojedyncze wystąpienie* usługi. Wszystkie składniki wymagające usługi `Singleton` odbierają wystąpienie tej samej usługi. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Za każdym razem, gdy składnik uzyskuje wystąpienie usługi `Transient` z kontenera usług, otrzymuje *nowe wystąpienie* usługi. |

System DI jest oparty na systemie DI w ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Żądanie usługi w składniku

Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą dyrektywy [wstrzykiwania Razor\@](xref:mvc/views/razor#inject) . `@inject` ma dwa parametry:

* Wpisz &ndash; typ usługi do dodania.
* Właściwość &ndash; nazwa właściwości otrzymującej wstrzykiwaną usługę App Service. Właściwość nie wymaga ręcznego tworzenia. Kompilator tworzy właściwość.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.

Użyj wielu instrukcji `@inject`, aby wstrzyknąć różne usługi.

Poniższy przykład pokazuje, jak używać `@inject`. Usługa implementująca `Services.IDataAccess` jest wstrzykiwana do właściwości składnika `DataRepository`. Zwróć uwagę, jak kod używa wyłącznie abstrakcji `IDataAccess`:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Wewnętrznie wygenerowana Właściwość (`DataRepository`) ma atrybut `InjectAttribute`. Zazwyczaj ten atrybut nie jest używany bezpośrednio. Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

W składnikach pochodnych klasy bazowej dyrektywa `@inject` nie jest wymagana. `InjectAttribute` klasy podstawowej jest wystarczająca:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Korzystanie z usług DI w

Złożone usługi mogą wymagać dodatkowych usług. W poprzednim przykładzie `DataAccess` może wymagać domyślnej usługi `HttpClient`. `@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach. Zamiast tego należy użyć *iniekcji konstruktora* . Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi. Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

Wymagania wstępne dotyczące iniekcji konstruktora:

* Jeden Konstruktor musi istnieć, którego argumenty mogą być zrealizowane przez DI. Dodatkowe parametry, które nie są objęte przez DI, są dozwolone, jeśli określają wartości domyślne.
* Odpowiedni Konstruktor musi być *publiczny*.
* Musi istnieć jeden odpowiedni Konstruktor. W przypadku niejednoznaczności, polecenie DI zgłasza wyjątek.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Klasy składników podstawowych narzędzi do zarządzania DI zakresem

W przypadku aplikacji ASP.NET Core usługi o określonym zakresie są zwykle objęte zakresem bieżącego żądania. Po zakończeniu żądania wszystkie usługi w zakresie lub przejściowym są usuwane przez system DI. W Blazor aplikacji serwerowych zakres żądań jest stosowany przez czas trwania połączenia klienta, co może spowodować, że usługi przejściowe i objęte zakresem będą dużo dłużej niż oczekiwano.

Aby ograniczyć zakres usług do okresu istnienia składnika, można użyć klas podstawowych `OwningComponentBase` i `OwningComponentBase<TService>`. Te klasy bazowe uwidaczniają Właściwość `ScopedServices` typu `IServiceProvider`, które rozwiązują usługi objęte zakresem czasu istnienia składnika. Aby utworzyć składnik, który dziedziczy z klasy podstawowej w Razor, użyj dyrektywy `@inherits`.

```cshtml
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> Usługi wprowadzone do składnika przy użyciu `@inject` lub `InjectAttribute` nie są tworzone w zakresie składnika i są powiązane z zakresem żądania.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
