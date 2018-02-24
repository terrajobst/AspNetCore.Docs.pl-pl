---
title: "Iniekcji zależności w ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak platformy ASP.NET Core implementuje iniekcji zależności i jak z niego korzystać."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 85e25b92b01d84279752deb7865987746c181c72
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Iniekcji zależności w ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)

Platformy ASP.NET Core zaprojektowano od podstaw w górę do obsługi i korzystać z iniekcji zależności. Platformy ASP.NET Core aplikacje mogą korzystać z usług wbudowana struktura, konfigurując je do metod w klasie uruchamiania, a także iniekcji można skonfigurować usługi aplikacji. Domyślny kontener usługi udostępniane przez platformy ASP.NET Core zawiera minimalnego funkcji ustawiony i nie jest przeznaczony do zastąpić inne kontenery.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Co to jest iniekcji zależności?

Iniekcji zależności (Podpisane) to technika służące osiągnięciu luźne powiązanie między obiektów i ich współpracownicy lub zależności. Zamiast bezpośrednio uruchamianiu współpracownicy lub przy użyciu statycznych odwołań obiektów klasy wymaga, aby jego akcje są przekazywane do klasy, w dowolny sposób. W większości przypadków deklaracją klasy ich zależności za pomocą ich konstruktora, dzięki czemu mogą wykonać [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/). Takie podejście jest określany jako "iniekcji konstruktora".

