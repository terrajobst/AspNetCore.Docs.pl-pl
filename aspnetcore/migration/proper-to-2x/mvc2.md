---
title: Migrowanie z programu ASP.NET do ASP.NET Core 2.0
author: isaac2004
description: Odbieranie wskazówki dotyczące migrowania istniejących aplikacji ASP.NET MVC lub Web API platformy ASP.NET Core 2.0.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/mvc2
ms.openlocfilehash: 006eeeba28dbd351698e46547abe3c96818a63d9
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090462"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="59c0b-103">Migrowanie z programu ASP.NET do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="59c0b-103">Migrate from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="59c0b-104">Przez [Levin Tomasz](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="59c0b-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="59c0b-105">Ten artykuł służy jako przewodnik odwołania do migrowania aplikacji ASP.NET do ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="59c0b-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59c0b-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="59c0b-106">Prerequisites</span></span>

<span data-ttu-id="59c0b-107">Zainstaluj **jeden** z następujących czynności z [.NET pliki do pobrania: Windows](https://www.microsoft.com/net/download/windows):</span><span class="sxs-lookup"><span data-stu-id="59c0b-107">Install **one** of the following from [.NET Downloads: Windows](https://www.microsoft.com/net/download/windows):</span></span>

* <span data-ttu-id="59c0b-108">Zestaw SDK dla platformy .NET core</span><span class="sxs-lookup"><span data-stu-id="59c0b-108">.NET Core SDK</span></span>
* <span data-ttu-id="59c0b-109">Program Visual Studio for Windows</span><span class="sxs-lookup"><span data-stu-id="59c0b-109">Visual Studio for Windows</span></span>
  * <span data-ttu-id="59c0b-110">**ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="59c0b-110">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="59c0b-111">**Programowanie dla wielu platform .NET core** obciążenia</span><span class="sxs-lookup"><span data-stu-id="59c0b-111">**.NET Core cross-platform development** workload</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="59c0b-112">Platformy docelowe</span><span class="sxs-lookup"><span data-stu-id="59c0b-112">Target frameworks</span></span>

<span data-ttu-id="59c0b-113">Projekty ASP.NET Core 2.0 oferują deweloperom elastyczność określania wartości docelowej platformy .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="59c0b-113">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="59c0b-114">Zobacz [Wybieranie między programami .NET Core i .NET Framework dla aplikacji serwerowych](/dotnet/standard/choosing-core-framework-server) ustalenie, której lokalizacja docelowa jest najbardziej odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="59c0b-114">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="59c0b-115">Podczas przeznaczonych dla platformy .NET Framework, projekty muszą odwoływać się do poszczególnych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="59c0b-115">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="59c0b-116">Przeznaczone dla platformy .NET Core pozwala wyeliminować wiele odwołań pakietu jawne, dzięki ASP.NET Core 2.0 [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="59c0b-116">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="59c0b-117">Zainstaluj `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all w projekcie:</span><span class="sxs-lookup"><span data-stu-id="59c0b-117">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.1.4" />
</ItemGroup>
```

<span data-ttu-id="59c0b-118">W przypadku meta Microsoft.aspnetcore.all żadnych pakietów, do którego odwołuje się meta Microsoft.aspnetcore.all są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="59c0b-118">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="59c0b-119">Store środowiska uruchomieniowego programu .NET Core obejmuje te zasoby, i są one wstępnie skompilowany do zwiększenia wydajności.</span><span class="sxs-lookup"><span data-stu-id="59c0b-119">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="59c0b-120">Zobacz <xref:fundamentals/metapackage> Aby uzyskać więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="59c0b-120">See <xref:fundamentals/metapackage> for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="59c0b-121">Różnice struktura projektu</span><span class="sxs-lookup"><span data-stu-id="59c0b-121">Project structure differences</span></span>

<span data-ttu-id="59c0b-122">*.Csproj* format pliku została uproszczona w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59c0b-122">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="59c0b-123">Pewne istotne zmiany obejmują:</span><span class="sxs-lookup"><span data-stu-id="59c0b-123">Some notable changes include:</span></span>

* <span data-ttu-id="59c0b-124">Dołączenie jawne plików nie jest niezbędne zostały uznane za częścią projektu.</span><span class="sxs-lookup"><span data-stu-id="59c0b-124">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="59c0b-125">Zmniejsza to ryzyko konfliktów scalania XML, pracując w dużych zespołach.</span><span class="sxs-lookup"><span data-stu-id="59c0b-125">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
* <span data-ttu-id="59c0b-126">Nie ma żadnych identyfikatora GUID na podstawie odwołań do innych projektów, co zwiększa czytelność pliku.</span><span class="sxs-lookup"><span data-stu-id="59c0b-126">There are no GUID-based references to other projects, which improves file readability.</span></span>
* <span data-ttu-id="59c0b-127">Plik można edytować bez rozładowywania go w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="59c0b-127">The file can be edited without unloading it in Visual Studio:</span></span>

  ![Edytowanie pliku CSPROJ menu kontekstowego w programie Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="59c0b-129">Zastąpienie pliku Global.asax</span><span class="sxs-lookup"><span data-stu-id="59c0b-129">Global.asax file replacement</span></span>

<span data-ttu-id="59c0b-130">Platforma ASP.NET Core wprowadzono nowy mechanizm do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="59c0b-130">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="59c0b-131">Punkt wejścia dla aplikacji platformy ASP.NET to *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="59c0b-131">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="59c0b-132">Zadania, takie jak Konfiguracja tras i filtrowania i obszaru rejestracje są obsługiwane w *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="59c0b-132">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="59c0b-133">To podejście couples aplikacji i serwera, na którym jest wdrożona w taki sposób, że koliduje z wykonywaniem.</span><span class="sxs-lookup"><span data-stu-id="59c0b-133">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="59c0b-134">W ramach działań zmierzających do oddzielenia [OWIN](http://owin.org/) została wprowadzona w celu zapewnienia bardziej przejrzysty sposób na korzystanie z wielu platform ze sobą.</span><span class="sxs-lookup"><span data-stu-id="59c0b-134">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="59c0b-135">OWIN zawiera potoku można dodać tylko do modułów, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="59c0b-135">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="59c0b-136">Środowisko hostingu zajmuje [uruchamiania](xref:fundamentals/startup) funkcja Konfigurowanie usług i potok żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="59c0b-136">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="59c0b-137">`Startup` rejestruje zestaw oprogramowania pośredniczącego z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="59c0b-137">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="59c0b-138">Dla każdego żądania aplikacja wywołuje wszystkich składników oprogramowania pośredniczącego przy użyciu głównego wskaźnika połączonej listy do istniejącego zestawu obsługi.</span><span class="sxs-lookup"><span data-stu-id="59c0b-138">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="59c0b-139">Każdy składnik oprogramowania pośredniczącego można dodać jeden lub więcej programów obsługi żądania obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="59c0b-139">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="59c0b-140">Jest to realizowane przez zwracanie odwołania do programu obsługi, który jest nowy nagłówek listy.</span><span class="sxs-lookup"><span data-stu-id="59c0b-140">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="59c0b-141">Każdy program obsługi jest odpowiedzialny za zapamiętywania i wywoływanie kolejna procedura obsługi na liście.</span><span class="sxs-lookup"><span data-stu-id="59c0b-141">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="59c0b-142">Platforma ASP.NET Core jest punkt wejścia do aplikacji `Startup`, i nie ma już zależności *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="59c0b-142">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="59c0b-143">Korzystając z OWIN za pomocą .NET Framework, należy użyć podobną do następującej jako potok:</span><span class="sxs-lookup"><span data-stu-id="59c0b-143">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="59c0b-144">Konfiguruje trasy domyślne, a wartość domyślna to XmlSerialization w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="59c0b-144">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="59c0b-145">Dodaj do tego potoku innym oprogramowaniu pośredniczącym, zgodnie z potrzebami (Trwa ładowanie usług, ustawienia konfiguracji, pliki statyczne itp.).</span><span class="sxs-lookup"><span data-stu-id="59c0b-145">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="59c0b-146">Platforma ASP.NET Core korzysta z podobnego podejścia, ale nie jest zależny od OWIN do obsługi wpis.</span><span class="sxs-lookup"><span data-stu-id="59c0b-146">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="59c0b-147">Zamiast tego zostanie to zrobione za pośrednictwem *Program.cs* `Main` — metoda (podobne do aplikacji konsoli) i `Startup` jest załadowany poprzez miejsca.</span><span class="sxs-lookup"><span data-stu-id="59c0b-147">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="59c0b-148">`Startup` musi zawierać `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="59c0b-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="59c0b-149">W `Configure`, dodać niezbędne oprogramowanie pośredniczące do potoku.</span><span class="sxs-lookup"><span data-stu-id="59c0b-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="59c0b-150">W poniższym przykładzie (z domyślnego szablonu witryny sieci web) kilka metod rozszerzenia są używane do konfigurowania potoku obsługę:</span><span class="sxs-lookup"><span data-stu-id="59c0b-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="59c0b-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="59c0b-151">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="59c0b-152">Strony błędów</span><span class="sxs-lookup"><span data-stu-id="59c0b-152">Error pages</span></span>
* <span data-ttu-id="59c0b-153">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="59c0b-153">Static files</span></span>
* <span data-ttu-id="59c0b-154">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="59c0b-154">ASP.NET Core MVC</span></span>
* <span data-ttu-id="59c0b-155">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="59c0b-155">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="59c0b-156">Host i aplikacji ma zostały odłączone zapewniającą elastyczność przechodzenia do różnych platform w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="59c0b-156">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="59c0b-157">Aby uzyskać więcej informacji na temat odwołania do platformy ASP.NET Core uruchamianie i oprogramowanie pośredniczące, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="59c0b-157">For a more in-depth reference to ASP.NET Core Startup and Middleware, see <xref:fundamentals/startup>.</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="59c0b-158">Zapisywanie konfiguracji</span><span class="sxs-lookup"><span data-stu-id="59c0b-158">Storing configurations</span></span>

<span data-ttu-id="59c0b-159">Obsługuje platformy ASP.NET z przechowywaniem ustawień.</span><span class="sxs-lookup"><span data-stu-id="59c0b-159">ASP.NET supports storing settings.</span></span> <span data-ttu-id="59c0b-160">Te ustawienia są używane, na przykład do obsługi środowiska, w której zostały wdrożone aplikacje.</span><span class="sxs-lookup"><span data-stu-id="59c0b-160">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="59c0b-161">Powszechną praktyką było przechowywać wszystkie niestandardowe pary klucz wartość w `<appSettings>` części *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="59c0b-161">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="59c0b-162">Aplikacje odczytywać te ustawienia przy użyciu `ConfigurationManager.AppSettings` kolekcji w `System.Configuration` przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="59c0b-162">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="59c0b-163">Platforma ASP.NET Core można przechowywać dane konfiguracji dla aplikacji w dowolnym pliku i załadować je jako część uruchamianie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="59c0b-163">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="59c0b-164">Domyślny plik używany w szablonach projektu jest *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="59c0b-164">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="59c0b-165">Załadowanie pliku jako wystąpienia elementu `IConfiguration` wewnątrz aplikacji odbywa się w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="59c0b-165">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="59c0b-166">Aplikacja odczytuje z `Configuration` pobieranie ustawień:</span><span class="sxs-lookup"><span data-stu-id="59c0b-166">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="59c0b-167">Istnieją rozszerzenia tego podejścia w celu uproszczenia procesu bardziej niezawodne, takiej jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI), aby obciążenia usługi za pomocą tych wartości.</span><span class="sxs-lookup"><span data-stu-id="59c0b-167">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="59c0b-168">Podejście DI zawiera zestaw silnie typizowanych obiektów konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="59c0b-168">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="59c0b-169">**Uwaga:** Aby uzyskać więcej informacji na temat odwołania do konfiguracji platformy ASP.NET Core, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="59c0b-169">**Note:** For a more in-depth reference to ASP.NET Core configuration, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="59c0b-170">Wstrzykiwanie zależności natywnych</span><span class="sxs-lookup"><span data-stu-id="59c0b-170">Native dependency injection</span></span>

<span data-ttu-id="59c0b-171">Ważne podczas kompilowania dużych, skalowalnych aplikacji ma luźne powiązanie składników i usług.</span><span class="sxs-lookup"><span data-stu-id="59c0b-171">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="59c0b-172">[Wstrzykiwanie zależności](xref:fundamentals/dependency-injection) to technika popularnych służące osiągnięciu tego celu i jest to natywny składnik ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59c0b-172">[Dependency injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="59c0b-173">W aplikacjach ASP.NET deweloperzy zależy od innej biblioteki, aby zaimplementować iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="59c0b-173">In ASP.NET applications, developers rely on a third-party library to implement dependency injection.</span></span> <span data-ttu-id="59c0b-174">Jednego takiego biblioteka jest [Unity](https://github.com/unitycontainer/unity), podana przez Microsoft Patterns & rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="59c0b-174">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="59c0b-175">Przykład konfigurowania wstrzykiwanie zależności za pomocą aparatu Unity implementuje `IDependencyResolver` to opakowuje `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="59c0b-175">An example of setting up dependency injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="59c0b-176">Utwórz wystąpienie usługi `UnityContainer`, a następnie zarejestrować usługę i ustawić mechanizm rozpoznawania zależności `HttpConfiguration` do nowego wystąpienia programu `UnityResolver` kontenera:</span><span class="sxs-lookup"><span data-stu-id="59c0b-176">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="59c0b-177">Wstrzykiwanie `IProductRepository` w razie potrzeby:</span><span class="sxs-lookup"><span data-stu-id="59c0b-177">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="59c0b-178">Wstrzykiwanie zależności jest częścią platformy ASP.NET Core, dlatego można dodać usługi `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59c0b-178">Because dependency injection is part of ASP.NET Core, you can add your service in the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="59c0b-179">Repozytorium może być wprowadzony dowolnego miejsca, podobnie jak przy użyciu aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="59c0b-179">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="59c0b-180">Aby uzyskać więcej informacji na temat wstrzykiwanie zależności w programie ASP.NET Core, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="59c0b-180">For more information on dependency injection in ASP.NET Core, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="59c0b-181">Dostarczania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="59c0b-181">Serving static files</span></span>

<span data-ttu-id="59c0b-182">Ważnym elementem programowania dla sieci web jest możliwość obsługi zasoby statyczne, po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="59c0b-182">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="59c0b-183">Najbardziej typowe przykłady plików statycznych są HTML, CSS, Javascript i obrazy.</span><span class="sxs-lookup"><span data-stu-id="59c0b-183">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="59c0b-184">Te pliki muszą być zapisane w lokalizacji opublikowany aplikacji (lub sieci CDN) i odwołania, dzięki czemu mogą być ładowane przez żądanie.</span><span class="sxs-lookup"><span data-stu-id="59c0b-184">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="59c0b-185">Ten proces został zmieniony w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59c0b-185">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="59c0b-186">W programie ASP.NET: pliki statyczne są przechowywane w różnych katalogach i przywoływany w widokach.</span><span class="sxs-lookup"><span data-stu-id="59c0b-186">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="59c0b-187">W programie ASP.NET Core, pliki statyczne są przechowywane w "root sieci web" (*&lt;zawartości głównego&gt;/wwwroot*), chyba że skonfigurowano inaczej.</span><span class="sxs-lookup"><span data-stu-id="59c0b-187">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="59c0b-188">Pliki są ładowane do potoku żądania za pomocą wywołania `UseStaticFiles` metoda rozszerzenia z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="59c0b-188">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="59c0b-189">**Uwaga:** Jeśli przeznaczony dla .NET Framework, należy zainstalować pakiet NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="59c0b-189">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="59c0b-190">Na przykład zasób obrazu w *wwwroot/obrazów* folder jest dostępny w przeglądarce w lokalizacji, takich jak `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="59c0b-190">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="59c0b-191">**Uwaga:** Aby uzyskać więcej informacji na temat odwołania do dostarczania plików statycznych z platformy ASP.NET Core, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="59c0b-191">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see <xref:fundamentals/static-files>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59c0b-192">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="59c0b-192">Additional resources</span></span>

* [<span data-ttu-id="59c0b-193">Przenoszenie bibliotek .NET Core</span><span class="sxs-lookup"><span data-stu-id="59c0b-193">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
