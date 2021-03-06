---
title: Wykrywanie zmian przy użyciu tokenów zmiany w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak śledzić zmiany przy użyciu tokenów zmian.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 70451e219f1295b854e2f84aac55f0cfd1786b19
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656346"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Wykrywanie zmian przy użyciu tokenów zmiany w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

*Tokenem zmiany* jest ogólnym blokiem konstrukcyjnym, który służy do śledzenia zmian stanu.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken, interfejs

<xref:Microsoft.Extensions.Primitives.IChangeToken> propaguje powiadomienia o wystąpieniu zmiany. `IChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>. Pakiet NuGet [Microsoft. Extensions.](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) jest niejawnie dostarczany do aplikacji ASP.NET Core.

`IChangeToken` ma dwie właściwości:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> wskazać, czy token aktywnie wywołuje wywołania zwrotne. Jeśli `ActiveChangedCallbacks` jest ustawiona na `false`, wywołanie zwrotne nigdy nie zostanie wywołane, a aplikacja musi sondować `HasChanged` pod kątem zmian. Istnieje również możliwość, że token nigdy nie zostanie anulowany, jeśli nie wystąpią żadne zmiany lub zostanie usunięty lub wyłączony odbiornik zmian.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> otrzymuje wartość wskazującą, czy wprowadzono zmianę.

Interfejs `IChangeToken` obejmuje metodę [RegisterChangeCallback (Action\<object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) , która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. przed wywołaniem wywołania zwrotnego należy ustawić `HasChanged`.

## <a name="changetoken-class"></a>Klasa ChangeToken

<xref:Microsoft.Extensions.Primitives.ChangeToken> jest klasą statyczną służącą do propagowania powiadomień, w których wystąpiła zmiana. `ChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>. Pakiet NuGet [Microsoft. Extensions.](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) jest niejawnie dostarczany do aplikacji ASP.NET Core.

