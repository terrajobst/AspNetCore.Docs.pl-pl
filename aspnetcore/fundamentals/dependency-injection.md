---
title: Wstrzykiwanie zależności w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób ASP.NET Core implementuje iniekcję zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/06/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 27ae8ac979c267c666d6d63f4d1dd862ff20edba
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819861"
---
# <a name="dependency-injection-in-aspnet-core"></a>Wstrzykiwanie zależności w ASP.NET Core

[Steve Kowalski](https://ardalis.com/), [Scott Addie](https://scottaddie.com)i [Luke Latham](https://github.com/guardrex)

ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia niewersji [kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.

Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Przegląd iniekcji zależności

*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt. Zapoznaj się `MyDependency` `WriteMessage` z następującą klasą, korzystając z metody, na której zależą inne klasy w aplikacji:

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

Wystąpienie `MyDependency` klasy można utworzyć w celu `WriteMessage` udostępnienia metody klasie. Klasa jest zależnością `IndexModel`klasy: `MyDependency`

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

Klasa tworzy i bezpośrednio zależy `MyDependency` od wystąpienia. Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:

* Aby zastąpić `MyDependency` inną implementacją, należy zmodyfikować klasę.
* Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę. W dużym projekcie z wieloma klasami `MyDependency`, w zależności od tego, kod konfiguracji jest rozmieszczany w całej aplikacji.
* Ta implementacja jest trudna do testowania jednostkowego. Aplikacja powinna używać klasy imitacji lub zastępczej `MyDependency` , która nie jest możliwa w przypadku tego podejścia.

Iniekcja zależności eliminuje te problemy w następujący sposób:

* Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.
* Rejestracja zależności w kontenerze usługi. ASP.NET Core udostępnia wbudowany kontener usługi, <xref:System.IServiceProvider>. Usługi są zarejestrowane w `Startup.ConfigureServices` metodzie aplikacji.
* *Iniekcja* usługi do konstruktora klasy, w której jest używana. Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.

W [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) `IMyDependency` interfejs definiuje metodę dostarczaną przez usługę do aplikacji:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

Ten interfejs jest implementowany przez konkretny typ `MyDependency`:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

`MyDependency`<xref:Microsoft.Extensions.Logging.ILogger`1> żąda w konstruktorze. Użycie iniekcji zależności w łańcuchu nie jest nietypowe. Każda żądana zależność z kolei żąda własnych zależności. Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi. Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.

`IMyDependency`i `ILogger<TCategoryName>` musi być zarejestrowany w kontenerze usługi. `IMyDependency`jest zarejestrowany w `Startup.ConfigureServices`. `ILogger<TCategoryName>`jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona](#framework-provided-services) przez platformę zarejestrowana domyślnie przez platformę.

Kontener jest rozpoznawany `ILogger<TCategoryName>` przez wykorzystanie [(rodzajowe) otwartych typów](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) konstruowanego typu](/dotnet/csharp/language-reference/language-specification/types#constructed-types):

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana w konkretnym typie `MyDependency`. Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania. [Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> Każda `services.Add{SERVICE_NAME}` Metoda rozszerzenia dodaje usługi (i potencjalnie konfiguruje). Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC. Zalecamy, aby aplikacje były zgodne z tą konwencją. Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.

Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`,, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):

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

W przykładowej aplikacji `IMyDependency` wystąpienie jest wymagane i używane do wywołania `WriteMessage` metody usługi:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a>Usługi udostępniane przez platformę

`Startup.ConfigureServices` Metoda jest odpowiedzialna za Definiowanie usług używanych przez aplikację, w tym funkcji platformy, takich jak Entity Framework Core i ASP.NET Core MVC. Początkowo, `IServiceCollection` `ConfigureServices` pod warunkiem, że zostały zdefiniowane następujące usługi (w zależności od [konfiguracji hosta](xref:fundamentals/index#host)):

| Typ usługi | Okres istnienia |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | Pojedynczego |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Przejściowe |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | Pojedynczego |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | Pojedynczego |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | Pojedynczego |

Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać `Add{SERVICE_NAME}` pojedynczej metody rozszerzającej do rejestrowania wszystkich usług wymaganych przez tę usługę. Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [\<AddDbContext TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>i <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:

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

Aby uzyskać więcej informacji, zapoznaj się z <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> klasą w dokumentacji interfejsu API.

## <a name="service-lifetimes"></a>Okresy istnienia usługi

Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi. Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:

### <a name="transient"></a>Przejściowe

Przejściowe usługi<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>okresu istnienia są tworzone za każdym razem, gdy zażądają one kontenera usług. Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.

### <a name="scoped"></a>Zakresie

Usługi okresu istnienia w<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>zakresie () są tworzone raz dla każdego żądania klienta (połączenie).

> [!WARNING]
> W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do `Invoke` metody `InvokeAsync` lub. Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.

### <a name="singleton"></a>Pojedynczego

Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub gdy `Startup.ConfigureServices` jest uruchamiany, a wystąpienie jest określone przy rejestracji usługi). Każde kolejne żądanie używa tego samego wystąpienia. Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi. Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.

> [!WARNING]
> Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne. Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.

## <a name="service-registration-methods"></a>Metody rejestracji usług

Każda metoda rozszerzenia rejestracji usługi oferuje przeciążenia, które są przydatne w określonych scenariuszach.

| Metoda | Automatyczne<br>object<br>myśl | Wielokrotne<br>implementacje | Przekaż argumenty |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br>Przykład:<br>`services.AddScoped<IMyDep, MyDep>();` | Yes | Yes | Nie |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | Tak | Yes | Tak |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br>Przykład:<br>`services.AddScoped<MyDep>();` | Yes | Nie | Nie |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | Nie | Yes | Yes |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br>Przykłady:<br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | Nie | Nie | Tak |

Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) . Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).

`TryAdd{LIFETIME}`Metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.

W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency`. `IMyDependency` Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` ma już zarejestrowaną implementację:

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

Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*. Wiele usług jest rozpoznawanych `IEnumerable<{SERVICE}>`za pośrednictwem. Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany. Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.

W poniższym przykładzie pierwszy wiersz rejestruje `MyDep`. `IMyDep1` Drugi wiersz rejestruje `MyDep`. `IMyDep2` Trzeci wiersz nie działa, ponieważ `IMyDep1` ma już zarejestrowana `MyDep`implementacja:

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
* <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>&ndash; Zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności. `ActivatorUtilities`jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modelu.

Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.

Gdy usługi są rozwiązane `IServiceProvider` przez `ActivatorUtilities`lub, iniekcja konstruktora wymaga konstruktora *publicznego* .

Gdy usługi są rozwiązane `ActivatorUtilities`przez, iniekcja konstruktora wymaga, aby istnieje tylko jeden odpowiedni Konstruktor. Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.

## <a name="entity-framework-contexts"></a>Konteksty Entity Framework

Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta. Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez Przeciążenie [\<AddDbContext TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Overload podczas rejestrowania kontekstu bazy danych. Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.

## <a name="lifetime-and-registration-options"></a>Opcje okresu istnienia i rejestracji

Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje `OperationId`z unikatowym identyfikatorem. W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

Interfejsy są zaimplementowane w `Operation` klasie. `Operation` Konstruktor generuje identyfikator GUID, jeśli nie został podany:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

Zarejestrowano, który zależy od poszczególnych `Operation` typów. `OperationService` Gdy `OperationService` żądanie jest wykonywane za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.

* Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` `IOperationTransient` `OperationId` usługa różni się od `OperationService`. `OperationService`odbiera nowe wystąpienie `IOperationTransient` klasy. Nowe wystąpienie daje inną `OperationId`wartość.
* Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` `IOperationScoped` usługa jest taka sama jak `OperationService` w ramach żądania klienta. W przypadku żądań klientów obie te usługi współdzielą inną `OperationId` wartość.
* Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane przez wszystkie żądania klientów i wszystkie usługi, `OperationId` jest to stała między wszystkimi żądaniami obsługi.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

W `Startup.ConfigureServices`programie każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

Usługa używa określonego wystąpienia o znanym `Guid.Empty`identyfikatorze. `IOperationSingletonInstance` Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).

Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi. Przykładowa aplikacja `IndexModel` wysyła żądania każdego `IOperation` rodzaju typu i `OperationService`. Następnie na stronie zostaną wyświetlone wszystkie `OperationId` wartości klasy modelu strony i usługi za pomocą przypisań właściwości:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:

**Pierwsze żądanie:**

Operacje kontrolera:

Przejściowy: d233e165-f417-469B-A866-1cf1935d2518  
Zakresie 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Np 00000000-0000-0000-0000-000000000000

`OperationService`składowa

Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Zakresie 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Np 00000000-0000-0000-0000-000000000000

**Drugie żądanie:**

Operacje kontrolera:

Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0  
Zakresie 31e820c5-4834-4d22-83fc-a60118acb9f4  
Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Np 00000000-0000-0000-0000-000000000000

`OperationService`składowa

Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Zakresie 31e820c5-4834-4d22-83fc-a60118acb9f4  
Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Np 00000000-0000-0000-0000-000000000000

Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:

* Obiekty przejściowe są zawsze różne. Wartość przejściowa `OperationId` dla pierwszego i drugiego żądania klienta różni się `OperationService` w zależności od operacji i między żądaniami klientów. Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.
* Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.
* *Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, `Operation` czy wystąpienie jest dostarczone `Startup.ConfigureServices`w.

## <a name="call-services-from-main"></a>Wywoływanie usług z głównego

Utwórz element <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory. isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji. Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania. Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService`: `Program.Main`

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

Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług sprawdza, czy:

* Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.
* Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.

Dostawca usług głównych jest tworzony, gdy <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> jest wywoływany. Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.

Usługi o określonym zakresie są usuwane przez kontener, który go utworzył. Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony. Sprawdzanie poprawności zakresów usług przechwytuje te `BuildServiceProvider` sytuacje, gdy jest wywoływana.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Usługi żądania

Usługi dostępne w ramach żądania `HttpContext` ASP.NET Core są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .

Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji. Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, nie. `ApplicationServices`

Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio. Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę. To daje klasy, które są łatwiejsze do przetestowania.

> [!NOTE]
> Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp `RequestServices` do kolekcji.

## <a name="design-services-for-dependency-injection"></a>Projektowanie usług do iniekcji zależności

Najlepsze rozwiązania:

* Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.
* Unikaj stanowych wywołań metod statycznych.
* Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług. Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.
* Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.

Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy. Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika. Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Usuwanie usług

Kontener wywołuje <xref:System.IDisposable.Dispose*> typy, które <xref:System.IDisposable> tworzy. Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.

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

Wbudowany kontener usługi jest przeznaczony do obsługi potrzeb platformy i większości aplikacji konsumenckich. Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, której nie obsługuje. Niektóre funkcje obsługiwane w kontenerach innych firm nie znajdują się w kontenerze wbudowanym:

* Iniekcja właściwości
* Iniekcja oparta na nazwie
* Kontenery podrzędne
* Niestandardowe zarządzanie okresem istnienia
* `Func<T>`Obsługa inicjowania z opóźnieniem

Zobacz [plik Readme.MD iniekcji zależności](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) , aby uzyskać listę niektórych kontenerów, które obsługują karty sieciowe.

Poniższy przykład zastępuje wbudowany kontener z [Autofac](https://autofac.org/):

* Zainstaluj odpowiednie pakiety kontenerów:

  * [Autofac](https://www.nuget.org/packages/Autofac/)
  * [Autofac. Extensions. DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Skonfigurowanie kontenera w programie `Startup.ConfigureServices` i `IServiceProvider`zwrócenie:

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

    Aby użyć kontenera innej firmy, `Startup.ConfigureServices` należy zwrócić `IServiceProvider`wartość.

* Skonfiguruj Autofac w `DefaultModule`:

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

W czasie wykonywania Autofac jest używany do rozpoznawania typów i wtryskiwania zależności. Aby dowiedzieć się więcej o korzystaniu z programu Autofac z ASP.NET Core, zobacz [dokumentację Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Bezpieczeństwo wątków

Twórz bezpieczne dla wątków usługi pojedyncze. Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.

Metoda fabryki pojedynczej usługi, taka jak drugi argument dla AddSingleton [\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo. Podobnie jak w przypadku`static`konstruktora typu (), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.

## <a name="recommendations"></a>Zalecenia

* `async/await`Rozpoznawanie `Task` usług na podstawie nie jest obsługiwane. C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.

* Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi. Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi. Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options). Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu. Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.

* Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).

* Unikaj używania *wzorca lokalizatora usługi*. Na przykład nie wywołuj <xref:System.IServiceProvider.GetService*> , aby uzyskać wystąpienie usługi, gdy można użyć di zamiast:

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

* Inna odmiana lokalizatora usługi, aby uniknąć, wprowadza fabrykę, która rozwiązuje zależności w czasie wykonywania. Obie te praktyki mieszają się z niewersjami strategii [kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .

* Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).

Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane. Wyjątki są rzadko&mdash;w większości specjalnych przypadków w ramach samej struktury.

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
