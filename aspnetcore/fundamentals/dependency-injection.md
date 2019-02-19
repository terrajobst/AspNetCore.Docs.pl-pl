---
title: Wstrzykiwanie zależności w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak platformy ASP.NET Core implementuje wstrzykiwanie zależności i jak z niej korzystać.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 5e5b9746da9bbc13a147b807aabfd3d9ab90a0ca
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410511"
---
# <a name="dependency-injection-in-aspnet-core"></a>Wstrzykiwanie zależności w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), i [Luke Latham](https://github.com/guardrex)

Platforma ASP.NET Core obsługuje zależności wzorzec projektowy oprogramowania iniekcji (DI), czyli technikę do osiągnięcia [Inwersja kontroli (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależności.

Aby uzyskać więcej informacji specyficznych dla wstrzykiwanie zależności w ramach kontrolerów MVC, zobacz <xref:mvc/controllers/dependency-injection>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Omówienie wstrzykiwanie zależności

A *zależności* jest dowolny obiekt, który wymaga innego obiektu. Sprawdź następujące `MyDependency` klasy `WriteMessage` metody, które zależą od innych klas w aplikacji:

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

::: moniker range=">= aspnetcore-2.1"

Wystąpienie `MyDependency` klasy mogą być tworzone się `WriteMessage` klasy dostępnej metody. `MyDependency` Klasa jest zależą od elementu `IndexModel` klasy:

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

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Wystąpienie `MyDependency` klasy mogą być tworzone się `WriteMessage` klasy dostępnej metody. `MyDependency` Klasa jest zależą od elementu `HomeController` klasy:

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

Klasa tworzy i zależy od bezpośrednio `MyDependency` wystąpienia. Zależności w kodzie (na przykład w poprzednim przykładzie) są problematyczne i należy unikać z następujących powodów:

* Aby zastąpić `MyDependency` z inną implementacją klasy muszą zostać zmodyfikowane.
* Jeśli `MyDependency` ma zależności, musi być skonfigurowany przez klasę. W dużym projekcie z wieloma klasami w zależności od `MyDependency`, kod konfiguracji staje się znajdują się na aplikację.
* Ta implementacja jest trudny do testów jednostkowych. Aplikację należy użyć pozorny lub namiastki `MyDependency` klasy, która nie jest możliwe w przypadku tej metody.

Wstrzykiwanie zależności rozwiązuje te problemy za pomocą:

* Użycie interfejsu tworzących warstwę abstrakcji implementacji zależności.
* Rejestracja zależności w kontenerze usługi. Platforma ASP.NET Core zapewnia kontener wbudowanej usługi [IServiceProvider](/dotnet/api/system.iserviceprovider). Usługi są zarejestrowane w usłudze aplikacji `Startup.ConfigureServices` metody.
* *Iniekcja* usługi do konstruktora klasy, w których jest używany. Struktura przejmuje odpowiedzialność za tworzenie wystąpienia zależności i usuwania je, gdy nie jest już potrzebny.

W [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` interfejs definiuje metodę, która udostępnia usługę do aplikacji:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

Ten interfejs jest implementowany przez konkretny typ `MyDependency`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

`MyDependency` żądania [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) w jego konstruktorze. Nie jest niczym niezwykłym używać wstrzykiwanie zależności w sposób połączonych. Poszczególne zależności żądanego żądań z kolei swoje własne zależności. Jest rozpoznawana jako zależności na wykresie i zwraca w pełni rozpoznać usługę kontenera. Zbiorczy zestaw zależności, które muszą być rozwiązane jest zwykle nazywany *drzewo zależności*, *wykres zależności*, lub *wykresu obiektu*.

`IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowana w kontenerze usługi. `IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`. `ILogger<TCategoryName>` jest on zarejestrowany infrastruktury abstrakcje rejestrowanie, dlatego ma [usługi dostarczane przez framework](#framework-provided-services) zarejestrowana domyślnie przez platformę.

W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana przy użyciu konkretnego typu `MyDependency`. Rejestracja zakresów okres istnienia usługi przez cały czas trwania pojedynczego żądania. [Okresy istnienia usługi](#service-lifetimes) są opisane w dalszej części tego tematu.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> Każdy `services.Add{SERVICE_NAME}` — metoda rozszerzenia dodaje (i potencjalnie konfiguruje) usługi. Na przykład `services.AddMvc()` dodaje usług, stronami Razor i wymagają MVC. Zaleca się, że aplikacje stosują taką Konwencję. Metody rozszerzające w miejscu [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) przestrzeni nazw w celu hermetyzacji grupy rejestracji usługi.

Jeśli Konstruktor usługi wymaga elementu podstawowego, takich jak `string`, element pierwotny może wprowadzone za pomocą [konfiguracji](xref:fundamentals/configuration/index) lub [wzorzec opcje](xref:fundamentals/configuration/options):

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

Za pośrednictwem konstruktora klasy, gdzie usługa jest używana i przypisane do pola prywatnego, wymagane jest wystąpienie usługi. Pole jest używane do uzyskania dostępu do usługi zgodnie z potrzebami w całej klasy.

W przykładowej aplikacji `IMyDependency` wystąpienie jest wymagane i używane do wywołania tej usługi `WriteMessage` metody:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a>Dostarczone do struktury usługi

`Startup.ConfigureServices` Metoda jest odpowiedzialna definiowanie usługi, którego korzysta aplikacja, łącznie z funkcjami platformy, takie jak Entity Framework Core i ASP.NET Core MVC. Początkowo `IServiceCollection` udostępniane `ConfigureServices` ma następujące usługi, które są zdefiniowane (w zależności od [konfiguracji hosta](xref:fundamentals/host/index)):

| Typ usługi | Okres istnienia |
| ------------ | -------- |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Przejściowe |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Przejściowe |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | pojedyncze |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Przejściowe |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | pojedyncze |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | pojedyncze |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | pojedyncze |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Przejściowe |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | pojedyncze |
| [System.Diagnostics.DiagnosticSource](/dotnet/core/api/system.diagnostics.diagnosticsource) | pojedyncze |
| [System.Diagnostics.DiagnosticListener](/dotnet/core/api/system.diagnostics.diagnosticlistener) | pojedyncze |

Po udostępnieniu register a service (i jej usługi zależne, jeśli jest to wymagane) metody rozszerzenia kolekcji usługi Konwencji jest użycie pojedynczego `Add{SERVICE_NAME}` metodę rozszerzenia, aby zarejestrować wszystkich usług wymaganych przez tę usługę. Poniższy kod jest przykładem sposobu dodawania dodatkowych usług do kontenera przy użyciu metody rozszerzenia [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), i [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

Aby uzyskać więcej informacji, zobacz [klasy ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) w dokumentacji interfejsu API.

## <a name="service-lifetimes"></a>Okresy istnienia usługi

Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi. Usługi ASP.NET Core mogą być skonfigurowane przy użyciu następujących okresów istnienia:

**Przejściowe**

Przejściowych okres istnienia usługi są tworzone w każdym razem, gdy są one wymagane. Ten okres istnienia najlepiej uproszczone, bezstanowych usług.

**O określonym zakresie**

Okres istnienia w zakresie usług są tworzone raz na każde żądanie.

> [!WARNING]
> Korzystając z usługi o określonym zakresie w oprogramowaniu pośredniczącym, wprowadzić usługę do `Invoke` lub `InvokeAsync` metody. Nie wstrzyknąć przy użyciu iniekcji konstruktora, ponieważ wymusza usługę, aby zachowywać się jak wzorzec singleton. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.

**pojedyncze**

Pojedyncze okres istnienia usługi są tworzone po raz pierwszy masz żądanej (lub gdy `ConfigureServices` jest uruchamiany i wystąpienie jest określony za pomocą rejestracji usługi). Każde kolejne żądanie używa tego samego wystąpienia. Jeśli aplikacja wymaga pojedynczego zachowanie, umożliwiając kontener usługi zarządzać okresem istnienia usługi jest zalecane. Nie implementuje wzorzec projektowy pojedyncze i podać kod użytkownika do zarządzania okres istnienia obiektu w klasie.

> [!WARNING]
> Niebezpiecznie usługi o określonym zakresie z pojedynczego rozwiązania. Może to spowodować usługi, aby nieprawidłowym stanie podczas przetwarzania kolejnych żądań.

### <a name="constructor-injection-behavior"></a>Zachowanie iniekcji konstruktora

Usługi można rozwiązać przez dwa mechanizmy:

* `IServiceProvider`
* [ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; pozwala na tworzenie obiektów bez rejestracji usługi w kontenera iniekcji zależności. `ActivatorUtilities` jest używana z abstrakcji widocznych dla użytkownika, takich jak pomocnicy tagów, integratorów modeli i kontrolerów MVC.

Konstruktory może akceptować argumenty, które nie są dostarczane przez wstrzykiwanie zależności, ale argumenty należy przypisać wartości domyślne.

Gdy usługi są rozwiązywane przez `IServiceProvider` lub `ActivatorUtilities`, wymaga iniekcji konstruktora *publicznych* konstruktora.

Gdy usługi są rozwiązywane przez `ActivatorUtilities`, iniekcji konstruktora wymaga, że tylko jeden konstruktor dotyczy istnieje. Konstruktor przeciążenia są obsługiwane, ale może istnieć tylko jednego przeciążenia, której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności.

## <a name="entity-framework-contexts"></a>Entity Framework kontekstów

Entity Framework kontekstów powinna być dodana do kontenera usługi przy użyciu zakresu okresu istnienia. Jest to obsługiwane automatycznie przy użyciu wywołania do [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) metody podczas rejestrowania kontekst bazy danych. Usługi, które używa kontekstu bazy danych należy używać również o określonym zakresie okresu istnienia.

## <a name="lifetime-and-registration-options"></a>Opcje okres istnienia i rejestracji

Aby zademonstrować różnica między opcjami okres istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacja o unikatowym identyfikatorze `OperationId`. W zależności od tego, jak okres istnienia usługi operations jest skonfigurowany dla następujących interfejsów kontener zawiera takie same lub inne wystąpienie usługi zleconą przez klasę:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

Interfejsy są implementowane w `Operation` klasy. `Operation` Konstruktor generuje identyfikator GUID, jeśli jeden nie został dostarczony:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

`OperationService` Jest zarejestrowany, zależy od wszystkich innych `Operation` typów. Gdy `OperationService` żądania za pomocą iniekcji zależności odbierze nowe wystąpienie klasy poszczególnych usług albo istniejącego wystąpienia oparte na okres istnienia usług zależnych.

* Jeśli przejściowy usługi są tworzone, gdy żądanie, `OperationId` z `IOperationTransient` usługi jest inny niż `OperationId` z `OperationService`. `OperationService` odbiera nowe wystąpienie klasy `IOperationTransient` klasy. Nowe wystąpienie daje innym `OperationId`.
* Jeśli usługi o określonym zakresie są tworzone na żądanie, `OperationId` z `IOperationScoped` usługi jest taka sama jak w przypadku `OperationService` w żądaniu. Żądań, obie usługi udostępniania innym `OperationId` wartość.
* Jeśli utworzono raz i stosować w przypadku wszystkich żądań i wszystkie usługi singleton i pojedyncze wystąpienie usługi `OperationId` jest stały we wszystkich żądań obsługi.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

W `Startup.ConfigureServices`, każdego typu jest dodawany do kontenera, zgodnie z jego nazwany okres istnienia:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

`IOperationSingletonInstance` Usługa korzysta z określonego wystąpienia o identyfikatorze znanych `Guid.Empty`. Może to być oczywiste, gdy ten typ jest w użyciu (jego identyfikator GUID jest zer).

::: moniker range=">= aspnetcore-2.1"

Przykładowa aplikacja pokazuje czasów istnienia obiektów wewnątrz i pomiędzy poszczególnych żądań. Przykładowa aplikacja `IndexModel` żądań każdego rodzaju `IOperation` typu i `OperationService`. Na stronie zostanie wyświetlony, wszystkie klasy modelu strony i usługi `OperationId` wartości za pomocą przypisania właściwości:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Przykładowa aplikacja pokazuje czasów istnienia obiektów wewnątrz i pomiędzy poszczególnych żądań. Przykładowa aplikacja zawiera `OperationsController` że żądania każdego rodzaju `IOperation` typu i `OperationService`. `Index` Akcji ustawia usług do `ViewBag` do wyświetlenia usługi `OperationId` wartości:

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

Następujące dwa dane wyjściowe wyświetla wyniki dwa żądania:

**Pierwsze żądanie:**

Operacje kontrolera:

Przejściowy: d233e165-f417-469b-a866-1cf1935d2518  
O określonym zakresie: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

`OperationService` operacje:

Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
O określonym zakresie: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

**Drugie żądanie:**

Operacje kontrolera:

Przejściowy: b63bd538-0a37-4ff1-90ba-081c5138dda0  
O określonym zakresie: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

`OperationService` operacje:

Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
O określonym zakresie: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Wystąpienie: 00000000-0000-0000-0000-000000000000

Sprawdź, które `OperationId` wartości różnią się w ramach żądania i między żądaniami:

* *Przejściowy* obiektów zawsze są różne. Należy pamiętać, że przejściowego `OperationId` wartość dla pierwszego i drugiego żądania są różne dla obu `OperationService` operacje wielu żądań. Nowe wystąpienie znajduje się na wszystkie usługi i żądania.
* *Zakres* obiekty są takie same, w żądaniu, ale o różnych żądań.
* *Pojedyncze* obiekty są takie same dla każdego obiektu, a każde żądanie, niezależnie od tego, czy `Operation` wystąpienie znajduje się w `ConfigureServices`.

## <a name="call-services-from-main"></a>Wywoływanie usług z głównego

Tworzenie [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) z [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) rozpoznawanie zakresu usługi w zakresie aplikacji. Takie podejście jest przydatne do dostępu do usługi o określonym zakresie przy uruchamianiu do uruchamiania zadań inicjowania. Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:

```csharp
public static void Main(string[] args)
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

    host.Run();
}
```

## <a name="scope-validation"></a>Weryfikacja zakresu

::: moniker range=">= aspnetcore-2.0"

Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług wykonuje testy, aby sprawdzić, czy:

* Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.
* Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.

::: moniker-end

Dostawcy usług główny jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana. Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.

Usługi o określonym zakresie są usuwane przez kontener, który je utworzył. Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta. Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Żądanie usługi

Usługi dostępne w ramach platformy ASP.NET Core poprosić `HttpContext` są udostępniane za pośrednictwem [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) kolekcji.

Żądanie usługi reprezentują usługi skonfigurowane, a żądane w ramach aplikacji. Gdy obiekty określić zależności, są one spełnione przez typów znalezionych w `RequestServices`, a nie `ApplicationServices`.

Ogólnie rzecz biorąc aplikacja nie należy bezpośrednio używać tych właściwości. Zamiast tego żądania typy, klasy wymagają za pomocą konstruktorów klas i zezwolić na platformę iniekcji zależności. Daje to klasy, które są łatwiejsze do testowania.

> [!NOTE]
> Preferuj żądania z zależności jako parametry konstruktora do uzyskiwania dostępu do `RequestServices` kolekcji.

## <a name="design-services-for-dependency-injection"></a>Projekt usług do wstrzykiwania zależności

Najlepsze rozwiązania są:

* Projektowanie usług na potrzeby uzyskiwania ich zależności iniekcji zależności.
* Należy unikać wywołania metody statycznej, stanowe.
* Należy unikać bezpośredniego wystąpienia klas zależnych w ramach usług. Bezpośrednie utworzenie wystąpienia couples kod do konkretnej implementacji.
* Małe, dobrze uwarunkowaną i łatwo przetestowane, należy utworzyć klasy aplikacji.

Jeśli klasa ma zbyt wiele zależności wprowadzonego, zwykle jest to znak, że klasa ma zbyt wiele obowiązki i narusza [pojedynczej odpowiedzialności zasady (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Próba Refaktoryzuj klasy przez przeniesienie niektórych jego obowiązki do nowej klasy. Należy pamiętać, który stron Razor strony modelu klasy i klas kontrolera MVC należy skoncentrować się na kwestie interfejsu użytkownika. Business reguł oraz danych dostęp do szczegółów implementacji powinny być przechowywane w odpowiednich do tych klas [oddzielić wątpliwości](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Usuwanie usługi

Wywołania kontenera `Dispose` dla `IDisposable` tworzy typy. Jeśli wystąpienie zostanie dodany do kontenera przez kod użytkownika, nie są usuwane automatycznie.

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

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> W programie ASP.NET Core 1.0 kontener wywołuje metodę dispose dla *wszystkich* `IDisposable` obiektów, w tym te nie zostały utworzone.

::: moniker-end

## <a name="default-service-container-replacement"></a>Domyślna usługa kontenera zastąpienia

Kontener Wbudowane usługi jest przeznaczona do potrzebami ramach i większość aplikacji klienta. Zalecamy używanie kontenerze Wbudowane, chyba że potrzebujesz określonych funkcji, która nie jest obsługiwana. Niektóre z funkcji obsługiwanych w 3 kontenerach ze stron nie można odnaleźć w kontenerze Wbudowane:

* Iniekcja właściwości
* Iniekcja na podstawie nazwy
* Kontenery podrzędne
* Zarządzanie okresem istnienia niestandardowe
* `Func<T>` Obsługa inicjowania z opóźnieniem

Zobacz [pliku readme.md wstrzykiwanie zależności](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) listę niektórych kontenerów, które obsługują kart.

Poniższy przykład zastępuje wbudowanych kontenerów za pomocą [Autofac](https://autofac.org/):

* Zainstaluj pakiety odpowiedniego kontenera:

  * [Autofac](https://www.nuget.org/packages/Autofac/)
  * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Konfiguruj kontener w `Startup.ConfigureServices` i zwracają `IServiceProvider`:

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    Do użycia kontener firm 3 `Startup.ConfigureServices` musi zwracać `IServiceProvider`.

* Konfigurowanie Autofac w `DefaultModule`:

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

W czasie wykonywania Autofac umożliwia rozwiązanie typów oraz wstrzykiwania zależności. Aby dowiedzieć się więcej o korzystaniu z Autofac za pomocą programu ASP.NET Core, zobacz [dokumentacji Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Bezpieczeństwo wątków

Usługami Singleton muszą być bezpieczne dla wątków. Jeśli usługi singleton ma zależność od usługi przejściowy, przejściowe usługi również może być konieczne zapewniać bezpieczeństwo wątkowe zależności od sposobu ich wykorzystania przez wzorzec singleton.

Metoda fabryki pojedynczej usługi, takie jak drugi argument [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), nie musi być metodą o bezpiecznych wątkach. Typ, takich jak (`static`) konstruktora, jego ma gwarantuje lze volat pouze jednou przez pojedynczy wątek.

## <a name="recommendations"></a>Zalecenia

* `async/await` i `Task` rozpoznawanie na podstawie usługi nie jest obsługiwane. C# nie obsługuje konstruktorów asynchroniczny, w związku z tym zalecany wzorzec jest użycie metod asynchronicznych po usunięciu synchronicznie usługi.

* Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi. Na przykład koszyka użytkownika zwykle nie należy dodać do kontenera usługi. Należy użyć konfiguracji [wzorzec opcje](xref:fundamentals/configuration/options). Podobnie należy unikać obiektów "symbol zastępczy danych", które istnieją tylko w celu umożliwienia dostępu do innego obiektu. Jest lepszym rozwiązaniem rzeczywisty element za pośrednictwem DI żądania.

* Unikaj statycznych dostęp do usług (na przykład statycznie — wpisanie [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) użytku innym miejscu).

* Unikaj używania *wzorzec lokalizatora usług*. Na przykład nie wywołać <xref:System.IServiceProvider.GetService*> uzyskać wystąpienie usługi, gdy zamiast tego użyj DI. Inna wersja locator service, aby uniknąć wprowadza fabryki, który jest rozpoznawany jako zależności w czasie wykonywania. Oba te rozwiązania mieszanego [Inwersja kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategii.

* Unikaj statycznych dostęp do `HttpContext` (na przykład [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).

Podobnie jak wszystkie zestawy zaleceń mogą wystąpić sytuacje, w których wymagane jest ignorowanie zaleceniem. Wyjątki występują rzadko&mdash;przede wszystkim specjalne przypadki w samej strukturze.

DI jest *alternatywnych* do wzorce dostępu i statyczne/globalne obiektu. Nie można korzystać z zalet DI, jeśli można łączyć z dostępem do obiektu statycznego.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [Pisanie czystego kodu w programie ASP.NET Core przy użyciu iniekcji zależności (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Projekt aplikacji zarządzanych przez kontener, Prelude: Gdzie jest należą kontenera?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Zasada jawne zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Inwersja kontroli kontenerów i wzorzec wstrzykiwanie zależności (Martina Fowlera)](https://www.martinfowler.com/articles/injection.html)
* [Jak zarejestrować usługi z wieloma interfejsami w programie ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
