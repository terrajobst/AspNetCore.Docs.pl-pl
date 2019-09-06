---
title: Wykrywanie zmian przy użyciu tokenów zmiany w ASP.NET Core
author: guardrex
description: Dowiedz się, jak śledzić zmiany przy użyciu tokenów zmian.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/27/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 86cde7b60f5c398fc6bb215b593643c05565cf3c
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384714"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="37221-103">Wykrywanie zmian przy użyciu tokenów zmiany w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37221-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="37221-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37221-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="37221-105">*Tokenem zmiany* jest ogólnym blokiem konstrukcyjnym, który służy do śledzenia zmian stanu.</span><span class="sxs-lookup"><span data-stu-id="37221-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="37221-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37221-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="37221-107">IChangeToken, interfejs</span><span class="sxs-lookup"><span data-stu-id="37221-107">IChangeToken interface</span></span>

<span data-ttu-id="37221-108"><xref:Microsoft.Extensions.Primitives.IChangeToken>propaguje powiadomienia o wystąpieniu zmiany.</span><span class="sxs-lookup"><span data-stu-id="37221-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="37221-109">`IChangeToken`znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="37221-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37221-110">Pakiet NuGet [Microsoft. Extensions.](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) jest niejawnie dostarczany do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37221-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="37221-111">`IChangeToken`ma dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="37221-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="37221-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks>wskaż, czy token aktywnie wywołuje wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="37221-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="37221-113">Jeśli `ActiveChangedCallbacks` jest ustawiona na `false`, wywołanie zwrotne nigdy nie zostanie wywołane, a aplikacja `HasChanged` musi sondować pod kątem zmian.</span><span class="sxs-lookup"><span data-stu-id="37221-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="37221-114">Istnieje również możliwość, że token nigdy nie zostanie anulowany, jeśli nie wystąpią żadne zmiany lub zostanie usunięty lub wyłączony odbiornik zmian.</span><span class="sxs-lookup"><span data-stu-id="37221-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="37221-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>odbiera wartość wskazującą, czy nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="37221-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="37221-116">Interfejs zawiera metodę [RegisterChangeCallback (obiekt akcji\<>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) , która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. `IChangeToken`</span><span class="sxs-lookup"><span data-stu-id="37221-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="37221-117">`HasChanged`musi być ustawiona przed wywołaniem wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="37221-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="37221-118">Klasa ChangeToken</span><span class="sxs-lookup"><span data-stu-id="37221-118">ChangeToken class</span></span>

<span data-ttu-id="37221-119"><xref:Microsoft.Extensions.Primitives.ChangeToken>jest klasą statyczną służącą do propagowania powiadomień, w których wystąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="37221-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="37221-120">`ChangeToken`znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="37221-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37221-121">Pakiet NuGet [Microsoft. Extensions.](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) jest niejawnie dostarczany do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37221-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="37221-122">[ChangeToken. OnChange (\<Func IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestruje `Action` metodę, aby wywołać za każdym razem, gdy zmieni się token:</span><span class="sxs-lookup"><span data-stu-id="37221-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="37221-123">`Func<IChangeToken>`tworzy token.</span><span class="sxs-lookup"><span data-stu-id="37221-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="37221-124">`Action`jest wywoływana, gdy zostanie zmieniony token.</span><span class="sxs-lookup"><span data-stu-id="37221-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="37221-125">[ChangeToken. OnChange\<TState > (\<Func IChangeToken >,\<Action TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) przyjmuje dodatkowy `TState` parametr, który jest przesyłany do odbiorcy `Action` tokenu .</span><span class="sxs-lookup"><span data-stu-id="37221-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="37221-126">`OnChange`Zwraca wartość <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="37221-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="37221-127">Wywołanie <xref:System.IDisposable.Dispose*> uniemożliwia wysłuchanie tokenu w celu wprowadzenia dalszych zmian i zwolnienia zasobów tokenu.</span><span class="sxs-lookup"><span data-stu-id="37221-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="37221-128">Przykładowe zastosowania tokenów zmiany w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37221-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="37221-129">Tokeny zmiany są używane w widocznych obszarach ASP.NET Core do monitorowania zmian obiektów:</span><span class="sxs-lookup"><span data-stu-id="37221-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="37221-130">W przypadku monitorowania zmian w plikach <xref:Microsoft.Extensions.FileProviders.IFileProvider> <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> Metoda tworzy `IChangeToken` dla określonych plików lub folderów, które mają być monitorowane.</span><span class="sxs-lookup"><span data-stu-id="37221-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="37221-131">`IChangeToken`można dodać tokeny do wpisów pamięci podręcznej, aby wyzwolić wykluczenia z pamięci podręcznej w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="37221-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="37221-132">W `TOptions` przypadku zmian Domyślna <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacja <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> maPrzeciążenie,któreakceptujeconajmniejjednowystąpienie.<xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1></span><span class="sxs-lookup"><span data-stu-id="37221-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="37221-133">Każde wystąpienie zwraca wartość `IChangeToken` , aby zarejestrować zmiany zwrotne powiadomień o zmianach opcji śledzenia.</span><span class="sxs-lookup"><span data-stu-id="37221-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="37221-134">Monitorowanie zmian konfiguracji</span><span class="sxs-lookup"><span data-stu-id="37221-134">Monitor for configuration changes</span></span>

<span data-ttu-id="37221-135">Domyślnie szablony ASP.NET Core korzystają z [plików konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, *appSettings. Development. JSON*i *appSettings. Production. JSON*) w celu załadowania ustawień konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="37221-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="37221-136">Te pliki są konfigurowane przy użyciu metody <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> rozszerzenia [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) , która akceptuje `reloadOnChange` parametr.</span><span class="sxs-lookup"><span data-stu-id="37221-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="37221-137">`reloadOnChange`wskazuje, czy należy ponownie załadować konfigurację w przypadku zmian w pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="37221-138">To ustawienie jest dostępne w <xref:Microsoft.Extensions.Hosting.Host> metodzie <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>wygodnej:</span><span class="sxs-lookup"><span data-stu-id="37221-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="37221-139">Konfiguracja oparta na plikach jest reprezentowana przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="37221-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="37221-140">`FileConfigurationSource`Program <xref:Microsoft.Extensions.FileProviders.IFileProvider> używa do monitorowania plików.</span><span class="sxs-lookup"><span data-stu-id="37221-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="37221-141">Domyślnie `IFileMonitor` jest on <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>dostarczany przez, który <xref:System.IO.FileSystemWatcher> służy do monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37221-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="37221-142">Przykładowa aplikacja przedstawia dwie implementacje monitorowania zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37221-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="37221-143">Jeśli którykolwiek z plików *AppSettings* zostanie zmieniony, obydwie implementacje monitorowania plików wykonują kod&mdash;niestandardowy, a Przykładowa aplikacja zapisuje komunikat w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="37221-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="37221-144">Plik `FileSystemWatcher` konfiguracji może wyzwolić wiele wywołań zwrotnych tokenów dla jednej zmiany pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37221-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="37221-145">Aby upewnić się, że kod niestandardowy jest uruchamiany tylko raz w przypadku wyzwolenia wielu wywołań zwrotnych tokenów, implementacja próbki sprawdza skróty plików.</span><span class="sxs-lookup"><span data-stu-id="37221-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="37221-146">W przykładzie zastosowano mieszanie plików SHA1.</span><span class="sxs-lookup"><span data-stu-id="37221-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="37221-147">Ponowna próba jest zaimplementowana z wycofywaniem wykładniczym.</span><span class="sxs-lookup"><span data-stu-id="37221-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="37221-148">Ponowienie próby jest możliwe, ponieważ może wystąpić zablokowanie pliku, który tymczasowo uniemożliwia Obliczanie nowego skrótu pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="37221-149">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="37221-150">Prosty token zmiany uruchamiania</span><span class="sxs-lookup"><span data-stu-id="37221-150">Simple startup change token</span></span>

<span data-ttu-id="37221-151">Zarejestruj wywołanie zwrotne `Action` odbiorcy tokenu dla powiadomień o zmianach w tokenie Załaduj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="37221-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="37221-152">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="37221-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="37221-153">`config.GetReloadToken()`udostępnia token.</span><span class="sxs-lookup"><span data-stu-id="37221-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="37221-154">Wywołanie zwrotne to `InvokeChanged` Metoda:</span><span class="sxs-lookup"><span data-stu-id="37221-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="37221-155">Wywołanie zwrotne jest używane do przekazywania `IWebHostEnvironment`w, co jest przydatne do określenia poprawnego pliku konfiguracyjnego *AppSettings* do monitorowania (na przykład appSettings. `state`  *Plik Development. JSON* w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="37221-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="37221-156">Skróty plików są używane, aby zapobiec `WriteConsole` uruchamianiu instrukcji wiele razy z powodu wywołania zwrotnego wielu tokenów, gdy plik konfiguracyjny został zmieniony tylko raz.</span><span class="sxs-lookup"><span data-stu-id="37221-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="37221-157">Ten system działa tak długo, jak działa aplikacja i nie może zostać wyłączona przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37221-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="37221-158">Monitorowanie zmian konfiguracji jako usługi</span><span class="sxs-lookup"><span data-stu-id="37221-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="37221-159">Przykład implementuje:</span><span class="sxs-lookup"><span data-stu-id="37221-159">The sample implements:</span></span>

* <span data-ttu-id="37221-160">Podstawowe monitorowanie tokenów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="37221-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="37221-161">Monitorowanie jako usługa.</span><span class="sxs-lookup"><span data-stu-id="37221-161">Monitoring as a service.</span></span>
* <span data-ttu-id="37221-162">Mechanizm do włączania i wyłączania monitorowania.</span><span class="sxs-lookup"><span data-stu-id="37221-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="37221-163">Przykład ustanawia `IConfigurationMonitor` interfejs.</span><span class="sxs-lookup"><span data-stu-id="37221-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="37221-164">*Rozszerzenia/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="37221-165">Konstruktor zaimplementowanej klasy `ConfigurationMonitor`, rejestruje wywołanie zwrotne dla powiadomień o zmianach:</span><span class="sxs-lookup"><span data-stu-id="37221-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="37221-166">`config.GetReloadToken()`dostarcza token.</span><span class="sxs-lookup"><span data-stu-id="37221-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="37221-167">`InvokeChanged`jest metodą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="37221-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="37221-168">W tym wystąpieniu jest odwołanie `IConfigurationMonitor` do wystąpienia, które jest używane w celu uzyskania dostępu do stanu monitorowania. `state`</span><span class="sxs-lookup"><span data-stu-id="37221-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="37221-169">Są używane dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="37221-169">Two properties are used:</span></span>

* <span data-ttu-id="37221-170">`MonitoringEnabled`&ndash; Wskazuje, czy wywołanie zwrotne powinno uruchamiać swój kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="37221-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="37221-171">`CurrentState`&ndash; Opisuje bieżący stan monitorowania do użycia w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37221-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="37221-172">`InvokeChanged` Metoda jest podobna do wcześniejszego podejścia, z tą różnicą, że:</span><span class="sxs-lookup"><span data-stu-id="37221-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="37221-173">Kod nie jest uruchamiany, `MonitoringEnabled` chyba `true`że jest.</span><span class="sxs-lookup"><span data-stu-id="37221-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="37221-174">Wyprowadza bieżącą `state` wartość w jej `WriteConsole` danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="37221-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="37221-175">Wystąpienie `ConfigurationMonitor` jest zarejestrowane jako usługa w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="37221-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="37221-176">Strona indeks zapewnia kontrolę nad konfiguracją przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37221-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="37221-177">Wystąpienie `IConfigurationMonitor` jest wstrzykiwane `IndexModel`do.</span><span class="sxs-lookup"><span data-stu-id="37221-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="37221-178">*Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="37221-179">Monitor konfiguracji (`_monitor`) służy do włączania lub wyłączania monitorowania oraz ustawiania bieżącego stanu dla opinii o interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="37221-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="37221-180">Gdy `OnPostStartMonitoring` jest wyzwalane, monitorowanie jest włączone, a bieżący stan jest wyczyszczony.</span><span class="sxs-lookup"><span data-stu-id="37221-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="37221-181">Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony na odzwierciedlenie tego, że monitorowanie nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="37221-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="37221-182">Przyciski w interfejsie użytkownika włączają i wyłączają monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="37221-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="37221-183">*Pages/index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37221-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="37221-184">Monitoruj zmiany plików w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="37221-184">Monitor cached file changes</span></span>

<span data-ttu-id="37221-185">Zawartość pliku może być buforowana w pamięci za <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="37221-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="37221-186">Buforowanie w pamięci jest opisane w temacie [pamięć podręczna w pamięci podręcznej](xref:performance/caching/memory) .</span><span class="sxs-lookup"><span data-stu-id="37221-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="37221-187">Bez podejmowania dodatkowych czynności, takich jak implementacja opisana poniżej, *nieodświeżone (przestarzałe* ) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zostaną zmienione.</span><span class="sxs-lookup"><span data-stu-id="37221-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="37221-188">Na przykład nie uwzględnia stanu pliku źródłowego w pamięci podręcznej podczas odnawiania przedziału czasu [wygaśnięcia](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) prowadzi do starych danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="37221-189">Każde żądanie dotyczące danych odnawia przedział czasu wygaśnięcia, ale plik nigdy nie jest ponownie ładowany do pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="37221-190">Wszystkie funkcje aplikacji korzystające z zawartości w pamięci podręcznej są uzależnione od potencjalnie otrzymywania nieodświeżonej zawartości.</span><span class="sxs-lookup"><span data-stu-id="37221-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="37221-191">Użycie tokenów zmiany w scenariuszu buforowania plików uniemożliwia obecność przestarzałych zawartości plików w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="37221-192">Przykładowa aplikacja prezentuje implementację metody.</span><span class="sxs-lookup"><span data-stu-id="37221-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="37221-193">Przykład używa `GetFileContent` do:</span><span class="sxs-lookup"><span data-stu-id="37221-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="37221-194">Zwróć zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-194">Return file content.</span></span>
* <span data-ttu-id="37221-195">Zaimplementuj algorytm ponawiania prób z wycofywaniem z powrotem, aby uwzględnić przypadki, w których blokada pliku tymczasowo uniemożliwia odczytywanie pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="37221-196">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="37221-197">`FileService` Jest tworzony w celu obsługi wyszukiwania plików w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="37221-198">Wywołanie metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i zwrócić ją do obiektu wywołującego (*Services/FileService. cs*). `GetFileContent`</span><span class="sxs-lookup"><span data-stu-id="37221-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="37221-199">Jeśli buforowana zawartość nie zostanie znaleziona przy użyciu klucza pamięci podręcznej, zostaną wykonane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="37221-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="37221-200">Zawartość pliku jest uzyskiwana przy `GetFileContent`użyciu.</span><span class="sxs-lookup"><span data-stu-id="37221-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="37221-201">Token zmiany jest uzyskiwany od dostawcy plików z [IFileProviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="37221-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="37221-202">Wywołanie zwrotne tokenu jest wyzwalane, gdy plik zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="37221-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="37221-203">Zawartość pliku jest buforowana z [ruchomym](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okresem ważności.</span><span class="sxs-lookup"><span data-stu-id="37221-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="37221-204">Token zmiany jest dołączony do [MemoryCacheEntryExtensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) w celu wykluczenia wpisu pamięci podręcznej, jeśli plik zostanie zmieniony w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="37221-205">W poniższym przykładzie pliki są przechowywane w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="37221-205">In the following example, files are stored in the app's content root.</span></span> <span data-ttu-id="37221-206">`IWebHostEnvironment.ContentRootFileProvider`służy do uzyskania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazujący na `IWebHostEnvironment.ContentRootPath`aplikację.</span><span class="sxs-lookup"><span data-stu-id="37221-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="37221-207">Jest on uzyskiwany za pomocą [IFileInfo. PhysicalPath.](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) `filePath`</span><span class="sxs-lookup"><span data-stu-id="37221-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="37221-208">Program `FileService` jest zarejestrowany w kontenerze usługi wraz z usługą buforowania pamięci.</span><span class="sxs-lookup"><span data-stu-id="37221-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="37221-209">W `Startup.ConfigureServices`programie:</span><span class="sxs-lookup"><span data-stu-id="37221-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="37221-210">Model strony ładuje zawartość pliku przy użyciu usługi.</span><span class="sxs-lookup"><span data-stu-id="37221-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="37221-211">Na stronie `OnGet` indeksu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="37221-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="37221-212">Klasa CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="37221-212">CompositeChangeToken class</span></span>

<span data-ttu-id="37221-213">Dla reprezentowania jednego lub większej `IChangeToken` liczby wystąpień w pojedynczym obiekcie, <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> Użyj klasy.</span><span class="sxs-lookup"><span data-stu-id="37221-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="37221-214">`HasChanged`w przypadku raportowania `true` tokenu złożonego, jeśli jakikolwiek reprezentowany `true`token `HasChanged` jest.</span><span class="sxs-lookup"><span data-stu-id="37221-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="37221-215">`ActiveChangeCallbacks`w przypadku raportowania `true` tokenu złożonego, jeśli jakikolwiek reprezentowany `true`token `ActiveChangeCallbacks` jest.</span><span class="sxs-lookup"><span data-stu-id="37221-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="37221-216">W przypadku wystąpienia wielu współbieżnych zdarzeń zmiany wywołanie zwrotne zmiany złożonego jest wywoływana jednokrotnie.</span><span class="sxs-lookup"><span data-stu-id="37221-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="37221-217">*Tokenem zmiany* jest ogólnym blokiem konstrukcyjnym, który służy do śledzenia zmian stanu.</span><span class="sxs-lookup"><span data-stu-id="37221-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="37221-218">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37221-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="37221-219">IChangeToken, interfejs</span><span class="sxs-lookup"><span data-stu-id="37221-219">IChangeToken interface</span></span>

<span data-ttu-id="37221-220"><xref:Microsoft.Extensions.Primitives.IChangeToken>propaguje powiadomienia o wystąpieniu zmiany.</span><span class="sxs-lookup"><span data-stu-id="37221-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="37221-221">`IChangeToken`znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="37221-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37221-222">W przypadku aplikacji, które nie korzystają z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu dla pakietu NuGet [Microsoft. Extensions. pierwotne](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) .</span><span class="sxs-lookup"><span data-stu-id="37221-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="37221-223">`IChangeToken`ma dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="37221-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="37221-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks>wskaż, czy token aktywnie wywołuje wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="37221-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="37221-225">Jeśli `ActiveChangedCallbacks` jest ustawiona na `false`, wywołanie zwrotne nigdy nie zostanie wywołane, a aplikacja `HasChanged` musi sondować pod kątem zmian.</span><span class="sxs-lookup"><span data-stu-id="37221-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="37221-226">Istnieje również możliwość, że token nigdy nie zostanie anulowany, jeśli nie wystąpią żadne zmiany lub zostanie usunięty lub wyłączony odbiornik zmian.</span><span class="sxs-lookup"><span data-stu-id="37221-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="37221-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>odbiera wartość wskazującą, czy nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="37221-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="37221-228">Interfejs zawiera metodę [RegisterChangeCallback (obiekt akcji\<>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) , która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony. `IChangeToken`</span><span class="sxs-lookup"><span data-stu-id="37221-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="37221-229">`HasChanged`musi być ustawiona przed wywołaniem wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="37221-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="37221-230">Klasa ChangeToken</span><span class="sxs-lookup"><span data-stu-id="37221-230">ChangeToken class</span></span>

<span data-ttu-id="37221-231"><xref:Microsoft.Extensions.Primitives.ChangeToken>jest klasą statyczną służącą do propagowania powiadomień, w których wystąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="37221-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="37221-232">`ChangeToken`znajduje się w <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="37221-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37221-233">W przypadku aplikacji, które nie korzystają z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu dla pakietu NuGet [Microsoft. Extensions. pierwotne](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) .</span><span class="sxs-lookup"><span data-stu-id="37221-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="37221-234">[ChangeToken. OnChange (\<Func IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestruje `Action` metodę, aby wywołać za każdym razem, gdy zmieni się token:</span><span class="sxs-lookup"><span data-stu-id="37221-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="37221-235">`Func<IChangeToken>`tworzy token.</span><span class="sxs-lookup"><span data-stu-id="37221-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="37221-236">`Action`jest wywoływana, gdy zostanie zmieniony token.</span><span class="sxs-lookup"><span data-stu-id="37221-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="37221-237">[ChangeToken. OnChange\<TState > (\<Func IChangeToken >,\<Action TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) przyjmuje dodatkowy `TState` parametr, który jest przesyłany do odbiorcy `Action` tokenu .</span><span class="sxs-lookup"><span data-stu-id="37221-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="37221-238">`OnChange`Zwraca wartość <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="37221-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="37221-239">Wywołanie <xref:System.IDisposable.Dispose*> uniemożliwia wysłuchanie tokenu w celu wprowadzenia dalszych zmian i zwolnienia zasobów tokenu.</span><span class="sxs-lookup"><span data-stu-id="37221-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="37221-240">Przykładowe zastosowania tokenów zmiany w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37221-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="37221-241">Tokeny zmiany są używane w widocznych obszarach ASP.NET Core do monitorowania zmian obiektów:</span><span class="sxs-lookup"><span data-stu-id="37221-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="37221-242">W przypadku monitorowania zmian w plikach <xref:Microsoft.Extensions.FileProviders.IFileProvider> <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> Metoda tworzy `IChangeToken` dla określonych plików lub folderów, które mają być monitorowane.</span><span class="sxs-lookup"><span data-stu-id="37221-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="37221-243">`IChangeToken`można dodać tokeny do wpisów pamięci podręcznej, aby wyzwolić wykluczenia z pamięci podręcznej w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="37221-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="37221-244">W `TOptions` przypadku zmian Domyślna <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacja <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> maPrzeciążenie,któreakceptujeconajmniejjednowystąpienie.<xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1></span><span class="sxs-lookup"><span data-stu-id="37221-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="37221-245">Każde wystąpienie zwraca wartość `IChangeToken` , aby zarejestrować zmiany zwrotne powiadomień o zmianach opcji śledzenia.</span><span class="sxs-lookup"><span data-stu-id="37221-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="37221-246">Monitorowanie zmian konfiguracji</span><span class="sxs-lookup"><span data-stu-id="37221-246">Monitor for configuration changes</span></span>

<span data-ttu-id="37221-247">Domyślnie szablony ASP.NET Core korzystają z [plików konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, *appSettings. Development. JSON*i *appSettings. Production. JSON*) w celu załadowania ustawień konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="37221-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="37221-248">Te pliki są konfigurowane przy użyciu metody <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> rozszerzenia [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) , która akceptuje `reloadOnChange` parametr.</span><span class="sxs-lookup"><span data-stu-id="37221-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="37221-249">`reloadOnChange`wskazuje, czy należy ponownie załadować konfigurację w przypadku zmian w pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="37221-250">To ustawienie jest dostępne w <xref:Microsoft.AspNetCore.WebHost> metodzie <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>wygodnej:</span><span class="sxs-lookup"><span data-stu-id="37221-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="37221-251">Konfiguracja oparta na plikach jest reprezentowana przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="37221-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="37221-252">`FileConfigurationSource`Program <xref:Microsoft.Extensions.FileProviders.IFileProvider> używa do monitorowania plików.</span><span class="sxs-lookup"><span data-stu-id="37221-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="37221-253">Domyślnie `IFileMonitor` jest on <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>dostarczany przez, który <xref:System.IO.FileSystemWatcher> służy do monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37221-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="37221-254">Przykładowa aplikacja przedstawia dwie implementacje monitorowania zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37221-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="37221-255">Jeśli którykolwiek z plików *AppSettings* zostanie zmieniony, obydwie implementacje monitorowania plików wykonują kod&mdash;niestandardowy, a Przykładowa aplikacja zapisuje komunikat w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="37221-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="37221-256">Plik `FileSystemWatcher` konfiguracji może wyzwolić wiele wywołań zwrotnych tokenów dla jednej zmiany pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37221-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="37221-257">Aby upewnić się, że kod niestandardowy jest uruchamiany tylko raz w przypadku wyzwolenia wielu wywołań zwrotnych tokenów, implementacja próbki sprawdza skróty plików.</span><span class="sxs-lookup"><span data-stu-id="37221-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="37221-258">W przykładzie zastosowano mieszanie plików SHA1.</span><span class="sxs-lookup"><span data-stu-id="37221-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="37221-259">Ponowna próba jest zaimplementowana z wycofywaniem wykładniczym.</span><span class="sxs-lookup"><span data-stu-id="37221-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="37221-260">Ponowienie próby jest możliwe, ponieważ może wystąpić zablokowanie pliku, który tymczasowo uniemożliwia Obliczanie nowego skrótu pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="37221-261">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="37221-262">Prosty token zmiany uruchamiania</span><span class="sxs-lookup"><span data-stu-id="37221-262">Simple startup change token</span></span>

<span data-ttu-id="37221-263">Zarejestruj wywołanie zwrotne `Action` odbiorcy tokenu dla powiadomień o zmianach w tokenie Załaduj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="37221-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="37221-264">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="37221-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="37221-265">`config.GetReloadToken()`udostępnia token.</span><span class="sxs-lookup"><span data-stu-id="37221-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="37221-266">Wywołanie zwrotne to `InvokeChanged` Metoda:</span><span class="sxs-lookup"><span data-stu-id="37221-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="37221-267">Wywołanie zwrotne jest używane do przekazywania `IHostingEnvironment`w, co jest przydatne do określenia poprawnego pliku konfiguracyjnego *AppSettings* do monitorowania (na przykład appSettings. `state`  *Plik Development. JSON* w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="37221-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="37221-268">Skróty plików są używane, aby zapobiec `WriteConsole` uruchamianiu instrukcji wiele razy z powodu wywołania zwrotnego wielu tokenów, gdy plik konfiguracyjny został zmieniony tylko raz.</span><span class="sxs-lookup"><span data-stu-id="37221-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="37221-269">Ten system działa tak długo, jak działa aplikacja i nie może zostać wyłączona przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37221-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="37221-270">Monitorowanie zmian konfiguracji jako usługi</span><span class="sxs-lookup"><span data-stu-id="37221-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="37221-271">Przykład implementuje:</span><span class="sxs-lookup"><span data-stu-id="37221-271">The sample implements:</span></span>

* <span data-ttu-id="37221-272">Podstawowe monitorowanie tokenów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="37221-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="37221-273">Monitorowanie jako usługa.</span><span class="sxs-lookup"><span data-stu-id="37221-273">Monitoring as a service.</span></span>
* <span data-ttu-id="37221-274">Mechanizm do włączania i wyłączania monitorowania.</span><span class="sxs-lookup"><span data-stu-id="37221-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="37221-275">Przykład ustanawia `IConfigurationMonitor` interfejs.</span><span class="sxs-lookup"><span data-stu-id="37221-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="37221-276">*Rozszerzenia/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="37221-277">Konstruktor zaimplementowanej klasy `ConfigurationMonitor`, rejestruje wywołanie zwrotne dla powiadomień o zmianach:</span><span class="sxs-lookup"><span data-stu-id="37221-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="37221-278">`config.GetReloadToken()`dostarcza token.</span><span class="sxs-lookup"><span data-stu-id="37221-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="37221-279">`InvokeChanged`jest metodą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="37221-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="37221-280">W tym wystąpieniu jest odwołanie `IConfigurationMonitor` do wystąpienia, które jest używane w celu uzyskania dostępu do stanu monitorowania. `state`</span><span class="sxs-lookup"><span data-stu-id="37221-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="37221-281">Są używane dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="37221-281">Two properties are used:</span></span>

* <span data-ttu-id="37221-282">`MonitoringEnabled`&ndash; Wskazuje, czy wywołanie zwrotne powinno uruchamiać swój kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="37221-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="37221-283">`CurrentState`&ndash; Opisuje bieżący stan monitorowania do użycia w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37221-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="37221-284">`InvokeChanged` Metoda jest podobna do wcześniejszego podejścia, z tą różnicą, że:</span><span class="sxs-lookup"><span data-stu-id="37221-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="37221-285">Kod nie jest uruchamiany, `MonitoringEnabled` chyba `true`że jest.</span><span class="sxs-lookup"><span data-stu-id="37221-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="37221-286">Wyprowadza bieżącą `state` wartość w jej `WriteConsole` danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="37221-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="37221-287">Wystąpienie `ConfigurationMonitor` jest zarejestrowane jako usługa w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="37221-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="37221-288">Strona indeks zapewnia kontrolę nad konfiguracją przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37221-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="37221-289">Wystąpienie `IConfigurationMonitor` jest wstrzykiwane `IndexModel`do.</span><span class="sxs-lookup"><span data-stu-id="37221-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="37221-290">*Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="37221-291">Monitor konfiguracji (`_monitor`) służy do włączania lub wyłączania monitorowania oraz ustawiania bieżącego stanu dla opinii o interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="37221-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="37221-292">Gdy `OnPostStartMonitoring` jest wyzwalane, monitorowanie jest włączone, a bieżący stan jest wyczyszczony.</span><span class="sxs-lookup"><span data-stu-id="37221-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="37221-293">Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony na odzwierciedlenie tego, że monitorowanie nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="37221-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="37221-294">Przyciski w interfejsie użytkownika włączają i wyłączają monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="37221-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="37221-295">*Pages/index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37221-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="37221-296">Monitoruj zmiany plików w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="37221-296">Monitor cached file changes</span></span>

<span data-ttu-id="37221-297">Zawartość pliku może być buforowana w pamięci za <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="37221-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="37221-298">Buforowanie w pamięci jest opisane w temacie [pamięć podręczna w pamięci podręcznej](xref:performance/caching/memory) .</span><span class="sxs-lookup"><span data-stu-id="37221-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="37221-299">Bez podejmowania dodatkowych czynności, takich jak implementacja opisana poniżej, *nieodświeżone (przestarzałe* ) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zostaną zmienione.</span><span class="sxs-lookup"><span data-stu-id="37221-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="37221-300">Na przykład nie uwzględnia stanu pliku źródłowego w pamięci podręcznej podczas odnawiania przedziału czasu [wygaśnięcia](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) prowadzi do starych danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="37221-301">Każde żądanie dotyczące danych odnawia przedział czasu wygaśnięcia, ale plik nigdy nie jest ponownie ładowany do pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="37221-302">Wszystkie funkcje aplikacji korzystające z zawartości w pamięci podręcznej są uzależnione od potencjalnie otrzymywania nieodświeżonej zawartości.</span><span class="sxs-lookup"><span data-stu-id="37221-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="37221-303">Użycie tokenów zmiany w scenariuszu buforowania plików uniemożliwia obecność przestarzałych zawartości plików w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="37221-304">Przykładowa aplikacja prezentuje implementację metody.</span><span class="sxs-lookup"><span data-stu-id="37221-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="37221-305">Przykład używa `GetFileContent` do:</span><span class="sxs-lookup"><span data-stu-id="37221-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="37221-306">Zwróć zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-306">Return file content.</span></span>
* <span data-ttu-id="37221-307">Zaimplementuj algorytm ponawiania prób z wycofywaniem z powrotem, aby uwzględnić przypadki, w których blokada pliku tymczasowo uniemożliwia odczytywanie pliku.</span><span class="sxs-lookup"><span data-stu-id="37221-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="37221-308">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="37221-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="37221-309">`FileService` Jest tworzony w celu obsługi wyszukiwania plików w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="37221-310">Wywołanie metody usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i zwrócić ją do obiektu wywołującego (*Services/FileService. cs*). `GetFileContent`</span><span class="sxs-lookup"><span data-stu-id="37221-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="37221-311">Jeśli buforowana zawartość nie zostanie znaleziona przy użyciu klucza pamięci podręcznej, zostaną wykonane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="37221-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="37221-312">Zawartość pliku jest uzyskiwana przy `GetFileContent`użyciu.</span><span class="sxs-lookup"><span data-stu-id="37221-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="37221-313">Token zmiany jest uzyskiwany od dostawcy plików z [IFileProviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="37221-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="37221-314">Wywołanie zwrotne tokenu jest wyzwalane, gdy plik zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="37221-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="37221-315">Zawartość pliku jest buforowana z [ruchomym](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okresem ważności.</span><span class="sxs-lookup"><span data-stu-id="37221-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="37221-316">Token zmiany jest dołączony do [MemoryCacheEntryExtensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) w celu wykluczenia wpisu pamięci podręcznej, jeśli plik zostanie zmieniony w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="37221-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="37221-317">W poniższym przykładzie pliki są przechowywane w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="37221-317">In the following example, files are stored in the app's content root.</span></span> <span data-ttu-id="37221-318">[IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) służy do uzyskania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazania wskazujące <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>na aplikację.</span><span class="sxs-lookup"><span data-stu-id="37221-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="37221-319">Jest on uzyskiwany za pomocą [IFileInfo. PhysicalPath.](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath) `filePath`</span><span class="sxs-lookup"><span data-stu-id="37221-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="37221-320">Program `FileService` jest zarejestrowany w kontenerze usługi wraz z usługą buforowania pamięci.</span><span class="sxs-lookup"><span data-stu-id="37221-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="37221-321">W `Startup.ConfigureServices`programie:</span><span class="sxs-lookup"><span data-stu-id="37221-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="37221-322">Model strony ładuje zawartość pliku przy użyciu usługi.</span><span class="sxs-lookup"><span data-stu-id="37221-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="37221-323">Na stronie `OnGet` indeksu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="37221-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="37221-324">Klasa CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="37221-324">CompositeChangeToken class</span></span>

<span data-ttu-id="37221-325">Dla reprezentowania jednego lub większej `IChangeToken` liczby wystąpień w pojedynczym obiekcie, <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> Użyj klasy.</span><span class="sxs-lookup"><span data-stu-id="37221-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="37221-326">`HasChanged`w przypadku raportowania `true` tokenu złożonego, jeśli jakikolwiek reprezentowany `true`token `HasChanged` jest.</span><span class="sxs-lookup"><span data-stu-id="37221-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="37221-327">`ActiveChangeCallbacks`w przypadku raportowania `true` tokenu złożonego, jeśli jakikolwiek reprezentowany `true`token `ActiveChangeCallbacks` jest.</span><span class="sxs-lookup"><span data-stu-id="37221-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="37221-328">W przypadku wystąpienia wielu współbieżnych zdarzeń zmiany wywołanie zwrotne zmiany złożonego jest wywoływana jednokrotnie.</span><span class="sxs-lookup"><span data-stu-id="37221-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="37221-329">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="37221-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
