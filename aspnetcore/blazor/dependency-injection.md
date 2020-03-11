---
title: ASP.NET Core Blazor iniekcji zależności
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658075"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core iniekcja zależności Blazor

Autorzy [Rainer Stropek](https://www.timecockpit.com) i [Jan Rousos](https://github.com/mjrousos)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection). Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu. Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.

DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji. Może to być przydatne w aplikacjach Blazor do:

* Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako *pojedyncze usługi.*
* Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań. Rozważmy na przykład `IDataAccess` interfejsu do uzyskiwania dostępu do danych w aplikacji. Interfejs jest implementowany przez konkretną klasę `DataAccess` i zarejestrowany jako usługa w kontenerze usługi aplikacji. Gdy składnik używa elementu DI do odbierania implementacji `IDataAccess`, składnik nie jest połączony z konkretnym typem. Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.

## <a name="default-services"></a>Usługi domyślne

Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.

| Usługa | Okres istnienia | Opis |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | pojedynczego | Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI.<br><br>Wystąpienie `HttpClient` w aplikacji webassembly Blazor używa przeglądarki do obsługi ruchu HTTP w tle.<br><br>Aplikacje serwera Blazor nie domyślnie zawierają `HttpClient` skonfigurowany jako usługa. Podaj `HttpClient` do aplikacji serwera Blazor.<br><br>Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton (Blazor webassembly)<br>W zakresie (serwer Blazor) | Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript. Aby uzyskać więcej informacji, zobacz <xref:blazor/call-javascript-from-dotnet>. |
| `NavigationManager` | Singleton (Blazor webassembly)<br>W zakresie (serwer Blazor) | Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji. Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers). |

Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli. W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.

## <a name="add-services-to-an-app"></a>Dodawanie usług do aplikacji

### <a name="blazor-webassembly"></a>Zestaw WebAssembly Blazor

Skonfiguruj usługi dla kolekcji usług aplikacji w `Main` metodzie *program.cs*. W poniższym przykładzie zarejestrowano implementację `MyDependency` dla `IMyDependency`:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

Po skompilowaniu hosta usługi można uzyskać dostęp z poziomu głównego DI zakresu przed renderowaniem wszystkich składników. Może to być przydatne do uruchamiania logiki inicjowania przed renderowaniem zawartości:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

Host udostępnia również centralne wystąpienie konfiguracji dla aplikacji. W poprzednim przykładzie adres URL usługi Pogoda jest przesyłany z domyślnego źródła konfiguracji (na przykład *appSettings. JSON*) do `InitializeWeatherAsync`:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a>Serwer Blazor

Po utworzeniu nowej aplikacji należy zapoznać się z `Startup.ConfigureServices` metodzie:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Metoda `ConfigureServices` jest przenoszona <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, która jest listą obiektów deskryptorów usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług. Poniższy przykład ilustruje koncepcję z interfejsem `IDataAccess` i jego konkretną implementacją `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a>Okres istnienia usługi

Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.

| Okres istnienia | Opis |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor aplikacje webassembly nie mają obecnie koncepcji DI Scopes. usługi zarejestrowane `Scoped`zachowują się jak usługi `Singleton` Services. Jednak model hostingu serwera Blazor obsługuje okres istnienia `Scoped`. W aplikacjach Blazor Server Rejestracja usługi w zakresie została objęta zakresem *połączenia*. Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI tworzy *pojedyncze wystąpienie* usługi. Wszystkie składniki wymagające usługi `Singleton` otrzymują wystąpienie tej samej usługi. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Za każdym razem, gdy składnik uzyskuje wystąpienie usługi `Transient` z kontenera usługi, otrzymuje *nowe wystąpienie* usługi. |

System DI jest oparty na systemie DI w ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Żądanie usługi w składniku

Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą dyrektywy [wstrzykiwania Razor\@](xref:mvc/views/razor#inject) . `@inject` ma dwa parametry:

* Wpisz &ndash; typ usługi do dodania.
* Właściwość &ndash; nazwą właściwości otrzymującej wstrzykiwaną usługę App Service. Właściwość nie wymaga ręcznego tworzenia. Kompilator tworzy właściwość.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.

Użyj wielu instrukcji `@inject`, aby wstrzyknąć różne usługi.

Poniższy przykład pokazuje, jak używać `@inject`. Usługa implementująca `Services.IDataAccess` jest wstrzykiwana do `DataRepository`właściwości składnika. Zwróć uwagę, jak kod używa wyłącznie abstrakcji `IDataAccess`:

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Wewnętrznie wygenerowana Właściwość (`DataRepository`) używa atrybutu `InjectAttribute`. Zazwyczaj ten atrybut nie jest używany bezpośrednio. Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

W składnikach pochodnych z klasy bazowej dyrektywa `@inject` nie jest wymagana. `InjectAttribute` klasy podstawowej jest wystarczająca:

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Korzystanie z usług DI w

Złożone usługi mogą wymagać dodatkowych usług. W poprzednim przykładzie `DataAccess` może wymagać `HttpClient` domyślnej usługi. `@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach. Zamiast tego należy użyć *iniekcji konstruktora* . Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi. Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.

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