Gdy klasy zostały zaprojektowane z Podpisane pamiętać, są one więcej luźno powiązane ponieważ nie mają bezpośredniego, ustalony zależności w swoich współpracowników. Wynika to [zasady odwracanie zależności](http://deviq.com/dependency-inversion-principle/), które stwierdza, że *"wysokiego poziomu moduły nie są zależne od niskiego poziomu modułów; powinien zależą od obiektów abstrakcyjnych".* Zamiast odwołania do określonej implementacji, klas żądania obiektów abstrakcyjnych (zazwyczaj `interfaces`) które są dostarczane do nich, gdy jest utworzony w klasie. Wyodrębnianie zależności do interfejsów i zapewnianie implementacje te interfejsy jako parametry jest również przykład [wzorzec projektowania strategii](http://deviq.com/strategy-design-pattern/).

Gdy system jest przeznaczony do użycia Podpisane, z wielu klas żąda ich zależności za pośrednictwem ich konstruktora (lub właściwości), warto mieć klasy dedykowane do tworzenia tych klas z ich skojarzone zależności. Tych klas są określane jako *kontenery*, lub w szczególności [Inwersja kontroli (IoC)](http://deviq.com/inversion-of-control/) kontenery lub zależności Iniekcja kontenerów. Kontener jest zasadniczo fabrykę, który jest odpowiedzialny za zapewnienie wystąpienia typów, których wymagane są od niego. Podany typ został zadeklarowany, zawiera on zależności, czy kontener został skonfigurowany, aby zapewnić typów zależności, utworzy zależności w ramach tworzenia żądanego wystąpienia. W ten sposób można podać wykresy złożonych zależności do klasy bez konieczności stosowania żadnych konstrukcji obiektów ustalony. Oprócz tworzenia obiektów z ich zależności, kontenery Zarządzanie zwykle okresy istnienia obiektu w aplikacji.

Platformy ASP.NET Core zawiera proste kontenera wbudowanych (reprezentowane przez `IServiceProvider` interfejsu) obsługującej iniekcji konstruktora domyślnie i program ASP.NET udostępnia pewnych usług za pośrednictwem Podpisane. ŚRODOWISKO ASP. Kontener dla NET odwołuje się do typów zarządza jako *usług*. W dalszej części tego artykułu *usług* odnoszą się do typów, które są zarządzane przez program ASP.NET Core Inwersja kontroli kontenera. Konfigurowanie usługi kontenera wbudowanych w `ConfigureServices` metody w aplikacji `Startup` klasy.

> [!NOTE]
> Pole Fowler został zapisany szeroką gamę artykułu na [Inwersja kontroli kontenerów i wzorzec iniekcji zależności](https://www.martinfowler.com/articles/injection.html). Microsoft Patterns and Practices ma również opis dużą [iniekcji zależności](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> W tym artykule omówiono iniekcji zależności mają zastosowanie do wszystkich aplikacji ASP.NET. Iniekcji zależności w ramach kontrolerów MVC jest objęte [iniekcji zależności i kontrolery](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Zachowanie iniekcji — Konstruktor

Konstruktor iniekcji wymaga, aby w Konstruktorze *publicznego*. W przeciwnym razie zgłosi aplikacji `InvalidOperationException`:

> Nie można odnaleźć odpowiedniego konstruktora dla typu "YourType". Upewnij się, typ jest konkretnych i usług są zarejestrowane dla wszystkich parametrów konstruktora publicznego.


Konstruktor iniekcji wymaga tego tylko jeden konstruktor dotyczy istnieje. Przeciążenia konstruktora są obsługiwane, ale tylko jedno przeciążenie może istnieć, którego argumenty mogą wszystkie spełniać iniekcji zależności. Jeśli istnieje więcej niż jeden, aplikacja będzie zgłaszać wyjątek `InvalidOperationException`:

> Znaleziono wiele konstruktorów akceptuje wszystkie typy podany argument typu "YourType". Powinien istnieć tylko jeden konstruktor zastosowania.

Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcji zależności, ale muszą one obsługuje przekazywania wartości domyślnych. Na przykład:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Za pomocą usług dostarczonych framework

`ConfigureServices` Metoda `Startup` klasy jest odpowiedzialny za definiowanie usługi aplikacja będzie korzystać, w tym funkcji platformy Entity Framework Core i ASP.NET Core MVC. Początkowo `IServiceCollection` do `ConfigureServices` ma następujące usługi zdefiniowane (w zależności od [konfiguracji hosta](xref:fundamentals/hosting)):

| Typ usługi | Okres istnienia |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | pojedyncze |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | pojedyncze |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Przejściowy |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Przejściowy |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | pojedyncze |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | pojedyncze |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Przejściowy |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | pojedyncze |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Przejściowy |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | pojedyncze |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | pojedyncze |

Poniżej znajduje się przykład sposobu dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzenia takich jak `AddDbContext`, `AddIdentity`, i `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Funkcje i oprogramowanie pośredniczące dostarczane przez platformę ASP.NET, takich jak MVC, wykonaj Konwencji przy użyciu pojedynczego Dodaj*ServiceName* — metoda rozszerzenia zarejestrować wszystkich usług wymaganych przez tej funkcji.

>[!TIP]
> Możesz poprosić o pewnych usług dostarczonych framework w `Startup` Zobacz metody za pomocą ich listy parametrów - [uruchamiania aplikacji](startup.md) więcej szczegółów.

## <a name="registering-services"></a>Rejestrowanie usługi

Usługi aplikacji można zarejestrować w następujący sposób. Pierwszy ogólny typ reprezentuje typ (zazwyczaj interfejs) żądany z kontenera. Drugi typ ogólny reprezentuje typu konkretnego, który zostanie uruchomiony przez kontener i będzie używana do spełnienia takich żądań.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Każdy `services.Add<ServiceName>` — metoda rozszerzenia dodaje (i potencjalnie konfiguruje) usługi. Na przykład `services.AddMvc()` dodaje usług wymaga MVC. Zalecane jest, wykonaj tę Konwencję, umieszczenia metod rozszerzenia w `Microsoft.Extensions.DependencyInjection` przestrzeni nazw w celu hermetyzacji grup rejestracji usługi.

`AddTransient` Metoda służy do mapowania typów abstrakcyjnych do konkretnych usług, które są tworzone oddzielnie dla każdego obiektu, który wymaga. Jest to nazywane usługi *okres istnienia*, a okres istnienia dodatkowe opcje zostały opisane poniżej. Należy wybrać odpowiedni istnienia dla każdej usługi, które należy zarejestrować. Należy podać nowe wystąpienie usługi do każdej klasy, która zażąda tego? Należy użyć jednego wystąpienia w całym żądania sieci web danego? Czy należy używać pojedynczego wystąpienia dla okresu istnienia aplikacji?

W przykładowym tego artykułu jest proste kontroler, który wyświetla znak nazwy, o nazwie `CharactersController`. Jego `Index` metoda Wyświetla bieżącą listę znaków, które były przechowywane w aplikacji i inicjuje połączenie z kilku znaków, jeśli nie ma żadnego. Należy pamiętać, że chociaż Entity Framework Core korzysta z tej aplikacji i `ApplicationDbContext` klasy jego trwałości, żaden z jest widoczna w kontrolerze. Zamiast tego mechanizmu dostępu określonych danych ma zostały usunięte za interfejsu `ICharacterRepository`, który następuje [wzorca repozytorium](http://deviq.com/repository-pattern/). Wystąpienie `ICharacterRepository` za pośrednictwem konstruktora i ma przypisaną do prywatnego pola, które są następnie używane do dostęp do znaków w razie potrzeby.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` Definiuje dwie metody kontrolera musi pracować `Character` wystąpień.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Ten interfejs z kolei jest implementowany przez typem konkretnym `CharacterRepository`, który jest używany w czasie wykonywania.

> [!NOTE]
> Sposób Podpisane jest używany z `CharacterRepository` klasy jest ogólny model można wykonać dla wszystkich usług aplikacji, nie tylko w "repozytoria" lub klas dostępu do danych.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Należy pamiętać, że `CharacterRepository` żądań `ApplicationDbContext` w jego konstruktora. Nie jest rzadko iniekcji zależności do użycia w sposób łańcuchowa podobny do tego, z każdej żądanej zależności z kolei żąda własnych zależności. Kontener jest odpowiedzialny za rozwiązywanie wszystkich zależności na wykresie i zwracanie całkowicie rozwiązany usługi.

> [!NOTE]
> Tworzenie żądanego obiektu i wszystkich obiektów wymaga i wszystkie obiekty te wymagają, jest czasami nazywany *wykres obiektu*. Podobnie, zbiorczy zestaw zależności, które muszą zostać rozstrzygnięte jest zwykle nazywany *drzewo zależności* lub *wykresu zależności*.

W takim przypadku zarówno `ICharacterRepository` i z kolei `ApplicationDbContext` musi być zarejestrowana w kontenerze usług w `ConfigureServices` w `Startup`. `ApplicationDbContext` skonfigurowano wywołanie metody rozszerzenia `AddDbContext<T>`. Poniższy kod przedstawia rejestracji `CharacterRepository` typu.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework kontekstów należy dodać do kontenera usług przy użyciu `Scoped` okres istnienia. To jest poświęcony na obsługę automatycznie Jeśli używasz metody pomocnicze, jak pokazano powyżej. Repozytoria, które korzystają z programu Entity Framework należy używać tego samego okresu istnienia.

>[!WARNING]
> Rozpoznaje główne zagrożenia dla Uważaj `Scoped` usługi z klasą pojedynczą. Prawdopodobnie w takim przypadku usługa ma niepoprawny stan podczas przetwarzania kolejnych żądań.

Usługi z zależnościami, należy zarejestrować je w kontenerze. Jeśli usługa Konstruktor wymaga typu pierwotnego, takich jak `string`, to mogą zostać dodane za pomocą [konfiguracji](xref:fundamentals/configuration/index) i [wzorzec opcje](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Opcje rejestracji i okresy istnienia usługi

Usługi ASP.NET można skonfigurować za pomocą następujących okresów:

Przejściowy

Przejściowa istnienia usług są tworzone zawsze, gdy są one wymagane. Ten okres istnienia jest najlepsza dla lekkich usług bezstanowych.

**Zakres**

Okres istnienia w zakresie usług są tworzone raz na każde żądanie.

pojedyncze

Pojedyncze okres istnienia usług są tworzone po raz pierwszy jest żądanej (lub gdy `ConfigureServices` jest uruchamiany, jeśli określone wystąpienie) i wszystkie kolejne żądania będą następnie używa tego samego wystąpienia. Jeśli aplikacja wymaga zachowania singleton, umożliwiając kontener usług do zarządzania istnienia usługi jest zalecane zamiast wzorca projektowego singleton wdrażanie i zarządzanie nim okres istnienia z obiektów w klasie samodzielnie.

Usługi mogą być rejestrowane w kontenerze na kilka sposobów. Już widzieliśmy rejestrowania implementacji usługi z danym typem przez określenie konkretnego typu do użycia. Ponadto fabrykę można określić, który zostanie następnie użyte do utworzenia wystąpienia na żądanie. Trzeci podejście jest bezpośrednio określić wystąpienie typu do użycia, w którego przypadku kontenera nigdy nie będzie podejmować próby utworzenia wystąpienia (ani będzie dysponować wystąpienia).

Aby zademonstrować różnica między te opcje okres istnienia i rejestracji, należy wziąć pod uwagę prosty interfejs, który reprezentuje jeden lub więcej zadań jako *operacji* z unikatowym identyfikatorem `OperationId`. W zależności od tego, w jaki sposób skonfigurować okres istnienia dla tej usługi kontener zawierają tych samych lub różnych wystąpień usługi do klasy wysyłającego żądanie. Aby umożliwić Wyczyść, którego okres istnienia jest wymagany, utworzymy jednego typu na opcja okresu istnienia:

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Wprowadzania tych interfejsów przy użyciu jednej klasy, `Operation`, która akceptuje `Guid` w konstruktorze lub używa nowego `Guid` Jeśli nie zostanie podana.

Następnie w `ConfigureServices`, każdy typ jest dodawany do kontenera zgodnie z jego istnienia o nazwie:

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Należy pamiętać, że `IOperationSingletonInstance` usługa używa określonego wystąpienia o identyfikatorze znane `Guid.Empty` , będzie on wyczyść podczas tego typu jest używana (jego identyfikator Guid będą same zera). Możemy także zarejestrowane `OperationService` to zależy od wszystkich innych `Operation` typy, dzięki czemu będzie wyczyść w ramach żądania określa, czy ta usługa jest wprowadzenie tego samego wystąpienia co kontroler, lub do nowego, dla każdego typu operacji. Ta usługa nie będzie prezentować zależnościami jako właściwości, co mogą być wyświetlane w widoku.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Aby zademonstrować okresy istnienia obiektu, w ramach i między oddzielne poszczególnych żądań do aplikacji, obejmuje próbki `OperationsController` który żąda każdego rodzaju `IOperation` typu, a także `OperationService`. `Index` Akcji następnie wyświetla wszystkie usługi i kontrolera `OperationId` wartości.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Teraz dwa oddzielne żądania zostały wprowadzone do tej akcji kontrolera:

![Widok operacji zależności iniekcji przykładowej aplikacji sieci web uruchomiony w programie Microsoft Edge przedstawiający operacji identyfikatory (GUID) dla przejściowy, polu, jako pojedynczej i wystąpienie kontrolera i operacji usługi operacji na pierwsze żądanie.](dependency-injection/_static/lifetimes_request1.png)

![Operacje wyświetlane, zawierające wartości Identyfikatora operacji drugie żądanie.](dependency-injection/_static/lifetimes_request2.png)

Sprawdź, które z `OperationId` wartości różnią się w obrębie żądanie i między żądaniami.

* *Przejściowa* obiektów zawsze są różne; nowe wystąpienie jest dostępne na każdym kontrolerze i każdej usługi.

* *Zakres* obiekty są takie same, w ramach żądania, ale różne na różnych żądań

* *Pojedyncze* obiekty są takie same dla każdego obiektu i każde żądanie (niezależnie od tego, czy wystąpienie znajduje się w `ConfigureServices`)

## <a name="scope-validation"></a>Weryfikacja zakresu

Gdy aplikacja jest uruchomiona w środowisku programistycznym na platformie ASP.NET Core 2.0 lub nowszej, domyślny dostawca usług sprawdza do sprawdzenia, czy:

* Usługi w zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług głównego.
* Usługi w zakresie nie są bezpośrednio lub pośrednio wprowadzić do pojedynczych wystąpień.

Dostawcy usług głównego jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana. Okres istnienia dostawcy usług głównego odpowiada istnienia aplikacji/serwer. Jeśli dostawca rozpoczyna się od aplikacji i został usunięty podczas zamykania aplikacji.

Usługi w zakresie są usuwane przez kontener, który je utworzył. Zakresami usługi jest tworzony w kontenerze katalogu głównego, okres istnienia usługi jest skutecznie podwyższany do pojedynczego wystąpienia ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwera aplikacji zostanie zamknięta. Walidacja zakresów usługi przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.

Aby uzyskać więcej informacji, zobacz [zakres sprawdzania poprawności w temacie hostingu](xref:fundamentals/hosting#scope-validation).

## <a name="request-services"></a>Żądanie usługi

Dostępne w programie ASP.NET usługi, poproś `HttpContext` dostępnych za pośrednictwem `RequestServices` kolekcji.

![Element HttpContext żądania usług Intellisense kontekstowe okna dialogowego informujący, że żądanie usługi pobiera lub ustawia element IServiceProvider, który zapewnia dostęp do kontenera usługi żądania.](dependency-injection/_static/request-services.png)

Żądanie usługi reprezentują usługi, konfigurowanie i żądania jako część aplikacji. Gdy obiektów Określ zależności, te są spełnione przez typy w `RequestServices`, a nie `ApplicationServices`.

Ogólnie rzecz biorąc nie można używać tych właściwości bezpośrednio, zamiast tego preferowanie do żądania z klas, które wymagają za pośrednictwem swojej klasy Konstruktor typów i pozwolić framework wstrzyknięcia zależności. Daje to klasy, które są łatwiejsze do testowania (zobacz [testowanie](../testing/index.md)) i są bardziej luźno powiązane.

> [!NOTE]
> Preferowane jest żądaniem zależności jako parametry konstruktora do uzyskiwania dostępu do `RequestServices` kolekcji.

## <a name="designing-services-for-dependency-injection"></a>Projektowanie usług iniekcji zależności

Zalecane jest zaprojektowanie usług na iniekcji zależności swoich współpracowników. Oznacza to, unikając stosowania wywołania metody statycznej stanowe (co spowodować zapachu kodu, znany jako [statycznych przylepna](http://deviq.com/static-cling/)) i bezpośrednie tworzenie wystąpień klas zależnych w ramach usługi. Pomocne może zapamiętać hasło, [nowych jest sklejki](https://ardalis.com/new-is-glue), w przypadku wybrania, czy można utworzyć wystąpienia typu lub żądania za pomocą iniekcji zależności. Wykonując [stałych zasad z zorientowane na projekt obiektu](http://deviq.com/solid/), klas naturalnie będzie często za mały, dobrze factored i łatwo przetestowane.

Co zrobić, jeśli okaże się, że klas zazwyczaj mają sposób zbyt wiele zależności, które są wstrzykiwane? Zwykle jest to znak klasy próbuje zrobić zbyt dużo i prawdopodobnie narusza zasady ograniczeń oprogramowania - [jednej zasady odpowiedzialności](http://deviq.com/single-responsibility-principle/). Zobacz, jeśli użytkownik Refaktoryzuj klasy przez przeniesienie niektórych jego obowiązki do nowej klasy. Należy pamiętać, że Twoje `Controller` klasy powinny koncentrować się na dotyczy interfejsu użytkownika, business reguł i danych access szczegóły implementacji powinna być przechowywana w odpowiednie do tych klas [oddzielnych dotyczy](http://deviq.com/separation-of-concerns/).

W szczególności w odniesieniu do dostępu do danych może wstrzyknąć `DbContext` w kontrolerach (przy założeniu EF zostały dodane do kontenera usług w `ConfigureServices`). Niektórzy deweloperzy wolą używać interfejsu repozytorium, w bazie danych zostanie wstrzyknięta `DbContext` bezpośrednio. Przy użyciu interfejsu w celu hermetyzacji danych logika dostępu w jednym miejscu można zminimalizować liczbę miejsc, trzeba będzie zmienić podczas zmiany bazy danych.

### <a name="disposing-of-services"></a>Usuwanie usług

Kontener wywoła `Dispose` dla `IDisposable` tworzy typy. Jednak jeśli wystąpienie dodać do kontenera samodzielnie, go nie zostanie usunięte.

Przykład:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> W wersji 1.0 kontenera wywołano metodę dispose *wszystkie* `IDisposable` obiektów, w tym te nie zostały utworzone.

## <a name="replacing-the-default-services-container"></a>Zastępowanie domyślnego kontenera usług

Kontener usług wbudowanych jest przeznaczona do potrzebami podstawowe ramach i większość aplikacji klienta oparty na jej. Jednak deweloperzy można zastąpić wbudowanych kontenera ich preferowanego kontenera. `ConfigureServices` Metoda zwykle zwraca `void`, ale zmiana jego sygnatury do zwrócenia `IServiceProvider`, innego kontenera można konfigurować i zwrócony. Brak dostępnych wiele kontenerów Inwersja kontroli dla platformy .NET. W tym przykładzie [Autofac](https://autofac.org/) jest używany pakiet.

Najpierw należy zainstalować pakiety odpowiedniego kontenera:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Skonfiguruj kontener w `ConfigureServices` i zwracać `IServiceProvider`:

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

> [!NOTE]
> Korzystając z kontenera Podpisane innych firm, należy zmienić `ConfigureServices` , tak aby zwracało `IServiceProvider` zamiast `void`.

Na koniec skonfiguruj Autofac normalnie w `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

W czasie wykonywania Autofac będzie służyć do rozwiązywania typów i wstrzyknięcia zależności. [Dowiedz się więcej o korzystaniu z Autofac i ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Bezpieczeństwo wątków

Usługi Singleton należy bezpieczeństwa wątków. Jeśli usługi singleton ma zależność usługi przejściowy, przejściowych usługi może być również konieczne zapewniać bezpieczeństwo wątkowe zależności od sposobu ich wykorzystywania singleton.

## <a name="recommendations"></a>Zalecenia

Podczas pracy z iniekcji zależności należy pamiętać o następujących zaleceń:

* Podpisane jest przeznaczony dla obiektów, które mają złożonych zależności. Kontrolery, usługi kart i repozytoriów przedstawiono wszystkie obiekty, które mogą zostać dodane do Podpisane.

* Unikaj przechowywania danych i konfiguracji bezpośrednio w Podpisane. Na przykład koszyk użytkownika zwykle nie należy dodać do kontenera usług. Należy używać konfiguracji [wzorzec opcje](xref:fundamentals/configuration/options). Podobnie należy unikać obiektów "symbol zastępczy danych", które istnieją tylko dostęp do innego obiektu. Warto żądania bieżącego elementu potrzebne za pośrednictwem Podpisane, jeśli to możliwe.

* Unikaj statycznych dostęp do usług.

* Unikaj lokalizacji usługi w kodzie aplikacji.

* Unikaj statycznych dostęp do `HttpContext`.

> [!NOTE]
> Podobnie jak wszystkie zestawy zaleceń mogą wystąpić sytuacje, w których zostanie zignorowany, co jest wymagane. Znaleziono wyjątki, aby się bardzo rzadko — przeważnie bardzo szczególnych przypadkach w ramach samego.

Należy pamiętać, że iniekcji zależności *alternatywnych* do wzorce dostępu do obiektu statyczne/globalne. Nie można wykorzystać zalety Podpisane, jeśli można mieszać z dostępem do obiektu statycznego.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Testowanie](xref:testing/index)
* [Aktywacji opartej na fabryki oprogramowania pośredniczącego](xref:fundamentals/middleware/extensibility)
* [Czysty kod platformy ASP.NET Core z iniekcji zależności (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Projekt aplikacji zarządzanych przez kontener, Prelude: Gdzie jest kontener należeć?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Zasada jawne zależności](http://deviq.com/explicit-dependencies-principle/)
* [Inwersja kontroli kontenerów i wzorzec iniekcji zależności](https://www.martinfowler.com/articles/injection.html) (Fowler)
