---
title: Wstrzykiwanie zależności składników razor
author: guardrex
description: Zobacz, jak Blazor i składniki Razor aplikacje mogą używać usług wbudowanych Pozwól, które są wstrzykiwane do składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 04be8371b6e746929448b24487b07c08cf03cb78
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668188"
---
# <a name="razor-components-dependency-injection"></a>Wstrzykiwanie zależności składników razor

Przez [Rainer Stropek](https://www.timecockpit.com)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

[Wstrzykiwanie zależności (DI)](/aspnet/core/fundamentals/dependency-injection) jest wbudowana. Aplikacje mogą używać usług wbudowanych Pozwól, które są wstrzykiwane do składników. Aplikacje można zdefiniować niestandardowych usług i udostępnić je za pośrednictwem DI.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

DI to technika do uzyskiwania dostępu do usługi skonfigurowane w centralnej lokalizacji. Może to być przydatne do:

* Udostępnić pojedyncze wystąpienie klasy usługi między wiele składników (nazywane *pojedyncze* usługi).
* Odłączanie składników z określonej usługi konkretnych klas i odwoływać się tylko elementy abstrakcji. Na przykład interfejs `IDataAccess` został zaimplementowany przez klasę konkretnych `DataAccess`. Gdy składnik używa DI do odbierania `IDataAccess` implementacji, składnik nie jest ściśle do konkretnego typu. Wdrożenia można wymieniać, być może do implementację testową w testach jednostkowych.

DI system jest odpowiedzialne za dostarczanie wystąpienia usług dla składników. DI rozwiązuje również rekursywnie zależności, aby uwierzytelnienia usługi może zależeć od dalszego usług. DI jest konfigurowany podczas uruchamiania aplikacji. Przykład przedstawiono w dalszej części tego tematu.

## <a name="add-services-to-di"></a>Dodawanie usług do DI

Po utworzeniu nowej aplikacji, należy zbadać `Startup.ConfigureServices` metody:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices` Metody jest przekazywana [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), który znajduje się lista obiektów deskryptora usługi ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). Usługi są dodawane, zapewniając deskryptory usługi do kolekcji usługi. W poniższym przykładzie kodu pokazano pojęcia:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Usługi mogą być skonfigurowane przy użyciu następujących okresów istnienia:

| Metoda      | Opis |
| ----------- | ----------- |
| [pojedyncze](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | Tworzy DI *pojedyncze wystąpienie* usługi. Wszystkie składniki, które wymagają zainstalowania tej usługi odbierać odwołanie do tego wystąpienia. |
| [Przejściowe](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | Zawsze, gdy składnik wymaga tej usługi, otrzymuje *nowe wystąpienie* usługi. |
| [O określonym zakresie](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | Blazor po stronie klienta nie ma obecnie koncepcji DI zakresów. `Scoped` zachowuje się jak `Singleton`. Jednak obsługuje składniki programu ASP.NET Core Razor `Scoped` okresu istnienia. W składniku Razor rejestracji usługi o określonym zakresie jest ograniczony do połączenia. Z tego powodu przy użyciu usługi o określonym zakresie była preferowana dla usług, które powinien być ograniczony z bieżącym użytkownikiem (nawet, jeśli bieżącym celem jest do uruchomienia po stronie klienta w przeglądarce). |

DI system jest oparty na systemie DI, w programie ASP.NET Core. Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności w programie ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Usług domyślnych

Usługi domyślne są automatycznie dodawane do kolekcji usługi aplikacji. W poniższej tabeli przedstawiono niektóre świadczonych usług przydatna.

| Metoda       | Opis |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | Zawiera metody służące do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego z użyciem identyfikatora URI (pojedyncze wystąpienie). Należy pamiętać, że to wystąpienie `HttpClient` korzysta z przeglądarki do obsługi ruchu HTTP w tle. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) automatycznie zostaje ustawiony poziom podstawowy prefiks identyfikatora URI aplikacji. `HttpClient` jest dostępna wyłącznie dla aplikacji Blazor po stronie klienta. |
| `IJSRuntime` | Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, do której może być wysyłane wywołania. Aby uzyskać więcej informacji, zobacz <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Pomocnicy do pracy ze stanem identyfikatory URI i nawigacji (pojedyncze wystąpienie). `IUriHelper` zapewnia obie aplikacje Blazor i ASP.NET Core Razor składniki po stronie klienta. |

Należy zauważyć, że można użyć dostawcy niestandardowych usług zamiast domyślnego dostawcę usługi, który jest dodawany przez szablon domyślny. Niestandardowe usługodawcy automatycznie nie dostarcza usługi domyślne wymienione w tabeli. Tych usług należy jawnie dodać do nowego dostawcę usługi.

## <a name="request-a-service-in-a-component"></a>Żądanie usługi w składniku

Gdy usługi są dodawane do kolekcji usługi, może zostać wprowadzona do składników programu szablony Razor przy użyciu `@inject` dyrektywy Razor. `@inject` ma dwa parametry:

* Nazwa typu: Typ usługi do dodania.
* Nazwa właściwości: Nazwa właściwości odbieranie usługi wprowadzonego kodu aplikacji. Należy pamiętać, że właściwość nie wymaga ręcznego tworzenia. Kompilator tworzy właściwość.

Wiele `@inject` instrukcji można używać do dodania do różnych usług.

Poniższy przykład pokazuje, jak używać `@inject`. Wdrażanie usługi `Services.IDataAccess` są wstrzykiwane do właściwości składnika `DataRepository`. Należy zauważyć, jak kod jest wyłącznie przy użyciu `IDataAccess` abstrakcji:

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

Wewnętrznie, wygenerowana właściwość (`DataRepository`) zostanie nadany `InjectAttribute` atrybutu. Zazwyczaj ten atrybut nie jest używany bezpośrednio. Jeśli klasa bazowa jest wymagana dla składników i właściwości wprowadzonego są również wymagane dla klasy bazowej, `InjectAttribute` można ręcznie dodać:

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Składniki pochodzące z klasy bazowej `@inject` dyrektywy nie jest wymagane. `InjectAttribute` Klasy bazowej jest wystarczająca:

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Wstrzykiwanie zależności w usługach

Złożone usługi może wymagać dodatkowych usług. W poprzednim przykładem `DataAccess` mogą wymagać `HttpClient` domyślnej usługi. `@inject` lub `InjectAttribute` nie mogą być używane w usługach. *Iniekcji konstruktora* należy użyć zamiast tego. Wymagane usługi są dodawane, dodając parametry do konstruktora tej usługi. Gdy wstrzykiwanie zależności tworzy usługę, rozpoznaje usług wymaga w Konstruktorze i udostępnia je w związku z tym.

W poniższym przykładzie kodu pokazano pojęcia:

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

Należy zwrócić uwagę następujących wymagań wstępnych dotyczących iniekcji konstruktora:

* Musi to być jeden konstruktor, w której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności. Należy pamiętać, że dozwolone są dodatkowe parametry, które nie są objęte DI, jeśli określono dla nich wartości domyślne.
* Zastosowanie Konstruktor musi być *publicznych*.
* Musi istnieć tylko jeden konstruktor dotyczy. W przypadku niejednoznaczności DI zgłasza wyjątek.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wstrzykiwanie zależności w programie ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
