---
title: Wykrywanie zmian przy użyciu tokenów zmiany w ASP.NET Core
author: guardrex
description: Dowiedz się, jak śledzić zmiany przy użyciu tokenów zmian.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: bb30d7a4c7dc82200821c60a49c314b246562111
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007209"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="0e8ca-103">Wykrywanie zmian przy użyciu tokenów zmiany w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e8ca-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="0e8ca-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0e8ca-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0e8ca-105">*Tokenem zmiany* jest ogólnym blokiem konstrukcyjnym, który służy do śledzenia zmian stanu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="0e8ca-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0e8ca-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="0e8ca-107">IChangeToken, interfejs</span><span class="sxs-lookup"><span data-stu-id="0e8ca-107">IChangeToken interface</span></span>

<span data-ttu-id="0e8ca-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propaguje powiadomienia o wystąpieniu zmiany.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="0e8ca-109">`IChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="0e8ca-110">Pakiet NuGet [Microsoft. Extensions.](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) jest niejawnie dostarczany do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="0e8ca-111">`IChangeToken` ma dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="0e8ca-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> wskazać, czy token aktywnie wywołuje wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="0e8ca-113">Jeśli `ActiveChangedCallbacks` jest ustawiona na `false`, wywołanie zwrotne nigdy nie zostanie wywołane, a aplikacja musi sondować `HasChanged` pod kątem zmian.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="0e8ca-114">Istnieje również możliwość, że token nigdy nie zostanie anulowany, jeśli nie wystąpią żadne zmiany lub zostanie usunięty lub wyłączony odbiornik zmian.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="0e8ca-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> otrzymuje wartość wskazującą, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="0e8ca-116">Interfejs `IChangeToken` obejmuje metodę [RegisterChangeCallback (Action\<object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) , która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="0e8ca-117">przed wywołaniem wywołania zwrotnego należy ustawić `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="0e8ca-118">Klasa ChangeToken</span><span class="sxs-lookup"><span data-stu-id="0e8ca-118">ChangeToken class</span></span>

<span data-ttu-id="0e8ca-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> jest klasą statyczną służącą do propagowania powiadomień, w których wystąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="0e8ca-120">`ChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="0e8ca-121">Pakiet NuGet [Microsoft. Extensions.](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) jest niejawnie dostarczany do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="0e8ca-122">Metoda [ChangeToken. OnChange (Func\<IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestruje `Action` do wywołania przy każdej zmianie tokenu:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="0e8ca-123">`Func<IChangeToken>` generuje token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="0e8ca-124">`Action` jest wywoływana, gdy zostanie zmieniony token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="0e8ca-125">[ChangeToken. Onchange\<TState > (Func\<IChangeToken >, Action\<TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) Przeciążenie przyjmuje dodatkowy parametr `TState`, który jest przesyłany do `Action`odbiorcy tokenu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="0e8ca-126">`OnChange` zwraca <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="0e8ca-127">Wywołanie <xref:System.IDisposable.Dispose*> uniemożliwia wysłuchanie tokenu przez dalsze zmiany i zwolnienie zasobów tokenu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="0e8ca-128">Przykładowe zastosowania tokenów zmiany w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e8ca-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="0e8ca-129">Tokeny zmiany są używane w widocznych obszarach ASP.NET Core do monitorowania zmian obiektów:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="0e8ca-130">W przypadku monitorowania zmian plików <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> Metoda tworzy `IChangeToken` dla określonych plików lub folderów do monitorowania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="0e8ca-131">tokeny `IChangeToken` można dodać do wpisów pamięci podręcznej, aby wyzwolić wykluczenia z pamięci podręcznej w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="0e8ca-132">W przypadku zmian `TOptions` domyślna <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacja <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ma Przeciążenie, które akceptuje co najmniej jedno wystąpienie <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="0e8ca-133">Każde wystąpienie zwraca `IChangeToken` do zarejestrowania zmian wywołania zwrotnego powiadomienia o zmianach dla opcji śledzenia.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="0e8ca-134">Monitorowanie zmian konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0e8ca-134">Monitor for configuration changes</span></span>

