---
title: Uruchamianie aplikacji w platformy ASP.NET Core
author: ardalis
description: "Odkryj, jak klasa początkowa w ASP.NET Core umożliwia skonfigurowanie usług i aplikacji żądania potoku."
keywords: "Platformy ASP.NET Core, uruchamianie, konfigurowanie metody, ConfigureServices — metoda"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 83b2647df8beec1feae33400224dacf9823be9b4
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="application-startup-in-aspnet-core"></a>Uruchamianie aplikacji w platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra/)

`Startup` Klasa umożliwia skonfigurowanie usług i aplikacji żądania potoku.

## <a name="the-startup-class"></a>Klasa startowa.

Aplikacje platformy ASP.NET Core wymagają `Startup` klasy, która ma nazwę `Startup` przez Konwencję. Należy określić nazwę klasy uruchamiania w `Main` programu [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metody. Zobacz [hostingu](xref:fundamentals/hosting) Aby dowiedzieć się więcej o `WebHostBuilder`, które są uruchamiane przed `Startup`.

Można zdefiniować oddzielne `Startup` klasy dla różnych środowisk i odpowiednie będzie można wybrać jedną w czasie wykonywania. Jeśli określisz `startupAssembly` w [konfiguracji WebHost](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) lub opcje hostingu spowoduje załadowanie tego zestawu uruchamiania i wyszukaj `Startup` lub `Startup[Environment]` typu. Klasy, których będą wysyłane bieżącego środowiska dopasowania sufiks nazw, więc jeśli aplikacja jest uruchamiana w *programowanie* środowisku i zawiera zarówno `Startup` i `StartupDevelopment` klasy, `StartupDevelopment` będzie klasy używane. Zobacz [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) w `StartupLoader` i [Praca w środowiskach wielu](environments.md#startup-conventions).

Alternatywnie można zdefiniować ustalonego `Startup` klasy, który będzie używany niezależnie od środowiska przez wywołanie metody `UseStartup<TStartup>`. Jest to zalecane podejście.

`Startup` Konstruktora klasy może zaakceptować zależności, które są realizowane za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection). Typowym podejściem jest użycie `IHostingEnvironment` do skonfigurowania [konfiguracji](xref:fundamentals/configuration/index) źródeł.

`Startup` Klasy musi zawierać `Configure` — metoda i opcjonalnie `ConfigureServices` metody, które są wywoływane podczas uruchamiania aplikacji. Klasy mogą również obejmować [określonego środowiska wersji tych metod](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, jeśli istnieje, jest wywoływana przed `Configure`.

Dowiedz się więcej o [Obsługa wyjątków podczas uruchamiania aplikacji](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) jest opcjonalne; ale jeśli używana, jest wywoływana przed `Configure` metody przez hosta sieci web. Host sieci web można skonfigurować niektóre usługi przed ``Startup`` metody są wywoływane (zobacz [hosting](xref:fundamentals/hosting)). Według konwencji [opcje konfiguracji](xref:fundamentals/configuration/index) są ustawione w tej metodzie.

Funkcje, które wymagają znacznej konfiguracji istnieją `Add[Service]` metody rozszerzenia na [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). W tym przykładzie z domyślnego szablonu witryny sieci web konfiguruje aplikację do używania usługi programu Entity Framework, tożsamości i MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Dodawanie usługi do kontenera usług udostępnia je w aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>Dostępne w uruchamiania usługi

Iniekcji zależności platformy ASP.NET Core udostępnia usługi podczas uruchamiania aplikacji. Mogą żądać tych usług w tym odpowiedni interfejs jako parametr w Twojej `Startup` konstruktora klasy lub jego `Configure` metody. `ConfigureServices` Metoda przyjmuje tylko `IServiceCollection` parametrów (ale żadnych może zarejestrowanej usługi można pobrać z tej kolekcji, więc dodatkowe parametry nie są konieczne).

Poniżej przedstawiono niektóre z tych usług, które zwykle wymagane przez `Startup` metod:

* W Konstruktorze: `IHostingEnvironment`,`ILogger<Startup>`
* W `ConfigureServices`:`IServiceCollection`
* W `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Wszystkie usługi dodane przez ``WebHostBuilder`` ``ConfigureServices`` zażąda — metoda ``Startup`` konstruktora klasy lub jego ``Configure`` metody. Użyj `WebHostBuilder` zapewnienie dowolnej usługi użytkownik musi podczas `Startup` metody.

## <a name="the-configure-method"></a>Konfiguruj — metoda

`Configure` Metoda jest używana do określania, jak aplikacja ASP.NET będzie odpowiadał na żądania HTTP. Potok żądań jest skonfigurowana przez dodanie [oprogramowanie pośredniczące](middleware.md) składników `IApplicationBuilder` wystąpienie, które są udostępniane przez iniekcji zależności.

W poniższym przykładzie z domyślnego szablonu witryny sieci web, kilka metod rozszerzenia są używane do konfigurowania potoku z obsługą [BrowserLink](http://vswebessentials.com/features/browserlink), stron błędów, pliki statyczne, ASP.NET MVC i tożsamości.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Każdy `Use` — metoda rozszerzenia dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware) składnik do potoku żądania. Na przykład `UseMvc` — metoda rozszerzenia dodaje [routingu](routing.md) oprogramowania pośredniczącego do potoku żądania i konfiguruje [MVC](xref:mvc/overview) jako domyślny program obsługi.

Aby uzyskać więcej informacji o sposobie używania `IApplicationBuilder`, zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware).

Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory` można także określić w podpisie metody w tym przypadku te usługi będą [wprowadzonym](dependency-injection.md) jeśli są dostępne. 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca w środowiskach wielu](xref:fundamentals/environments)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware)
* [Rejestrowanie](xref:fundamentals/logging/index)
* [Konfiguracja](xref:fundamentals/configuration/index)
