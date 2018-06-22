---
title: Migrowanie ASP.NET ASP.NET Core
author: isaac2004
description: Odbieranie wskazówki dotyczące migrowania istniejących aplikacji ASP.NET MVC lub Web API do ASP.NET Core.web
ms.author: scaddie
ms.date: 08/27/2017
uid: migration/proper-to-2x/index
ms.openlocfilehash: 285f252767e4eacbb6a9ccc50bd98d1916e26bd4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273511"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a><span data-ttu-id="b03d6-103">Migrowanie ASP.NET ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b03d6-103">Migrate from ASP.NET to ASP.NET Core</span></span>

<span data-ttu-id="b03d6-104">Przez [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="b03d6-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="b03d6-105">W tym artykule służy jako Podręcznik migrowanie aplikacji ASP.NET do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b03d6-105">This article serves as a reference guide for migrating ASP.NET apps to ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b03d6-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b03d6-106">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

## <a name="target-frameworks"></a><span data-ttu-id="b03d6-107">Docelowych platform</span><span class="sxs-lookup"><span data-stu-id="b03d6-107">Target frameworks</span></span>

<span data-ttu-id="b03d6-108">Projektów platformy ASP.NET Core oferuje deweloperom elastyczność przeznaczonych dla platformy .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b03d6-108">ASP.NET Core projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="b03d6-109">Zobacz [wybór między .NET Core i .NET Framework dla aplikacji serwera](/dotnet/standard/choosing-core-framework-server) do określenia, które platforma docelowa jest najbardziej odpowiednia.</span><span class="sxs-lookup"><span data-stu-id="b03d6-109">See [Choosing between .NET Core and .NET Framework for server apps](/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="b03d6-110">Jeśli celem .NET Framework, projekty muszą odwołania się do poszczególnych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="b03d6-110">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="b03d6-111">Przeznaczonych dla platformy .NET Core pozwala wyeliminować wiele odwołań jawne pakietu, dzięki użyciu platformy ASP.NET Core [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b03d6-111">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="b03d6-112">Zainstaluj `Microsoft.AspNetCore.All` metapackage w projekcie:</span><span class="sxs-lookup"><span data-stu-id="b03d6-112">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="b03d6-113">W przypadku metapackage żadnych pakietów, do którego odwołuje się metapackage zostały wdrożone za pomocą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b03d6-113">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="b03d6-114">Magazynie środowiska uruchomieniowego .NET Core obejmuje te zasoby oraz są one wstępnie skompilowana do zwiększenia wydajności.</span><span class="sxs-lookup"><span data-stu-id="b03d6-114">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="b03d6-115">Zobacz [metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x](xref:fundamentals/metapackage) uzyskać więcej szczegółowych informacji.</span><span class="sxs-lookup"><span data-stu-id="b03d6-115">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="b03d6-116">Różnice struktury projektu</span><span class="sxs-lookup"><span data-stu-id="b03d6-116">Project structure differences</span></span>

<span data-ttu-id="b03d6-117">*.Csproj* format pliku jest teraz prostszy w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b03d6-117">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="b03d6-118">Niektóre ważne zmiany obejmują:</span><span class="sxs-lookup"><span data-stu-id="b03d6-118">Some notable changes include:</span></span>

- <span data-ttu-id="b03d6-119">Dołączenie jawne plików nie jest niezbędne do uważany za część projektu.</span><span class="sxs-lookup"><span data-stu-id="b03d6-119">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="b03d6-120">Zmniejsza ryzyko wystąpienia konfliktów scalania XML, podczas pracy w dużych zespołów.</span><span class="sxs-lookup"><span data-stu-id="b03d6-120">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="b03d6-121">Brak żadnych odwołań na podstawie identyfikatora GUID do innych projektów, które poprawia czytelność pliku.</span><span class="sxs-lookup"><span data-stu-id="b03d6-121">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="b03d6-122">Plik można edytować bez zwalnianie go w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b03d6-122">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Edytuj CSPROJ menu kontekstowego w Visual Studio 2017 r.](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="b03d6-124">Zastąpienia pliku Global.asax</span><span class="sxs-lookup"><span data-stu-id="b03d6-124">Global.asax file replacement</span></span>

<span data-ttu-id="b03d6-125">Platformy ASP.NET Core wprowadzono nowe mechanizm uruchamianie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b03d6-125">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="b03d6-126">Punkt wejścia dla aplikacji ASP.NET jest *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="b03d6-126">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="b03d6-127">Zadania, takie jak Konfiguracja tras i rejestracji filtru i obszaru są obsługiwane w *Global.asax* pliku.</span><span class="sxs-lookup"><span data-stu-id="b03d6-127">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="b03d6-128">Takie podejście couples aplikacji i serwera, na którym jest wdrożona w taki sposób, aby zakłócać implementacji.</span><span class="sxs-lookup"><span data-stu-id="b03d6-128">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="b03d6-129">W ramach działań zmierzających do oddziel [OWIN](http://owin.org/) wprowadzono w celu umożliwiają czyszczący jednocześnie używać wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b03d6-129">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="b03d6-130">OWIN zawiera potoku można dodać tylko moduły, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="b03d6-130">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="b03d6-131">Pobiera środowisko macierzyste [uruchamiania](xref:fundamentals/startup) funkcja Konfigurowanie usług i aplikacji żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="b03d6-131">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="b03d6-132">`Startup` rejestruje zestaw oprogramowania pośredniczącego z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="b03d6-132">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="b03d6-133">Dla każdego żądania aplikacja wywołuje wszystkich składników oprogramowania pośredniczącego ze wskaźnikiem head połączonej listy do istniejącego zestawu programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="b03d6-133">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="b03d6-134">Do żądania obsługi potoku poszczególnych składników oprogramowania pośredniczącego można dodać jeden lub więcej programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="b03d6-134">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="b03d6-135">Jest to osiągane przez zwracanie odwołania do obsługi, która jest nowy nagłówek listy.</span><span class="sxs-lookup"><span data-stu-id="b03d6-135">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="b03d6-136">Każdy program obsługi jest odpowiedzialny za zapamiętywanie i wywoływanie dalej obsługi na liście.</span><span class="sxs-lookup"><span data-stu-id="b03d6-136">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="b03d6-137">Jest punkt wejścia do aplikacji platformy ASP.NET Core `Startup`, i nie ma już zależność *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="b03d6-137">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="b03d6-138">Jeśli używasz OWIN z .NET Framework, użyj przypominać następujące jako potoku:</span><span class="sxs-lookup"><span data-stu-id="b03d6-138">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="b03d6-139">Konfiguruje trasy domyślnej i domyślnie XmlSerialization w formacie Json.</span><span class="sxs-lookup"><span data-stu-id="b03d6-139">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="b03d6-140">Dodaj do tego potoku innym oprogramowaniu pośredniczącym, zgodnie z potrzebami (ładowania usług, ustawienia konfiguracji, pliki statyczne, itp.).</span><span class="sxs-lookup"><span data-stu-id="b03d6-140">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="b03d6-141">Platformy ASP.NET Core używa podejście podobne, ale nie zależą od OWIN do obsługi wpis.</span><span class="sxs-lookup"><span data-stu-id="b03d6-141">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="b03d6-142">Zamiast tego, który odbywa się za pośrednictwem *Program.cs* `Main` metody (podobnie jak aplikacje konsoli) i `Startup` jest ładowany przez niego.</span><span class="sxs-lookup"><span data-stu-id="b03d6-142">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="b03d6-143">`Startup` musi zawierać `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="b03d6-143">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="b03d6-144">W `Configure`, Dodaj niezbędne oprogramowanie pośredniczące do potoku.</span><span class="sxs-lookup"><span data-stu-id="b03d6-144">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="b03d6-145">W poniższym przykładzie (z domyślnego szablonu witryny sieci web) kilka metod rozszerzenia są używane do konfigurowania potoku o obsługę:</span><span class="sxs-lookup"><span data-stu-id="b03d6-145">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="b03d6-146">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="b03d6-146">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="b03d6-147">Strony błędów</span><span class="sxs-lookup"><span data-stu-id="b03d6-147">Error pages</span></span>
* <span data-ttu-id="b03d6-148">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="b03d6-148">Static files</span></span>
* <span data-ttu-id="b03d6-149">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b03d6-149">ASP.NET Core MVC</span></span>
* <span data-ttu-id="b03d6-150">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="b03d6-150">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="b03d6-151">Hosta i aplikacji ma została odłączona zapewniające elastyczność przechodzenia do różnych platform w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="b03d6-151">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="b03d6-152">Aby uzyskać więcej informacji na temat odwołanie do platformy ASP.NET Core uruchamiania i oprogramowanie pośredniczące, zobacz [uruchamiania w ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="b03d6-152">For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="store-configurations"></a><span data-ttu-id="b03d6-153">Konfiguracje magazynu</span><span class="sxs-lookup"><span data-stu-id="b03d6-153">Store configurations</span></span>

<span data-ttu-id="b03d6-154">Program ASP.NET obsługuje ustawienia przechowywania.</span><span class="sxs-lookup"><span data-stu-id="b03d6-154">ASP.NET supports storing settings.</span></span> <span data-ttu-id="b03d6-155">Te ustawienia są używane, na przykład do obsługi środowiska, w której zostały wdrożone aplikacje.</span><span class="sxs-lookup"><span data-stu-id="b03d6-155">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="b03d6-156">Popularną praktyką było przechowywać wszystkie niestandardowe pary klucz wartość w `<appSettings>` sekcji *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="b03d6-156">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="b03d6-157">Aplikacje odczytywać te ustawienia przy użyciu `ConfigurationManager.AppSettings` kolekcji w `System.Configuration` przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="b03d6-157">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="b03d6-158">Platformy ASP.NET Core można przechowywać dane konfiguracyjne dla aplikacji w żadnym pliku i załadować je jako część uruchamianie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="b03d6-158">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="b03d6-159">Domyślny plik używany w szablonach projektu jest *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b03d6-159">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="b03d6-160">Ładowanie tego pliku do wystąpienia `IConfiguration` wewnątrz aplikacji zostało wykonane w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b03d6-160">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="b03d6-161">Odczytuje aplikacji z `Configuration` pobieranie ustawień:</span><span class="sxs-lookup"><span data-stu-id="b03d6-161">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="b03d6-162">Brak rozszerzenia do tej metody w celu uproszczenia procesu bardziej niezawodne, takiej jak [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane) można załadować usługi za pomocą tych wartości.</span><span class="sxs-lookup"><span data-stu-id="b03d6-162">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="b03d6-163">Podejście Podpisane zapewnia zbiór silnie typizowanych obiektów konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b03d6-163">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> <span data-ttu-id="b03d6-164">Więcej informacji na temat odwołanie do konfiguracji platformy ASP.NET Core, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b03d6-164">For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="b03d6-165">Iniekcji zależności natywnego</span><span class="sxs-lookup"><span data-stu-id="b03d6-165">Native dependency injection</span></span>

<span data-ttu-id="b03d6-166">Celem ważne podczas kompilowania dużych, skalowalnych aplikacji jest luźne powiązanie składników i usług.</span><span class="sxs-lookup"><span data-stu-id="b03d6-166">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="b03d6-167">[Iniekcji zależności](xref:fundamentals/dependency-injection) to technika popularnych na osiągnięcie tego celu, i jest natywny składnik ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b03d6-167">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="b03d6-168">W aplikacjach ASP.NET deweloperzy korzystają z biblioteki innych firm, aby zaimplementować iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="b03d6-168">In ASP.NET apps, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="b03d6-169">Jedna takie biblioteka jest [Unity](https://github.com/unitycontainer/unity), podany przez firmę Microsoft Patterns & rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="b03d6-169">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span>

<span data-ttu-id="b03d6-170">Przykład konfigurowania iniekcji zależności z Unity implementuje `IDependencyResolver` który opakowuje `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="b03d6-170">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="b03d6-171">Utwórz wystąpienie programu `UnityContainer`, Zarejestruj usługi i ustawienie mechanizm rozpoznawania zależności `HttpConfiguration` na nowe wystąpienie klasy `UnityResolver` z kontenera:</span><span class="sxs-lookup"><span data-stu-id="b03d6-171">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="b03d6-172">Wstaw `IProductRepository` w razie potrzeby:</span><span class="sxs-lookup"><span data-stu-id="b03d6-172">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="b03d6-173">Ponieważ iniekcji zależności jest częścią platformy ASP.NET Core, można dodać usługi w `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b03d6-173">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="b03d6-174">Repozytorium mogą zostać dodane wszędzie, podobnie jak z Unity.</span><span class="sxs-lookup"><span data-stu-id="b03d6-174">The repository can be injected anywhere, as was true with Unity.</span></span>

> [!NOTE]
> <span data-ttu-id="b03d6-175">Aby uzyskać szczegółowe informacje do iniekcji zależności w ASP.NET Core, zobacz [iniekcji zależności w ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="b03d6-175">For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="b03d6-176">Obsługiwać pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="b03d6-176">Serve static files</span></span>

<span data-ttu-id="b03d6-177">Ważnym elementem projektowanie witryn sieci web jest możliwość obsługi zasoby statyczne, po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b03d6-177">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="b03d6-178">Najbardziej typowe przykłady pliki statyczne są HTML, CSS, Javascript i obrazów.</span><span class="sxs-lookup"><span data-stu-id="b03d6-178">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="b03d6-179">Te pliki muszą być zapisane w lokalizacji opublikowanej aplikacji (lub CDN) i odwołuje się do, mogą być ładowane przez żądanie.</span><span class="sxs-lookup"><span data-stu-id="b03d6-179">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="b03d6-180">Ten proces został zmieniony w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b03d6-180">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="b03d6-181">W programie ASP.NET pliki statyczne są przechowywane w różnych katalogach i przywoływany w widokach.</span><span class="sxs-lookup"><span data-stu-id="b03d6-181">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="b03d6-182">W ASP.NET Core pliki statyczne są przechowywane w katalogu"web" (*&lt;zawartości głównego&gt;/wwwroot*), chyba że skonfigurowano inaczej.</span><span class="sxs-lookup"><span data-stu-id="b03d6-182">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="b03d6-183">Pliki są ładowane do potoku żądania, wywołując `UseStaticFiles` — metoda rozszerzenia z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b03d6-183">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> <span data-ttu-id="b03d6-184">Jeśli przeznaczonych dla platformy .NET Framework, zainstaluj pakiet NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="b03d6-184">If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="b03d6-185">Na przykład zasób obrazu w *wwwroot/obrazy* takie jak folder jest dostępny dla przeglądarki w lokalizacji `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="b03d6-185">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

> [!NOTE]
> <span data-ttu-id="b03d6-186">Aby uzyskać więcej informacji na temat odwołanie do dostarczania plików statycznych w ASP.NET Core, zobacz [pliki statyczne](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b03d6-186">For a more in-depth reference to serving static files in ASP.NET Core, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b03d6-187">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b03d6-187">Additional resources</span></span>

- [<span data-ttu-id="b03d6-188">Eksportowanie bibliotek .NET Core</span><span class="sxs-lookup"><span data-stu-id="b03d6-188">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
