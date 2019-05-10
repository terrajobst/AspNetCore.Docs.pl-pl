---
title: Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0
author: Rick-Anderson
description: Pakiet meta Microsoft.aspnetcore.all nie jest zalecane dla platformy ASP.NET Core 2.1 lub nowszych.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 5d49213e6d694f121d8301c94ba71782b2dc45cf
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086937"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0

> [!NOTE]
> Firma Microsoft zaleca, aby aplikacje przeznaczone na platformy ASP.NET Core 2.1 i później użyć [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) zamiast tego pakietu. Zobacz [migrowany pakiet do Microsoft.AspNetCore.App](#migrate) w tym artykule.

Ta funkcja wymaga platformy ASP.NET Core 2.x określania wartości docelowej .NET Core 2.x.

[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) jest meta Microsoft.aspnetcore.all, która odwołuje się do udostępnionej platformy. A *udostępnionej platformy* to zbiór zestawów (*.dll* pliki) nie znajdują się w folderach aplikacji. Udostępnionej platformy musi być zainstalowany na komputerze, aby uruchomić aplikację. Aby uzyskać więcej informacji, zobacz [udostępnionej platformy](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

Udostępnionej platformy, `Microsoft.AspNetCore.All` odwołuje się do obejmuje:

* Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.
* Wszystkie obsługiwane pakiety przez Entity Framework Core.
* Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.

Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu. Domyślne szablony projektów przeznaczonych dla platformy ASP.NET Core 2.0, użyj tego pakietu.

Numer wersji `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all reprezentuje minimalnej wersji platformy ASP.NET Core i Entity Framework Core wersji.

Następujące *.csproj* pliku odwołania `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all dla platformy ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Niejawne przechowywania wersji

W programie ASP.NET Core 2.1 lub nowszej, możesz określić `Microsoft.AspNetCore.All` odwołania, które bez wersji pakietu. Jeśli wersja nie jest określona, niejawne wersja jest określona przez zestaw SDK (`Microsoft.NET.Sdk.Web`). Firma Microsoft zaleca, opierając się na niejawne wersji określony przez zestaw SDK i nie zostały jawnie ustawienie numeru wersji na odwołanie do pakietu. Jeśli masz pytania na temat tego podejścia, pozostaw komentarz GitHub na [dyskusję odnośnie do wersji niejawne Microsoft.AspNetCore.App](https://github.com/aspnet/AspNetCore.Docs/issues/6430).

Jest ustawiona wersja niejawne `major.minor.0` przenośne aplikacji. Mechanizm przodu udostępnionej platformy uruchamia aplikację na najnowszej zgodnej wersji spośród zainstalowanych platform udostępnionych. Aby zagwarantować, że ta sama wersja jest używana podczas tworzenia, testowania i produkcji, upewnij się, że tę samą wersję udostępnionego framework jest zainstalowana we wszystkich środowiskach. Dla aplikacji niezależna numer wersji niejawne został ustawiony na `major.minor.patch` udostępnionego Framework powiązane zainstalowanego zestawu SDK.

Określenie numeru wersji na `Microsoft.AspNetCore.All` jest odwołanie do pakietu **nie** gwarantuje daną wersję udostępnionego framework jest wybierany. Na przykład załóżmy, że wersja "2.1.1" jest określona, ale zainstalowano "2.1.3". W takim przypadku aplikacja będzie używać "2.1.3". Chociaż nie jest to zalecane, możesz wyłączyć przenoszenia do przodu (poprawki i/lub pomocnicze). Aby uzyskać więcej informacji na temat hosta dotnet przodu i konfigurowania jej zachowanie zobacz [dotnet hosta przenoszenia do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Zestaw SDK projektu musi być równa `Microsoft.NET.Sdk.Web` w pliku projektu użyć niejawnego wersji `Microsoft.AspNetCore.All`. Gdy `Microsoft.NET.Sdk` zestawu SDK jest określony (`<Project Sdk="Microsoft.NET.Sdk">` u góry pliku projektu), jest generowane ostrzeżenie następujące:

*Ostrzeżenie NU1604: Zależności projektu pakiet nie zawiera włącznie dolną granicę. Uwzględnij dolną granicę w wersji zależności, aby zapewnić przywracania na poziomie wyniki.*

Jest to znany problem z zestawu SDK programu .NET Core 2.1 i zostanie rozwiązany w .NET Core 2.2 SDK.

::: moniker-end

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

## <a name="update-aspnet-core-21"></a>Aktualizacja platformy ASP.NET Core 2.1

Zalecamy przeprowadzić migrację do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all 2.1 i nowszych. Aby nadal korzystać z `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all i upewnij się, jest wdrażana w najnowszej wersji poprawki:

* Na komputerach deweloperskich i serwerach kompilacji: Zainstaluj najnowszą wersję [zestawu .NET Core SDK](https://www.microsoft.com/net/download).
* Na serwerach wdrożenia: Zainstaluj najnowszą wersję [środowisko uruchomieniowe programu .NET Core](https://www.microsoft.com/net/download).
 Twojej aplikacji będą uaktualniane do najnowszej zainstalowanej wersji na ponowne uruchomienie aplikacji.
