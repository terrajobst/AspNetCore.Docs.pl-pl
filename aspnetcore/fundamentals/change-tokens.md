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
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="25969-103">Wykrywanie zmian za pomocą tokenów zmiany w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25969-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="25969-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="25969-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="25969-105">A *zmienić token* jest ogólnego przeznaczenia, niskiego poziomu bloku konstrukcyjnego używane do śledzenia zmian stanu.</span><span class="sxs-lookup"><span data-stu-id="25969-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="25969-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="25969-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="25969-107">Interfejs IChangeToken</span><span class="sxs-lookup"><span data-stu-id="25969-107">IChangeToken interface</span></span>

<span data-ttu-id="25969-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propaguje powiadomienia, że nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="25969-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="25969-109">`IChangeToken` znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="25969-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="25969-110">W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), utworzyć odwołania do pakietu dla [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="25969-110">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="25969-111">`IChangeToken` ma dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="25969-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="25969-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> Wskazuje, jeśli token aktywnego generuje wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="25969-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="25969-113">Jeśli `ActiveChangedCallbacks` ustawiono `false`, wywołanie zwrotne nigdy nie jest wywoływana, a aplikacja musi wykonać sondowanie `HasChanged` zmian.</span><span class="sxs-lookup"><span data-stu-id="25969-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="25969-114">Istnieje także możliwość token nigdy nie zostać anulowana, jeśli występują bez zmian lub podstawowego odbiornika zmian jest usunięte lub wyłączone.</span><span class="sxs-lookup"><span data-stu-id="25969-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="25969-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> odbiera wartość, która wskazuje, jeśli nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="25969-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="25969-116">`IChangeToken` Interfejs zawiera [RegisterChangeCallback (Akcja\<obiektu >, obiekt)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) metody, która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="25969-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="25969-117">`HasChanged` należy ustawić przed wywołaniem zwrotnym.</span><span class="sxs-lookup"><span data-stu-id="25969-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="25969-118">Klasa właściwości ChangeToken</span><span class="sxs-lookup"><span data-stu-id="25969-118">ChangeToken class</span></span>

<span data-ttu-id="25969-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> Klasa statyczna służy do propagowania powiadomienia, że nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="25969-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="25969-120">`ChangeToken` znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="25969-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="25969-121">W przypadku aplikacji, które nie używają [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), utworzyć odwołania do pakietu dla [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="25969-121">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="25969-122">[ChangeToken.OnChange (Func\<IChangeToken >, akcji)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestrów metoda `Action` do wywołania w każdym przypadku, gdy zmieni się tokenu:</span><span class="sxs-lookup"><span data-stu-id="25969-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="25969-123">`Func<IChangeToken>` tworzy token.</span><span class="sxs-lookup"><span data-stu-id="25969-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="25969-124">`Action` jest wywoływana, gdy zmieni się tokenu.</span><span class="sxs-lookup"><span data-stu-id="25969-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="25969-125">[ChangeToken.OnChange\<stanu dławienia > (Func\<IChangeToken >, Akcja\<stanu dławienia >, stanu dławienia)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) przeciążenia zajmuje dodatkowy `TState` parametr, który jest przekazywany do tokenu konsument `Action`.</span><span class="sxs-lookup"><span data-stu-id="25969-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="25969-126">`OnChange` Zwraca <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="25969-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="25969-127">Wywoływanie <xref:System.IDisposable.Dispose*> zatrzymuje token nasłuchiwanie wprowadzanie dalszych zmian i zwalnia zasoby tokenu.</span><span class="sxs-lookup"><span data-stu-id="25969-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="25969-128">Przykład używa tokenów zmiany w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25969-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="25969-129">Zmiana tokenów są używane w widocznym obszarach programu ASP.NET Core do monitorowania zmian do obiektów:</span><span class="sxs-lookup"><span data-stu-id="25969-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="25969-130">Do monitorowania zmian w plikach, <xref:Microsoft.Extensions.FileProviders.IFileProvider>firmy <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> metoda tworzy `IChangeToken` dla określonych plików lub folder do obserwacji.</span><span class="sxs-lookup"><span data-stu-id="25969-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="25969-131">`IChangeToken` tokenów można dodać do wpisów pamięci podręcznej, aby wyzwolić Eksmisje pamięci podręcznej w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="25969-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="25969-132">Dla `TOptions` zmienia domyślny <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacji <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> musi być przeciążona, który akceptuje co najmniej jeden <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="25969-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="25969-133">Każde wystąpienie, zwraca `IChangeToken` zarejestrować zmiany wywołania zwrotnego dla śledzenia opcje zmiany.</span><span class="sxs-lookup"><span data-stu-id="25969-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="25969-134">Monitorowanie zmian konfiguracji</span><span class="sxs-lookup"><span data-stu-id="25969-134">Monitor for configuration changes</span></span>

