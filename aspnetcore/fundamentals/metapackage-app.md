---
title: Metapackage Microsoft.AspNetCore.App dla platformy ASP.NET Core 2.1 i nowsze
author: Rick-Anderson
description: Microsoft.AspNetCore.App metapackage obejmuje wszystkie obsługiwane pakiety Entity Framework Core i ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: 7c7f69a6176d3f7982a67106cb823ff42200b50e
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Metapackage Microsoft.AspNetCore.App dla platformy ASP.NET Core 2.1

Ta funkcja wymaga platformy ASP.NET Core 2.1 i nowsze, przeznaczonych dla platformy .NET Core 2.1 i nowszymi.

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) dla platformy ASP.NET Core:

* Nie ma zależności innych firm, z wyjątkiem [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), i [Async Popraw](https://www.nuget.org/packages/System.Interactive.Async/). Te zależności 3rd firm uważa za zapewnienie struktur głównych funkcji.
* Zawiera wszystkie pakiety obsługiwanych przez zespół platformy ASP.NET Core, z wyjątkiem tych, które zawierają zależności innych firm (inne niż wymienione wcześniej).
* Zawiera wszystkie pakiety obsługiwanych przez zespół Entity Framework Core, z wyjątkiem tych, które zawierają zależności innych firm (inne niż wymienione wcześniej).

Obejmuje wszystkie funkcje platformy ASP.NET Core 2.1 i nowsze i Entity Framework Core 2.1 i nowsze `Microsoft.AspNetCore.App` pakietu. Domyślne szablony przeznaczonych dla platformy ASP.NET Core 2.1 projektu i później użyć tego pakietu. Firma Microsoft zaleca aplikacji przeznaczonych dla platformy ASP.NET Core 2.1 i nowsze oraz Entity Framework Core 2.1 i później użyć `Microsoft.AspNetCore.App` pakietu.

Numer wersji `Microsoft.AspNetCore.App` metapackage reprezentuje wersji platformy ASP.NET Core i wersji programu Entity Framework Core.

Przy użyciu `Microsoft.AspNetCore.App` metapackage zapewnia ograniczenia wersji, służących do ochrony aplikacji:

* Jeśli pakiet jest uwzględniony w pakiecie w mający przechodnie zależności (bezpośrednich nie) `Microsoft.AspNetCore.App`i tych numerów wersji są różne, NuGet spowoduje wystąpienie błędu.
* Inne pakiety dodany do aplikacji nie można zmienić wersji pakietów zawarte w `Microsoft.AspNetCore.App`.
* Spójność wersji zapewnia niezawodne środowisko. `Microsoft.AspNetCore.App` została zaprojektowana w celu zapobiegania kombinacji wersji zastosowaniem bitów powiązane ze sobą używany w tej samej aplikacji.

Aplikacje używające `Microsoft.AspNetCore.App` metapackage automatycznie korzystać udostępnionego platformy ASP.NET Core. Jeśli używasz `Microsoft.AspNetCore.App` metapackage, **nie** zasobów, z którym związane są odwołania pakietów platformy ASP.NET Core NuGet zostały wdrożone za pomocą aplikacji &mdash; framework udostępnionego platformy ASP.NET Core zawiera te zasoby. Zasoby w ramach udostępnionego są wstępnie skompilowana zwiększające czasu uruchomienia aplikacji. Aby uzyskać więcej informacji, zobacz "udostępnionego framework" w [pakowania dystrybucji .NET Core](/dotnet/core/build/distribution-packaging).

Następujące *.csproj* odwołania do pliku projektu `Microsoft.AspNetCore.App` metapackage dla platformy ASP.NET Core:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

Poprzedni kod znaczników reprezentuje typowy platformy ASP.NET Core 2.1 i nowsze szablonu. Nie określono numeru wersji `Microsoft.AspNetCore.App` odwołanie do pakietu. Jeśli nie określono wersji, [niejawne](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) wersja jest określona przez zestaw SDK, czyli `Microsoft.NET.Sdk.Web`. Firma Microsoft zaleca polegania na niejawne wersja określona przez zestaw SDK i nie jawne ustawienie numer wersji na odwołanie do pakietu. Możesz pozostawić komentarz GitHub w [dyskusji dla wersji niejawne Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).

Niejawne wersji ma ustawioną wartość `major.minor.0` dla aplikacji przenośnej. Mechanizm przewijaniem do udostępnionego framework uruchomi aplikacji z najnowszą wersją zgodne między zainstalowanych platform udostępnionego. Aby zagwarantować, że ta sama wersja jest używana w rozwoju, testów i produkcji, upewnij się, że ta sama wersja programu udostępnionego framework jest zainstalowana we wszystkich środowiskach. Aplikacje samodzielną numer wersji niejawne ustawiono `major.minor.patch` powiązane zainstalowanego zestawu SDK platformy udostępnionego.

Określenie numeru wersji w `Microsoft.AspNetCore.App` odwołania jest **nie** zagwarantować tej wersji udostępnionego framework zostanie wybrany. Na przykład załóżmy, że określono wersji "2.1.1", ale "2.1.3" jest zainstalowany. W takim przypadku aplikacja będzie używać "2.1.3". Chociaż nie jest to zalecane, można wyłączyć przenoszenia do przodu (poprawki i/lub pomocniczej). Aby uzyskać więcej informacji dotyczących hosta dotnet przewijaniem do i konfigurowanie zachowania, zobacz [dotnet hosta przenoszenia do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

`Microsoft.AspNetCore.App` [Metapackage](/dotnet/core/packages#metapackages) nie jest tradycyjnych pakietów, które są aktualizowane z pakietu NuGet. Podobnie jak `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` reprezentuje udostępnionego środowiska uruchomieniowego, którego ma semantykę specjalne versioning obsługiwane poza NuGet. Aby uzyskać więcej informacji, zobacz [pakietów, metapackages i platform](/dotnet/core/packages).

Jeśli wcześniej używana aplikacja `Microsoft.AspNetCore.All`, zobacz [migrowania Microsoft.AspNetCore.All do Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
