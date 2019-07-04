---
title: Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać tokenów zmian do śledzenia zmian.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/03/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 8b73b72d093b33edeb91bc78080e05aa312579ec
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561663"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

A *zmienić token* jest ogólnego przeznaczenia, niskiego poziomu bloku konstrukcyjnego używane do śledzenia zmian stanu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfejs IChangeToken

<xref:Microsoft.Extensions.Primitives.IChangeToken> propaguje powiadomienia, że nastąpiła zmiana. `IChangeToken` znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw. W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), utworzyć odwołania do pakietu dla [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet.

`IChangeToken` ma dwie właściwości:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> Wskazuje, jeśli token aktywnego generuje wywołania zwrotne. Jeśli `ActiveChangedCallbacks` ustawiono `false`, wywołanie zwrotne nigdy nie jest wywoływana, a aplikacja musi wykonać sondowanie `HasChanged` zmian. Istnieje także możliwość token nigdy nie zostać anulowana, jeśli występują bez zmian lub podstawowego odbiornika zmian jest usunięte lub wyłączone.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> odbiera wartość, która wskazuje, jeśli nastąpiła zmiana.

`IChangeToken` Interfejs zawiera [RegisterChangeCallback (Akcja\<obiektu >, obiekt)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) metody, która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. `HasChanged` należy ustawić przed wywołaniem zwrotnym.

## <a name="changetoken-class"></a>Klasa właściwości ChangeToken

<xref:Microsoft.Extensions.Primitives.ChangeToken> Klasa statyczna służy do propagowania powiadomienia, że nastąpiła zmiana. `ChangeToken` znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw. W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), utworzyć odwołania do pakietu dla [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet.

[ChangeToken.OnChange (Func\<IChangeToken >, akcji)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestrów metoda `Action` do wywołania w każdym przypadku, gdy zmieni się tokenu:

* `Func<IChangeToken>` tworzy token.
* `Action` jest wywoływana, gdy zmieni się tokenu.

[ChangeToken.OnChange\<stanu dławienia > (Func\<IChangeToken >, Akcja\<stanu dławienia >, stanu dławienia)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) przeciążenia zajmuje dodatkowy `TState` parametr, który jest przekazywany do tokenu konsument `Action`.

`OnChange` Zwraca <xref:System.IDisposable>. Wywoływanie <xref:System.IDisposable.Dispose*> zatrzymuje token nasłuchiwanie wprowadzanie dalszych zmian i zwalnia zasoby tokenu.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Przykład używa tokenów zmiany w programie ASP.NET Core

Zmiana tokenów są używane w widocznym obszarach programu ASP.NET Core do monitorowania zmian do obiektów:

* Do monitorowania zmian w plikach, <xref:Microsoft.Extensions.FileProviders.IFileProvider>firmy <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> metoda tworzy `IChangeToken` dla określonych plików lub folder do obserwacji.
* `IChangeToken` tokenów można dodać do wpisów pamięci podręcznej, aby wyzwolić Eksmisje pamięci podręcznej w przypadku zmiany.
* Dla `TOptions` zmienia domyślny <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacji <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> musi być przeciążona, który akceptuje co najmniej jeden <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> wystąpień. Każde wystąpienie, zwraca `IChangeToken` zarejestrować zmiany wywołania zwrotnego dla śledzenia opcje zmiany.

## <a name="monitor-for-configuration-changes"></a>Monitorowanie zmian konfiguracji

Domyślnie używają szablony ASP.NET Core [pliki konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, i *appsettings. Production.JSON*) można załadować ustawień konfiguracji aplikacji.

Te pliki są skonfigurowane przy użyciu [AddJsonFile (IConfigurationBuilder, String, Boolean, atrybut typu wartość logiczna)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) metody rozszerzenia <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> akceptujący `reloadOnChange` parametru. `reloadOnChange` Wskazuje, jeśli konfiguracja powinna ładowane na zmiany w plikach. To ustawienie jest wyświetlane w <xref:Microsoft.AspNetCore.WebHost> wygodna metoda <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

Konfiguracja na podstawie pliku jest reprezentowany przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>. `FileConfigurationSource` używa <xref:Microsoft.Extensions.FileProviders.IFileProvider> monitorować pliki.

Domyślnie `IFileMonitor` są dostarczane przez <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, który używa <xref:System.IO.FileSystemWatcher> do monitorowania zmian w pliku konfiguracji.

Przykładowa aplikacja pokazuje dwie implementacje dla monitorowania zmian konfiguracji. Jeśli dowolny z *appsettings* zmiany plików, należy wykonać obie monitorowania implementacje pliku niestandardowego kodu&mdash;Przykładowa aplikacja zapisuje komunikat do konsoli.

Plik konfiguracji `FileSystemWatcher` może wyzwolić wiele tokenów wywołań zwrotnych zmiana konfiguracji pojedynczego pliku. Aby upewnić się, kod niestandardowy tylko jest uruchamiany po w przypadku wyzwolenia wielu wywołań zwrotnych tokenu, przykład implementacji sprawdza, czy skróty plików. W przykładzie użyto SHA1 pliku wyznaczania wartości skrótu. Ponowienie próby jest implementowane za pomocą wykładniczego wycofywania. Ponowienie próby jest obecny, ponieważ blokowanie plików może wystąpić, uniemożliwiające tymczasowo Obliczanie nowego skrótu w pliku.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a>Uruchamianie prostego Zmień token

Zarejestruj odbiorców tokenu `Action` wywołanie zwrotne dla powiadomień o zmianie do tokenu ponowne załadowanie konfiguracji.

W `Startup.Configure`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

`config.GetReloadToken()` zawiera token. Wywołanie zwrotne jest `InvokeChanged` metody:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

`state` Wywołania zwrotnego jest używany do przekazywania `IHostingEnvironment`, co jest przydatne do określenia prawidłowego *appsettings* pliku konfiguracji monitorowania (na przykład *appsettings. Development.JSON* w środowisku programistycznym). Skróty plików są używane, aby zapobiec `WriteConsole` instrukcji uruchamianie wielokrotne z powodu wielu tokenów wywołania zwrotne, gdy plik konfiguracji został zmieniony tylko raz.

Ten system działa tak długo, jak aplikacja jest uruchomiona i nie może być wyłączony przez użytkownika.

### <a name="monitor-configuration-changes-as-a-service"></a>Monitorowanie zmian konfiguracji jako usługa

Implementuje przykładu:

* Podstawowe tokenu monitorowanie uruchamiania.
* Monitorowanie jako usługa.
* Mechanizm do włączania i wyłączania monitorowania.

Przykład ustanawia `IConfigurationMonitor` interfejsu.

*Extensions/ConfigurationMonitor.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Konstruktor klasy zaimplementowane `ConfigurationMonitor`, rejestruje wywołanie zwrotne dla powiadomień o zmianie:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` dostarcza token. `InvokeChanged` jest metodą wywołania zwrotnego. `state` w tym wystąpieniu jest odwołaniem do `IConfigurationMonitor` wystąpienie, które umożliwia dostęp do monitorowania stanu. Są używane dwie właściwości:

* `MonitoringEnabled` &ndash; Wskazuje, wywołanie zwrotne powinien uruchamiać jego kod niestandardowy.
* `CurrentState` &ndash; w tym artykule opisano bieżący stan monitorowania do użytku w interfejsie użytkownika.

`InvokeChanged` Metoda jest podobna do wcześniejszych podejście, z wyjątkiem że:

* Nie działa kod, chyba że `MonitoringEnabled` jest `true`.
* Wyświetla bieżący `state` w jego `WriteConsole` danych wyjściowych.

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Wystąpienie `ConfigurationMonitor` jest zarejestrowany jako usługa w `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Strona indeksu oferuje kontrolki użytkownika za pośrednictwem konfiguracji monitorowania. Wystąpienie `IConfigurationMonitor` są wstrzykiwane do `IndexModel`.

*Pages/Index.cshtml.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

Monitor konfiguracji (`_monitor`) umożliwia włączanie lub wyłączanie monitorowania i Ustaw bieżący stan opinii o interfejsie użytkownika:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

Gdy `OnPostStartMonitoring` jest wyzwalane, jest włączone monitorowanie, oraz bieżący stan jest usuwany. Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony, aby odzwierciedlić, że nie ma miejsce monitorowania.

Przyciski w interfejsie użytkownika Włączanie i wyłączanie monitorowania.

*Pages/Index.cshtml*:

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a>Monitorowanie zmian plików w pamięci podręcznej

Zawartość pliku może być buforowany w pamięci przy użyciu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Buforowanie w pamięci jest opisana w [pamięci podręcznej w pamięci](xref:performance/caching/memory) tematu. Bez konieczności wykonania dodatkowych kroków, takich jak wykonanie opisanych poniżej, *starych* (przestarzałe) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zmienią się.

Na przykład, nie biorąc pod uwagę stan pliku źródłowego w pamięci podręcznej podczas odnawiania [przedłużanie ważności](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okres prowadzi do starych plików w pamięci podręcznej danych. Każde żądanie danych odnawia przewijania okres ważności, ale plik nigdy nie zostanie ponownie załadowana do pamięci podręcznej. Wszystkie funkcje aplikacji używających zawartość pamięci podręcznej podlegają prawdopodobnie odbiera zawartość.

Przy użyciu tokenów zmiany w pliku pamięci podręcznej scenariusz zapobiega obecność starych plików zawartości w pamięci podręcznej. Przykładowa aplikacja pokazuje implementację metody.

W przykładzie użyto `GetFileContent` do:

* Zwraca zawartość pliku.
* Implementowanie ponownych prób algorytmu, gdy wycofywanie wykładnicze obejmujące przypadki, gdzie plik blokada tymczasowo zapobiega Odczyt pliku.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

Element `FileService` jest utworzona w celu obsługi wyszukiwania plików z pamięci podręcznej. `GetFileContent` Wywołania metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i przywrócić go do obiektu wywołującego (*Services/FileService.cs*).

Jeśli zawartość z pamięci podręcznej nie zostanie znaleziona, przy użyciu klucza pamięci podręcznej, są podejmowane następujące akcje:

1. Zawartość pliku są uzyskiwane przy użyciu `GetFileContent`.
1. Token zmiany są uzyskiwane z dostawcy plików za pomocą [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*). Wywołanie zwrotne tokenu jest wyzwalany, gdy plik zostanie zmodyfikowany.
1. Zawartość plików jest buforowana przy użyciu [przedłużanie ważności](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okres. Zmień token jest połączona z [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) do wykluczenia wpisu pamięci podręcznej, jeśli plik ulegnie zmianie podczas, gdy jest ona buforowana.

W poniższym przykładzie pliki są przechowywane w katalogu głównym zawartości aplikacji. [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) służy do uzyskiwania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazywanym przez aplikację <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>. `filePath` Są uzyskiwane z [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

`FileService` Jest zarejestrowany w kontenerze usługi wraz z pamięci, pamięci podręcznej usługi.

W `Startup.ConfigureServices`:

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

Model strona ładuje zawartość pliku przy użyciu usługi.

Na stronie indeksu `OnGet` — metoda (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Klasa CompositeChangeToken

Do reprezentowania jednego lub więcej `IChangeToken` Użyj wystąpień w pojedynczy obiekt <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> klasy.

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

`HasChanged` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `HasChanged` jest `true`. `ActiveChangeCallbacks` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `ActiveChangeCallbacks` jest `true`. Jeśli występuje wiele równoczesnych zmian zdarzeń, wywołanie zwrotne zmiany złożonego jest wywoływana jeden raz.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
