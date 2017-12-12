---
title: Co to jest nowe w programie ASP.NET 2.0 Core
author: rick-anderson
description: Co to jest nowe w programie ASP.NET 2.0 Core
keywords: Platformy ASP.NET Core, informacje o wersji, nowe funkcje
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: 08c9f457-9c24-40f9-a08b-47dc251e4cec
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: 98af3788652e87f6222551cb4a8e5427b312660c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="a32e1-104">Co to jest nowe w programie ASP.NET 2.0 Core</span><span class="sxs-lookup"><span data-stu-id="a32e1-104">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="a32e1-105">W tym artykule omówiono najbardziej znaczących zmian w programie ASP.NET 2.0 Core, wraz z łączami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="a32e1-105">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="a32e1-106">Stron razor</span><span class="sxs-lookup"><span data-stu-id="a32e1-106">Razor Pages</span></span>

<span data-ttu-id="a32e1-107">Stron razor to nowa funkcja platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.</span><span class="sxs-lookup"><span data-stu-id="a32e1-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="a32e1-108">Aby uzyskać więcej informacji zobacz wprowadzenie i samouczek:</span><span class="sxs-lookup"><span data-stu-id="a32e1-108">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="a32e1-109">Wprowadzenie do stron Razor</span><span class="sxs-lookup"><span data-stu-id="a32e1-109">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="a32e1-110">Wprowadzenie do stron Razor</span><span class="sxs-lookup"><span data-stu-id="a32e1-110">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="a32e1-111">Metapackage platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a32e1-111">ASP.NET Core metapackage</span></span>

