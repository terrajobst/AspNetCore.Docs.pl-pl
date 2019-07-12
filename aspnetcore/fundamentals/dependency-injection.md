---
title: Wstrzykiwanie zależności w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak platformy ASP.NET Core implementuje wstrzykiwanie zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 9293de38dcca1c0672f9cc3defa8d3c1b0b13d5a
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855897"
---
# <a name="dependency-injection-in-aspnet-core"></a>Wstrzykiwanie zależności w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), i [Luke Latham](https://github.com/guardrex)

Platforma ASP.NET Core obsługuje zależności wzorzec projektowy oprogramowania iniekcji (DI), czyli technikę do osiągnięcia [Inwersja kontroli (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależności.

Aby uzyskać więcej informacji specyficznych dla wstrzykiwanie zależności w ramach kontrolerów MVC, zobacz <xref:mvc/controllers/dependency-injection>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

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

Klasa tworzy i zależy od bezpośrednio `MyDependency` wystąpienia. Zależności w kodzie (na przykład w poprzednim przykładzie) są problematyczne i należy unikać z następujących powodów:

* Aby zastąpić `MyDependency` z inną implementacją klasy muszą zostać zmodyfikowane.
* Jeśli `MyDependency` ma zależności, musi być skonfigurowany przez klasę. W dużym projekcie z wieloma klasami w zależności od `MyDependency`, kod konfiguracji staje się znajdują się na aplikację.
* Ta implementacja jest trudny do testów jednostkowych. Aplikację należy użyć pozorny lub namiastki `MyDependency` klasy, która nie jest możliwe w przypadku tej metody.

Wstrzykiwanie zależności rozwiązuje te problemy za pomocą:

* Użycie interfejsu lub klasy bazowej tworzących warstwę abstrakcji implementacji zależności.
* Rejestracja zależności w kontenerze usługi. Platforma ASP.NET Core zapewnia kontener wbudowanej usługi <xref:System.IServiceProvider>. Usługi są zarejestrowane w usłudze aplikacji `Startup.ConfigureServices` metody.
* *Iniekcja* usługi do konstruktora klasy, w których jest używany. Struktura przejmuje odpowiedzialność za tworzenie wystąpienia zależności i usuwania je, gdy nie jest już potrzebny.

W [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` interfejs definiuje metodę, która udostępnia usługę do aplikacji:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

Ten interfejs jest implementowany przez konkretny typ `MyDependency`:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

`MyDependency` żądania <xref:Microsoft.Extensions.Logging.ILogger`1> w jego konstruktorze. Nie jest niczym niezwykłym używać wstrzykiwanie zależności w sposób połączonych. Poszczególne zależności żądanego żądań z kolei swoje własne zależności. Jest rozpoznawana jako zależności na wykresie i zwraca w pełni rozpoznać usługę kontenera. Zbiorczy zestaw zależności, które muszą być rozwiązane jest zwykle nazywany *drzewo zależności*, *wykres zależności*, lub *wykresu obiektu*.

`IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowana w kontenerze usługi. `IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`. `ILogger<TCategoryName>` jest on zarejestrowany infrastruktury abstrakcje rejestrowanie, dlatego ma [usługi dostarczane przez framework](#framework-provided-services) zarejestrowana domyślnie przez platformę.

Kontener jest rozpoznawana jako `ILogger<TCategoryName>` , wykorzystując [typy (Ogólne) otwórz](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność łączenia się zarejestrować co [(ogólny) skonstruowany typ](/dotnet/csharp/language-reference/language-specification/types#constructed-types):

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana przy użyciu konkretnego typu `MyDependency`. Rejestracja zakresów okres istnienia usługi przez cały czas trwania pojedynczego żądania. [Okresy istnienia usługi](#service-lifetimes) są opisane w dalszej części tego tematu.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> Każdy `services.Add{SERVICE_NAME}` — metoda rozszerzenia dodaje (i potencjalnie konfiguruje) usługi. Na przykład `services.AddMvc()` dodaje usług, stronami Razor i wymagają MVC. Zaleca się, że aplikacje stosują taką Konwencję. Metody rozszerzające w miejscu [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) przestrzeni nazw w celu hermetyzacji grupy rejestracji usługi.

Jeśli Konstruktor usługi wymaga [typ wbudowany](/dotnet/csharp/language-reference/keywords/built-in-types-table), takich jak `string`, typ może wprowadzone za pomocą [konfiguracji](xref:fundamentals/configuration/index) lub [wzorzec opcje](xref:fundamentals/configuration/options):

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

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a>Dostarczone do struktury usługi

`Startup.ConfigureServices` Metoda jest odpowiedzialna definiowanie usługi, którego korzysta aplikacja, łącznie z funkcjami platformy, takie jak Entity Framework Core i ASP.NET Core MVC. Początkowo `IServiceCollection` udostępniane `ConfigureServices` ma następujące usługi, które są zdefiniowane (w zależności od [konfiguracji hosta](xref:fundamentals/index#host)):

| Typ usługi | Okres istnienia |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | pojedyncze |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | pojedyncze |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | pojedyncze |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | pojedyncze |

Po udostępnieniu register a service (i jej usługi zależne, jeśli jest to wymagane) metody rozszerzenia kolekcji usługi Konwencji jest użycie pojedynczego `Add{SERVICE_NAME}` metodę rozszerzenia, aby zarejestrować wszystkich usług wymaganych przez tę usługę. Poniższy kod jest przykładem sposobu dodawania dodatkowych usług do kontenera przy użyciu metody rozszerzenia [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, i <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:

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

Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> klasy w dokumentacji interfejsu API.

## <a name="service-lifetimes"></a>Okresy istnienia usługi

Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi. Usługi ASP.NET Core mogą być skonfigurowane przy użyciu następujących okresów istnienia:

### <a name="transient"></a>Przejściowe

Usługi przejściowych okres istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) są tworzone za każdym razem, zleconej z kontenera usług. Ten okres istnienia najlepiej uproszczone, bezstanowych usług.

### <a name="scoped"></a>O określonym zakresie

Zakres usług okres istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) są tworzone w jeden raz dla każdego żądania klienta (połączenie).

> [!WARNING]
> Korzystając z usługi o określonym zakresie w oprogramowaniu pośredniczącym, wprowadzić usługę do `Invoke` lub `InvokeAsync` metody. Nie wstrzyknąć przy użyciu iniekcji konstruktora, ponieważ wymusza usługę, aby zachowywać się jak wzorzec singleton. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.

### <a name="singleton"></a>pojedyncze

Pojedyncze okres istnienia usługi (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) są tworzone po raz pierwszy masz żądanej (lub gdy `Startup.ConfigureServices` jest uruchamiany i wystąpienie jest określony za pomocą rejestracji usługi). Każde kolejne żądanie używa tego samego wystąpienia. Jeśli aplikacja wymaga pojedynczego zachowanie, umożliwiając kontener usługi zarządzać okresem istnienia usługi jest zalecane. Nie implementuje wzorzec projektowy pojedyncze i podać kod użytkownika do zarządzania okres istnienia obiektu w klasie.

> [!WARNING]
> Niebezpiecznie usługi o określonym zakresie z pojedynczego rozwiązania. Może to spowodować usługi, aby nieprawidłowym stanie podczas przetwarzania kolejnych żądań.

## <a name="service-registration-methods"></a>Metody rejestracji usługi

Każda metoda rozszerzenia rejestracji usługa oferuje przeciążenia, które są przydatne w określonych scenariuszach.

| Metoda | Automatyczne<br>object<br>likwidacji | Wielokrotne<br>implementacje | Przekazywanie argumentów |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br>Przykład:<br>`services.AddScoped<IMyDep, MyDep>();` | Yes | Yes | Nie |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | Yes | Yes | Yes |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br>Przykład:<br>`services.AddScoped<MyDep>();` | Tak | Nie | Nie |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | Nie | Yes | Tak |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | Nie | Nie | Yes |

Aby uzyskać więcej informacji na temat usuwania typów, zobacz [usuwania usług](#disposal-of-services) sekcji. Jest to typowy scenariusz, w wielu implementacjach [pozorowanie typów testowych](xref:test/integration-tests#inject-mock-services).

`TryAdd{LIFETIME}` metody zarejestrować usługę tylko, jeśli go nie ma już implementację zarejestrowany.

W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency` dla `IMyDependency`. Drugi wiersz nie obowiązuje, ponieważ `IMyDependency` ma już zarejestrowanej implementacji:

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

[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) Jeśli go nie ma już implementację metody tylko zarejestrować usługę *tego samego typu*. Wiele usług są rozwiązywane za pośrednictwem `IEnumerable<{SERVICE}>`. Podczas rejestrowania usług, deweloper tylko chce, aby dodać wystąpienie, jeśli jeden z tego samego typu nie został już dodany. Ogólnie rzecz biorąc ta metoda jest używana przez autorów biblioteki w celu uniknięcia rejestrowanie dwie kopie wystąpienia w kontenerze.

W poniższym przykładzie pierwszy wiersz rejestruje `MyDep` dla `IMyDep1`. Drugi wiersz rejestruje `MyDep` dla `IMyDep2`. Trzeci wiersz nie obowiązuje, ponieważ `IMyDep1` ma już zarejestrowanej implementacji `MyDep`:

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

Usługi można rozwiązać przez dwa mechanizmy:

* <xref:System.IServiceProvider>
* <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Pozwala na tworzenie obiektów bez rejestracji usługi w kontenera iniekcji zależności. `ActivatorUtilities` jest używana z abstrakcji widocznych dla użytkownika, takich jak pomocnicy tagów, integratorów modeli i kontrolerów MVC.

Konstruktory może akceptować argumenty, które nie są dostarczane przez wstrzykiwanie zależności, ale argumenty należy przypisać wartości domyślne.

Gdy usługi są rozwiązywane przez `IServiceProvider` lub `ActivatorUtilities`, wymaga iniekcji konstruktora *publicznych* konstruktora.

Gdy usługi są rozwiązywane przez `ActivatorUtilities`, iniekcji konstruktora wymaga, że tylko jeden konstruktor dotyczy istnieje. Konstruktor przeciążenia są obsługiwane, ale może istnieć tylko jednego przeciążenia, której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności.

## <a name="entity-framework-contexts"></a>Entity Framework kontekstów

Entity Framework kontekstów są zwykle dodawane do usługi kontenera przy użyciu [o określonym zakresie okres istnienia](#service-lifetimes) ponieważ operacji bazy danych w aplikacji sieci web są zazwyczaj ograniczone do żądania klienta. Domyślny okres istnienia jest zakresem, jeśli okres istnienia nie jest określony przez [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) przeciążenia podczas rejestrowania kontekst bazy danych. Usługi danego okresu istnienia nie używaj kontekstu bazy danych o okresie istnienia krótszy niż usługi.

## <a name="lifetime-and-registration-options"></a>Opcje okres istnienia i rejestracji

Aby zademonstrować różnica między opcjami okres istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacja o unikatowym identyfikatorze `OperationId`. W zależności od tego, jak okres istnienia usługi operations jest skonfigurowany dla następujących interfejsów kontener zawiera takie same lub inne wystąpienie usługi zleconą przez klasę:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

Interfejsy są implementowane w `Operation` klasy. `Operation` Konstruktor generuje identyfikator GUID, jeśli jeden nie został dostarczony:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

`OperationService` Jest zarejestrowany, zależy od wszystkich innych `Operation` typów. Gdy `OperationService` żądania za pomocą iniekcji zależności odbierze nowe wystąpienie klasy poszczególnych usług albo istniejącego wystąpienia oparte na okres istnienia usług zależnych.

* Podczas tworzenia usługi przejściowy zleconą z kontenera, `OperationId` z `IOperationTransient` usługi jest inny niż `OperationId` z `OperationService`. `OperationService` odbiera nowe wystąpienie klasy `IOperationTransient` klasy. Nowe wystąpienie daje innym `OperationId`.
* Podczas tworzenia usługi o określonym zakresie dla każdego żądania klienta `OperationId` z `IOperationScoped` usługi jest taka sama jak w przypadku `OperationService` w żądaniu klienta. Na żądania klientów obie te usługi udostępniania innym `OperationId` wartość.
* W przypadku pojedynczego wystąpienia i pojedyncze wystąpienie usługi są tworzone po i stosować w przypadku wszystkich żądań klientów i wszystkich usług `OperationId` jest stały we wszystkich żądań obsługi.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

W `Startup.ConfigureServices`, każdego typu jest dodawany do kontenera, zgodnie z jego nazwany okres istnienia:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

`IOperationSingletonInstance` Usługa korzysta z określonego wystąpienia o identyfikatorze znanych `Guid.Empty`. Może to być oczywiste, gdy ten typ jest w użyciu (jego identyfikator GUID jest zer).

Przykładowa aplikacja pokazuje czasów istnienia obiektów wewnątrz i pomiędzy poszczególnych żądań. Przykładowa aplikacja `IndexModel` żądań każdego rodzaju `IOperation` typu i `OperationService`. Na stronie zostanie wyświetlony, wszystkie klasy modelu strony i usługi `OperationId` wartości za pomocą przypisania właściwości:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

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

* *Przejściowy* obiektów zawsze są różne. Przejściowy `OperationId` wartość pierwszego i drugiego klient żąda różnią się w obu `OperationService` operacje wielu żądań klientów. Nowe wystąpienie znajduje się do każdego żądania obsługi i żądanie klienta.
* *Zakres* obiekty są takie same, w ramach żądania klienta, ale o różnych żądań klienta.
* *Pojedyncze* obiekty są takie same dla każdego obiektu, a każde żądanie, niezależnie od tego, czy `Operation` wystąpienie znajduje się w `Startup.ConfigureServices`.

## <a name="call-services-from-main"></a>Wywoływanie usług z głównego

Tworzenie <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> z [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) rozpoznawanie zakresu usługi w zakresie aplikacji. Takie podejście jest przydatne do dostępu do usługi o określonym zakresie przy uruchamianiu do uruchamiania zadań inicjowania. Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:

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

Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług wykonuje testy, aby sprawdzić, czy:

* Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.
* Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.

Dostawcy usług główny jest tworzone, gdy <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> jest wywoływana. Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.

Usługi o określonym zakresie są usuwane przez kontener, który je utworzył. Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta. Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Żądanie usługi

Usługi dostępne w ramach platformy ASP.NET Core poprosić `HttpContext` są udostępniane za pośrednictwem [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) kolekcji.

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

Wywołania kontenera <xref:System.IDisposable.Dispose*> dla <xref:System.IDisposable> tworzy typy. Jeśli wystąpienie zostanie dodany do kontenera przez kod użytkownika, nie są usuwane automatycznie.

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

Tworzenie usługi singleton metodą o bezpiecznych wątkach. Usługi singleton ma zależność od usługi przejściowy, przejściowe usługa może również wymagać bezpieczeństwo wątków, w zależności od sposobu ich wykorzystania przez wzorzec singleton.

Metoda fabryki pojedynczej usługi, takie jak drugi argument [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być metodą o bezpiecznych wątkach. Typ, takich jak (`static`) konstruktora, jego ma gwarantuje lze volat pouze jednou przez pojedynczy wątek.

## <a name="recommendations"></a>Zalecenia

* `async/await` i `Task` rozpoznawanie na podstawie usługi nie jest obsługiwane. C#nie obsługuje konstruktorów asynchronicznego; w związku z tym zalecany wzorzec jest użycie metod asynchronicznych po usunięciu synchronicznie usługi.

* Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi. Na przykład koszyka użytkownika zwykle nie należy dodać do kontenera usługi. Należy użyć konfiguracji [wzorzec opcje](xref:fundamentals/configuration/options). Podobnie należy unikać obiektów "symbol zastępczy danych", które istnieją tylko w celu umożliwienia dostępu do innego obiektu. Jest lepszym rozwiązaniem rzeczywisty element za pośrednictwem DI żądania.

* Unikaj statycznych dostęp do usług (na przykład statycznie — wpisanie [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) użytku innym miejscu).

* Unikaj używania *wzorzec lokalizatora usług*. Na przykład nie wywołać <xref:System.IServiceProvider.GetService*> uzyskać wystąpienie usługi, gdy zamiast tego użyj DI:

  **Niepoprawne:**

  ```csharp
  public void MyMethod()
  {
      var options = 
          _services.GetService<IOptionsMonitor<MyOptions>>();
      var option = options.CurrentValue.Option;

      ...
  }
  ```

  **Poprawne**:

  ```csharp
  private readonly MyOptions _options;

  public MyClass(IOptionsMonitor<MyOptions> options)
  {
      _options = options.CurrentValue;
  }

  public void MyMethod()
  {
      var option = _options.Option;

      ...
  }
  ```

* Inna wersja locator service, aby uniknąć wprowadza fabryki, który jest rozpoznawany jako zależności w czasie wykonywania. Oba te rozwiązania mieszanego [Inwersja kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategii.

* Unikaj statycznych dostęp do `HttpContext` (na przykład [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).

Podobnie jak wszystkie zestawy zaleceń mogą wystąpić sytuacje, w których wymagane jest ignorowanie zaleceniem. Wyjątki występują rzadko&mdash;przede wszystkim specjalne przypadki w samej strukturze.

DI jest *alternatywnych* do wzorce dostępu i statyczne/globalne obiektu. Nie można korzystać z zalet DI, jeśli można łączyć z dostępem do obiektu statycznego.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [Pisanie czystego kodu w programie ASP.NET Core przy użyciu iniekcji zależności (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Zasada jawne zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Inwersja kontroli kontenerów i wzorzec wstrzykiwanie zależności (Martina Fowlera)](https://www.martinfowler.com/articles/injection.html)
* [Jak zarejestrować usługi z wieloma interfejsami w programie ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
