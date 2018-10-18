---
title: Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać tokenów zmian do śledzenia zmian.
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/change-tokens
ms.openlocfilehash: 1cf3693764919a8fd064584ab7b7ad237e8465b3
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391444"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

A *zmienić token* jest ogólnego przeznaczenia, niskiego poziomu bloku konstrukcyjnego używane do śledzenia zmian.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/change-tokens/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfejs IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaguje powiadomienia, że nastąpiła zmiana. `IChangeToken` znajduje się w [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) przestrzeni nazw. W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), odwołanie [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet w pliku projektu.

`IChangeToken` ma dwie właściwości:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) wskazują, jeśli token aktywnego generuje wywołania zwrotne. Jeśli `ActiveChangedCallbacks` ustawiono `false`, wywołanie zwrotne nigdy nie jest wywoływana, a aplikacja musi wykonać sondowanie `HasChanged` zmian. Istnieje także możliwość token nigdy nie zostać anulowana, jeśli występują bez zmian lub podstawowego odbiornika zmian jest usunięte lub wyłączone.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) pobiera wartość wskazującą, jeśli nastąpiła zmiana.

Interfejs ma jedną metodę, [RegisterChangeCallback (Akcja&lt;obiektu&gt;, obiekt)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), który rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. `HasChanged` należy ustawić przed wywołaniem zwrotnym.

## <a name="changetoken-class"></a>Klasa właściwości ChangeToken

`ChangeToken` Klasa statyczna służy do propagowania powiadomienia, że nastąpiła zmiana. `ChangeToken` znajduje się w [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) przestrzeni nazw. W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), odwołanie [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet w pliku projektu.

`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, akcji)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) rejestrów metoda `Action` do wywołania w każdym przypadku, gdy zmieni się tokenu:

* `Func<IChangeToken>` tworzy token.
* `Action` jest wywoływana, gdy zmieni się tokenu.

`ChangeToken` ma [OnChange&lt;stanu dławienia&gt;(Func&lt;IChangeToken&gt;, Akcja&lt;stanu dławienia&gt;, stanu dławienia)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) przeciążenia przyjmującego dodatkowego `TState` parametr, który jest przekazywany do odbiorców tokenu `Action`.

`OnChange` Zwraca [interfejsu IDisposable](/dotnet/api/system.idisposable). Wywoływanie [Dispose](/dotnet/api/system.idisposable.dispose) zatrzymuje token nasłuchiwanie wprowadzanie dalszych zmian i zwalnia zasoby tokenu.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Przykład używa tokenów zmiany w programie ASP.NET Core

Zmiana tokenów są używane w widocznym obszarach programu ASP.NET Core, monitorując zmiany do obiektów:

* Do monitorowania zmian w plikach, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)firmy [Obejrzyj](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda tworzy `IChangeToken` dla określonych plików lub folder do obserwacji.
* `IChangeToken` tokenów można dodać do wpisów pamięci podręcznej, aby wyzwolić Eksmisje pamięci podręcznej w przypadku zmiany.
* Aby uzyskać `TOptions` zmienia domyślny [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementacji [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) musi być przeciążona, który akceptuje co najmniej jeden [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)wystąpień. Każde wystąpienie, zwraca `IChangeToken` zarejestrować zmiany wywołania zwrotnego dla śledzenia opcje zmiany.

## <a name="monitoring-for-configuration-changes"></a>Monitorowanie zmian konfiguracji

Domyślnie używają szablony ASP.NET Core [pliki konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, i *appsettings. Production.JSON*) można załadować ustawień konfiguracji aplikacji.

Te pliki są skonfigurowane przy użyciu [AddJsonFile (IConfigurationBuilder, String, Boolean, atrybut typu wartość logiczna)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) metody rozszerzenia [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) akceptujący `reloadOnChange` parametru (ASP.NET Core 1.1 i nowszych). `reloadOnChange` Wskazuje, jeśli konfiguracja powinna ładowane na zmiany w plikach. Zobacz to ustawienie w [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) wygodna metoda [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Konfiguracja na podstawie pliku jest reprezentowany przez [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` używa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) monitorować pliki.

Domyślnie `IFileMonitor` są dostarczane przez [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), który używa [klasy FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) do monitorowania zmian w pliku konfiguracji.

Przykładowa aplikacja pokazuje dwie implementacje dla monitorowania zmian konfiguracji. Jeśli *appsettings.json* zmiany zmiany plików lub środowiska wersję pliku, każda implementacja wykonuje kod niestandardowy. Przykładowa aplikacja zapisuje komunikat do konsoli.

