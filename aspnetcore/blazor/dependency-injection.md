---
title: ASP.NET Core iniekcja zależności Blazor
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: a2bfa0cbe951e817ed6264f1a151d5a716cd795c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310350"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core iniekcja zależności Blazor

Autor [Rainer Stropek](https://www.timecockpit.com)

Blazor obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection). Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu. Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.

DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji. Może to być przydatne w aplikacjach Blazor do:

* Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako *pojedyncze usługi.*
* Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań. Rozważmy na przykład interfejs `IDataAccess` do uzyskiwania dostępu do danych w aplikacji. Interfejs jest implementowany przez konkretną `DataAccess` klasę i zarejestrowany jako usługa w kontenerze usługi aplikacji. Gdy składnik używa elementu di do odbierania `IDataAccess` implementacji, składnik nie jest połączony z konkretnym typem. Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.

## <a name="default-services"></a>Usługi domyślne

Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.

| Usługa | Okres istnienia | Opis |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Pojedynczego | Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI. Należy zauważyć, że to `HttpClient` wystąpienie programu używa przeglądarki do obsługi ruchu HTTP w tle. [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) jest automatycznie ustawiany na podstawowy prefiks identyfikatora URI aplikacji. Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>. |
| `IJSRuntime` | Pojedynczego | Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>. |
| `NavigationManager` | Pojedynczego | Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji. Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers). |

Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli. W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.

## <a name="add-services-to-an-app"></a>Dodawanie usług do aplikacji

Po utworzeniu nowej aplikacji zapoznaj się z `Startup.ConfigureServices` tą metodą:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Metoda jest przenoszona<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>a ,którajestlistąobiektówdeskryptorausługi().<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> `ConfigureServices` Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług. Poniższy przykład demonstruje koncepcję z `IDataAccess` interfejsem i jego konkretną implementacją: `DataAccess`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.

| Okres istnienia | Opis |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Po stronie klienta Blazor nie ma obecnie koncepcji DI Scopes. `Scoped`-zarejestrowane usługi zachowują `Singleton` się jak usługi. Jednak model hostingu po stronie serwera obsługuje `Scoped` okres istnienia. W składniku Razor Rejestracja usługi w zakresie jest objęta zakresem połączenia. Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI tworzy *pojedyncze wystąpienie* usługi. Wszystkie składniki wymagające `Singleton` usługi odbierają wystąpienie tej samej usługi. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Za każdym razem, gdy składnik uzyskuje wystąpienie `Transient` usługi z kontenera usługi, otrzymuje *nowe wystąpienie* usługi. |

System DI jest oparty na systemie DI w ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Żądanie usługi w składniku

Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą [ \@dyrektywy wstrzyknięcia](xref:mvc/views/razor#inject) Razor. `@inject`ma dwa parametry:

* Wpisz &ndash; typ usługi do dodania.
* Właściwość &ndash; nazwa właściwości otrzymującej wstrzykiwanej usługi App Service. Właściwość nie wymaga ręcznego tworzenia. Kompilator tworzy właściwość.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.

Użyj wielu `@inject` instrukcji, aby wstrzyknąć różne usługi.

Poniższy przykład pokazuje, jak używać `@inject`. Implementowanie `Services.IDataAccess` usługi jest wstrzykiwane do właściwości `DataRepository`składnika. Zwróć uwagę, jak kod używa `IDataAccess` tylko abstrakcji:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Wewnętrznie wygenerowana Właściwość (`DataRepository`) jest uzupełniona `InjectAttribute` atrybutem. Zazwyczaj ten atrybut nie jest używany bezpośrednio. Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, należy ręcznie dodać `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

W składnikach pochodnych klasy `@inject` bazowej dyrektywa nie jest wymagana. `InjectAttribute` Klasa bazowa jest wystarczająca:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Korzystanie z usług DI w

Złożone usługi mogą wymagać dodatkowych usług. W poprzednim przykładzie `DataAccess` może być `HttpClient` wymagana usługa domyślna. `@inject`(lub `InjectAttribute`) nie jest dostępny do użytku w usługach. Zamiast tego należy użyć *iniekcji konstruktora* . Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi. Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.

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

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
