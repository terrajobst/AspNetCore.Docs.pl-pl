---
title: Wstrzykiwanie zależności w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób ASP.NET Core implementuje iniekcję zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: c46e7322e86c2836a15bd0720995a8634bb185be
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634009"
---
# <a name="dependency-injection-in-aspnet-core"></a>Wstrzykiwanie zależności w ASP.NET Core

[Steve Kowalski](https://ardalis.com/), [Scott Addie](https://scottaddie.com)i [Luke Latham](https://github.com/guardrex)

ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia [niewersji kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.

Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Przegląd iniekcji zależności

*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt. Zapoznaj się z następującą klasą `MyDependency` przy użyciu metody `WriteMessage`, z której zależą inne klasy w aplikacji:

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

Wystąpienie klasy `MyDependency` można utworzyć w celu udostępnienia metody `WriteMessage` dla klasy. Klasa `MyDependency` jest zależnością klasy `IndexModel`:

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

Klasa tworzy i bezpośrednio zależy od wystąpienia `MyDependency`. Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:

* Aby zastąpić `MyDependency` z inną implementacją, należy zmodyfikować klasę.
* Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę. W dużym projekcie z wieloma klasami, w zależności od `MyDependency`, kod konfiguracji zostanie rozłożona przez aplikację.
* Ta implementacja jest trudna do testowania jednostkowego. Aplikacja powinna korzystać z klasy imitacji lub stub `MyDependency`, co nie jest możliwe w przypadku tego podejścia.

Iniekcja zależności eliminuje te problemy w następujący sposób:

* Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.
* Rejestracja zależności w kontenerze usługi. ASP.NET Core zawiera wbudowany kontener usługi <xref:System.IServiceProvider>. Usługi są zarejestrowane w metodzie `Startup.ConfigureServices` aplikacji.
* *Iniekcja* usługi do konstruktora klasy, w której jest używana. Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.

W [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)interfejs `IMyDependency` definiuje metodę dostarczaną przez usługę do aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

Ten interfejs jest implementowany przez konkretny typ, `MyDependency`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

`MyDependency` żąda <xref:Microsoft.Extensions.Logging.ILogger`1> w konstruktorze. Użycie iniekcji zależności w łańcuchu nie jest nietypowe. Każda żądana zależność z kolei żąda własnych zależności. Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi. Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.

w kontenerze usługi `IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowany. `IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`. `ILogger<TCategoryName>` jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona przez platformę](#framework-provided-services) zarejestrowana domyślnie przez platformę.

Kontener rozwiązuje `ILogger<TCategoryName>` dzięki wykorzystaniu [typów otwartych (rodzajowych)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) typu konstruowanego](/dotnet/csharp/language-reference/language-specification/types#constructed-types):

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

W przykładowej aplikacji usługa `IMyDependency` jest zarejestrowana z typem konkretnym `MyDependency`. Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania. [Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> Każda metoda rozszerzenia `services.Add{SERVICE_NAME}` dodaje usługi (i potencjalnie konfiguruje). Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC. Zalecamy, aby aplikacje były zgodne z tą konwencją. Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.

Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

Wystąpienie usługi jest wymagane za pośrednictwem konstruktora klasy, w której jest używana usługa i jest przypisywana do pola prywatnego. To pole jest używane w celu uzyskania dostępu do usługi w zależności od potrzeb w całej klasie.

W przykładowej aplikacji jest wymagane wystąpienie `IMyDependency` i używane do wywołania metody `WriteMessage` usługi:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

## <a name="services-injected-into-startup"></a>Usługi wstrzykiwane do uruchamiania

Tylko następujące typy usług można wstrzyknąć do konstruktora `Startup`, gdy jest używany host generyczny (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

Usługi można wstrzyknąć do `Startup.Configure`:

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

## <a name="framework-provided-services"></a>Usługi udostępniane przez platformę

Metoda `Startup.ConfigureServices` jest odpowiedzialna za Definiowanie usług, z których korzysta aplikacja, w tym funkcji platformy, takich jak Entity Framework Core i ASP.NET Core MVC. Początkowo `IServiceCollection` zapewniane `ConfigureServices` ma usługi zdefiniowane przez platformę w zależności od [konfiguracji hosta](xref:fundamentals/index#host). Aplikacja oparta na ASP.NET Core szablonie nie jest nietypowa, aby mieć setki usług zarejestrowanych przez platformę. W poniższej tabeli wymieniono niewielki przykład usług zarejestrowanych w ramach platformy.

::: moniker range=">= aspnetcore-3.0"

| Typ usługi | Okres istnienia |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Administracyjnej |
| `IHostApplicationLifetime` | pojedynczego |
| `IWebHostEnvironment` | pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | pojedynczego |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | pojedynczego |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | pojedynczego |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Typ usługi | Okres istnienia |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | pojedynczego |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Administracyjnej |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | pojedynczego |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | pojedynczego |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | pojedynczego |

::: moniker-end

## <a name="register-additional-services-with-extension-methods"></a>Rejestrowanie dodatkowych usług przy użyciu metod rozszerzających

Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać pojedynczej metody rozszerzenia `Add{SERVICE_NAME}`, aby zarejestrować wszystkie usługi wymagane przez tę usługę. Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [AddDbContext \<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) i <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

Aby uzyskać więcej informacji, zapoznaj się z klasą <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> w dokumentacji interfejsu API.

## <a name="service-lifetimes"></a>Okresy istnienia usługi

Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi. Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:

### <a name="transient"></a>Administracyjnej

Przejściowe usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) są tworzone za każdym razem, gdy zażądają one kontenera usług. Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.

### <a name="scoped"></a>Zakresie

Usługi okresu istnienia w zakresie (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) są tworzone raz dla każdego żądania klienta (połączenie).

> [!WARNING]
> W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do metody `Invoke` lub `InvokeAsync`. Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.

### <a name="singleton"></a>pojedynczego

Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub po uruchomieniu `Startup.ConfigureServices`, a wystąpienie jest określone przy rejestracji usługi). Każde kolejne żądanie używa tego samego wystąpienia. Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi. Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.

> [!WARNING]
> Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne. Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.

## <a name="service-registration-methods"></a>Metody rejestracji usług

Metody rozszerzenia rejestracji usług oferują przeciążenia, które są przydatne w określonych scenariuszach.

| Metoda | Automatyczne<br>object<br>myśl | Wielokrotne<br>implementacje | Przekaż argumenty |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br>Przykład:<br>`services.AddSingleton<IMyDep, MyDep>();` | Tak | Tak | Nie |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | Tak | Tak | Tak |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br>Przykład:<br>`services.AddSingleton<MyDep>();` | Tak | Nie | Nie |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | Nie | Tak | Tak |
| `AddSingleton(new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | Nie | Nie | Tak |

Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) . Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).

`TryAdd{LIFETIME}` metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.

W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency` dla `IMyDependency`. Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` już ma zarejestrowanej implementacji:

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

Aby uzyskać więcej informacji, zobacz:

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*. Wiele usług jest rozpoznawanych za pośrednictwem `IEnumerable<{SERVICE}>`. Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany. Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.

W poniższym przykładzie pierwszy wiersz rejestruje `MyDep` dla `IMyDep1`. Drugi wiersz rejestruje `MyDep` dla `IMyDep2`. Trzeci wiersz nie działa, ponieważ `IMyDep1` już ma zarejestrowanej implementacji `MyDep`:

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a>Zachowanie iniekcji konstruktora

Usługi mogą być rozwiązywane przez dwa mechanizmy:

* <xref:System.IServiceProvider>
* <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności. `ActivatorUtilities` jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modeli.

Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.

Gdy usługi są rozwiązane przez `IServiceProvider` lub `ActivatorUtilities`, iniekcja konstruktora wymaga konstruktora *publicznego* .

Gdy usługi są rozpoznawane przez `ActivatorUtilities`, iniekcja konstruktora wymaga, aby tylko jeden odpowiedni Konstruktor istniał. Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.

## <a name="entity-framework-contexts"></a>Konteksty Entity Framework

Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta. Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez [AddDbContext \<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Przeciążenie podczas rejestrowania kontekstu bazy danych. Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.

## <a name="lifetime-and-registration-options"></a>Opcje okresu istnienia i rejestracji

Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje z unikatowym identyfikatorem, `OperationId`. W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

Interfejsy są zaimplementowane w klasie `Operation`. Konstruktor `Operation` generuje identyfikator GUID, jeśli nie został podany:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

Zarejestrowano `OperationService`, który zależy od poszczególnych typów `Operation`. Gdy żądanie `OperationService` za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.

* Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` usługi `IOperationTransient` są inne niż `OperationId` `OperationService`. `OperationService` otrzymuje nowe wystąpienie klasy `IOperationTransient`. Nowe wystąpienie daje różne `OperationId`.
* Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` usługi `IOperationScoped` jest taka sama jak w przypadku `OperationService` w ramach żądania klienta. W przypadku żądań klientów obie usługi współdzielą inną wartość `OperationId`.
* Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane między wszystkimi żądaniami klientów i wszystkimi usługami, `OperationId` jest stałą we wszystkich żądaniach obsługi.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

W `Startup.ConfigureServices` każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

Usługa `IOperationSingletonInstance` używa określonego wystąpienia o znanym IDENTYFIKATORze `Guid.Empty`. Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).

Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi. `IndexModel` Przykładowa aplikacja żąda każdego rodzaju typu `IOperation` i `OperationService`. Na stronie zostaną wyświetlone wszystkie wartości `OperationId` klasy modelu strony i usługi za pomocą przypisań właściwości:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:

**Pierwsze żądanie:**

Operacje kontrolera:

Przejściowy: d233e165-f417-469B-A866-1cf1935d2518  
Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

`OperationService` operacje:

Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

**Drugie żądanie:**

Operacje kontrolera:

Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0  
Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

`OperationService` operacje:

Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:

* Obiekty *przejściowe* są zawsze różne. Przejściowa wartość `OperationId` dla pierwszych i drugich żądań klienta różni się w zależności od operacji `OperationService` i między żądaniami klientów. Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.
* Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.
* *Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, czy wystąpienie `Operation` jest dostępne w `Startup.ConfigureServices`.

## <a name="call-services-from-main"></a>Wywoływanie usług z głównego

Utwórz <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> przy użyciu [IServiceScopeFactory. Isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji. Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania. Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:

::: moniker range=">= aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

        using (var serviceScope = host.Services.CreateScope())
        {
            var services = serviceScope.ServiceProvider;

            try
            {
                var serviceContext = services.GetRequiredService<MyScopedService>();
                // Use the context here
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred.");
            }
        }
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();

        using (var serviceScope = host.Services.CreateScope())
        {
            var services = serviceScope.ServiceProvider;

            try
            {
                var serviceContext = services.GetRequiredService<MyScopedService>();
                // Use the context here
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred.");
            }
        }
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="scope-validation"></a>Weryfikacja zakresu

Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług sprawdza, czy:

* Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.
* Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.

Główny dostawca usług jest tworzony, gdy zostanie wywołane <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>. Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.

Usługi o określonym zakresie są usuwane przez kontener, który go utworzył. Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony. Sprawdzanie poprawności zakresów usług przechwytuje te sytuacje w przypadku wywołania `BuildServiceProvider`.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Usługi żądania

Usługi dostępne w ramach żądania ASP.NET Core od `HttpContext` są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .

Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji. Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, a nie `ApplicationServices`.

Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio. Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę. To daje klasy, które są łatwiejsze do przetestowania.

> [!NOTE]
> Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp do kolekcji `RequestServices`.

## <a name="design-services-for-dependency-injection"></a>Projektowanie usług do iniekcji zależności

Najlepsze rozwiązania:

* Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.
* Unikaj stanowych, statycznych klas i elementów członkowskich. Zaprojektuj aplikacje do korzystania z pojedynczych usług, aby uniknąć tworzenia stanu globalnego.
* Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług. Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.
* Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.

Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy. Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika. Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Usuwanie usług

Kontener wywołuje <xref:System.IDisposable.Dispose*> dla typów <xref:System.IDisposable>, które tworzy. Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

## <a name="default-service-container-replacement"></a>Zastępowanie kontenera usług domyślnych

Wbudowany kontener usług został zaprojektowany z myślą o potrzebach platformy i większości aplikacji konsumenckich. Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, która nie jest obsługiwana przez wbudowany kontener, na przykład:

* Iniekcja właściwości
* Iniekcja oparta na nazwie
* Kontenery podrzędne
* Niestandardowe zarządzanie okresem istnienia
* Obsługa `Func<T>` w przypadku inicjowania z opóźnieniem

Za pomocą aplikacji ASP.NET Core można używać następujących kontenerów innych firm:

* [Autofac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [DryIoc](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [Prolongaty](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [LightInject](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [Lamar](https://jasperfx.github.io/lamar/)
* [Stashbox](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [Unity](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a>Bezpieczeństwo wątków

Twórz bezpieczne dla wątków usługi pojedyncze. Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.

Metoda fabryki pojedynczej usługi, taka jak drugi argument dla [AddSingleton \<TService > (IServiceCollection, Func \<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo. Podobnie jak w przypadku konstruktora typu (`static`), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.

## <a name="recommendations"></a>Mając

* Rozpoznawanie usług `async/await` i `Task` nie jest obsługiwane. C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.

* Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi. Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi. Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options). Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu. Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.

* Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).

* Unikaj używania *wzorca lokalizatora usługi*. Na przykład nie należy wywoływać <xref:System.IServiceProvider.GetService*>, aby uzyskać wystąpienie usługi, gdy można użyć DI zamiast:

  **Prawidłowy**

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  **Poprawne**:

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* Inna odmiana lokalizatora usługi, aby uniknąć, wprowadza fabrykę, która rozwiązuje zależności w czasie wykonywania. Obie te praktyki mieszają się [z niewersjami strategii kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .

* Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).

Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane. Wyjątkami są rzadkie &mdash;mostly specjalne przypadki w ramach samej struktury.

DI jest *alternatywą* dla wzorców dostępu do obiektów static/Global. Możesz nie być w stanie korzystać z zalet programu DI w przypadku jego mieszania z dostępem do obiektów statycznych.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [Pisanie czystego kodu w ASP.NET Core z iniekcją zależności (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Zasada jawnych zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Niewersja kontenerów sterowania i wzorzec iniekcji zależności (Martin Fowlera)](https://www.martinfowler.com/articles/injection.html)
* [Jak zarejestrować usługę z wieloma interfejsami w ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
