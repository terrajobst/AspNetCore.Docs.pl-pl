---
title: Pakiet Microsoft. AspNetCore. App dla ASP.NET Core
author: Rick-Anderson
description: Współdzielona platforma Microsoft. AspNetCore. App
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/24/2019
uid: fundamentals/metapackage-app
ms.openlocfilehash: 3ce74bc7329a88ffc6f77baf6b8a311c02951318
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663143"
---
# <a name="microsoftaspnetcoreapp-for-aspnet-core"></a>Microsoft. AspNetCore. App dla ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

 ASP.NET Core Shared Framework (`Microsoft.AspNetCore.App`) zawiera zestawy, które są opracowywane i obsługiwane przez firmę Microsoft. `Microsoft.AspNetCore.App` jest instalowany podczas instalowania [zestawu SDK platformy .NET Core 3,0 lub nowszego](https://dotnet.microsoft.com/download/dotnet-core/3.0) . *Platforma udostępniona* to zestaw zestawów (plików*dll* ), które są zainstalowane na komputerze i zawiera składnik środowiska uruchomieniowego oraz pakiet docelowy. Aby uzyskać więcej informacji, zobacz [udostępnioną strukturę](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

* Projekty przeznaczone dla zestawu `Microsoft.NET.Sdk.Web` SDK niejawnie odwołują się do struktury `Microsoft.AspNetCore.App`.

Dla tych projektów nie są wymagane żadne dodatkowe odwołania:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>
    ...
</Project>
```

ASP.NET Core współdzielona struktura:

* Nie obejmuje zależności innych firm.
* Zawiera wszystkie obsługiwane pakiety przez zespół ASP.NET Core.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ta funkcja wymaga ASP.NET Core 2. x przeznaczonych dla platformy .NET Core 2. x.

Pakiet [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [](/dotnet/core/packages#metapackages) dla ASP.NET Core:

* Nie obejmuje zależności innych firm, z wyjątkiem [JSON.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [remotion. LINQ](https://www.nuget.org/packages/Remotion.Linq/)i [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Te zależności innych firm są uważane za niezbędne do zapewnienia funkcji głównych funkcji.
* Obejmuje wszystkie obsługiwane pakiety przez zespół ASP.NET Core, z wyjątkiem tych, które zawierają zależności innych firm (inne niż wspomniane wcześniej).
* Obejmuje wszystkie obsługiwane pakiety przez zespół Entity Framework Core, z wyjątkiem tych, które zawierają zależności innych firm (inne niż wspomniane wcześniej).

Wszystkie funkcje ASP.NET Core 2. x i Entity Framework Core 2. x są zawarte w pakiecie `Microsoft.AspNetCore.App`. Domyślne szablony projektu dla ASP.NET Core 2. x używają tego pakietu. Zalecamy stosowanie aplikacji przeznaczonych dla ASP.NET Core 2. x i Entity Framework Core 2. x przy użyciu pakietu `Microsoft.AspNetCore.App`.

Numer wersji pakietu `Microsoft.AspNetCore.App`ow reprezentuje minimalną wersję ASP.NET Core i wersję Entity Framework Core.

Korzystanie z pakietu `Microsoft.AspNetCore.App`, zapewnia ograniczenia wersji chroniące Twoją aplikację:

* W przypadku dołączenia pakietu, który ma zależność przechodnią (nie bezpośrednią) w pakiecie w `Microsoft.AspNetCore.App`i te numery wersji różnią się, pakiet NuGet wygeneruje błąd.
* Inne pakiety dodane do aplikacji nie mogą zmienić wersji pakietów uwzględnionych w `Microsoft.AspNetCore.App`.
* Spójność wersji zapewnia niezawodne środowisko pracy. `Microsoft.AspNetCore.App` zaprojektowano tak, aby uniemożliwić nietestowane kombinacje powiązanych bitów w tej samej aplikacji.

Aplikacje, które używają pakietu `Microsoft.AspNetCore.App`, automatycznie wykorzystują ASP.NET Core współdzielonej platformy. W przypadku korzystania z pakietu `Microsoft.AspNetCore.App`, żadne zasoby z przywoływanych ASP.NET Core pakietów NuGet **nie** są wdrażane wraz z aplikacją,&mdash;ASP.NET Core współdzielona architektura zawiera te zasoby. Zasoby w udostępnionej architekturze są wstępnie skompilowane w celu poprawienia czasu uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz [udostępnioną strukturę](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

Następujący plik projektu odwołuje się do `Microsoft.AspNetCore.App` pakietu dla ASP.NET Core i reprezentuje typowy szablon ASP.NET Core 2,2:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

Poprzedzający znacznik reprezentuje typowy szablon ASP.NET Core 2. x. Nie określa numeru wersji dla odwołania do pakietu `Microsoft.AspNetCore.App`. W przypadku nieokreślenia wersji [niejawna](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) wersja jest określana przez zestaw SDK, czyli `Microsoft.NET.Sdk.Web`. Zalecamy użycie niejawnej wersji określonej przez zestaw SDK i niejawne ustawienie numeru wersji w odwołaniu do pakietu. Jeśli masz pytania dotyczące tego podejścia, pozostaw komentarz w serwisie GitHub w [dyskusji dotyczącej wersji niejawnej Microsoft. AspNetCore. app](https://github.com/dotnet/AspNetCore.Docs/issues/6430).

Niejawna wersja jest ustawiona na `major.minor.0` dla aplikacji przenośnych. Mechanizm przekazujący przechodzenie do platformy udostępnionej będzie uruchamiał aplikację w najnowszej zgodnej wersji wśród zainstalowanych platform udostępnionych. Aby zagwarantować, że ta sama wersja jest używana w środowisku deweloperskim, testowym i produkcyjnym, upewnij się, że ta sama wersja udostępnionej platformy jest zainstalowana we wszystkich środowiskach. W przypadku aplikacji z własnym niejawnym numerem wersji jest ustawiany `major.minor.patch` udostępnionej struktury powiązanej z zainstalowanym zestawem SDK.

Określenie numeru wersji w odwołaniu `Microsoft.AspNetCore.App` **nie gwarantuje,** że zostanie wybrana wersja udostępnionej platformy. Na przykład załóżmy, że jest określona wersja "2.2.1", ale zainstalowano "2.2.3". W takim przypadku aplikacja będzie używać "2.2.3". Chociaż nie jest to zalecane, można wyłączyć funkcję wycofywania do przodu (poprawka i/lub pomocnicza). Aby uzyskać więcej informacji na temat przetworzenia i konfigurowania zachowań hosta dotnet, zobacz [przewinięcie hosta dotnet do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`<Project Sdk` musi być ustawiona na `Microsoft.NET.Sdk.Web`, aby można było użyć niejawnej wersji `Microsoft.AspNetCore.App`. Gdy jest używana `<Project Sdk="Microsoft.NET.Sdk">` (bez końcowego `.Web`):

* Zostanie wygenerowane następujące ostrzeżenie:

  *Ostrzeżenie NU1604: zależność projektu Microsoft. AspNetCore. App nie zawiera mniejszej granicy. Uwzględnij dolną granicę w wersji zależności, aby zapewnić spójne wyniki przywracania.*

* Jest to znany problem z zestawem SDK platformy .NET Core 2,1.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<a name="update"></a>

## <a name="update-aspnet-core"></a>ASP.NET Core aktualizacji

[Pakiet](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` nie jest tradycyjnym pakietem, który został zaktualizowany przy użyciu narzędzia NuGet. Podobnie jak w przypadku `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` reprezentuje udostępnione środowisko uruchomieniowe, które ma specjalną semantykę wersji obsłużoną poza pakietem NuGet. Aby uzyskać więcej informacji, zobacz [pakiety, aplikacje i struktury](/dotnet/core/packages).

Aby zaktualizować ASP.NET Core:

* Na komputerach deweloperskich i serwerach kompilacji: Pobierz i zainstaluj [zestaw .NET Core SDK](https://www.microsoft.com/net/download).
* Na serwerach wdrażania: Pobierz i zainstaluj [środowisko uruchomieniowe programu .NET Core](https://www.microsoft.com/net/download).

 Aplikacje zostaną przesunięte do najnowszej zainstalowanej wersji przy ponownym uruchomieniu aplikacji. Nie trzeba aktualizować numeru wersji `Microsoft.AspNetCore.App` w pliku projektu. Aby uzyskać więcej informacji, zobacz [aplikacje zależne od platformy — przewinięcie do przodu](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Jeśli aplikacja była wcześniej używana `Microsoft.AspNetCore.All`, zobacz [Migrowanie z Microsoft. AspNetCore. All do Microsoft. AspNetCore. app](xref:fundamentals/metapackage#migrate).

::: moniker-end