W przypadku aplikacji ASP.NET Core usługi o określonym zakresie są zwykle objęte zakresem bieżącego żądania. Po zakończeniu żądania wszystkie usługi w zakresie lub przejściowym są usuwane przez system DI. W Blazor aplikacji serwerowych zakres żądań jest stosowany przez czas trwania połączenia klienta, co może spowodować, że usługi przejściowe i objęte zakresem będą dużo dłużej niż oczekiwano. W Blazor aplikacje webassembly usługi zarejestrowane w okresie istnienia w określonym zakresie są traktowane jako pojedyncze, tak aby znajdowały się one dłużej niż usługi w zakresie w typowej aplikacji ASP.NET Core.

Podejście ograniczające okres istnienia usługi w aplikacjach Blazor korzysta z typu `OwningComponentBase`. `OwningComponentBase` jest typem abstrakcyjnym pochodnym `ComponentBase`, który tworzy zakres DI odpowiadający okresowi istnienia składnika. Korzystając z tego zakresu, możliwe jest korzystanie z usługi DI Services z okresem istnienia w zakresie i posiadanie ich na żywo tak długo, jak w przypadku składnika. Gdy składnik zostanie zniszczony, usługi z dostawcy usług w zasięgu składnika również zostaną usunięte. Może to być przydatne w przypadku usług, które:

* Należy ponownie użyć w składniku, ponieważ przejściowy okres istnienia jest nieodpowiedni.
* Nie powinny być współużytkowane przez składniki, ponieważ pojedynczy okres istnienia jest nieodpowiedni.

Dostępne są dwie wersje typu `OwningComponentBase`:

* `OwningComponentBase` to abstrakcyjny, jednorazowy element podrzędny typu `ComponentBase` z chronioną właściwością `ScopedServices` typu `IServiceProvider`. Ten dostawca może służyć do rozpoznawania usług objętych zakresem czasu istnienia składnika.

  W zakresie składnika nie są tworzone usługi DI, które zostały dodane do składnika przy użyciu `@inject` lub `InjectAttribute` (`[Inject]`). Aby użyć zakresu składnika, należy rozwiązać usługi przy użyciu `ScopedServices.GetRequiredService` lub `ScopedServices.GetService`. Wszystkie usługi rozpoznane przy użyciu dostawcy `ScopedServices` mają swoje zależności z tego samego zakresu.

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* `OwningComponentBase<T>` pochodzi od `OwningComponentBase` i dodaje `Service` właściwości, która zwraca wystąpienie `T` z dostawcy i zakresu. Ten typ jest wygodnym sposobem uzyskiwania dostępu do usług objętych zakresem bez użycia wystąpienia `IServiceProvider`, gdy istnieje jedna usługa podstawowa wymagana przez aplikację z kontenera DI używającego zakresu składnika. Właściwość `ScopedServices` jest dostępna, aby aplikacja mogła uzyskać usługi innych typów, w razie potrzeby.

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a>Korzystanie z Entity Framework DbContext z elementu DI

Jednym z typowych typów usług pobieranych z aplikacji w sieci Web jest Entity Framework (EF) `DbContext` obiekty. Rejestrowanie usług EF przy użyciu `IServiceCollection.AddDbContext` domyślnie dodaje `DbContext` jako usługę objętą zakresem. Rejestracja w ramach usługi o określonym zakresie może prowadzić do problemów w aplikacjach Blazor, ponieważ powoduje to, że `DbContext` wystąpienia są długotrwałe i udostępniane w całej aplikacji. `DbContext` nie jest bezpieczny wątkowo i nie może być używany współbieżnie.

W zależności od aplikacji użycie `OwningComponentBase` do ograniczenia zakresu `DbContext` do jednego składnika *może* rozwiązać ten problem. Jeśli składnik nie używa równolegle `DbContext`, wyprowadza składnik z `OwningComponentBase` i pobieranie `DbContext` z `ScopedServices` jest wystarczające, ponieważ zapewnia:

* Oddzielne składniki nie współdzielą `DbContext`.
* `DbContext` mieszka tylko tak długo, jak i w zależności od tego, jak działa składnik.

Jeśli pojedynczy składnik może używać `DbContext` współbieżnie (na przykład za każdym razem, gdy użytkownik wybierze przycisk), nawet przy użyciu `OwningComponentBase` nie pozwala uniknąć problemów z współbieżnymi operacjami EF. W takim przypadku należy użyć innego `DbContext` dla każdej operacji logicznej EF. Użyj jednej z następujących metod:

* Utwórz `DbContext` bezpośrednio przy użyciu `DbContextOptions<TContext>` jako argumentu, który można pobrać z elementu "i" jest bezpieczny wątkowo.

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* Zarejestruj `DbContext` w kontenerze usługi z przejściowym okresem istnienia:
  * Podczas rejestrowania kontekstu użyj `ServiceLifetime.Transient`. Metoda rozszerzenia `AddDbContext` przyjmuje dwa opcjonalne parametry typu `ServiceLifetime`. Aby użyć tego podejścia, należy `ServiceLifetime.Transient`tylko parametr `contextLifetime`. `optionsLifetime` może zachować wartość domyślną `ServiceLifetime.Scoped`.

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * Przejściowe `DbContext` można wstrzyknąć jako normalne (przy użyciu `@inject`) do składników, które nie wykonują równolegle wielu operacji EF. Te, które mogą wykonywać wiele operacji EF jednocześnie, mogą zażądać oddzielnych `DbContext` obiektów dla każdej operacji równoległej przy użyciu `IServiceProvider.GetRequiredService`.

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