Plik konfiguracji `FileSystemWatcher` może wyzwolić wiele tokenów wywołań zwrotnych zmiana konfiguracji pojedynczego pliku. Przykład implementacji chroni przed tym problemem, sprawdzając skróty plików w plikach konfiguracji. Sprawdzanie skróty plików gwarantuje, że co najmniej jeden z plików konfiguracji został zmieniony przed uruchomieniem kodu niestandardowego. W przykładzie użyto SHA1 pliku wyznaczania wartości skrótu (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Ponowienie próby jest implementowane za pomocą wykładniczego wycofywania. Ponów próbę występuje, ponieważ blokowanie plików może wystąpić, uniemożliwiające tymczasowo obliczeń nowy skrót na jeden z plików.

### <a name="simple-startup-change-token"></a>Uruchamianie prostego Zmień token

Zarejestruj odbiorców tokenu `Action` wywołanie zwrotne dla powiadomień o zmianie do tokenu ponowne załadowanie konfiguracji (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` zawiera token. Wywołanie zwrotne jest `InvokeChanged` metody:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`state` Wywołania zwrotnego jest używany do przekazywania `IHostingEnvironment`. Dzięki takiemu grupowaniu można określić poprawny *appsettings* pliku JSON konfiguracji, aby monitorować, *appsettings.&lt; Środowisko&gt;.json*. Skróty plików są używane, aby zapobiec `WriteConsole` instrukcji uruchamianie wielokrotne z powodu wielu tokenów wywołania zwrotne, gdy plik konfiguracji został zmieniony tylko raz.

Ten system działa tak długo, jak aplikacja jest uruchomiona i nie może być wyłączony przez użytkownika.

### <a name="monitoring-configuration-changes-as-a-service"></a>Monitorowanie zmian w konfiguracji jako usługa

Implementuje przykładu:

* Podstawowe tokenu monitorowanie uruchamiania.
* Monitorowanie jako usługa.
* Mechanizm do włączania i wyłączania monitorowania.

Przykład ustanawia `IConfigurationMonitor` interfejsu (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Konstruktor klasy zaimplementowane `ConfigurationMonitor`, rejestruje wywołanie zwrotne dla powiadomień o zmianie:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` dostarcza token. `InvokeChanged` jest metodą wywołania zwrotnego. `state` w tym wystąpieniu jest odwołaniem do `IConfigurationMonitor` wystąpienie, które umożliwia dostęp do monitorowania stanu. Są używane dwie właściwości:

* `MonitoringEnabled` Wskazuje, wywołanie zwrotne powinien uruchamiać jego kod niestandardowy.
* `CurrentState` w tym artykule opisano bieżący stan monitorowania do użytku w interfejsie użytkownika.

`InvokeChanged` Metoda jest podobna do wcześniejszych podejście, z wyjątkiem że:

* Nie działa kod, chyba że `MonitoringEnabled` jest `true`.
* Informacje o bieżącej `state` w jego `WriteConsole` danych wyjściowych.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Wystąpienie `ConfigurationMonitor` jest zarejestrowany jako usługa w `ConfigureServices` z *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

Strona indeksu oferuje kontrolki użytkownika za pośrednictwem konfiguracji monitorowania. Wystąpienie `IConfigurationMonitor` są wstrzykiwane do `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Przycisk włącza i wyłącza monitorowanie:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Gdy `OnPostStartMonitoring` jest wyzwalane, jest włączone monitorowanie, oraz bieżący stan jest usuwany. Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony, aby odzwierciedlić, że nie ma miejsce monitorowania.

## <a name="monitoring-cached-file-changes"></a>Monitorowanie zmian plików w pamięci podręcznej

Zawartość pliku może być buforowany w pamięci przy użyciu [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Buforowanie w pamięci jest opisana w [pamięci podręcznej w pamięci](xref:performance/caching/memory) tematu. Bez konieczności wykonania dodatkowych kroków, takich jak wykonanie opisanych poniżej, *starych* (przestarzałe) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zmienią się.

Nie biorąc pod uwagę stan pliku źródłowego w pamięci podręcznej podczas odnawiania [przedłużanie ważności](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) okres prowadzi do starych danych w pamięci podręcznej. Każde żądanie danych odnawia przewijania okres ważności, ale plik nigdy nie zostanie ponownie załadowana do pamięci podręcznej. Wszystkie funkcje aplikacji używających zawartość pamięci podręcznej podlegają prawdopodobnie odbiera zawartość.

Przy użyciu tokenów zmiany w pliku pamięci podręcznej scenariusz zapobiega starych plików zawartości w pamięci podręcznej. Przykładowa aplikacja pokazuje implementację metody.

W przykładzie użyto `GetFileContent` do:

* Zwraca zawartość pliku.
* Implementowanie ponownych prób algorytmu, gdy wycofywanie wykładnicze obejmujące przypadki, gdzie blokady pliku tymczasowo uniemożliwia pliku odczytywany.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Element `FileService` jest utworzona w celu obsługi wyszukiwania plików z pamięci podręcznej. `GetFileContent` Wywołania metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i przywrócić go do obiektu wywołującego (*Services/FileService.cs*).

Jeśli zawartość z pamięci podręcznej nie zostanie znaleziona, przy użyciu klucza pamięci podręcznej, są podejmowane następujące akcje:

1. Zawartość pliku są uzyskiwane przy użyciu `GetFileContent`.
1. Token zmiany są uzyskiwane z dostawcy plików za pomocą [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Wywołanie zwrotne tokenu jest wyzwalany, gdy plik zostanie zmodyfikowany.
1. Zawartość plików jest buforowana przy użyciu [przedłużanie ważności](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) okres. Zmień token jest połączona z [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) do wykluczenia wpisu pamięci podręcznej, jeśli plik ulegnie zmianie podczas, gdy jest ona buforowana.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` Jest zarejestrowany w kontenerze usługi wraz z pamięci, pamięci podręcznej usługi (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

Model strona ładuje zawartość pliku przy użyciu usługi (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Klasa CompositeChangeToken

Do reprezentowania jednego lub więcej `IChangeToken` Użyj wystąpień w pojedynczy obiekt [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) klasy.

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

`HasChanged` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `HasChanged` jest `true`. `ActiveChangeCallbacks` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `ActiveChangeCallbacks` jest `true`. Jeśli występuje wiele równoczesnych zmian zdarzeń, wywołania zwrotnego zmian złożone jest wywoływane dokładnie jeden raz.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Praca z rozproszoną pamięcią podręczną](xref:performance/caching/distributed)
* [Buforowanie odpowiedzi](xref:performance/caching/response)
* [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
* [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
