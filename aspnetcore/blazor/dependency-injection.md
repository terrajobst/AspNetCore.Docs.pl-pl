---
title: Wstrzykiwanie zależności Blazor
author: guardrex
description: Zobacz, jak aplikacje Blazor można wstawić usług do składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 9e19596dec5582e11212d95a9fea72862baa2046
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983005"
---
# <a name="blazor-dependency-injection"></a>Wstrzykiwanie zależności Blazor

Przez [Rainer Stropek](https://www.timecockpit.com)

Obsługuje Blazor [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Aplikacje mogą używać wbudowanych usług przez iniekcję je na składniki. Aplikacje można zdefiniować i zarejestrować niestandardowych usług i udostępnić je w całej aplikacji za pośrednictwem DI.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

DI to technika do uzyskiwania dostępu do usługi skonfigurowane w centralnej lokalizacji. Może to być przydatne w aplikacjach Blazor:

* Udostępnić pojedyncze wystąpienie klasy usługi w wielu składników, znane jako *pojedyncze* usługi.
* Oddzielenie składników od klas konkretnych usług, za pomocą abstrakcje odwołania. Na przykład, należy wziąć pod uwagę interfejs `IDataAccess` do uzyskiwania dostępu do danych w aplikacji. Interfejs jest implementowany przez konkretny `DataAccess` klasy i zarejestrowany jako usługa w kontenerze usługi aplikacji. Gdy składnik używa DI do odbierania `IDataAccess` implementacji, składnik nie jest ściśle do konkretnego typu. Wdrożenia można wymieniać, być może do implementację testową w testach jednostkowych.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="add-services-to-di"></a>Dodawanie usług do DI

Po utworzeniu nowej aplikacji, należy zbadać `Startup.ConfigureServices` metody:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices` Metody jest przekazywana <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, który znajduje się lista obiektów deskryptora usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Usługi są dodawane, zapewniając deskryptory usługi do kolekcji usługi. W poniższym przykładzie zademonstrowano pojęcia z `IDataAccess` interfejsu i jego konkretną implementację `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Usługi mogą być skonfigurowane przy użyciu okresy istnienia pokazano w poniższej tabeli.

| Okres istnienia | Opis |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | Tworzy DI *pojedyncze wystąpienie* usługi. Wszystkie składniki wymagające `Singleton` usługa otrzymywać wystąpienia tej samej usługi. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Zawsze, gdy składnik uzyskuje wystąpienie `Transient` usługi z kontenera usługi odbiera *nowe wystąpienie* usługi. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor po stronie klienta nie ma obecnie koncepcji DI zakresów. `Scoped` zachowuje się jak `Singleton`. Jednak model hostowania po stronie serwera obsługuje `Scoped` okresu istnienia. W składniku Razor rejestracji usługi o określonym zakresie jest ograniczony do połączenia. Z tego powodu przy użyciu usługi o określonym zakresie była preferowana dla usług, które powinien być ograniczony do bieżącego użytkownika, nawet jeśli bieżącym celem jest do uruchomienia po stronie klienta w przeglądarce. |

DI system jest oparty na systemie DI, w programie ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="default-services"></a>Usług domyślnych

Usługi domyślne są automatycznie dodawane do kolekcji usługi aplikacji.

| Usługa | Opis |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Zawiera metody służące do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego z użyciem identyfikatora URI (pojedyncze wystąpienie). Należy pamiętać, że to wystąpienie `HttpClient` korzysta z przeglądarki do obsługi ruchu HTTP w tle. [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) automatycznie zostaje ustawiony poziom podstawowy prefiks identyfikatora URI aplikacji. `HttpClient` jest dostępna wyłącznie dla aplikacji Blazor po stronie klienta. |
| `IJSRuntime` | Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, do której może być wysyłane wywołania. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>. |
| `IUriHelper` | Zawiera pomocnicy do pracy ze stanem identyfikatory URI i nawigacji (pojedyncze wystąpienie). |

Istnieje możliwość używania dostawcy niestandardowego usługi zamiast domyślnego dostawcę usługi dodane przez szablon domyślny. Niestandardowe usługodawcy automatycznie nie dostarcza usługi domyślne wymienione w tabeli. Jeśli używasz niestandardowego usługodawcy i wymaga żadnej usług przedstawionych w tabeli, Dodaj wymagane usługi, do nowego dostawcę usługi.

## <a name="request-a-service-in-a-component"></a>Żądanie usługi w składniku

Po dodaniu usługi do kolekcji usługi wstrzyknąć usług do składników programu szablony Razor przy użyciu [ \@wstrzyknąć](xref:mvc/views/razor#section-4) dyrektywy Razor. `@inject` ma dwa parametry:

* Nazwa typu: Typ usługi do dodania.
* Nazwa właściwości: Nazwa właściwości odbieranie usługi wprowadzonego kodu aplikacji. Należy pamiętać, że właściwość nie wymaga ręcznego tworzenia. Kompilator tworzy właściwość.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.

Używanych jest wiele `@inject` instrukcje, aby wstawić różnych usług.

Poniższy przykład pokazuje, jak używać `@inject`. Wdrażanie usługi `Services.IDataAccess` są wstrzykiwane do właściwości składnika `DataRepository`. Należy zauważyć, jak kod jest wyłącznie przy użyciu `IDataAccess` abstrakcji:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Wewnętrznie, wygenerowana właściwość (`DataRepository`) zostanie nadany `InjectAttribute` atrybutu. Zazwyczaj ten atrybut nie jest używany bezpośrednio. Jeśli klasa bazowa jest wymagana dla składników i właściwości wprowadzonego są również wymagane dla klasy bazowej, `InjectAttribute` można ręcznie dodać:

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Składniki pochodzące z klasy bazowej `@inject` dyrektywy nie jest wymagane. `InjectAttribute` Klasy bazowej jest wystarczająca:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>Wstrzykiwanie zależności w usługach

Złożone usługi może wymagać dodatkowych usług. W poprzednim przykładem `DataAccess` mogą wymagać `HttpClient` domyślnej usługi. `@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach. *Iniekcji konstruktora* należy użyć zamiast tego. Wymagane usługi są dodawane, dodając parametry do konstruktora tej usługi. Gdy wstrzykiwanie zależności tworzy usługę, rozpoznaje usług wymaga w Konstruktorze i udostępnia je w związku z tym.

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

* Musi to być jeden konstruktor, w której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności. Pamiętaj, że dodatkowe parametry, które nie są objęte DI są dozwolone, jeśli są określone wartości domyślne.
* Zastosowanie Konstruktor musi być *publicznych*.
* Musi istnieć tylko jeden konstruktor dotyczy. W przypadku niejednoznaczności DI zgłasza wyjątek.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