<span data-ttu-id="25969-135">Domyślnie używają szablony ASP.NET Core [pliki konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, i *appsettings. Production.JSON*) można załadować ustawień konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25969-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="25969-136">Te pliki są skonfigurowane przy użyciu [AddJsonFile (IConfigurationBuilder, String, Boolean, atrybut typu wartość logiczna)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) metody rozszerzenia <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> akceptujący `reloadOnChange` parametru.</span><span class="sxs-lookup"><span data-stu-id="25969-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="25969-137">`reloadOnChange` Wskazuje, jeśli konfiguracja powinna ładowane na zmiany w plikach.</span><span class="sxs-lookup"><span data-stu-id="25969-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="25969-138">To ustawienie jest wyświetlane w <xref:Microsoft.AspNetCore.WebHost> wygodna metoda <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="25969-138">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="25969-139">Konfiguracja na podstawie pliku jest reprezentowany przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="25969-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="25969-140">`FileConfigurationSource` używa <xref:Microsoft.Extensions.FileProviders.IFileProvider> monitorować pliki.</span><span class="sxs-lookup"><span data-stu-id="25969-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="25969-141">Domyślnie `IFileMonitor` są dostarczane przez <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, który używa <xref:System.IO.FileSystemWatcher> do monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25969-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="25969-142">Przykładowa aplikacja pokazuje dwie implementacje dla monitorowania zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25969-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="25969-143">Jeśli dowolny z *appsettings* zmiany plików, należy wykonać obie monitorowania implementacje pliku niestandardowego kodu&mdash;Przykładowa aplikacja zapisuje komunikat do konsoli.</span><span class="sxs-lookup"><span data-stu-id="25969-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="25969-144">Plik konfiguracji `FileSystemWatcher` może wyzwolić wiele tokenów wywołań zwrotnych zmiana konfiguracji pojedynczego pliku.</span><span class="sxs-lookup"><span data-stu-id="25969-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="25969-145">Aby upewnić się, kod niestandardowy tylko jest uruchamiany po w przypadku wyzwolenia wielu wywołań zwrotnych tokenu, przykład implementacji sprawdza, czy skróty plików.</span><span class="sxs-lookup"><span data-stu-id="25969-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="25969-146">W przykładzie użyto SHA1 pliku wyznaczania wartości skrótu.</span><span class="sxs-lookup"><span data-stu-id="25969-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="25969-147">Ponowienie próby jest implementowane za pomocą wykładniczego wycofywania.</span><span class="sxs-lookup"><span data-stu-id="25969-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="25969-148">Ponowienie próby jest obecny, ponieważ blokowanie plików może wystąpić, uniemożliwiające tymczasowo Obliczanie nowego skrótu w pliku.</span><span class="sxs-lookup"><span data-stu-id="25969-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="25969-149">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="25969-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="25969-150">Uruchamianie prostego Zmień token</span><span class="sxs-lookup"><span data-stu-id="25969-150">Simple startup change token</span></span>

<span data-ttu-id="25969-151">Zarejestruj odbiorców tokenu `Action` wywołanie zwrotne dla powiadomień o zmianie do tokenu ponowne załadowanie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25969-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="25969-152">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="25969-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="25969-153">`config.GetReloadToken()` zawiera token.</span><span class="sxs-lookup"><span data-stu-id="25969-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="25969-154">Wywołanie zwrotne jest `InvokeChanged` metody:</span><span class="sxs-lookup"><span data-stu-id="25969-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="25969-155">`state` Wywołania zwrotnego jest używany do przekazywania `IHostingEnvironment`, co jest przydatne do określenia prawidłowego *appsettings* pliku konfiguracji monitorowania (na przykład *appsettings. Development.JSON* w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="25969-155">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="25969-156">Skróty plików są używane, aby zapobiec `WriteConsole` instrukcji uruchamianie wielokrotne z powodu wielu tokenów wywołania zwrotne, gdy plik konfiguracji został zmieniony tylko raz.</span><span class="sxs-lookup"><span data-stu-id="25969-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="25969-157">Ten system działa tak długo, jak aplikacja jest uruchomiona i nie może być wyłączony przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="25969-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="25969-158">Monitorowanie zmian konfiguracji jako usługa</span><span class="sxs-lookup"><span data-stu-id="25969-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="25969-159">Implementuje przykładu:</span><span class="sxs-lookup"><span data-stu-id="25969-159">The sample implements:</span></span>