<span data-ttu-id="a32e1-112">Nowe metapackage platformy ASP.NET Core obejmuje wszystkie pakiety wprowadzone i obsługiwane przez zespoły programu Entity Framework Core i ASP.NET Core, wraz z ich zależności wewnętrzne i 3rd firm.</span><span class="sxs-lookup"><span data-stu-id="a32e1-112">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="a32e1-113">Nie trzeba wybrać poszczególne platformy ASP.NET Core funkcje przez pakiet.</span><span class="sxs-lookup"><span data-stu-id="a32e1-113">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="a32e1-114">Wszystkie funkcje dostępne w [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pakietu.</span><span class="sxs-lookup"><span data-stu-id="a32e1-114">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="a32e1-115">Szablony domyślne, użyj tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="a32e1-115">The default templates use this package.</span></span>

<span data-ttu-id="a32e1-116">Aby uzyskać więcej informacji, zobacz [metapackage Microsoft.AspNetCore.All dla programu ASP.NET 2.0 Core](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="a32e1-116">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="a32e1-117">Środowisko uruchomieniowe magazynu</span><span class="sxs-lookup"><span data-stu-id="a32e1-117">Runtime Store</span></span>

<span data-ttu-id="a32e1-118">Aplikacje używające `Microsoft.AspNetCore.All` metapackage automatycznie korzystać z nowego magazynu środowiska uruchomieniowego .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a32e1-118">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="a32e1-119">Magazyn zawiera wszystkie zasoby środowiska uruchomieniowego potrzebne do uruchomienia aplikacji platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a32e1-119">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="a32e1-120">Jeśli używasz `Microsoft.AspNetCore.All` metapackage, nie zasoby, z którym związane są odwołania pakietów platformy ASP.NET Core NuGet są wdrażane w aplikacji, ponieważ są one już przechowywane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="a32e1-120">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="a32e1-121">Zasoby w magazynie środowiska uruchomieniowego są również wstępnie skompilowanym skrócić czas uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a32e1-121">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="a32e1-122">Aby uzyskać więcej informacji, zobacz [magazynu środowiska wykonawczego](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="a32e1-122">For more information, see [Runtime store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="a32e1-123">.NET 2.0 standardowe</span><span class="sxs-lookup"><span data-stu-id="a32e1-123">.NET Standard 2.0</span></span>

<span data-ttu-id="a32e1-124">Pakiety programu ASP.NET 2.0 Core docelowe .NET 2.0 standardowa.</span><span class="sxs-lookup"><span data-stu-id="a32e1-124">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="a32e1-125">Pakiety mogą odwoływać się inne biblioteki .NET 2.0 standardowych i mogą być uruchamiane na standardowe .NET 2.0 zgodne implementacje .NET, w tym .NET Core 2.0 i .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="a32e1-125">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="a32e1-126">`Microsoft.AspNetCore.All` Metapackage dotyczy tylko .NET Core 2.0, ponieważ jest on przeznaczony do użycia ze sklepem środowiska uruchomieniowego .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a32e1-126">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it is intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="a32e1-127">Aktualizacja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a32e1-127">Configuration update</span></span>

<span data-ttu-id="a32e1-128">`IConfiguration` Wystąpienia jest domyślnie w programie ASP.NET 2.0 Core dodawany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="a32e1-128">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="a32e1-129">`IConfiguration`w usługach kontenera ułatwia tworzenie aplikacji można pobrać wartości konfiguracji z kontenera.</span><span class="sxs-lookup"><span data-stu-id="a32e1-129">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="a32e1-130">Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem GitHub](https://github.com/aspnet/Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="a32e1-130">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="a32e1-131">Rejestrowanie aktualizacji</span><span class="sxs-lookup"><span data-stu-id="a32e1-131">Logging update</span></span>

<span data-ttu-id="a32e1-132">W programie ASP.NET 2.0 Core rejestrowanie jest domyślnie włączona do systemu (Podpisane) iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="a32e1-132">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="a32e1-133">Dodawanie dostawcy i skonfigurować filtrowanie w *Program.cs* zamiast w pliku *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a32e1-133">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="a32e1-134">Wartością domyślną `ILoggerFactory` obsługuje filtrowanie w taki sposób, który pozwala używać jednego elastyczne podejście zarówno dla filtrowania krzyżowego dostawcy i filtrowanie określonego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="a32e1-134">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="a32e1-135">Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="a32e1-135">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="a32e1-136">Aktualizacja uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a32e1-136">Authentication update</span></span>

<span data-ttu-id="a32e1-137">Nowy model uwierzytelniania ułatwia konfigurowanie uwierzytelniania dla aplikacji przy użyciu Podpisane.</span><span class="sxs-lookup"><span data-stu-id="a32e1-137">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="a32e1-138">Nowe szablony są dostępne do konfigurowania uwierzytelniania dla aplikacji sieci web i interfejsów API przy użyciu [usługi Azure AD B2C] sieci web (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="a32e1-138">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="a32e1-139">Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem GitHub](https://github.com/aspnet/Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="a32e1-139">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="a32e1-140">Aktualizacja tożsamości</span><span class="sxs-lookup"><span data-stu-id="a32e1-140">Identity update</span></span>

<span data-ttu-id="a32e1-141">Ułatwiliśmy ułatwia tworzenie bezpiecznej przy użyciu tożsamości w programie ASP.NET 2.0 podstawowych interfejsów API sieci web.</span><span class="sxs-lookup"><span data-stu-id="a32e1-141">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="a32e1-142">W przypadku uzyskania tokenów dostępu do uzyskiwania dostępu do sieci web API przy użyciu [biblioteki uwierzytelniania firmy Microsoft (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="a32e1-142">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="a32e1-143">Aby uzyskać więcej informacji na temat uwierzytelniania zmian w 2.0 zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="a32e1-143">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="a32e1-144">Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a32e1-144">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="a32e1-145">Włączenie generowania kodu QR uwierzytelniania aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a32e1-145">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="a32e1-146">Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="a32e1-146">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="a32e1-147">Szablony SPA</span><span class="sxs-lookup"><span data-stu-id="a32e1-147">SPA templates</span></span>

<span data-ttu-id="a32e1-148">Jednej strony aplikacji JEDNOSTRONICOWEJ szablonów projektu dla kątową, Aurelia Knockout.js, React.js i React.js z Redux są dostępne.</span><span class="sxs-lookup"><span data-stu-id="a32e1-148">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="a32e1-149">Dyrektywy Angular szablon został uaktualniony do dyrektywy Angular 4.</span><span class="sxs-lookup"><span data-stu-id="a32e1-149">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="a32e1-150">Szablony kątową i bibliotece React. domyślnie są dostępne; Aby uzyskać informacje o sposobie pobrania innych szablonów, zobacz [tworzenia nowego projektu SPA](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="a32e1-150">The Angular and React templates are available by default; for information about how to get the other templates, see [Creating a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="a32e1-151">Aby uzyskać informacje o sposobie budowania SPA w ASP.NET Core, zobacz [przy użyciu JavaScriptServices tworzenie aplikacji jednej strony](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="a32e1-151">For information about how to build a SPA in ASP.NET Core, see [Using JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="a32e1-152">Ulepszenia kestrel</span><span class="sxs-lookup"><span data-stu-id="a32e1-152">Kestrel improvements</span></span>

<span data-ttu-id="a32e1-153">Serwer sieci web Kestrel ma nowe funkcje ułatwiające odpowiedniejsze jako serwer internetowy.</span><span class="sxs-lookup"><span data-stu-id="a32e1-153">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="a32e1-154">Dodano wiele opcji konfiguracji serwera ograniczenia w `KestrelServerOptions` na nowe klasy `Limits` właściwości.</span><span class="sxs-lookup"><span data-stu-id="a32e1-154">We’ve added a number of server constraint configuration options in the `KestrelServerOptions` class’s new `Limits` property.</span></span> <span data-ttu-id="a32e1-155">Można teraz dodawać limity dla następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="a32e1-155">You can now add limits for the following:</span></span>

- <span data-ttu-id="a32e1-156">Maksymalna liczba połączeń klientów</span><span class="sxs-lookup"><span data-stu-id="a32e1-156">Maximum client connections</span></span>
- <span data-ttu-id="a32e1-157">Żądanie maksymalny rozmiar treści</span><span class="sxs-lookup"><span data-stu-id="a32e1-157">Maximum request body size</span></span>
- <span data-ttu-id="a32e1-158">Szybkość danych treści żądania minimalna</span><span class="sxs-lookup"><span data-stu-id="a32e1-158">Minimum request body data rate</span></span>

<span data-ttu-id="a32e1-159">Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a32e1-159">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="a32e1-160">WebListener zmieniona do pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a32e1-160">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="a32e1-161">Pakiety `Microsoft.AspNetCore.Server.WebListener` i `Microsoft.Net.Http.Server` zostały scalone w nowym pakiecie `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="a32e1-161">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="a32e1-162">Przestrzenie nazw zostały zaktualizowane do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="a32e1-162">The namespaces have been updated to match.</span></span>

<span data-ttu-id="a32e1-163">Aby uzyskać więcej informacji, zobacz [HTTP.sys implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a32e1-163">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="a32e1-164">Ulepszona obsługa nagłówka HTTP</span><span class="sxs-lookup"><span data-stu-id="a32e1-164">Enhanced HTTP header support</span></span>

<span data-ttu-id="a32e1-165">W przypadku używania MVC do przesyłania `FileStreamResult` lub `FileContentResult`, masz teraz opcję, aby ustawić `ETag` lub `LastModified` Data przesyłania zawartości.</span><span class="sxs-lookup"><span data-stu-id="a32e1-165">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="a32e1-166">Można ustawić te wartości zwracane zawartości z kodem podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="a32e1-166">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="a32e1-167">Powrót do odwiedzających plik zostanie ozdobione odpowiednie nagłówki HTTP dla `ETag` i `LastModified` wartości.</span><span class="sxs-lookup"><span data-stu-id="a32e1-167">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="a32e1-168">Jeśli obiekt odwiedzający żądań zawartości aplikacji przy użyciu nagłówka żądania zakresu, ASP.NET będzie uznają, że i obsługiwać tego nagłówka.</span><span class="sxs-lookup"><span data-stu-id="a32e1-168">If an application visitor requests content with a Range Request header, ASP.NET will recognize that and handle that header.</span></span> <span data-ttu-id="a32e1-169">Jeśli żądanej zawartości mogą być dostarczane częściowo, ASP.NET odpowiednio Pomiń i zwrócić tylko żądany zestaw bajtów.</span><span class="sxs-lookup"><span data-stu-id="a32e1-169">If the requested content can be partially delivered, ASP.NET will appropriately skip and return just the requested set of bytes.</span></span>  <span data-ttu-id="a32e1-170">Nie trzeba zapisać wszelkie specjalne obsługi do metody zmienić lub obsługi tej funkcji; odbywa się automatycznie za Ciebie.</span><span class="sxs-lookup"><span data-stu-id="a32e1-170">You do not need to write any special handlers into your methods to adapt or handle this feature; it is automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="a32e1-171">Hosting uruchamiania i usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="a32e1-171">Hosting startup and Application Insights</span></span>

<span data-ttu-id="a32e1-172">Środowiskach hostingu można teraz wstrzyknięcia zależności dodatkowych pakietów i wykonanie kodu podczas uruchamiania aplikacji bez konieczności jawnie zależności lub wywołać żadnych metod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a32e1-172">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="a32e1-173">Ta funkcja umożliwia włączanie niektórych środowiskach do funkcji "światła w górę" unikatowe dla tego środowiska bez dokładnej znajomości wcześniejsze aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a32e1-173">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="a32e1-174">W programie ASP.NET 2.0 Core, ta funkcja jest używana, można automatycznie włączyć usługi Application Insights diagnostyczne podczas debugowania w programie Visual Studio i (po skorzystaniu z rozwiązania) uruchomionej w usłudze Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="a32e1-174">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="a32e1-175">W związku z tym szablony projektów nie jest już Dodawanie pakietów usługi Application Insights i kodu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a32e1-175">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="a32e1-176">Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem GitHub](https://github.com/aspnet/Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="a32e1-176">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="a32e1-177">Automatyczne stosowanie tokenów zabezpieczających przed sfałszowaniem</span><span class="sxs-lookup"><span data-stu-id="a32e1-177">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="a32e1-178">Platformy ASP.NET Core zawsze pomogła kodowanie HTML zawartości domyślnie, ale z nową wersją Przenosimy dodatkowy krok, aby zapobiec fałszerstwie żądania międzywitrynowego (XSRF).</span><span class="sxs-lookup"><span data-stu-id="a32e1-178">ASP.NET Core has always helped HTML-encode your content by default, but with the new version we’re taking an extra step to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="a32e1-179">Platformy ASP.NET Core teraz Emituj tokenów zabezpieczających przed sfałszowaniem domyślnie i weryfikacji ich na stronach bez dodatkowej konfiguracji i akcji POST formularza.</span><span class="sxs-lookup"><span data-stu-id="a32e1-179">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="a32e1-180">Aby uzyskać więcej informacji, zobacz [zapobieganie Cross-Site żądania (XSRF/CSRF) Fałszerstwie w ASP.NET Core](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="a32e1-180">For more information, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="a32e1-181">Automatyczne wstępnej kompilacji</span><span class="sxs-lookup"><span data-stu-id="a32e1-181">Automatic precompilation</span></span>

<span data-ttu-id="a32e1-182">Wstępna kompilacja widoku razor jest domyślnie podczas publikowania, zmniejszenie jego rozmiar dane wyjściowe publikowania i aplikacji czas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a32e1-182">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="a32e1-183">Obsługa razor 7.1 C#</span><span class="sxs-lookup"><span data-stu-id="a32e1-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="a32e1-184">Do pracy z nowy kompilator Roslyn został zaktualizowany przez aparat widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="a32e1-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="a32e1-185">Która obejmuje obsługę funkcji C# 7.1 jak domyślne wyrażenia, wywnioskować nazwy spójnej kolekcji i dopasowywanie do wzorca z ogólnymi.</span><span class="sxs-lookup"><span data-stu-id="a32e1-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="a32e1-186">Aby użyć 7.1 C# w projekcie, dodaj następującą właściwość w pliku projektu, a następnie ponownie załaduj rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="a32e1-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="a32e1-187">Aby uzyskać informacje o stanie funkcji 7.1 C#, zobacz [repozytorium Roslyn GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="a32e1-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="a32e1-188">Inne aktualizacje dokumentacji 2.0</span><span class="sxs-lookup"><span data-stu-id="a32e1-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="a32e1-189">Tworzenie profilów dla programu Visual Studio i MSBuild, jak wdrażać aplikacje platformy ASP.NET Core publikowania</span><span class="sxs-lookup"><span data-stu-id="a32e1-189">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>](xref:publishing/web-publishing-vs)
* [<span data-ttu-id="a32e1-190">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="a32e1-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="a32e1-191">Konfigurowanie uwierzytelniania usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="a32e1-191">Configuring Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="a32e1-192">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="a32e1-192">Configuring Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="a32e1-193">Konfigurowanie uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="a32e1-193">Configuring Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="a32e1-194">Konfigurowanie uwierzytelniania Account firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="a32e1-194">Configuring Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="a32e1-195">Wskazówki dotyczące migracji</span><span class="sxs-lookup"><span data-stu-id="a32e1-195">Migration guidance</span></span>

<span data-ttu-id="a32e1-196">Aby uzyskać wskazówki dotyczące sposobu przeprowadzenia migracji platformy ASP.NET Core 1.x aplikacji platformy ASP.NET Core 2.0 zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="a32e1-196">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="a32e1-197">Migrowanie z platformy ASP.NET Core 1.x do platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="a32e1-197">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="a32e1-198">Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="a32e1-198">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="a32e1-199">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="a32e1-199">Additional Information</span></span>

<span data-ttu-id="a32e1-200">Aby uzyskać pełną listę zmian, zobacz [2.0 informacje o wersji platformy ASP.NET Core](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="a32e1-200">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="a32e1-201">Jeśli chcesz się połączyć z postęp i plany zespół deweloperów platformy ASP.NET Core, dostroić tygodniowych [Standup społeczności ASP.NET](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="a32e1-201">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
