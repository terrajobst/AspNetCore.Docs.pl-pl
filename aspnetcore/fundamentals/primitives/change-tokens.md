---
title: Wykrywanie zmian z tokenami zmiany w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać tokeny zmian do śledzenia zmian.
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 165602587d73907416f47a7ce82a3081e8d74c4b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276896"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Wykrywanie zmian z tokenami zmiany w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

A *zmienić token* jest ogólnego przeznaczenia, niskiego poziomu bloku konstrukcyjnego używane do śledzenia zmian.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfejs IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaguje powiadomienia, że nastąpiła zmiana. `IChangeToken` znajduje się w [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) przestrzeni nazw. W przypadku aplikacji, które nie używają [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej), odwołanie [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet w pliku projektu.

`IChangeToken` ma dwie właściwości:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) wskazują, jeśli token z wyprzedzeniem zgłasza wywołań zwrotnych. Jeśli `ActiveChangedCallbacks` ustawiono `false`, nigdy nie jest wywoływana wywołania zwrotnego i aplikacja musi wykonać sondowanie `HasChanged` zmiany. Istnieje również możliwość token nigdy nie zostanie anulowana, jeśli nie zmian lub podstawowej odbiornik zmian jest usunięty lub wyłączona.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) pobiera wartość wskazującą, czy nastąpiła zmiana.

