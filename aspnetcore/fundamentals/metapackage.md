---
title: Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0
author: Rick-Anderson
description: Pakiet meta Microsoft.aspnetcore.all obejmuje wszystkie obsługiwane pakiety platformy ASP.NET Core i Entity Framework Core, wraz z ich zależnościami.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123830"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0

> [!NOTE]
> Firma Microsoft zaleca, aby aplikacje przeznaczone na platformy ASP.NET Core 2.1 i później użyć [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) zamiast tego pakietu. Zobacz [migrowany pakiet do Microsoft.AspNetCore.App](#migrate) w tym artykule.

Ta funkcja wymaga platformy ASP.NET Core 2.x określania wartości docelowej .NET Core 2.x.

[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta Microsoft.aspnetcore.all dla platformy ASP.NET Core obejmuje:

* Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.
* Wszystkie obsługiwane pakiety przez Entity Framework Core.
* Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.

Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu. Domyślne szablony projektów przeznaczonych dla platformy ASP.NET Core 2.0, użyj tego pakietu.

Numer wersji `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all reprezentuje wersję platformy ASP.NET Core i Entity Framework Core wersji.

Aplikacje, które używają `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all automatycznie korzystać z zalet [Store środowiska uruchomieniowego programu .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Store środowiska uruchomieniowego zawiera wszystkie zasoby środowiska uruchomieniowego potrzebnych do uruchomienia programu ASP.NET Core 2.x aplikacji. Kiedy używasz `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all, **nie** zasoby z pakietów platformy ASP.NET Core NuGet, do którego istnieje odwołanie, są wdrażane przy użyciu aplikacji &mdash; Store środowiska uruchomieniowego programu .NET Core zawiera te zasoby. Zasoby w Store środowiska uruchomieniowego są wstępnie skompilowane, aby poprawić czas uruchamiania aplikacji.

Aby usunąć pakiety, które nie są używane, można użyć procesu przycinania pakietu. Przycięty pakiety są wyłączone w danych wyjściowych w opublikowanej aplikacji.

Następujące *.csproj* pliku odwołania `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all dla platformy ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migrowanie z pakiet do Microsoft.AspNetCore.App

Następujące pakiety są objęte `Microsoft.AspNetCore.All` , ale nie `Microsoft.AspNetCore.App` pakietu. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Aby przenieść z `Microsoft.AspNetCore.All` do `Microsoft.AspNetCore.App`, jeśli aplikacja korzysta z żadnych interfejsów API z powyższych pakietów lub pakietów plików odwoływanych przez te pakiety, należy dodać odwołania do tych pakietów w projekcie.

Wszelkie zależności poprzedniego pakiety, które w przeciwnym razie nie ma zależności `Microsoft.AspNetCore.App` nie są domyślnie włączone. Na przykład:

* `StackExchange.Redis` jako zależą od elementu `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` jako zależą od elementu `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
