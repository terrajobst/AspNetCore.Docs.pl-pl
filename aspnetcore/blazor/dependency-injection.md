---
title: Wstrzykiwanie zależności programu ASP.NET Core Blazor
author: guardrex
description: Zobacz, jak aplikacje Blazor można wstawić usług do składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 394d99656ba52f6c561007121e63634c7cb84f37
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538473"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>Wstrzykiwanie zależności programu ASP.NET Core Blazor

Przez [Rainer Stropek](https://www.timecockpit.com)

Obsługuje Blazor [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Aplikacje mogą używać wbudowanych usług przez iniekcję je na składniki. Aplikacje można zdefiniować i zarejestrować niestandardowych usług i udostępnić je w całej aplikacji za pośrednictwem DI.

DI to technika do uzyskiwania dostępu do usługi skonfigurowane w centralnej lokalizacji. Może to być przydatne w aplikacjach Blazor:

* Udostępnić pojedyncze wystąpienie klasy usługi w wielu składników, znane jako *pojedyncze* usługi.
* Oddzielenie składników od klas konkretnych usług, za pomocą abstrakcje odwołania. Na przykład, należy wziąć pod uwagę interfejs `IDataAccess` do uzyskiwania dostępu do danych w aplikacji. Interfejs jest implementowany przez konkretny `DataAccess` klasy i zarejestrowany jako usługa w kontenerze usługi aplikacji. Gdy składnik używa DI do odbierania `IDataAccess` implementacji, składnik nie jest ściśle do konkretnego typu. Wdrożenia można wymieniać, być może uzyskać implementację testową w testach jednostkowych.

## <a name="default-services"></a>Usług domyślnych

Usługi domyślne są automatycznie dodawane do kolekcji usługi aplikacji.

| Usługa | Okres istnienia | Opis |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | pojedyncze | Zawiera metody służące do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego z użyciem identyfikatora URI. Należy pamiętać, że to wystąpienie `HttpClient` korzysta z przeglądarki do obsługi ruchu HTTP w tle. [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) automatycznie zostaje ustawiony poziom podstawowy prefiks identyfikatora URI aplikacji. Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>. |
| `IJSRuntime` | pojedyncze | Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript gdzie wysłaniem wywołania języka JavaScript. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>. |
| `IUriHelper` | pojedyncze | Zawiera pomocnicy do pracy ze stanem identyfikatory URI i nawigacji. Aby uzyskać więcej informacji, zobacz [identyfikator URI i nawigacji pomocników stanu](xref:blazor/routing#uri-and-navigation-state-helpers). |

Niestandardowe usługodawcy automatycznie nie dostarcza usługi domyślne wymienione w tabeli. Jeśli używasz niestandardowego usługodawcy i wymaga żadnej usług przedstawionych w tabeli, Dodaj wymagane usługi, do nowego dostawcę usługi.

## <a name="add-services-to-an-app"></a>Dodawanie usług do aplikacji

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
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Po stronie klienta Blazor aktualnie nie ma koncepcji DI zakresów. `Scoped`-zarejestrowane usługi zachowują się jak `Singleton` usług. Jednak model hostowania po stronie serwera obsługuje `Scoped` okresu istnienia. W składniku Razor rejestracji usługi o określonym zakresie jest ograniczony do połączenia. Z tego powodu przy użyciu usługi o określonym zakresie była preferowana dla usług, które powinien być ograniczony do bieżącego użytkownika, nawet jeśli bieżącym celem jest do uruchomienia po stronie klienta w przeglądarce. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | Tworzy DI *pojedyncze wystąpienie* usługi. Wszystkie składniki wymagające `Singleton` usługa otrzymywać wystąpienia tej samej usługi. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Zawsze, gdy składnik uzyskuje wystąpienie `Transient` usługi z kontenera usługi odbiera *nowe wystąpienie* usługi. |

DI system jest oparty na systemie DI, w programie ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Żądanie usługi w składniku

Po dodaniu usługi do kolekcji usługi wstrzyknąć usług do składników za pomocą [ \@wstrzyknąć](xref:mvc/views/razor#section-4) dyrektywy Razor. `@inject` ma dwa parametry:

* Typ &ndash; typ usługi do dodania.
* Właściwość &ndash; nazwa właściwości odbieranie usługi wprowadzonego kodu aplikacji. Właściwość nie wymaga ręcznego tworzenia. Kompilator tworzy właściwość.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.

Używanych jest wiele `@inject` instrukcje, aby wstawić różnych usług.

Poniższy przykład pokazuje, jak używać `@inject`. Wdrażanie usługi `Services.IDataAccess` są wstrzykiwane do właściwości składnika `DataRepository`. Należy zauważyć, jak kod jest wyłącznie przy użyciu `IDataAccess` abstrakcji:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Wewnętrznie, wygenerowana właściwość (`DataRepository`) zostanie nadany `InjectAttribute` atrybutu. Zazwyczaj ten atrybut nie jest używany bezpośrednio. Jeśli klasa bazowa jest wymagana dla składników i właściwości wprowadzonego są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
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

## <a name="use-di-in-services"></a>Użyj DI w usługach

Złożone usługi może wymagać dodatkowych usług. W poprzednim przykładem `DataAccess` mogą wymagać `HttpClient` domyślnej usługi. `@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach. *Iniekcji konstruktora* należy użyć zamiast tego. Wymagane usługi są dodawane, dodając parametry do konstruktora tej usługi. Gdy DI tworzy usługę, rozpoznaje usług wymaga w Konstruktorze i udostępnia je w związku z tym.

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

* Jeden konstruktor musi istnieć, której argumenty mogą wszystkie zostać spełnione przez DI. Dodatkowe parametry, które nie są objęte DI są dozwolone, gdy ich określanie wartości domyślnych.
* Zastosowanie Konstruktor musi być *publicznych*.
* Musi istnieć jeden konstruktor dotyczy. W przypadku niejednoznaczności DI zgłasza wyjątek.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
