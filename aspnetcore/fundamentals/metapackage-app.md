---
title: Microsoft.AspNetCore.App meta Microsoft.aspnetcore.all, dla platformy ASP.NET Core 2.1 lub nowszych
author: Rick-Anderson
description: Meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App obejmuje wszystkie obsługiwane pakiety platformy ASP.NET Core i Entity Framework Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 37e911052d9d33daebf0f2424868f9589f9a04b0
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373335"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App, dla platformy ASP.NET Core 2.1

Ta funkcja wymaga platformy ASP.NET Core 2.1 i nowsze, przeznaczonych dla platformy .NET Core 2.1 lub nowszy.

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [meta Microsoft.aspnetcore.all](/dotnet/core/packages#metapackages) dla platformy ASP.NET Core:

* Nie ma zależności innych firm, z wyjątkiem [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), i [IX Async](https://www.nuget.org/packages/System.Interactive.Async/). Te zależności innych firm 3 będą konieczne zapewnienie struktury główne funkcje działają.
* Zawiera wszystkie pakiety obsługiwanych przez zespół programu ASP.NET Core, z wyjątkiem tych, które zawierają zależności innych firm (innych niż wymienione wcześniej).
* Zawiera wszystkie pakiety obsługiwanych przez zespół programu Entity Framework Core, z wyjątkiem tych, które zawierają zależności innych firm (innych niż wymienione wcześniej).

Wszystkie funkcje platformy ASP.NET Core 2.1 lub nowszy i Entity Framework Core 2.1 i nowsze, są uwzględnione w `Microsoft.AspNetCore.App` pakietu. Domyślne szablony przeznaczone dla platformy ASP.NET Core 2.1 projektu, a później użyć tego pakietu. Firma Microsoft zaleca, aby aplikacje przeznaczone na platformy ASP.NET Core 2.1 lub nowszy i Entity Framework Core 2.1 i później użyć `Microsoft.AspNetCore.App` pakietu.

Numer wersji `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all reprezentuje wersję platformy ASP.NET Core i Entity Framework Core wersji.

Za pomocą `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all zapewnia ograniczenia wersji, które chronią aplikację:

* Jeśli jest dołączony pakiet, który ma przechodnie zależności (nie bezpośrednie) w pakiecie w `Microsoft.AspNetCore.App`i różnią się w tych numerów wersji, NuGet zostanie wygenerowany błąd.
* Inne pakiety dodane do Twojej aplikacji nie można zmienić wersję pakietów objęte `Microsoft.AspNetCore.App`.
* Spójność wersji zapewnia niezawodne środowisko. `Microsoft.AspNetCore.App` zaprojektowano tak, aby zapobiec kombinacji wersji nieprzetestowanych powiązane bitów, które są używane razem w ramach tej samej aplikacji.

Aplikacje, które używają `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all automatycznie skorzystaj z udostępnionej platformy ASP.NET Core. Kiedy używasz `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, **nie** zasoby z pakietów platformy ASP.NET Core NuGet, do którego istnieje odwołanie, są wdrażane przy użyciu aplikacji&mdash;udostępnionej platformy ASP.NET Core zawiera te zasoby. Zasoby w ramach udostępnionego są wstępnie skompilowane, aby poprawić czas uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz "udostępnione framework" w [tworzenie pakietów dystrybucji platformy .NET Core](/dotnet/core/build/distribution-packaging).

Następujące projektu odwołania do pliku `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all dla platformy ASP.NET Core i przedstawia typowy platformy ASP.NET Core 2.1 szablonu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.3" />
  </ItemGroup>

</Project>
```

Numer wersji na `Microsoft.AspNetCore.App` odwołania jest **nie** gwarantuje daną wersję udostępnionego framework, który będzie używany. Na przykład, załóżmy, że wersja `2.1.1` jest określony, ale `2.1.3` jest zainstalowany. W takim przypadku aplikacja używa `2.1.3`. Chociaż nie jest to zalecane, możesz wyłączyć zachowanie przodu (poprawki i/lub pomocnicze). Aby uzyskać więcej informacji na temat zachowania przodu wersji pakietu, zobacz [dotnet hosta przenoszenia do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

## <a name="update-aspnet-core"></a>Aktualizacja platformy ASP.NET Core

`Microsoft.AspNetCore.App` [Meta Microsoft.aspnetcore.all](/dotnet/core/packages#metapackages) nie są tradycyjny pakiet, który jest aktualizowany z pakietów NuGet. Podobnie jak `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` reprezentuje udostępnionego środowiska uruchomieniowego, który ma semantykę specjalne versioning obsługiwane poza NuGet. Aby uzyskać więcej informacji, zobacz [pakiety, metapakiety i struktury](/dotnet/core/packages).

Aby zaktualizować platformy ASP.NET Core:

* Na komputerach deweloperskich i serwery kompilacji: Pobierz i zainstaluj [zestawu .NET Core SDK](https://www.microsoft.com/net/download).
* Na serwerach wdrożenia: Pobierz i zainstaluj [środowisko uruchomieniowe programu .NET Core](https://www.microsoft.com/net/download).

 Aplikacje będą uaktualniane do najnowszej zainstalowanej wersji na ponowne uruchomienie aplikacji. Nie jest konieczne zaktualizować `Microsoft.AspNetCore.App` numer wersji w pliku projektu. Aby uzyskać więcej informacji, zobacz [zależny od struktury aplikacji uaktualniane](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Jeśli wcześniej używano aplikacji `Microsoft.AspNetCore.All`, zobacz [migrowany pakiet do Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
