---
title: Migrowanie z programu ASP.NET do ASP.NET Core
author: isaac2004
description: Odbieranie wskazówki dotyczące migrowania istniejących aplikacji ASP.NET MVC lub internetowych interfejsów API do Core.web platformy ASP.NET
ms.author: scaddie
ms.date: 12/11/2018
uid: migration/proper-to-2x/index
ms.openlocfilehash: a9eef832a68afa1a73e3c7c545378da190602ce2
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284399"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a><span data-ttu-id="9a02a-103">Migrowanie z programu ASP.NET do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a02a-103">Migrate from ASP.NET to ASP.NET Core</span></span>

<span data-ttu-id="9a02a-104">Przez [Levin Tomasz](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="9a02a-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="9a02a-105">Ten artykuł służy jako przewodnik migracji aplikacji ASP.NET do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a02a-105">This article serves as a reference guide for migrating ASP.NET apps to ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a02a-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9a02a-106">Prerequisites</span></span>

[<span data-ttu-id="9a02a-107">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="9a02a-107">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download)

## <a name="target-frameworks"></a><span data-ttu-id="9a02a-108">Platformy docelowe</span><span class="sxs-lookup"><span data-stu-id="9a02a-108">Target frameworks</span></span>

<span data-ttu-id="9a02a-109">Projekty ASP.NET Core oferują deweloperom elastyczność określania wartości docelowej platformy .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9a02a-109">ASP.NET Core projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="9a02a-110">Zobacz [Wybieranie między programami .NET Core i .NET Framework dla aplikacji serwerowych](/dotnet/standard/choosing-core-framework-server) ustalenie, której lokalizacja docelowa jest najbardziej odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="9a02a-110">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="9a02a-111">Podczas przeznaczonych dla platformy .NET Framework, projekty muszą odwoływać się do poszczególnych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="9a02a-111">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="9a02a-112">Przeznaczone dla platformy .NET Core pozwala wyeliminować wiele odwołań pakietu jawne, dzięki zastosowaniu platformy ASP.NET Core [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9a02a-112">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core [metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9a02a-113">Zainstaluj `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all w projekcie:</span><span class="sxs-lookup"><span data-stu-id="9a02a-113">Install the `Microsoft.AspNetCore.App` metapackage in your project:</span></span>

```xml
<ItemGroup>
   <PackageReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

<span data-ttu-id="9a02a-114">W przypadku meta Microsoft.aspnetcore.all żadnych pakietów, do którego odwołuje się meta Microsoft.aspnetcore.all są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9a02a-114">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="9a02a-115">Store środowiska uruchomieniowego programu .NET Core obejmuje te zasoby, i są one wstępnie skompilowany do zwiększenia wydajności.</span><span class="sxs-lookup"><span data-stu-id="9a02a-115">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="9a02a-116">Zobacz [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App, dla platformy ASP.NET Core](xref:fundamentals/metapackage-app) Aby uzyskać więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="9a02a-116">See [Microsoft.AspNetCore.App metapackage for ASP.NET Core](xref:fundamentals/metapackage-app) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="9a02a-117">Różnice struktura projektu</span><span class="sxs-lookup"><span data-stu-id="9a02a-117">Project structure differences</span></span>

<span data-ttu-id="9a02a-118">*.Csproj* format pliku została uproszczona w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a02a-118">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="9a02a-119">Pewne istotne zmiany obejmują:</span><span class="sxs-lookup"><span data-stu-id="9a02a-119">Some notable changes include:</span></span>

- <span data-ttu-id="9a02a-120">Dołączenie jawne plików nie jest niezbędne zostały uznane za częścią projektu.</span><span class="sxs-lookup"><span data-stu-id="9a02a-120">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="9a02a-121">Zmniejsza to ryzyko konfliktów scalania XML, pracując w dużych zespołach.</span><span class="sxs-lookup"><span data-stu-id="9a02a-121">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="9a02a-122">Nie ma żadnych identyfikatora GUID na podstawie odwołań do innych projektów, co zwiększa czytelność pliku.</span><span class="sxs-lookup"><span data-stu-id="9a02a-122">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="9a02a-123">Plik można edytować bez rozładowywania go w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9a02a-123">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Edytowanie pliku CSPROJ menu kontekstowego w programie Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="9a02a-125">Zastąpienie pliku Global.asax</span><span class="sxs-lookup"><span data-stu-id="9a02a-125">Global.asax file replacement</span></span>

<span data-ttu-id="9a02a-126">Platforma ASP.NET Core wprowadzono nowy mechanizm do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9a02a-126">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="9a02a-127">Punkt wejścia dla aplikacji platformy ASP.NET to *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="9a02a-127">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="9a02a-128">Zadania, takie jak Konfiguracja tras i filtrowania i obszaru rejestracje są obsługiwane w *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="9a02a-128">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="9a02a-129">To podejście couples aplikacji i serwera, na którym jest wdrożona w taki sposób, że koliduje z wykonywaniem.</span><span class="sxs-lookup"><span data-stu-id="9a02a-129">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="9a02a-130">W ramach działań zmierzających do oddzielenia [OWIN](http://owin.org/) została wprowadzona w celu zapewnienia bardziej przejrzysty sposób na korzystanie z wielu platform ze sobą.</span><span class="sxs-lookup"><span data-stu-id="9a02a-130">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="9a02a-131">OWIN zawiera potoku można dodać tylko do modułów, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="9a02a-131">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="9a02a-132">Środowisko hostingu zajmuje [uruchamiania](xref:fundamentals/startup) funkcja Konfigurowanie usług i potok żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9a02a-132">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="9a02a-133">`Startup` rejestruje zestaw oprogramowania pośredniczącego z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="9a02a-133">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="9a02a-134">Dla każdego żądania aplikacja wywołuje wszystkich składników oprogramowania pośredniczącego przy użyciu głównego wskaźnika połączonej listy do istniejącego zestawu obsługi.</span><span class="sxs-lookup"><span data-stu-id="9a02a-134">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="9a02a-135">Każdy składnik oprogramowania pośredniczącego można dodać jeden lub więcej programów obsługi żądania obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="9a02a-135">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="9a02a-136">Jest to realizowane przez zwracanie odwołania do programu obsługi, który jest nowy nagłówek listy.</span><span class="sxs-lookup"><span data-stu-id="9a02a-136">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="9a02a-137">Każdy program obsługi jest odpowiedzialny za zapamiętywania i wywoływanie kolejna procedura obsługi na liście.</span><span class="sxs-lookup"><span data-stu-id="9a02a-137">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="9a02a-138">Platforma ASP.NET Core jest punkt wejścia do aplikacji `Startup`, i nie ma już zależności *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="9a02a-138">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="9a02a-139">Korzystając z OWIN za pomocą .NET Framework, należy użyć podobną do następującej jako potok:</span><span class="sxs-lookup"><span data-stu-id="9a02a-139">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="9a02a-140">Konfiguruje trasy domyślne, a wartość domyślna to XmlSerialization w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="9a02a-140">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="9a02a-141">Dodaj do tego potoku innym oprogramowaniu pośredniczącym, zgodnie z potrzebami (Trwa ładowanie usług, ustawienia konfiguracji, pliki statyczne itp.).</span><span class="sxs-lookup"><span data-stu-id="9a02a-141">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="9a02a-142">Platforma ASP.NET Core korzysta z podobnego podejścia, ale nie jest zależny od OWIN do obsługi wpis.</span><span class="sxs-lookup"><span data-stu-id="9a02a-142">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="9a02a-143">Zamiast tego zostanie to zrobione za pośrednictwem *Program.cs* `Main` — metoda (podobne do aplikacji konsoli) i `Startup` jest załadowany poprzez miejsca.</span><span class="sxs-lookup"><span data-stu-id="9a02a-143">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="9a02a-144">`Startup` musi zawierać `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="9a02a-144">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="9a02a-145">W `Configure`, dodać niezbędne oprogramowanie pośredniczące do potoku.</span><span class="sxs-lookup"><span data-stu-id="9a02a-145">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="9a02a-146">W poniższym przykładzie (z domyślnego szablonu witryny sieci web) metody rozszerzenia, należy skonfigurować potok o obsługę:</span><span class="sxs-lookup"><span data-stu-id="9a02a-146">In the following example (from the default web site template), extension methods configure the pipeline with support for:</span></span>

* <span data-ttu-id="9a02a-147">Strony błędów</span><span class="sxs-lookup"><span data-stu-id="9a02a-147">Error pages</span></span>
* <span data-ttu-id="9a02a-148">Zabezpieczenia transportu Strict HTTP</span><span class="sxs-lookup"><span data-stu-id="9a02a-148">HTTP Strict Transport Security</span></span>
* <span data-ttu-id="9a02a-149">Przekierowywanie HTTP do HTTPS</span><span class="sxs-lookup"><span data-stu-id="9a02a-149">HTTP redirection to HTTPS</span></span>
* <span data-ttu-id="9a02a-150">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="9a02a-150">ASP.NET Core MVC</span></span>

[!code-csharp[](samples/startup.cs)]

<span data-ttu-id="9a02a-151">Host i aplikacji ma zostały odłączone zapewniającą elastyczność przechodzenia do różnych platform w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="9a02a-151">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="9a02a-152">Aby uzyskać więcej informacji na temat odwołania do platformy ASP.NET Core uruchamianie i oprogramowanie pośredniczące, zobacz [uruchamiania w programie ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="9a02a-152">For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="store-configurations"></a><span data-ttu-id="9a02a-153">Konfiguracje Store</span><span class="sxs-lookup"><span data-stu-id="9a02a-153">Store configurations</span></span>

<span data-ttu-id="9a02a-154">Obsługuje platformy ASP.NET z przechowywaniem ustawień.</span><span class="sxs-lookup"><span data-stu-id="9a02a-154">ASP.NET supports storing settings.</span></span> <span data-ttu-id="9a02a-155">Te ustawienia są używane, na przykład do obsługi środowiska, w której zostały wdrożone aplikacje.</span><span class="sxs-lookup"><span data-stu-id="9a02a-155">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="9a02a-156">Powszechną praktyką było przechowywać wszystkie niestandardowe pary klucz wartość w `<appSettings>` części *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="9a02a-156">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="9a02a-157">Aplikacje odczytywać te ustawienia przy użyciu `ConfigurationManager.AppSettings` kolekcji w `System.Configuration` przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="9a02a-157">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="9a02a-158">Platforma ASP.NET Core można przechowywać dane konfiguracji dla aplikacji w dowolnym pliku i załadować je jako część uruchamianie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9a02a-158">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="9a02a-159">Domyślny plik używany w szablonach projektu jest *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9a02a-159">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="9a02a-160">Załadowanie pliku jako wystąpienia elementu `IConfiguration` wewnątrz aplikacji odbywa się w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9a02a-160">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="9a02a-161">Aplikacja odczytuje z `Configuration` pobieranie ustawień:</span><span class="sxs-lookup"><span data-stu-id="9a02a-161">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="9a02a-162">Istnieją rozszerzenia tego podejścia w celu uproszczenia procesu bardziej niezawodne, takiej jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI), aby obciążenia usługi za pomocą tych wartości.</span><span class="sxs-lookup"><span data-stu-id="9a02a-162">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="9a02a-163">Podejście DI zawiera zestaw silnie typizowanych obiektów konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9a02a-163">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> <span data-ttu-id="9a02a-164">Aby uzyskać więcej informacji na temat odwołania do konfiguracji platformy ASP.NET Core, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9a02a-164">For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="9a02a-165">Wstrzykiwanie zależności natywnych</span><span class="sxs-lookup"><span data-stu-id="9a02a-165">Native dependency injection</span></span>

<span data-ttu-id="9a02a-166">Ważne podczas kompilowania dużych, skalowalnych aplikacji ma luźne powiązanie składników i usług.</span><span class="sxs-lookup"><span data-stu-id="9a02a-166">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="9a02a-167">[Wstrzykiwanie zależności](xref:fundamentals/dependency-injection) to technika popularnych służące osiągnięciu tego celu i jest to natywny składnik ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a02a-167">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="9a02a-168">W aplikacjach ASP.NET deweloperzy zależy od innej biblioteki, aby zaimplementować iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="9a02a-168">In ASP.NET apps, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="9a02a-169">Jednego takiego biblioteka jest [Unity](https://github.com/unitycontainer/unity), podana przez Microsoft Patterns & rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="9a02a-169">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="9a02a-170">Przykład konfigurowania wstrzykiwanie zależności za pomocą aparatu Unity implementuje `IDependencyResolver` to opakowuje `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="9a02a-170">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="9a02a-171">Utwórz wystąpienie usługi `UnityContainer`, a następnie zarejestrować usługę i ustawić mechanizm rozpoznawania zależności `HttpConfiguration` do nowego wystąpienia programu `UnityResolver` kontenera:</span><span class="sxs-lookup"><span data-stu-id="9a02a-171">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="9a02a-172">Wstrzykiwanie `IProductRepository` w razie potrzeby:</span><span class="sxs-lookup"><span data-stu-id="9a02a-172">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="9a02a-173">Wstrzykiwanie zależności jest częścią platformy ASP.NET Core, dlatego można dodać usługi `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9a02a-173">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="9a02a-174">Repozytorium może być wprowadzony dowolnego miejsca, podobnie jak przy użyciu aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="9a02a-174">The repository can be injected anywhere, as was true with Unity.</span></span>

> [!NOTE]
> <span data-ttu-id="9a02a-175">Aby uzyskać więcej informacji na temat wstrzykiwanie zależności, zobacz [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9a02a-175">For more information on dependency injection, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="9a02a-176">Obsługa plików statycznych</span><span class="sxs-lookup"><span data-stu-id="9a02a-176">Serve static files</span></span>

<span data-ttu-id="9a02a-177">Ważnym elementem programowania dla sieci web jest możliwość obsługi zasoby statyczne, po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="9a02a-177">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="9a02a-178">Najbardziej typowe przykłady plików statycznych są HTML, CSS, Javascript i obrazy.</span><span class="sxs-lookup"><span data-stu-id="9a02a-178">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="9a02a-179">Te pliki muszą być zapisane w lokalizacji opublikowany aplikacji (lub sieci CDN) i odwołania, dzięki czemu mogą być ładowane przez żądanie.</span><span class="sxs-lookup"><span data-stu-id="9a02a-179">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="9a02a-180">Ten proces został zmieniony w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a02a-180">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="9a02a-181">W programie ASP.NET: pliki statyczne są przechowywane w różnych katalogach i przywoływany w widokach.</span><span class="sxs-lookup"><span data-stu-id="9a02a-181">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="9a02a-182">W programie ASP.NET Core, pliki statyczne są przechowywane w "root sieci web" (*&lt;zawartości głównego&gt;/wwwroot*), chyba że skonfigurowano inaczej.</span><span class="sxs-lookup"><span data-stu-id="9a02a-182">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="9a02a-183">Pliki są ładowane do potoku żądania za pomocą wywołania `UseStaticFiles` metoda rozszerzenia z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9a02a-183">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> <span data-ttu-id="9a02a-184">Jeśli przeznaczony dla .NET Framework, należy zainstalować pakiet NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="9a02a-184">If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="9a02a-185">Na przykład zasób obrazu w *wwwroot/obrazów* folder jest dostępny w przeglądarce w lokalizacji, takich jak `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="9a02a-185">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

> [!NOTE]
> <span data-ttu-id="9a02a-186">Aby uzyskać więcej informacji na temat odwołania do dostarczania plików statycznych z platformy ASP.NET Core, zobacz [pliki statyczne](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="9a02a-186">For a more in-depth reference to serving static files in ASP.NET Core, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a02a-187">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9a02a-187">Additional resources</span></span>

- [<span data-ttu-id="9a02a-188">Przenoszenie bibliotek .NET Core</span><span class="sxs-lookup"><span data-stu-id="9a02a-188">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