<span data-ttu-id="0e8ca-135">Domyślnie szablony ASP.NET Core korzystają z [plików konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, *appSettings. Development. JSON*i *appSettings. Production. JSON*) w celu załadowania ustawień konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="0e8ca-136">Te pliki są konfigurowane przy użyciu metody rozszerzenia [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, która akceptuje parametr `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="0e8ca-137">`reloadOnChange` wskazuje, czy należy ponownie załadować konfigurację w przypadku zmian w pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="0e8ca-138">To ustawienie jest dostępne w <xref:Microsoft.Extensions.Hosting.Host> metodzie wygody <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="0e8ca-139">Konfiguracja oparta na plikach jest reprezentowana przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="0e8ca-140">`FileConfigurationSource` używa <xref:Microsoft.Extensions.FileProviders.IFileProvider> do monitorowania plików.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="0e8ca-141">Domyślnie `IFileMonitor` jest udostępniana przez <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, która używa <xref:System.IO.FileSystemWatcher> do monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="0e8ca-142">Przykładowa aplikacja przedstawia dwie implementacje monitorowania zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="0e8ca-143">Jeśli którykolwiek z plików *AppSettings* zostanie zmieniony, oba implementacje monitorowania plików wykonują kod niestandardowy&mdash;Przykładowa aplikacja zapisuje komunikat w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="0e8ca-144">`FileSystemWatcher` pliku konfiguracji może wyzwolić wiele wywołań zwrotnych tokenów dla jednej zmiany pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="0e8ca-145">Aby upewnić się, że kod niestandardowy jest uruchamiany tylko raz w przypadku wyzwolenia wielu wywołań zwrotnych tokenów, implementacja próbki sprawdza skróty plików.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="0e8ca-146">W przykładzie zastosowano mieszanie plików SHA1.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="0e8ca-147">Ponowna próba jest zaimplementowana z wycofywaniem wykładniczym.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="0e8ca-148">Ponowienie próby jest możliwe, ponieważ może wystąpić zablokowanie pliku, który tymczasowo uniemożliwia Obliczanie nowego skrótu pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="0e8ca-149">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="0e8ca-150">Prosty token zmiany uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0e8ca-150">Simple startup change token</span></span>

<span data-ttu-id="0e8ca-151">Zarejestruj odbiorcę tokenu `Action` wywołanie zwrotne dla powiadomień o zmianach do tokenu Załaduj ponownie konfigurację.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="0e8ca-152">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="0e8ca-153">`config.GetReloadToken()` udostępnia token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="0e8ca-154">Wywołanie zwrotne jest metodą `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="0e8ca-155">`state` wywołania zwrotnego jest używany do przekazywania w `IWebHostEnvironment`, co jest przydatne do określania poprawnego pliku konfiguracyjnego *AppSettings* do monitorowania (na przykład *appSettings. Plik Development. JSON* w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="0e8ca-156">Skróty plików są używane do zapobiegania wielokrotnemu uruchamianiu instrukcji `WriteConsole` ze względu na wiele wywołań zwrotnych tokenów, gdy plik konfiguracyjny został zmieniony tylko raz.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="0e8ca-157">Ten system działa tak długo, jak działa aplikacja i nie może zostać wyłączona przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="0e8ca-158">Monitorowanie zmian konfiguracji jako usługi</span><span class="sxs-lookup"><span data-stu-id="0e8ca-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="0e8ca-159">Przykład implementuje:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-159">The sample implements:</span></span>

* <span data-ttu-id="0e8ca-160">Podstawowe monitorowanie tokenów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="0e8ca-161">Monitorowanie jako usługa.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-161">Monitoring as a service.</span></span>
* <span data-ttu-id="0e8ca-162">Mechanizm do włączania i wyłączania monitorowania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="0e8ca-163">Przykład ustanawia interfejs `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="0e8ca-164">*Rozszerzenia/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="0e8ca-165">Konstruktor zaimplementowanej klasy `ConfigurationMonitor`rejestruje wywołanie zwrotne dla powiadomień o zmianach:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="0e8ca-166">`config.GetReloadToken()` dostarcza token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="0e8ca-167">`InvokeChanged` jest metodą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="0e8ca-168">`state` w tym wystąpieniu jest odwołaniem do wystąpienia `IConfigurationMonitor`, które jest używane w celu uzyskania dostępu do stanu monitorowania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="0e8ca-169">Są używane dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-169">Two properties are used:</span></span>

* <span data-ttu-id="0e8ca-170">`MonitoringEnabled` &ndash; wskazuje, czy wywołanie zwrotne powinno uruchamiać swój kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="0e8ca-171">`CurrentState` &ndash; opisuje bieżący stan monitorowania do użycia w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="0e8ca-172">Metoda `InvokeChanged` jest podobna do wcześniejszego podejścia, z tą różnicą, że:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="0e8ca-173">Nie uruchamia tego kodu, chyba że `MonitoringEnabled` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="0e8ca-174">Wyprowadza bieżącą `state` w jej `WriteConsole` danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="0e8ca-175">`ConfigurationMonitor` wystąpienia jest zarejestrowany jako usługa w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="0e8ca-176">Strona indeks zapewnia kontrolę nad konfiguracją przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="0e8ca-177">Wystąpienie `IConfigurationMonitor` jest wstrzykiwane do `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="0e8ca-178">*Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0e8ca-179">Monitor konfiguracji (`_monitor`) służy do włączania lub wyłączania monitorowania oraz ustawiania bieżącego stanu na potrzeby przesyłania opinii o interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0e8ca-180">Gdy `OnPostStartMonitoring` jest wyzwalane, monitorowanie jest włączone, a bieżący stan jest wyczyszczony.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="0e8ca-181">Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony na odzwierciedlenie tego, że monitorowanie nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="0e8ca-182">Przyciski w interfejsie użytkownika włączają i wyłączają monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="0e8ca-183">*Pages/index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="0e8ca-184">Monitoruj zmiany plików w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="0e8ca-184">Monitor cached file changes</span></span>

<span data-ttu-id="0e8ca-185">Zawartość pliku może być buforowana w pamięci przy użyciu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="0e8ca-186">Buforowanie w pamięci jest opisane w temacie [pamięć podręczna w pamięci podręcznej](xref:performance/caching/memory) .</span><span class="sxs-lookup"><span data-stu-id="0e8ca-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="0e8ca-187">Bez podejmowania dodatkowych czynności, takich jak implementacja opisana poniżej, *nieodświeżone (przestarzałe* ) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zostaną zmienione.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="0e8ca-188">Na przykład nie uwzględnia stanu pliku źródłowego w pamięci podręcznej podczas odnawiania przedziału czasu [wygaśnięcia](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) prowadzi do starych danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="0e8ca-189">Każde żądanie dotyczące danych odnawia przedział czasu wygaśnięcia, ale plik nigdy nie jest ponownie ładowany do pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="0e8ca-190">Wszystkie funkcje aplikacji korzystające z zawartości w pamięci podręcznej są uzależnione od potencjalnie otrzymywania nieodświeżonej zawartości.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="0e8ca-191">Użycie tokenów zmiany w scenariuszu buforowania plików uniemożliwia obecność przestarzałych zawartości plików w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="0e8ca-192">Przykładowa aplikacja prezentuje implementację metody.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="0e8ca-193">Przykład używa `GetFileContent` do:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="0e8ca-194">Zwróć zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-194">Return file content.</span></span>
* <span data-ttu-id="0e8ca-195">Zaimplementuj algorytm ponawiania prób z wycofywaniem z powrotem, aby uwzględnić przypadki, w których blokada pliku tymczasowo uniemożliwia odczytywanie pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="0e8ca-196">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="0e8ca-197">`FileService` jest tworzony w celu obsługi wyszukiwań w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="0e8ca-198">Wywołanie metody `GetFileContent`j usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i zwrócić ją do obiektu wywołującego (*Services/FileService. cs*).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="0e8ca-199">Jeśli buforowana zawartość nie zostanie znaleziona przy użyciu klucza pamięci podręcznej, zostaną wykonane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="0e8ca-200">Zawartość pliku jest uzyskiwana przy użyciu `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="0e8ca-201">Token zmiany jest uzyskiwany od dostawcy plików z [IFileProviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="0e8ca-202">Wywołanie zwrotne tokenu jest wyzwalane, gdy plik zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="0e8ca-203">Zawartość pliku jest buforowana z [ruchomym](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okresem ważności.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="0e8ca-204">Token zmiany jest dołączony do [MemoryCacheEntryExtensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) w celu wykluczenia wpisu pamięci podręcznej, jeśli plik zostanie zmieniony w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="0e8ca-205">W poniższym przykładzie pliki są przechowywane w [katalogu głównym zawartości](xref:fundamentals/index#content-root)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-205">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="0e8ca-206">`IWebHostEnvironment.ContentRootFileProvider` służy do uzyskiwania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazujących `IWebHostEnvironment.ContentRootPath`aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="0e8ca-207">`filePath` jest uzyskiwany za pomocą [IFileInfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="0e8ca-208">`FileService` jest zarejestrowany w kontenerze usługi wraz z usługą buforowania pamięci.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="0e8ca-209">W `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="0e8ca-210">Model strony ładuje zawartość pliku przy użyciu usługi.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="0e8ca-211">W metodzie `OnGet` strony indeksu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="0e8ca-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="0e8ca-212">Klasa CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="0e8ca-212">CompositeChangeToken class</span></span>

<span data-ttu-id="0e8ca-213">Dla reprezentowania jednego lub więcej wystąpień `IChangeToken` w jednym obiekcie, użyj klasy <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="0e8ca-214">`HasChanged` w raportach tokenów złożonych, `true`, jeśli `true``HasChanged` tokenem reprezentowanego.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="0e8ca-215">`ActiveChangeCallbacks` w raportach tokenów złożonych, `true`, jeśli `true``ActiveChangeCallbacks` tokenem reprezentowanego.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="0e8ca-216">W przypadku wystąpienia wielu współbieżnych zdarzeń zmiany wywołanie zwrotne zmiany złożonego jest wywoływana jednokrotnie.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0e8ca-217">*Tokenem zmiany* jest ogólnym blokiem konstrukcyjnym, który służy do śledzenia zmian stanu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="0e8ca-218">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0e8ca-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="0e8ca-219">IChangeToken, interfejs</span><span class="sxs-lookup"><span data-stu-id="0e8ca-219">IChangeToken interface</span></span>

<span data-ttu-id="0e8ca-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propaguje powiadomienia o wystąpieniu zmiany.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="0e8ca-221">`IChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="0e8ca-222">W przypadku aplikacji, które nie korzystają z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu dla pakietu NuGet [Microsoft. Extensions. pierwotne](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) .</span><span class="sxs-lookup"><span data-stu-id="0e8ca-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="0e8ca-223">`IChangeToken` ma dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="0e8ca-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> wskazać, czy token aktywnie wywołuje wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="0e8ca-225">Jeśli `ActiveChangedCallbacks` jest ustawiona na `false`, wywołanie zwrotne nigdy nie zostanie wywołane, a aplikacja musi sondować `HasChanged` pod kątem zmian.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="0e8ca-226">Istnieje również możliwość, że token nigdy nie zostanie anulowany, jeśli nie wystąpią żadne zmiany lub zostanie usunięty lub wyłączony odbiornik zmian.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="0e8ca-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> otrzymuje wartość wskazującą, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="0e8ca-228">Interfejs `IChangeToken` obejmuje metodę [RegisterChangeCallback (Action\<object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) , która rejestruje wywołanie zwrotne, które jest wywoływane, gdy token został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="0e8ca-229">przed wywołaniem wywołania zwrotnego należy ustawić `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="0e8ca-230">Klasa ChangeToken</span><span class="sxs-lookup"><span data-stu-id="0e8ca-230">ChangeToken class</span></span>

<span data-ttu-id="0e8ca-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> jest klasą statyczną służącą do propagowania powiadomień, w których wystąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="0e8ca-232">`ChangeToken` znajduje się w przestrzeni nazw <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="0e8ca-233">W przypadku aplikacji, które nie korzystają z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu dla pakietu NuGet [Microsoft. Extensions. pierwotne](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) .</span><span class="sxs-lookup"><span data-stu-id="0e8ca-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="0e8ca-234">Metoda [ChangeToken. OnChange (Func\<IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) rejestruje `Action` do wywołania przy każdej zmianie tokenu:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="0e8ca-235">`Func<IChangeToken>` generuje token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="0e8ca-236">`Action` jest wywoływana, gdy zostanie zmieniony token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="0e8ca-237">[ChangeToken. Onchange\<TState > (Func\<IChangeToken >, Action\<TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) Przeciążenie przyjmuje dodatkowy parametr `TState`, który jest przesyłany do `Action`odbiorcy tokenu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="0e8ca-238">`OnChange` zwraca <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="0e8ca-239">Wywołanie <xref:System.IDisposable.Dispose*> uniemożliwia wysłuchanie tokenu przez dalsze zmiany i zwolnienie zasobów tokenu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="0e8ca-240">Przykładowe zastosowania tokenów zmiany w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e8ca-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="0e8ca-241">Tokeny zmiany są używane w widocznych obszarach ASP.NET Core do monitorowania zmian obiektów:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="0e8ca-242">W przypadku monitorowania zmian plików <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> Metoda tworzy `IChangeToken` dla określonych plików lub folderów do monitorowania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="0e8ca-243">tokeny `IChangeToken` można dodać do wpisów pamięci podręcznej, aby wyzwolić wykluczenia z pamięci podręcznej w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="0e8ca-244">W przypadku zmian `TOptions` domyślna <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementacja <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ma Przeciążenie, które akceptuje co najmniej jedno wystąpienie <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="0e8ca-245">Każde wystąpienie zwraca `IChangeToken` do zarejestrowania zmian wywołania zwrotnego powiadomienia o zmianach dla opcji śledzenia.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="0e8ca-246">Monitorowanie zmian konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0e8ca-246">Monitor for configuration changes</span></span>

<span data-ttu-id="0e8ca-247">Domyślnie szablony ASP.NET Core korzystają z [plików konfiguracji JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, *appSettings. Development. JSON*i *appSettings. Production. JSON*) w celu załadowania ustawień konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="0e8ca-248">Te pliki są konfigurowane przy użyciu metody rozszerzenia [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, która akceptuje parametr `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="0e8ca-249">`reloadOnChange` wskazuje, czy należy ponownie załadować konfigurację w przypadku zmian w pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="0e8ca-250">To ustawienie jest dostępne w <xref:Microsoft.AspNetCore.WebHost> metodzie wygody <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="0e8ca-251">Konfiguracja oparta na plikach jest reprezentowana przez <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="0e8ca-252">`FileConfigurationSource` używa <xref:Microsoft.Extensions.FileProviders.IFileProvider> do monitorowania plików.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="0e8ca-253">Domyślnie `IFileMonitor` jest udostępniana przez <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, która używa <xref:System.IO.FileSystemWatcher> do monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="0e8ca-254">Przykładowa aplikacja przedstawia dwie implementacje monitorowania zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="0e8ca-255">Jeśli którykolwiek z plików *AppSettings* zostanie zmieniony, oba implementacje monitorowania plików wykonują kod niestandardowy&mdash;Przykładowa aplikacja zapisuje komunikat w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="0e8ca-256">`FileSystemWatcher` pliku konfiguracji może wyzwolić wiele wywołań zwrotnych tokenów dla jednej zmiany pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="0e8ca-257">Aby upewnić się, że kod niestandardowy jest uruchamiany tylko raz w przypadku wyzwolenia wielu wywołań zwrotnych tokenów, implementacja próbki sprawdza skróty plików.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="0e8ca-258">W przykładzie zastosowano mieszanie plików SHA1.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="0e8ca-259">Ponowna próba jest zaimplementowana z wycofywaniem wykładniczym.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="0e8ca-260">Ponowienie próby jest możliwe, ponieważ może wystąpić zablokowanie pliku, który tymczasowo uniemożliwia Obliczanie nowego skrótu pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="0e8ca-261">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="0e8ca-262">Prosty token zmiany uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0e8ca-262">Simple startup change token</span></span>

<span data-ttu-id="0e8ca-263">Zarejestruj odbiorcę tokenu `Action` wywołanie zwrotne dla powiadomień o zmianach do tokenu Załaduj ponownie konfigurację.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="0e8ca-264">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="0e8ca-265">`config.GetReloadToken()` udostępnia token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="0e8ca-266">Wywołanie zwrotne jest metodą `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="0e8ca-267">`state` wywołania zwrotnego jest używany do przekazywania w `IHostingEnvironment`, co jest przydatne do określania poprawnego pliku konfiguracyjnego *AppSettings* do monitorowania (na przykład *appSettings. Plik Development. JSON* w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="0e8ca-268">Skróty plików są używane do zapobiegania wielokrotnemu uruchamianiu instrukcji `WriteConsole` ze względu na wiele wywołań zwrotnych tokenów, gdy plik konfiguracyjny został zmieniony tylko raz.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="0e8ca-269">Ten system działa tak długo, jak działa aplikacja i nie może zostać wyłączona przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="0e8ca-270">Monitorowanie zmian konfiguracji jako usługi</span><span class="sxs-lookup"><span data-stu-id="0e8ca-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="0e8ca-271">Przykład implementuje:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-271">The sample implements:</span></span>

* <span data-ttu-id="0e8ca-272">Podstawowe monitorowanie tokenów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="0e8ca-273">Monitorowanie jako usługa.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-273">Monitoring as a service.</span></span>
* <span data-ttu-id="0e8ca-274">Mechanizm do włączania i wyłączania monitorowania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="0e8ca-275">Przykład ustanawia interfejs `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="0e8ca-276">*Rozszerzenia/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="0e8ca-277">Konstruktor zaimplementowanej klasy `ConfigurationMonitor`rejestruje wywołanie zwrotne dla powiadomień o zmianach:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="0e8ca-278">`config.GetReloadToken()` dostarcza token.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="0e8ca-279">`InvokeChanged` jest metodą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="0e8ca-280">`state` w tym wystąpieniu jest odwołaniem do wystąpienia `IConfigurationMonitor`, które jest używane w celu uzyskania dostępu do stanu monitorowania.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="0e8ca-281">Są używane dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-281">Two properties are used:</span></span>

* <span data-ttu-id="0e8ca-282">`MonitoringEnabled` &ndash; wskazuje, czy wywołanie zwrotne powinno uruchamiać swój kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="0e8ca-283">`CurrentState` &ndash; opisuje bieżący stan monitorowania do użycia w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="0e8ca-284">Metoda `InvokeChanged` jest podobna do wcześniejszego podejścia, z tą różnicą, że:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="0e8ca-285">Nie uruchamia tego kodu, chyba że `MonitoringEnabled` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="0e8ca-286">Wyprowadza bieżącą `state` w jej `WriteConsole` danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="0e8ca-287">`ConfigurationMonitor` wystąpienia jest zarejestrowany jako usługa w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="0e8ca-288">Strona indeks zapewnia kontrolę nad konfiguracją przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="0e8ca-289">Wystąpienie `IConfigurationMonitor` jest wstrzykiwane do `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="0e8ca-290">*Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0e8ca-291">Monitor konfiguracji (`_monitor`) służy do włączania lub wyłączania monitorowania oraz ustawiania bieżącego stanu na potrzeby przesyłania opinii o interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0e8ca-292">Gdy `OnPostStartMonitoring` jest wyzwalane, monitorowanie jest włączone, a bieżący stan jest wyczyszczony.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="0e8ca-293">Gdy `OnPostStopMonitoring` jest wyzwalane, monitorowanie jest wyłączone, a stan jest ustawiony na odzwierciedlenie tego, że monitorowanie nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="0e8ca-294">Przyciski w interfejsie użytkownika włączają i wyłączają monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="0e8ca-295">*Pages/index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="0e8ca-296">Monitoruj zmiany plików w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="0e8ca-296">Monitor cached file changes</span></span>

<span data-ttu-id="0e8ca-297">Zawartość pliku może być buforowana w pamięci przy użyciu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="0e8ca-298">Buforowanie w pamięci jest opisane w temacie [pamięć podręczna w pamięci podręcznej](xref:performance/caching/memory) .</span><span class="sxs-lookup"><span data-stu-id="0e8ca-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="0e8ca-299">Bez podejmowania dodatkowych czynności, takich jak implementacja opisana poniżej, *nieodświeżone (przestarzałe* ) dane są zwracane z pamięci podręcznej, jeśli dane źródłowe zostaną zmienione.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="0e8ca-300">Na przykład nie uwzględnia stanu pliku źródłowego w pamięci podręcznej podczas odnawiania przedziału czasu [wygaśnięcia](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) prowadzi do starych danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="0e8ca-301">Każde żądanie dotyczące danych odnawia przedział czasu wygaśnięcia, ale plik nigdy nie jest ponownie ładowany do pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="0e8ca-302">Wszystkie funkcje aplikacji korzystające z zawartości w pamięci podręcznej są uzależnione od potencjalnie otrzymywania nieodświeżonej zawartości.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="0e8ca-303">Użycie tokenów zmiany w scenariuszu buforowania plików uniemożliwia obecność przestarzałych zawartości plików w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="0e8ca-304">Przykładowa aplikacja prezentuje implementację metody.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="0e8ca-305">Przykład używa `GetFileContent` do:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="0e8ca-306">Zwróć zawartość pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-306">Return file content.</span></span>
* <span data-ttu-id="0e8ca-307">Zaimplementuj algorytm ponawiania prób z wycofywaniem z powrotem, aby uwzględnić przypadki, w których blokada pliku tymczasowo uniemożliwia odczytywanie pliku.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="0e8ca-308">*Narzędzia/Narzędzia. cs*:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="0e8ca-309">`FileService` jest tworzony w celu obsługi wyszukiwań w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="0e8ca-310">Wywołanie metody `GetFileContent`j usługi próbuje uzyskać zawartość pliku z pamięci podręcznej w pamięci i zwrócić ją do obiektu wywołującego (*Services/FileService. cs*).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="0e8ca-311">Jeśli buforowana zawartość nie zostanie znaleziona przy użyciu klucza pamięci podręcznej, zostaną wykonane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="0e8ca-312">Zawartość pliku jest uzyskiwana przy użyciu `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="0e8ca-313">Token zmiany jest uzyskiwany od dostawcy plików z [IFileProviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="0e8ca-314">Wywołanie zwrotne tokenu jest wyzwalane, gdy plik zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="0e8ca-315">Zawartość pliku jest buforowana z [ruchomym](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) okresem ważności.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="0e8ca-316">Token zmiany jest dołączony do [MemoryCacheEntryExtensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) w celu wykluczenia wpisu pamięci podręcznej, jeśli plik zostanie zmieniony w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="0e8ca-317">W poniższym przykładzie pliki są przechowywane w [katalogu głównym zawartości](xref:fundamentals/index#content-root)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-317">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="0e8ca-318">[IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) służy do uzyskiwania <xref:Microsoft.Extensions.FileProviders.IFileProvider> wskazujących <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="0e8ca-319">`filePath` jest uzyskiwany za pomocą [IFileInfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="0e8ca-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="0e8ca-320">`FileService` jest zarejestrowany w kontenerze usługi wraz z usługą buforowania pamięci.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="0e8ca-321">W `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0e8ca-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="0e8ca-322">Model strony ładuje zawartość pliku przy użyciu usługi.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="0e8ca-323">W metodzie `OnGet` strony indeksu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="0e8ca-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="0e8ca-324">Klasa CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="0e8ca-324">CompositeChangeToken class</span></span>

<span data-ttu-id="0e8ca-325">Dla reprezentowania jednego lub więcej wystąpień `IChangeToken` w jednym obiekcie, użyj klasy <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="0e8ca-326">`HasChanged` w raportach tokenów złożonych, `true`, jeśli `true``HasChanged` tokenem reprezentowanego.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="0e8ca-327">`ActiveChangeCallbacks` w raportach tokenów złożonych, `true`, jeśli `true``ActiveChangeCallbacks` tokenem reprezentowanego.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="0e8ca-328">W przypadku wystąpienia wielu współbieżnych zdarzeń zmiany wywołanie zwrotne zmiany złożonego jest wywoływana jednokrotnie.</span><span class="sxs-lookup"><span data-stu-id="0e8ca-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0e8ca-329">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0e8ca-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