Interfejs ma jedną metodę [RegisterChangeCallback (akcji&lt;obiektu&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), który rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. `HasChanged` należy ustawić przed wywołaniem jest wywołania zwrotnego.

## <a name="changetoken-class"></a>Właściwości ChangeToken — klasa

`ChangeToken` Klasa statyczna służy do propagacji powiadomienia, że nastąpiła zmiana. `ChangeToken` znajduje się w [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) przestrzeni nazw. W przypadku aplikacji, które nie używają [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), odwołanie [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet w pliku projektu.

`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, akcji)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) metoda rejestrów `Action` wywołać przy każdej zmianie token:

* `Func<IChangeToken>` tworzy token.
* `Action` jest wywoływane, gdy zmienia tokenu.

`ChangeToken` ma [OnChange&lt;stanu dławienia&gt;(Func&lt;IChangeToken&gt;, Akcja&lt;stanu dławienia&gt;, stanu dławienia)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) przeciążenia, które przyjmuje dodatkowych `TState` parametr przekazywany do odbiorców tokenu `Action`.

`OnChange` Zwraca [IDisposable](/dotnet/api/system.idisposable). Wywoływanie [Dispose](/dotnet/api/system.idisposable.dispose) zatrzymuje token nasłuchiwania wprowadzanie dalszych zmian i zwalnia zasoby tokenu.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Przykład korzysta z tokenów zmiany w ASP.NET Core

Tokeny zmiany są używane w widocznym zakresie platformy ASP.NET Core monitorowania zmian do obiektów:

* Do monitorowania zmian w plikach, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)w [czujki](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda tworzy `IChangeToken` dla określonych plików lub folder do monitorowania.
* `IChangeToken` tokeny można dodać do wpisów pamięci podręcznej, aby wyzwolić wykluczenia pamięci podręcznej w przypadku zmiany.
* Dla `TOptions` zmienia domyślne [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementacja [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) ma przeciążenia, które akceptuje jeden lub więcej [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)wystąpień. Zwraca każde wystąpienie `IChangeToken` zarejestrować zwrotnego zmiany do zmiany opcji śledzenia.

## <a name="monitoring-for-configuration-changes"></a>Monitorowanie zmian konfiguracji

Domyślnie używają szablonów platformy ASP.NET Core [pliki konfiguracji w formacie JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings. Development.JSON*, i *appsettings. Production.JSON*) można załadować ustawień konfiguracji aplikacji.

Te pliki są skonfigurowane przy użyciu [AddJsonFile (IConfigurationBuilder, String, Boolean, wartość logiczna)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) — metoda rozszerzenia na [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) która akceptuje `reloadOnChange` parametr (ASP.NET Podstawowa 1.1 lub nowszej). `reloadOnChange` Wskazuje, jeśli konfiguracja powinna ładowane na zmiany pliku. Zobacz to ustawienie w [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) wygodne metody [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Na podstawie pliku konfiguracji jest reprezentowana przez [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` używa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) monitorować pliki.

Domyślnie `IFileMonitor` są dostarczane przez [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), który korzysta z [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) do monitorowania zmian w pliku konfiguracji.

Przykładowa aplikacja przedstawiono dwa implementacje monitorowania zmian konfiguracji. Jeśli dowolny *appsettings.json* zmiany pliku lub wersji środowiska pliku zmianę, każda implementacja wykonuje kod niestandardowy. Przykładowa aplikacja zapisuje komunikat do konsoli.

Plik konfiguracji `FileSystemWatcher` może wyzwolić wielu tokenów wywołań zwrotnych dla zmian w konfiguracji pojedynczego pliku. Przykładowe zastosowanie chroni przed ten problem, sprawdzając skróty plików w plikach konfiguracji. Sprawdzenie skróty plików gwarantuje, że co najmniej jeden z plików konfiguracyjnych zmieniła się przed uruchomieniem kodu niestandardowego. Przykład używa skrótu pliku SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Ponowna próba jest realizowana za pomocą wykładniczej wycofania. Spróbuj ponownie występuje, ponieważ blokowanie plików, może wystąpić uniemożliwiający tymczasowo obliczeniowych nowy skrót na jeden z plików.

### <a name="simple-startup-change-token"></a>Token zmiany proste uruchamiania

Zarejestruj odbiorców tokenu `Action` wywołania zwrotnego dla powiadomień o zmianie do tokenu ponownego załadowania konfiguracji (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` zawiera token. Wywołanie zwrotne jest `InvokeChanged` metody:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`state` Wywołania zwrotnego jest używany do przekazywania w `IHostingEnvironment`. Jest to przydatne w celu określenia odpowiedniego *appsettings* pliku JSON konfiguracji do monitorowania, *appsettings.&lt; Środowisko&gt;JSON*. Skróty plików są używane, aby zapobiec `WriteConsole` instrukcji uruchamiany wiele razy z powodu wielu tokenów wywołań zwrotnych gdy plik konfiguracji zostanie zmieniona tylko raz.

Ten system jest uruchamiany tak długo, jak aplikacja jest uruchomiona i nie może być wyłączone przez użytkownika.

### <a name="monitoring-configuration-changes-as-a-service"></a>Monitorowanie zmian konfiguracji jako usługa

Implementuje próbki:

* Uruchamianie podstawowych tokenu monitorowania.
* Monitorowanie jako usługa.
* Mechanizm, aby włączyć lub wyłączyć monitorowanie.

Ustanawia próbki `IConfigurationMonitor` interfejsu (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Konstruktor klasy zaimplementowane `ConfigurationMonitor`, rejestruje wywołanie zwrotne powiadomienia o zmianach:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` dostarcza token. `InvokeChanged` jest to metoda wywołania zwrotnego. `state` w tym wystąpieniu jest odwołaniem do `IConfigurationMonitor` wystąpienia, który służy do monitorowania stanu. Używane są dwie właściwości:

* `MonitoringEnabled` Wskazuje, czy wywołanie zwrotne uruchomione jego kod niestandardowy.
* `CurrentState` w tym artykule opisano bieżący stan monitorowania do użycia w interfejsie użytkownika.

`InvokeChanged` Metoda jest podobna do wcześniejszych podejście, ale:

* Nie działa jego kod, chyba że `MonitoringEnabled` jest `true`.
* Uwagi dotyczące bieżącego `state` w jego `WriteConsole` danych wyjściowych.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Wystąpienie `ConfigurationMonitor` jest zarejestrowany jako usługa w `ConfigureServices` z *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

Strona indeksu oferuje kontrola użytkownika nad konfiguracji monitorowania. Wystąpienie `IConfigurationMonitor` wstrzykuje się `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Przycisk włącza i wyłącza monitorowanie:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Gdy `OnPostStartMonitoring` jest wyzwalane, jest włączone monitorowanie, oraz bieżący stan jest usuwany. Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan ma ustawioną wartość odzwierciedlają, że nie jest wykonywane monitorowanie.

## <a name="monitoring-cached-file-changes"></a>Monitorowanie zmian plików w pamięci podręcznej

Zawartość pliku mogą być buforowane w pamięci przy użyciu [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Buforowanie w pamięci jest opisany w [pamięci podręcznej w pamięci](xref:performance/caching/memory) tematu. Bez wykonywania dodatkowych czynności, takich jak wykonanie opisanych poniżej, *starych* (przestarzałe) dane są zwracane z pamięci podręcznej, jeśli zmieni się źródło danych.

Nie biorąc pod uwagę stan pliku źródłowego w pamięci podręcznej podczas odnawiania [przedłużanie ważności](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) okres prowadzi do starych danych w pamięci podręcznej. Każde żądanie danych odnawia przesuwanego okresem wygaśnięcia, ale plik nie zostanie ponownie załadowana do pamięci podręcznej. Wszystkie funkcje aplikacji używających plików zawartości w pamięci podręcznej podlegają prawdopodobnie odbieranie zawartość.

Tokeny zmiany w pliku buforowanie scenariusz zapobiega starych plików zawartości w pamięci podręcznej. Przykładowa aplikacja przedstawia implementację metody.

W przykładzie użyto `GetFileContent` do:

* Zwraca zawartość pliku.
* Implementuje algorytmu ponownych prób z wykładniczej wycofania obejmujące przypadki, w którym blokady pliku tymczasowo uniemożliwia pliku odczytu.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

A `FileService` jest tworzone do obsługi plików w pamięci podręcznej wyszukiwania. `GetFileContent` Wywołanie metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i przywrócić go do wywołującego (*Services/FileService.cs*).

Jeśli zawartość z pamięci podręcznej nie zostanie odnaleziony, przy użyciu klucza pamięci podręcznej, są podejmowane następujące akcje:

1. Zawartość pliku są uzyskiwane przy użyciu `GetFileContent`.
1. Token zmiany są uzyskiwane z dostawcy plików z [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Wywołanie zwrotne tokenu jest wyzwalane, gdy plik jest modyfikowany.
1. Zawartość plików jest buforowana z [przedłużanie ważności](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) okresu. Token zmiany jest podłączonych do [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) do wykluczenia wpisu pamięci podręcznej, w przypadku zmiany pliku, gdy jest buforowany.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` Jest zarejestrowany w kontenerze usługi wraz z pamięci buforowanie usługi (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

Model strony ładuje zawartość pliku przy użyciu usługi (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Klasa CompositeChangeToken

Reprezentujący co najmniej jeden `IChangeToken` wystąpień w jeden obiekt, użyj [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) klasy.

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

`HasChanged` na złożone raporty tokenu `true` Jeśli żadnego reprezentowane token `HasChanged` jest `true`. `ActiveChangeCallbacks` na złożone raporty tokenu `true` Jeśli żadnego reprezentowane token `ActiveChangeCallbacks` jest `true`. Jeśli występuje wiele równoczesnych zmian zdarzeń, wywołania zwrotnego zmiany złożonego jest wywoływany dokładnie jeden raz.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Praca z rozproszoną pamięcią podręczną](xref:performance/caching/distributed)
* [Buforowanie odpowiedzi](xref:performance/caching/response)
* [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
* [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
