---
title: Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać tokenów zmian do śledzenia zmian.
ms.author: riande
ms.date: 11/10/2017
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: ac80e4c9da6d38e5f1acf8aa6f7ac3f4803ffb33
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927855"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="e763c-103">Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e763c-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="e763c-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e763c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e763c-105">A *zmienić token* jest ogólnego przeznaczenia, niskiego poziomu bloku konstrukcyjnego używane do śledzenia zmian.</span><span class="sxs-lookup"><span data-stu-id="e763c-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="e763c-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e763c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="e763c-107">Interfejs IChangeToken</span><span class="sxs-lookup"><span data-stu-id="e763c-107">IChangeToken interface</span></span>

<span data-ttu-id="e763c-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaguje powiadomienia, że nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="e763c-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="e763c-109">`IChangeToken` znajduje się w [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e763c-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="e763c-110">W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), odwołanie [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="e763c-110">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="e763c-111">`IChangeToken` ma dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="e763c-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="e763c-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) wskazują, jeśli token aktywnego generuje wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="e763c-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="e763c-113">Jeśli `ActiveChangedCallbacks` ustawiono `false`, wywołanie zwrotne nigdy nie jest wywoływana, a aplikacja musi wykonać sondowanie `HasChanged` zmian.</span><span class="sxs-lookup"><span data-stu-id="e763c-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="e763c-114">Istnieje także możliwość token nigdy nie zostać anulowana, jeśli występują bez zmian lub podstawowego odbiornika zmian jest usunięte lub wyłączone.</span><span class="sxs-lookup"><span data-stu-id="e763c-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="e763c-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) pobiera wartość wskazującą, jeśli nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="e763c-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="e763c-116">Interfejs ma jedną metodę, [RegisterChangeCallback (Akcja&lt;obiektu&gt;, obiekt)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), który rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="e763c-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="e763c-117">`HasChanged` należy ustawić przed wywołaniem zwrotnym.</span><span class="sxs-lookup"><span data-stu-id="e763c-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="e763c-118">Klasa właściwości ChangeToken</span><span class="sxs-lookup"><span data-stu-id="e763c-118">ChangeToken class</span></span>