Metoda [ChangeToken. OnChange (Func\<IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestruje `Action` do wywołania przy każdej zmianie tokenu:

* `Func<IChangeToken>` generuje token.
* `Action` jest wywoływana, gdy zostanie zmieniony token.

[ChangeToken. Onchange\<TState > (Func\<IChangeToken >, Action\<TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) Przeciążenie przyjmuje dodatkowy parametr `TState`, który jest przesyłany do `Action`odbiorcy tokenu.

`OnChange` zwraca <xref:System.IDisposable>. Wywołanie <xref:System.IDisposable.Dispose*> uniemożliwia wysłuchanie tokenu przez dalsze zmiany i zwolnienie zasobów tokenu.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Przykładowe zastosowania tokenów zmiany w ASP.NET Core

Tokeny zmiany są używane w widocznych obszarach ASP.NET Core do monitorowania zmian obiektów:

* W przypadku monitorowania zmian plików <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> Metoda tworzy `IChangeToken` dla określonych plików lub folderów do monitorowania.
* tokeny `IChangeToken` można dodać do wpisów pamięci podręcznej, aby wyzwolić wykluczenia z pamięci podręcznej w przypadku zmiany.
* W przypadku zmian `TOptions` domyślna <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacja <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ma Przeciążenie, które akceptuje co najmniej jedno wystąpienie <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>. Każde wystąpienie zwraca `IChangeToken` do zarejestrowania zmian wywołania zwrotnego powiadomienia o zmianach dla opcji śledzenia.

## <a name="monitor-for-configuration-changes"></a>Monitorowanie zmian konfiguracji

Domyślnie szablony ASP.NET Core korzystają z [plików konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, *appSettings. Development. JSON*i *appSettings. Production. JSON*) w celu załadowania ustawień konfiguracji aplikacji.

Te pliki są konfigurowane przy użyciu metody rozszerzenia [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, która akceptuje parametr `reloadOnChange`. `reloadOnChange` wskazuje, czy należy ponownie załadować konfigurację w przypadku zmian w pliku. To ustawienie jest dostępne w <xref:Microsoft.Extensions.Hosting.Host> metodzie wygody <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

Konfiguracja oparta na plikach jest reprezentowana przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>. `FileConfigurationSource` używa <xref:Microsoft.Extensions.FileProviders.IFileProvider> do monitorowania plików.

Domyślnie `IFileMonitor` jest udostępniana przez <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, która używa <xref:System.IO.FileSystemWatcher> do monitorowania zmian w pliku konfiguracji.

Przykładowa aplikacja przedstawia dwie implementacje monitorowania zmian konfiguracji. Jeśli którykolwiek z plików *AppSettings* zostanie zmieniony, oba implementacje monitorowania plików wykonują kod niestandardowy&mdash;Przykładowa aplikacja zapisuje komunikat w konsoli programu.

`FileSystemWatcher` pliku konfiguracji może wyzwolić wiele wywołań zwrotnych tokenów dla jednej zmiany pliku konfiguracji. Aby upewnić się, że kod niestandardowy jest uruchamiany tylko raz w przypadku wyzwolenia wielu wywołań zwrotnych tokenów, implementacja próbki sprawdza skróty plików. W przykładzie zastosowano mieszanie plików SHA1. Ponowna próba jest zaimplementowana z wycofywaniem wykładniczym. Ponowienie próby jest możliwe, ponieważ może wystąpić zablokowanie pliku, który tymczasowo uniemożliwia Obliczanie nowego skrótu pliku.

*Narzędzia/Narzędzia. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>Prosty token zmiany uruchamiania

Zarejestruj odbiorcę tokenu `Action` wywołanie zwrotne dla powiadomień o zmianach do tokenu Załaduj ponownie konfigurację.

W pliku `Startup.Configure`:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` udostępnia token. Wywołanie zwrotne jest metodą `InvokeChanged`:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

`state` wywołania zwrotnego jest używany do przekazywania w `IWebHostEnvironment`, co jest przydatne do określania poprawnego pliku konfiguracyjnego *AppSettings* do monitorowania (na przykład *appSettings. Plik Development. JSON* w środowisku programistycznym). Skróty plików są używane do zapobiegania wielokrotnemu uruchamianiu instrukcji `WriteConsole` ze względu na wiele wywołań zwrotnych tokenów, gdy plik konfiguracyjny został zmieniony tylko raz.

Ten system działa tak długo, jak działa aplikacja i nie może zostać wyłączona przez użytkownika.

### <a name="monitor-configuration-changes-as-a-service"></a>Monitorowanie zmian konfiguracji jako usługi

Przykład implementuje:

* Podstawowe monitorowanie tokenów uruchamiania.
* Monitorowanie jako usługa.
* Mechanizm do włączania i wyłączania monitorowania.

Przykład ustanawia interfejs `IConfigurationMonitor`.

*Rozszerzenia/ConfigurationMonitor. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Konstruktor zaimplementowanej klasy `ConfigurationMonitor`rejestruje wywołanie zwrotne dla powiadomień o zmianach:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` dostarcza token. `InvokeChanged` jest metodą wywołania zwrotnego. `state` w tym wystąpieniu jest odwołaniem do wystąpienia `IConfigurationMonitor`, które jest używane w celu uzyskania dostępu do stanu monitorowania. Są używane dwie właściwości:

* `MonitoringEnabled` &ndash; wskazuje, czy wywołanie zwrotne powinno uruchamiać swój kod niestandardowy.
* `CurrentState` &ndash; opisuje bieżący stan monitorowania do użycia w interfejsie użytkownika.

Metoda `InvokeChanged` jest podobna do wcześniejszego podejścia, z tą różnicą, że:

* Nie uruchamia tego kodu, chyba że `MonitoringEnabled` jest `true`.
* Wyprowadza bieżącą `state` w jej `WriteConsole` danych wyjściowych.

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

`ConfigurationMonitor` wystąpienia jest zarejestrowany jako usługa w `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

Strona indeks zapewnia kontrolę nad konfiguracją przez użytkownika. Wystąpienie `IConfigurationMonitor` jest wstrzykiwane do `IndexModel`.

*Pages/index. cshtml. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

Monitor konfiguracji (`_monitor`) służy do włączania lub wyłączania monitorowania oraz ustawiania bieżącego stanu na potrzeby przesyłania opinii o interfejsie użytkownika:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

Gdy `OnPostStartMonitoring` jest wyzwalane, monitorowanie jest włączone, a bieżący stan jest wyczyszczony. Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony na odzwierciedlenie tego, że monitorowanie nie jest wykonywane.

Przyciski w interfejsie użytkownika włączają i wyłączają monitorowanie.

*Pages/index. cshtml*:

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>Monitoruj zmiany plików w pamięci podręcznej

Zawartość pliku może być buforowana w pamięci przy użyciu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Buforowanie w pamięci jest opisane w temacie [pamięć podręczna w pamięci podręcznej](xref:performance/caching/memory) . Bez podejmowania dodatkowych czynności, takich jak implementacja opisana poniżej, *nieodświeżone (przestarzałe* ) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zostaną zmienione.

Na przykład nie uwzględnia stanu pliku źródłowego w pamięci podręcznej podczas odnawiania przedziału czasu [wygaśnięcia](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) prowadzi do starych danych w pamięci podręcznej. Każde żądanie dotyczące danych odnawia przedział czasu wygaśnięcia, ale plik nigdy nie jest ponownie ładowany do pamięci podręcznej. Wszystkie funkcje aplikacji korzystające z zawartości w pamięci podręcznej są uzależnione od potencjalnie otrzymywania nieodświeżonej zawartości.

Użycie tokenów zmiany w scenariuszu buforowania plików uniemożliwia obecność przestarzałych zawartości plików w pamięci podręcznej. Przykładowa aplikacja prezentuje implementację metody.

Przykład używa `GetFileContent` do:

* Zwróć zawartość pliku.
* Zaimplementuj algorytm ponawiania prób z wycofywaniem z powrotem, aby uwzględnić przypadki, w których blokada pliku tymczasowo uniemożliwia odczytywanie pliku.

*Narzędzia/Narzędzia. cs*:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

`FileService` jest tworzony w celu obsługi wyszukiwań w pamięci podręcznej. Wywołanie metody `GetFileContent`j usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i zwrócić ją do obiektu wywołującego (*Services/FileService. cs*).

Jeśli buforowana zawartość nie zostanie znaleziona przy użyciu klucza pamięci podręcznej, zostaną wykonane następujące akcje:

1. Zawartość pliku jest uzyskiwana przy użyciu `GetFileContent`.
1. Token zmiany jest uzyskiwany od dostawcy plików z [IFileProviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*). Wywołanie zwrotne tokenu jest wyzwalane, gdy plik zostanie zmodyfikowany.
1. Zawartość pliku jest buforowana z [ruchomym](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okresem ważności. Token zmiany jest dołączony do [MemoryCacheEntryExtensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) w celu wykluczenia wpisu pamięci podręcznej, jeśli plik zostanie zmieniony w pamięci podręcznej.

W poniższym przykładzie pliki są przechowywane w [katalogu głównym zawartości](xref:fundamentals/index#content-root)aplikacji. `IWebHostEnvironment.ContentRootFileProvider` służy do uzyskiwania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazujących `IWebHostEnvironment.ContentRootPath`aplikacji. `filePath` jest uzyskiwany za pomocą [IFileInfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService` jest zarejestrowany w kontenerze usługi wraz z usługą buforowania pamięci.

W pliku `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

Model strony ładuje zawartość pliku przy użyciu usługi.

W metodzie `OnGet` strony indeksu (*strony/index. cshtml. cs*):

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Klasa CompositeChangeToken

Dla reprezentowania jednego lub więcej wystąpień `IChangeToken` w jednym obiekcie, użyj klasy <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged` w raportach tokenów złożonych, `true`, jeśli `true``HasChanged` tokenem reprezentowanego. `ActiveChangeCallbacks` w raportach tokenów złożonych, `true`, jeśli `true``ActiveChangeCallbacks` tokenem reprezentowanego. W przypadku wystąpienia wielu współbieżnych zdarzeń zmiany wywołanie zwrotne zmiany złożonego jest wywoływana jednokrotnie.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Tokenem zmiany* jest ogólnym blokiem konstrukcyjnym, który służy do śledzenia zmian stanu.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken, interfejs

<xref:Microsoft.Extensions.Primitives.IChangeToken> propaguje powiadomienia o wystąpieniu zmiany. `IChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>. W przypadku aplikacji, które nie korzystają z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu dla pakietu NuGet [Microsoft. Extensions. pierwotne](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) .

`IChangeToken` ma dwie właściwości:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> wskazać, czy token aktywnie wywołuje wywołania zwrotne. Jeśli `ActiveChangedCallbacks` jest ustawiona na `false`, wywołanie zwrotne nigdy nie zostanie wywołane, a aplikacja musi sondować `HasChanged` pod kątem zmian. Istnieje również możliwość, że token nigdy nie zostanie anulowany, jeśli nie wystąpią żadne zmiany lub zostanie usunięty lub wyłączony odbiornik zmian.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> otrzymuje wartość wskazującą, czy wprowadzono zmianę.

Interfejs `IChangeToken` obejmuje metodę [RegisterChangeCallback (Action\<object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) , która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. przed wywołaniem wywołania zwrotnego należy ustawić `HasChanged`.

## <a name="changetoken-class"></a>Klasa ChangeToken

<xref:Microsoft.Extensions.Primitives.ChangeToken> jest klasą statyczną służącą do propagowania powiadomień, w których wystąpiła zmiana. `ChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>. W przypadku aplikacji, które nie korzystają z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu dla pakietu NuGet [Microsoft. Extensions. pierwotne](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) .

Metoda [ChangeToken. OnChange (Func\<IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestruje `Action` do wywołania przy każdej zmianie tokenu:

* `Func<IChangeToken>` generuje token.
* `Action` jest wywoływana, gdy zostanie zmieniony token.

[ChangeToken. Onchange\<TState > (Func\<IChangeToken >, Action\<TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) Przeciążenie przyjmuje dodatkowy parametr `TState`, który jest przesyłany do `Action`odbiorcy tokenu.

`OnChange` zwraca <xref:System.IDisposable>. Wywołanie <xref:System.IDisposable.Dispose*> uniemożliwia wysłuchanie tokenu przez dalsze zmiany i zwolnienie zasobów tokenu.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Przykładowe zastosowania tokenów zmiany w ASP.NET Core

Tokeny zmiany są używane w widocznych obszarach ASP.NET Core do monitorowania zmian obiektów:

* W przypadku monitorowania zmian plików <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> Metoda tworzy `IChangeToken` dla określonych plików lub folderów do monitorowania.
* tokeny `IChangeToken` można dodać do wpisów pamięci podręcznej, aby wyzwolić wykluczenia z pamięci podręcznej w przypadku zmiany.
* W przypadku zmian `TOptions` domyślna <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacja <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ma Przeciążenie, które akceptuje co najmniej jedno wystąpienie <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>. Każde wystąpienie zwraca `IChangeToken` do zarejestrowania zmian wywołania zwrotnego powiadomienia o zmianach dla opcji śledzenia.

## <a name="monitor-for-configuration-changes"></a>Monitorowanie zmian konfiguracji

Domyślnie szablony ASP.NET Core korzystają z [plików konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, *appSettings. Development. JSON*i *appSettings. Production. JSON*) w celu załadowania ustawień konfiguracji aplikacji.

Te pliki są konfigurowane przy użyciu metody rozszerzenia [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, która akceptuje parametr `reloadOnChange`. `reloadOnChange` wskazuje, czy należy ponownie załadować konfigurację w przypadku zmian w pliku. To ustawienie jest dostępne w <xref:Microsoft.AspNetCore.WebHost> metodzie wygody <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

Konfiguracja oparta na plikach jest reprezentowana przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>. `FileConfigurationSource` używa <xref:Microsoft.Extensions.FileProviders.IFileProvider> do monitorowania plików.

Domyślnie `IFileMonitor` jest udostępniana przez <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, która używa <xref:System.IO.FileSystemWatcher> do monitorowania zmian w pliku konfiguracji.

Przykładowa aplikacja przedstawia dwie implementacje monitorowania zmian konfiguracji. Jeśli którykolwiek z plików *AppSettings* zostanie zmieniony, oba implementacje monitorowania plików wykonują kod niestandardowy&mdash;Przykładowa aplikacja zapisuje komunikat w konsoli programu.

`FileSystemWatcher` pliku konfiguracji może wyzwolić wiele wywołań zwrotnych tokenów dla jednej zmiany pliku konfiguracji. Aby upewnić się, że kod niestandardowy jest uruchamiany tylko raz w przypadku wyzwolenia wielu wywołań zwrotnych tokenów, implementacja próbki sprawdza skróty plików. W przykładzie zastosowano mieszanie plików SHA1. Ponowna próba jest zaimplementowana z wycofywaniem wykładniczym. Ponowienie próby jest możliwe, ponieważ może wystąpić zablokowanie pliku, który tymczasowo uniemożliwia Obliczanie nowego skrótu pliku.

*Narzędzia/Narzędzia. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>Prosty token zmiany uruchamiania

Zarejestruj odbiorcę tokenu `Action` wywołanie zwrotne dla powiadomień o zmianach do tokenu Załaduj ponownie konfigurację.

W pliku `Startup.Configure`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` udostępnia token. Wywołanie zwrotne jest metodą `InvokeChanged`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

`state` wywołania zwrotnego jest używany do przekazywania w `IHostingEnvironment`, co jest przydatne do określania poprawnego pliku konfiguracyjnego *AppSettings* do monitorowania (na przykład *appSettings. Plik Development. JSON* w środowisku programistycznym). Skróty plików są używane do zapobiegania wielokrotnemu uruchamianiu instrukcji `WriteConsole` ze względu na wiele wywołań zwrotnych tokenów, gdy plik konfiguracyjny został zmieniony tylko raz.

Ten system działa tak długo, jak działa aplikacja i nie może zostać wyłączona przez użytkownika.

### <a name="monitor-configuration-changes-as-a-service"></a>Monitorowanie zmian konfiguracji jako usługi

Przykład implementuje:

* Podstawowe monitorowanie tokenów uruchamiania.
* Monitorowanie jako usługa.
* Mechanizm do włączania i wyłączania monitorowania.

Przykład ustanawia interfejs `IConfigurationMonitor`.

*Rozszerzenia/ConfigurationMonitor. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Konstruktor zaimplementowanej klasy `ConfigurationMonitor`rejestruje wywołanie zwrotne dla powiadomień o zmianach:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` dostarcza token. `InvokeChanged` jest metodą wywołania zwrotnego. `state` w tym wystąpieniu jest odwołaniem do wystąpienia `IConfigurationMonitor`, które jest używane w celu uzyskania dostępu do stanu monitorowania. Są używane dwie właściwości:

* `MonitoringEnabled` &ndash; wskazuje, czy wywołanie zwrotne powinno uruchamiać swój kod niestandardowy.
* `CurrentState` &ndash; opisuje bieżący stan monitorowania do użycia w interfejsie użytkownika.

Metoda `InvokeChanged` jest podobna do wcześniejszego podejścia, z tą różnicą, że:

* Nie uruchamia tego kodu, chyba że `MonitoringEnabled` jest `true`.
* Wyprowadza bieżącą `state` w jej `WriteConsole` danych wyjściowych.

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

`ConfigurationMonitor` wystąpienia jest zarejestrowany jako usługa w `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Strona indeks zapewnia kontrolę nad konfiguracją przez użytkownika. Wystąpienie `IConfigurationMonitor` jest wstrzykiwane do `IndexModel`.

*Pages/index. cshtml. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

Monitor konfiguracji (`_monitor`) służy do włączania lub wyłączania monitorowania oraz ustawiania bieżącego stanu na potrzeby przesyłania opinii o interfejsie użytkownika:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

Gdy `OnPostStartMonitoring` jest wyzwalane, monitorowanie jest włączone, a bieżący stan jest wyczyszczony. Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony na odzwierciedlenie tego, że monitorowanie nie jest wykonywane.

Przyciski w interfejsie użytkownika włączają i wyłączają monitorowanie.

*Pages/index. cshtml*:

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>Monitoruj zmiany plików w pamięci podręcznej

Zawartość pliku może być buforowana w pamięci przy użyciu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Buforowanie w pamięci jest opisane w temacie [pamięć podręczna w pamięci podręcznej](xref:performance/caching/memory) . Bez podejmowania dodatkowych czynności, takich jak implementacja opisana poniżej, *nieodświeżone (przestarzałe* ) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zostaną zmienione.

Na przykład nie uwzględnia stanu pliku źródłowego w pamięci podręcznej podczas odnawiania przedziału czasu [wygaśnięcia](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) prowadzi do starych danych w pamięci podręcznej. Każde żądanie dotyczące danych odnawia przedział czasu wygaśnięcia, ale plik nigdy nie jest ponownie ładowany do pamięci podręcznej. Wszystkie funkcje aplikacji korzystające z zawartości w pamięci podręcznej są uzależnione od potencjalnie otrzymywania nieodświeżonej zawartości.

Użycie tokenów zmiany w scenariuszu buforowania plików uniemożliwia obecność przestarzałych zawartości plików w pamięci podręcznej. Przykładowa aplikacja prezentuje implementację metody.

Przykład używa `GetFileContent` do:

* Zwróć zawartość pliku.
* Zaimplementuj algorytm ponawiania prób z wycofywaniem z powrotem, aby uwzględnić przypadki, w których blokada pliku tymczasowo uniemożliwia odczytywanie pliku.

*Narzędzia/Narzędzia. cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

`FileService` jest tworzony w celu obsługi wyszukiwań w pamięci podręcznej. Wywołanie metody `GetFileContent`j usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i zwrócić ją do obiektu wywołującego (*Services/FileService. cs*).

Jeśli buforowana zawartość nie zostanie znaleziona przy użyciu klucza pamięci podręcznej, zostaną wykonane następujące akcje:

1. Zawartość pliku jest uzyskiwana przy użyciu `GetFileContent`.
1. Token zmiany jest uzyskiwany od dostawcy plików z [IFileProviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*). Wywołanie zwrotne tokenu jest wyzwalane, gdy plik zostanie zmodyfikowany.
1. Zawartość pliku jest buforowana z [ruchomym](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okresem ważności. Token zmiany jest dołączony do [MemoryCacheEntryExtensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) w celu wykluczenia wpisu pamięci podręcznej, jeśli plik zostanie zmieniony w pamięci podręcznej.

W poniższym przykładzie pliki są przechowywane w [katalogu głównym zawartości](xref:fundamentals/index#content-root)aplikacji. [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) służy do uzyskiwania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazujących <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>aplikacji. `filePath` jest uzyskiwany za pomocą [IFileInfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService` jest zarejestrowany w kontenerze usługi wraz z usługą buforowania pamięci.

W pliku `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

Model strony ładuje zawartość pliku przy użyciu usługi.

W metodzie `OnGet` strony indeksu (*strony/index. cshtml. cs*):

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Klasa CompositeChangeToken

Dla reprezentowania jednego lub więcej wystąpień `IChangeToken` w jednym obiekcie, użyj klasy <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged` w raportach tokenów złożonych, `true`, jeśli `true``HasChanged` tokenem reprezentowanego. `ActiveChangeCallbacks` w raportach tokenów złożonych, `true`, jeśli `true``ActiveChangeCallbacks` tokenem reprezentowanego. W przypadku wystąpienia wielu współbieżnych zdarzeń zmiany wywołanie zwrotne zmiany złożonego jest wywoływana jednokrotnie.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