* <span data-ttu-id="25969-160">Podstawowe tokenu monitorowanie uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="25969-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="25969-161">Monitorowanie jako usługa.</span><span class="sxs-lookup"><span data-stu-id="25969-161">Monitoring as a service.</span></span>
* <span data-ttu-id="25969-162">Mechanizm do włączania i wyłączania monitorowania.</span><span class="sxs-lookup"><span data-stu-id="25969-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="25969-163">Przykład ustanawia `IConfigurationMonitor` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="25969-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="25969-164">*Extensions/ConfigurationMonitor.cs*:</span><span class="sxs-lookup"><span data-stu-id="25969-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="25969-165">Konstruktor klasy zaimplementowane `ConfigurationMonitor`, rejestruje wywołanie zwrotne dla powiadomień o zmianie:</span><span class="sxs-lookup"><span data-stu-id="25969-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="25969-166">`config.GetReloadToken()` dostarcza token.</span><span class="sxs-lookup"><span data-stu-id="25969-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="25969-167">`InvokeChanged` jest metodą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="25969-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="25969-168">`state` w tym wystąpieniu jest odwołaniem do `IConfigurationMonitor` wystąpienie, które umożliwia dostęp do monitorowania stanu.</span><span class="sxs-lookup"><span data-stu-id="25969-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="25969-169">Są używane dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="25969-169">Two properties are used:</span></span>