<span data-ttu-id="e763c-119">`ChangeToken` Klasa statyczna służy do propagowania powiadomienia, że nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="e763c-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="e763c-120">`ChangeToken` znajduje się w [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e763c-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="e763c-121">W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), odwołanie [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="e763c-121">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="e763c-122">`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, akcji)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) rejestrów metoda `Action` do wywołania w każdym przypadku, gdy zmieni się tokenu:</span><span class="sxs-lookup"><span data-stu-id="e763c-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="e763c-123">`Func<IChangeToken>` tworzy token.</span><span class="sxs-lookup"><span data-stu-id="e763c-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="e763c-124">`Action` jest wywoływana, gdy zmieni się tokenu.</span><span class="sxs-lookup"><span data-stu-id="e763c-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="e763c-125">`ChangeToken` ma [OnChange&lt;stanu dławienia&gt;(Func&lt;IChangeToken&gt;, Akcja&lt;stanu dławienia&gt;, stanu dławienia)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) przeciążenia przyjmującego dodatkowego `TState` parametr, który jest przekazywany do odbiorców tokenu `Action`.</span><span class="sxs-lookup"><span data-stu-id="e763c-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="e763c-126">`OnChange` Zwraca [interfejsu IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="e763c-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="e763c-127">Wywoływanie [Dispose](/dotnet/api/system.idisposable.dispose) zatrzymuje token nasłuchiwanie wprowadzanie dalszych zmian i zwalnia zasoby tokenu.</span><span class="sxs-lookup"><span data-stu-id="e763c-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="e763c-128">Przykład używa tokenów zmiany w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e763c-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="e763c-129">Zmiana tokenów są używane w widocznym obszarach programu ASP.NET Core, monitorując zmiany do obiektów:</span><span class="sxs-lookup"><span data-stu-id="e763c-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="e763c-130">Do monitorowania zmian w plikach, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)firmy [Obejrzyj](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda tworzy `IChangeToken` dla określonych plików lub folder do obserwacji.</span><span class="sxs-lookup"><span data-stu-id="e763c-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="e763c-131">`IChangeToken` tokenów można dodać do wpisów pamięci podręcznej, aby wyzwolić Eksmisje pamięci podręcznej w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="e763c-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="e763c-132">Aby uzyskać `TOptions` zmienia domyślny [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementacji [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) musi być przeciążona, który akceptuje co najmniej jeden [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)wystąpień.</span><span class="sxs-lookup"><span data-stu-id="e763c-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="e763c-133">Każde wystąpienie, zwraca `IChangeToken` zarejestrować zmiany wywołania zwrotnego dla śledzenia opcje zmiany.</span><span class="sxs-lookup"><span data-stu-id="e763c-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="e763c-134">Monitorowanie zmian konfiguracji</span><span class="sxs-lookup"><span data-stu-id="e763c-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="e763c-135">Domyślnie używają szablony ASP.NET Core [pliki konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, i *appsettings. Production.JSON*) można załadować ustawień konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e763c-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="e763c-136">Te pliki są skonfigurowane przy użyciu [AddJsonFile (IConfigurationBuilder, String, Boolean, atrybut typu wartość logiczna)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) metody rozszerzenia [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) akceptujący `reloadOnChange` parametru (ASP.NET Core 1.1 i nowszych).</span><span class="sxs-lookup"><span data-stu-id="e763c-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="e763c-137">`reloadOnChange` Wskazuje, jeśli konfiguracja powinna ładowane na zmiany w plikach.</span><span class="sxs-lookup"><span data-stu-id="e763c-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="e763c-138">Zobacz to ustawienie w [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) wygodna metoda [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span><span class="sxs-lookup"><span data-stu-id="e763c-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="e763c-139">Konfiguracja na podstawie pliku jest reprezentowany przez [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="e763c-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="e763c-140">`FileConfigurationSource` używa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) monitorować pliki.</span><span class="sxs-lookup"><span data-stu-id="e763c-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="e763c-141">Domyślnie `IFileMonitor` są dostarczane przez [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), który używa [klasy FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) do monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e763c-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="e763c-142">Przykładowa aplikacja pokazuje dwie implementacje dla monitorowania zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e763c-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="e763c-143">Jeśli *appsettings.json* zmiany zmiany plików lub środowiska wersję pliku, każda implementacja wykonuje kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="e763c-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="e763c-144">Przykładowa aplikacja zapisuje komunikat do konsoli.</span><span class="sxs-lookup"><span data-stu-id="e763c-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="e763c-145">Plik konfiguracji `FileSystemWatcher` może wyzwolić wiele tokenów wywołań zwrotnych zmiana konfiguracji pojedynczego pliku.</span><span class="sxs-lookup"><span data-stu-id="e763c-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="e763c-146">Przykład implementacji chroni przed tym problemem, sprawdzając skróty plików w plikach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e763c-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="e763c-147">Sprawdzanie skróty plików gwarantuje, że co najmniej jeden z plików konfiguracji został zmieniony przed uruchomieniem kodu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="e763c-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="e763c-148">W przykładzie użyto SHA1 pliku wyznaczania wartości skrótu (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="e763c-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="e763c-149">Ponowienie próby jest implementowane za pomocą wykładniczego wycofywania.</span><span class="sxs-lookup"><span data-stu-id="e763c-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="e763c-150">Ponów próbę występuje, ponieważ blokowanie plików może wystąpić, uniemożliwiające tymczasowo obliczeń nowy skrót na jeden z plików.</span><span class="sxs-lookup"><span data-stu-id="e763c-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="e763c-151">Uruchamianie prostego Zmień token</span><span class="sxs-lookup"><span data-stu-id="e763c-151">Simple startup change token</span></span>

<span data-ttu-id="e763c-152">Zarejestruj odbiorców tokenu `Action` wywołanie zwrotne dla powiadomień o zmianie do tokenu ponowne załadowanie konfiguracji (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="e763c-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="e763c-153">`config.GetReloadToken()` zawiera token.</span><span class="sxs-lookup"><span data-stu-id="e763c-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="e763c-154">Wywołanie zwrotne jest `InvokeChanged` metody:</span><span class="sxs-lookup"><span data-stu-id="e763c-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="e763c-155">`state` Wywołania zwrotnego jest używany do przekazywania `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="e763c-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="e763c-156">Dzięki takiemu grupowaniu można określić poprawny *appsettings* pliku JSON konfiguracji, aby monitorować, *appsettings.&lt; Środowisko&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="e763c-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="e763c-157">Skróty plików są używane, aby zapobiec `WriteConsole` instrukcji uruchamianie wielokrotne z powodu wielu tokenów wywołania zwrotne, gdy plik konfiguracji został zmieniony tylko raz.</span><span class="sxs-lookup"><span data-stu-id="e763c-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="e763c-158">Ten system działa tak długo, jak aplikacja jest uruchomiona i nie może być wyłączony przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e763c-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="e763c-159">Monitorowanie zmian w konfiguracji jako usługa</span><span class="sxs-lookup"><span data-stu-id="e763c-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="e763c-160">Implementuje przykładu:</span><span class="sxs-lookup"><span data-stu-id="e763c-160">The sample implements:</span></span>

* <span data-ttu-id="e763c-161">Podstawowe tokenu monitorowanie uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e763c-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="e763c-162">Monitorowanie jako usługa.</span><span class="sxs-lookup"><span data-stu-id="e763c-162">Monitoring as a service.</span></span>
* <span data-ttu-id="e763c-163">Mechanizm do włączania i wyłączania monitorowania.</span><span class="sxs-lookup"><span data-stu-id="e763c-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="e763c-164">Przykład ustanawia `IConfigurationMonitor` interfejsu (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="e763c-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="e763c-165">Konstruktor klasy zaimplementowane `ConfigurationMonitor`, rejestruje wywołanie zwrotne dla powiadomień o zmianie:</span><span class="sxs-lookup"><span data-stu-id="e763c-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="e763c-166">`config.GetReloadToken()` dostarcza token.</span><span class="sxs-lookup"><span data-stu-id="e763c-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="e763c-167">`InvokeChanged` jest metodą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="e763c-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="e763c-168">`state` w tym wystąpieniu jest odwołaniem do `IConfigurationMonitor` wystąpienie, które umożliwia dostęp do monitorowania stanu.</span><span class="sxs-lookup"><span data-stu-id="e763c-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that is used to access the monitoring state.</span></span> <span data-ttu-id="e763c-169">Są używane dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="e763c-169">Two properties are used:</span></span>

* <span data-ttu-id="e763c-170">`MonitoringEnabled` Wskazuje, wywołanie zwrotne powinien uruchamiać jego kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="e763c-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="e763c-171">`CurrentState` w tym artykule opisano bieżący stan monitorowania do użytku w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e763c-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="e763c-172">`InvokeChanged` Metoda jest podobna do wcześniejszych podejście, z wyjątkiem że:</span><span class="sxs-lookup"><span data-stu-id="e763c-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="e763c-173">Nie działa kod, chyba że `MonitoringEnabled` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="e763c-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="e763c-174">Informacje o bieżącej `state` w jego `WriteConsole` danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="e763c-174">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="e763c-175">Wystąpienie `ConfigurationMonitor` jest zarejestrowany jako usługa w `ConfigureServices` z *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e763c-175">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e763c-176">Strona indeksu oferuje kontrolki użytkownika za pośrednictwem konfiguracji monitorowania.</span><span class="sxs-lookup"><span data-stu-id="e763c-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="e763c-177">Wystąpienie `IConfigurationMonitor` są wstrzykiwane do `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="e763c-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="e763c-178">Przycisk włącza i wyłącza monitorowanie:</span><span class="sxs-lookup"><span data-stu-id="e763c-178">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="e763c-179">Gdy `OnPostStartMonitoring` jest wyzwalane, jest włączone monitorowanie, oraz bieżący stan jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="e763c-179">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="e763c-180">Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony, aby odzwierciedlić, że nie ma miejsce monitorowania.</span><span class="sxs-lookup"><span data-stu-id="e763c-180">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="e763c-181">Monitorowanie zmian plików w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="e763c-181">Monitoring cached file changes</span></span>

<span data-ttu-id="e763c-182">Zawartość pliku może być buforowany w pamięci przy użyciu [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="e763c-182">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="e763c-183">Buforowanie w pamięci jest opisana w [pamięci podręcznej w pamięci](xref:performance/caching/memory) tematu.</span><span class="sxs-lookup"><span data-stu-id="e763c-183">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="e763c-184">Bez konieczności wykonania dodatkowych kroków, takich jak wykonanie opisanych poniżej, *starych* (przestarzałe) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zmienią się.</span><span class="sxs-lookup"><span data-stu-id="e763c-184">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="e763c-185">Nie biorąc pod uwagę stan pliku źródłowego w pamięci podręcznej podczas odnawiania [przedłużanie ważności](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) okres prowadzi do starych danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="e763c-185">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="e763c-186">Każde żądanie danych odnawia przewijania okres ważności, ale plik nigdy nie zostanie ponownie załadowana do pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="e763c-186">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="e763c-187">Wszystkie funkcje aplikacji używających zawartość pamięci podręcznej podlegają prawdopodobnie odbiera zawartość.</span><span class="sxs-lookup"><span data-stu-id="e763c-187">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="e763c-188">Przy użyciu tokenów zmiany w pliku pamięci podręcznej scenariusz zapobiega starych plików zawartości w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="e763c-188">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="e763c-189">Przykładowa aplikacja pokazuje implementację metody.</span><span class="sxs-lookup"><span data-stu-id="e763c-189">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="e763c-190">W przykładzie użyto `GetFileContent` do:</span><span class="sxs-lookup"><span data-stu-id="e763c-190">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="e763c-191">Zwraca zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="e763c-191">Return file content.</span></span>
* <span data-ttu-id="e763c-192">Implementowanie ponownych prób algorytmu, gdy wycofywanie wykładnicze obejmujące przypadki, gdzie blokady pliku tymczasowo uniemożliwia pliku odczytywany.</span><span class="sxs-lookup"><span data-stu-id="e763c-192">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="e763c-193">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="e763c-193">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="e763c-194">Element `FileService` jest utworzona w celu obsługi wyszukiwania plików z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="e763c-194">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="e763c-195">`GetFileContent` Wywołania metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i przywrócić go do obiektu wywołującego (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="e763c-195">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="e763c-196">Jeśli zawartość z pamięci podręcznej nie zostanie znaleziona, przy użyciu klucza pamięci podręcznej, są podejmowane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="e763c-196">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="e763c-197">Zawartość pliku są uzyskiwane przy użyciu `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="e763c-197">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="e763c-198">Token zmiany są uzyskiwane z dostawcy plików za pomocą [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="e763c-198">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="e763c-199">Wywołanie zwrotne tokenu jest wyzwalany, gdy plik zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="e763c-199">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="e763c-200">Zawartość plików jest buforowana przy użyciu [przedłużanie ważności](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) okres.</span><span class="sxs-lookup"><span data-stu-id="e763c-200">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="e763c-201">Zmień token jest połączona z [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) do wykluczenia wpisu pamięci podręcznej, jeśli plik ulegnie zmianie podczas, gdy jest ona buforowana.</span><span class="sxs-lookup"><span data-stu-id="e763c-201">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="e763c-202">`FileService` Jest zarejestrowany w kontenerze usługi wraz z pamięci, pamięci podręcznej usługi (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="e763c-202">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="e763c-203">Model strona ładuje zawartość pliku przy użyciu usługi (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="e763c-203">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="e763c-204">Klasa CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="e763c-204">CompositeChangeToken class</span></span>

<span data-ttu-id="e763c-205">Do reprezentowania jednego lub więcej `IChangeToken` Użyj wystąpień w pojedynczy obiekt [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) klasy.</span><span class="sxs-lookup"><span data-stu-id="e763c-205">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="e763c-206">`HasChanged` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `HasChanged` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="e763c-206">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="e763c-207">`ActiveChangeCallbacks` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `ActiveChangeCallbacks` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="e763c-207">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="e763c-208">Jeśli występuje wiele równoczesnych zmian zdarzeń, wywołania zwrotnego zmian złożone jest wywoływane dokładnie jeden raz.</span><span class="sxs-lookup"><span data-stu-id="e763c-208">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e763c-209">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e763c-209">Additional resources</span></span>

* [<span data-ttu-id="e763c-210">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="e763c-210">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="e763c-211">Praca z rozproszoną pamięcią podręczną</span><span class="sxs-lookup"><span data-stu-id="e763c-211">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="e763c-212">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="e763c-212">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="e763c-213">Oprogramowanie pośredniczące buforowania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="e763c-213">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="e763c-214">Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="e763c-214">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="e763c-215">Pomocnik tagu rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="e763c-215">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
