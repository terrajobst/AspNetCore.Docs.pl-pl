---
title: Microsoft. AspNetCore. All, pakiet dla ASP.NET Core 2,0
author: Rick-Anderson
description: Nie zaleca się stosowania pakietu Microsoft. AspNetCore. All w przypadku ASP.NET Core 2,1 i nowszych.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: cc00c075909da5c17a4aa2fd252c9e662e5a0fc9
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511070"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Microsoft. AspNetCore. All, pakiet dla ASP.NET Core 2,0

::: moniker range=">= aspnetcore-3.0"

Pakiet `Microsoft.AspNetCore.All` nie jest zawarty w ASP.NET Core 3,0 i nowszych. Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/Announcements/issues/314)w serwisie GitHub.

::: moniker-end

> [!NOTE]
> Zalecamy stosowanie aplikacji przeznaczonych dla ASP.NET Core 2,1 i nowszych przy użyciu [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a nie tego pakietu. Zobacz [Migrowanie z Microsoft. AspNetCore. All do Microsoft. AspNetCore. app](#migrate) w tym artykule.

Ta funkcja wymaga ASP.NET Core 2. x przeznaczonych dla platformy .NET Core 2. x.

[Microsoft. AspNetCore. All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) to pakiet, który odwołuje się do udostępnionej struktury. *Struktura udostępniona* to zbiór zestawów (plików*dll* ), które nie znajdują się w folderach aplikacji. Aby można było uruchomić aplikację, na komputerze musi być zainstalowana struktura udostępniona. Aby uzyskać więcej informacji, zobacz [udostępnioną strukturę](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

Platforma udostępniona, do której od`Microsoft.AspNetCore.All` się to obejmuje:

* Wszystkie obsługiwane pakiety przez zespół ASP.NET Core.
* Wszystkie obsługiwane pakiety przez Entity Framework Core.
* Zależności wewnętrzne i inne firmy używane przez ASP.NET Core i Entity Framework Core.

Wszystkie funkcje ASP.NET Core 2. x i Entity Framework Core 2. x są zawarte w pakiecie `Microsoft.AspNetCore.All`. Domyślne szablony projektu dla ASP.NET Core 2,0 używają tego pakietu.

Numer wersji pakietu `Microsoft.AspNetCore.All`ow reprezentuje minimalną wersję ASP.NET Core i wersję Entity Framework Core.

Następujący plik *. csproj* odwołuje się do `Microsoft.AspNetCore.All` pakietu dla ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Niejawna wersja

W ASP.NET Core 2,1 lub nowszej można określić odwołanie do pakietu `Microsoft.AspNetCore.All` bez wersji. Gdy wersja nie jest określona, zestaw SDK (`Microsoft.NET.Sdk.Web`) określa niejawną wersję. Zalecamy użycie niejawnej wersji określonej przez zestaw SDK i niejawne ustawienie numeru wersji w odwołaniu do pakietu. Jeśli masz pytania dotyczące tego podejścia, pozostaw komentarz w serwisie GitHub w [dyskusji dotyczącej wersji niejawnej Microsoft. AspNetCore. app](https://github.com/dotnet/AspNetCore.Docs/issues/6430).

Niejawna wersja jest ustawiona na `major.minor.0` dla aplikacji przenośnych. Mechanizm przekazujący przechodzenie do platformy udostępnionej uruchamia aplikację w najnowszej zgodnej wersji wśród zainstalowanych platform udostępnionych. Aby zagwarantować, że ta sama wersja jest używana w środowisku deweloperskim, testowym i produkcyjnym, upewnij się, że ta sama wersja udostępnionej platformy jest zainstalowana we wszystkich środowiskach. W przypadku aplikacji samodzielnych niejawny numer wersji jest ustawiany na `major.minor.patch` udostępnionej struktury powiązanej z zainstalowanym zestawem SDK.

Określenie numeru wersji w odwołaniu do pakietu `Microsoft.AspNetCore.All` nie **gwarantuje,** że jest wybrana wersja udostępnionej platformy. Załóżmy na przykład, że jest określona wersja "2.1.1", ale jest zainstalowana wartość "2.1.3". W takim przypadku aplikacja będzie używać "2.1.3". Chociaż nie jest to zalecane, można wyłączyć funkcję wycofywania do przodu (poprawka i/lub pomocnicza). Aby uzyskać więcej informacji na temat przetworzenia i konfigurowania zachowań hosta dotnet, zobacz [przewinięcie hosta dotnet do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Zestaw SDK projektu musi być ustawiony na `Microsoft.NET.Sdk.Web` w pliku projektu, aby można było użyć niejawnej wersji `Microsoft.AspNetCore.All`. Gdy zostanie określony zestaw `Microsoft.NET.Sdk` SDK (`<Project Sdk="Microsoft.NET.Sdk">` w górnej części pliku projektu), generowane jest następujące ostrzeżenie:

*Ostrzeżenie NU1604: zależność projektu Microsoft. AspNetCore. All nie zawiera mniejszej granicy. Uwzględnij dolną granicę w wersji zależności, aby zapewnić spójne wyniki przywracania.*

Jest to znany problem z zestawem SDK platformy .NET Core 2,1 i zostanie naprawiony w zestawie SDK platformy .NET Core 2,2.

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migrowanie z Microsoft. AspNetCore. All do Microsoft. AspNetCore. App

Następujące pakiety są zawarte w `Microsoft.AspNetCore.All`, ale nie w pakiecie `Microsoft.AspNetCore.App`.

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

Aby przejść z `Microsoft.AspNetCore.All` do `Microsoft.AspNetCore.App`, jeśli aplikacja używa dowolnych interfejsów API z powyższych pakietów lub pakietów wprowadzonych przez te pakiety, Dodaj odwołania do tych pakietów w projekcie.

Wszystkie zależności poprzedzających pakietów, które w przeciwnym razie nie są zależnościami `Microsoft.AspNetCore.App` nie są uwzględnione niejawnie. Na przykład:

* `StackExchange.Redis` jako zależność `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` jako zależność `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`

## <a name="update-aspnet-core-21"></a>Aktualizacja ASP.NET Core 2,1

Zalecamy przeprowadzenie migracji do `Microsoft.AspNetCore.App` pakietu operacyjnego w wersji 2,1 lub nowszej. Aby nadal korzystać z pakietu `Microsoft.AspNetCore.All` i upewnić się, że jest wdrożona Najnowsza wersja poprawki:

* Na komputerach deweloperskich i serwerach kompilacji: Zainstaluj najnowsze [zestaw .NET Core SDK](https://dotnet.microsoft.com/download).
* Na serwerach wdrożenia: Zainstaluj najnowsze [środowisko uruchomieniowe programu .NET Core](https://dotnet.microsoft.com/download).
 Aplikacja zostanie przeniesiona do najnowszej zainstalowanej wersji przy ponownym uruchomieniu aplikacji.