* <span data-ttu-id="25969-170">`MonitoringEnabled` &ndash; Wskazuje, wywołanie zwrotne powinien uruchamiać jego kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="25969-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="25969-171">`CurrentState` &ndash; w tym artykule opisano bieżący stan monitorowania do użytku w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="25969-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="25969-172">`InvokeChanged` Metoda jest podobna do wcześniejszych podejście, z wyjątkiem że:</span><span class="sxs-lookup"><span data-stu-id="25969-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="25969-173">Nie działa kod, chyba że `MonitoringEnabled` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="25969-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="25969-174">Wyświetla bieżący `state` w jego `WriteConsole` danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="25969-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="25969-175">Wystąpienie `ConfigurationMonitor` jest zarejestrowany jako usługa w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25969-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="25969-176">Strona indeksu oferuje kontrolki użytkownika za pośrednictwem konfiguracji monitorowania.</span><span class="sxs-lookup"><span data-stu-id="25969-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="25969-177">Wystąpienie `IConfigurationMonitor` są wstrzykiwane do `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="25969-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="25969-178">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="25969-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="25969-179">Monitor konfiguracji (`_monitor`) umożliwia włączanie lub wyłączanie monitorowania i Ustaw bieżący stan opinii o interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="25969-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="25969-180">Gdy `OnPostStartMonitoring` jest wyzwalane, jest włączone monitorowanie, oraz bieżący stan jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="25969-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="25969-181">Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony, aby odzwierciedlić, że nie ma miejsce monitorowania.</span><span class="sxs-lookup"><span data-stu-id="25969-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="25969-182">Przyciski w interfejsie użytkownika Włączanie i wyłączanie monitorowania.</span><span class="sxs-lookup"><span data-stu-id="25969-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="25969-183">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25969-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="25969-184">Monitorowanie zmian plików w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="25969-184">Monitor cached file changes</span></span>

<span data-ttu-id="25969-185">Zawartość pliku może być buforowany w pamięci przy użyciu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="25969-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="25969-186">Buforowanie w pamięci jest opisana w [pamięci podręcznej w pamięci](xref:performance/caching/memory) tematu.</span><span class="sxs-lookup"><span data-stu-id="25969-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="25969-187">Bez konieczności wykonania dodatkowych kroków, takich jak wykonanie opisanych poniżej, *starych* (przestarzałe) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zmienią się.</span><span class="sxs-lookup"><span data-stu-id="25969-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="25969-188">Na przykład, nie biorąc pod uwagę stan pliku źródłowego w pamięci podręcznej podczas odnawiania [przedłużanie ważności](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okres prowadzi do starych plików w pamięci podręcznej danych.</span><span class="sxs-lookup"><span data-stu-id="25969-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="25969-189">Każde żądanie danych odnawia przewijania okres ważności, ale plik nigdy nie zostanie ponownie załadowana do pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="25969-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="25969-190">Wszystkie funkcje aplikacji używających zawartość pamięci podręcznej podlegają prawdopodobnie odbiera zawartość.</span><span class="sxs-lookup"><span data-stu-id="25969-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="25969-191">Przy użyciu tokenów zmiany w pliku pamięci podręcznej scenariusz zapobiega obecność starych plików zawartości w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="25969-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="25969-192">Przykładowa aplikacja pokazuje implementację metody.</span><span class="sxs-lookup"><span data-stu-id="25969-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="25969-193">W przykładzie użyto `GetFileContent` do:</span><span class="sxs-lookup"><span data-stu-id="25969-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="25969-194">Zwraca zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="25969-194">Return file content.</span></span>
* <span data-ttu-id="25969-195">Implementowanie ponownych prób algorytmu, gdy wycofywanie wykładnicze obejmujące przypadki, gdzie plik blokada tymczasowo zapobiega Odczyt pliku.</span><span class="sxs-lookup"><span data-stu-id="25969-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="25969-196">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="25969-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="25969-197">Element `FileService` jest utworzona w celu obsługi wyszukiwania plików z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="25969-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="25969-198">`GetFileContent` Wywołania metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i przywrócić go do obiektu wywołującego (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="25969-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="25969-199">Jeśli zawartość z pamięci podręcznej nie zostanie znaleziona, przy użyciu klucza pamięci podręcznej, są podejmowane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="25969-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="25969-200">Zawartość pliku są uzyskiwane przy użyciu `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="25969-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="25969-201">Token zmiany są uzyskiwane z dostawcy plików za pomocą [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="25969-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="25969-202">Wywołanie zwrotne tokenu jest wyzwalany, gdy plik zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="25969-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="25969-203">Zawartość plików jest buforowana przy użyciu [przedłużanie ważności](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okres.</span><span class="sxs-lookup"><span data-stu-id="25969-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="25969-204">Zmień token jest połączona z [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) do wykluczenia wpisu pamięci podręcznej, jeśli plik ulegnie zmianie podczas, gdy jest ona buforowana.</span><span class="sxs-lookup"><span data-stu-id="25969-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="25969-205">W poniższym przykładzie pliki są przechowywane w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25969-205">In the following example, files are stored in the app's content root.</span></span> <span data-ttu-id="25969-206">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) służy do uzyskiwania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazywanym przez aplikację <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span><span class="sxs-lookup"><span data-stu-id="25969-206">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="25969-207">`filePath` Są uzyskiwane z [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="25969-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="25969-208">`FileService` Jest zarejestrowany w kontenerze usługi wraz z pamięci, pamięci podręcznej usługi.</span><span class="sxs-lookup"><span data-stu-id="25969-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="25969-209">W `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25969-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="25969-210">Model strona ładuje zawartość pliku przy użyciu usługi.</span><span class="sxs-lookup"><span data-stu-id="25969-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="25969-211">Na stronie indeksu `OnGet` — metoda (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="25969-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="25969-212">Klasa CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="25969-212">CompositeChangeToken class</span></span>

<span data-ttu-id="25969-213">Do reprezentowania jednego lub więcej `IChangeToken` Użyj wystąpień w pojedynczy obiekt <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> klasy.</span><span class="sxs-lookup"><span data-stu-id="25969-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="25969-214">`HasChanged` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `HasChanged` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="25969-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="25969-215">`ActiveChangeCallbacks` w złożonych raportów tokenu `true` Jeśli dowolny reprezentowane token `ActiveChangeCallbacks` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="25969-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="25969-216">Jeśli występuje wiele równoczesnych zmian zdarzeń, wywołanie zwrotne zmiany złożonego jest wywoływana jeden raz.</span><span class="sxs-lookup"><span data-stu-id="25969-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25969-217">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="25969-217">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
