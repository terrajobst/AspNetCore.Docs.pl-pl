---
title: Migrowanie ASP.NET ASP.NET Core 2.0
author: isaac2004
description: "To odwołanie dokument zawiera wskazówki dotyczące migrowania istniejących aplikacji ASP.NET MVC lub Web API platformy ASP.NET Core 2.0."
keywords: ASP.NET Core,MVC,migrating
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/proper-to-2x/index
ms.openlocfilehash: c4ec21a50bc959f24131d9d4612c879a32c77356
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>Migrowanie ASP.NET ASP.NET Core 2.0

Przez [Isaac Levin](https://isaaclevin.com)

W tym artykule służy jako przewodnik odwołania dla migrowanie aplikacji ASP.NET do platformy ASP.NET Core 2.0.

## <a name="prerequisites"></a>Wymagania wstępne

* [Oprogramowanie .NET core 2.0.0 SDK](https://dot.net/core) lub nowszym.

## <a name="target-frameworks"></a>Docelowych platform
Projektów platformy ASP.NET Core 2.0 oferuje deweloperom elastyczność przeznaczonych dla platformy .NET Core i .NET Framework. Zobacz [wybór między .NET Core i .NET Framework dla aplikacji serwera](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) do określenia, które platforma docelowa jest najbardziej odpowiednia.

Jeśli celem .NET Framework, projekty muszą odwołania się do poszczególnych pakietów NuGet.

Przeznaczonych dla platformy .NET Core pozwala wyeliminować wiele odwołań jawne pakietu, dzięki użyciu składnika ASP.NET 2.0 Core [metapackage](xref:fundamentals/metapackage). Zainstaluj `Microsoft.AspNetCore.All` metapackage w projekcie:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

W przypadku metapackage żadnych pakietów, do którego odwołuje się metapackage zostały wdrożone za pomocą aplikacji. Magazynie środowiska uruchomieniowego .NET Core obejmuje te zasoby oraz są one wstępnie skompilowana, aby zwiększyć wydajność. Zobacz [metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x](xref:fundamentals/metapackage) uzyskać więcej szczegółowych informacji.

## <a name="project-structure-differences"></a>Różnice struktury projektu
*.Csproj* format pliku jest teraz prostszy w ASP.NET Core. Niektóre ważne zmiany obejmują:
- Dołączenie jawne plików nie jest niezbędne do uważany za część projektu. Zmniejsza ryzyko wystąpienia konfliktów scalania XML, podczas pracy w dużych zespołów.
- Brak żadnych odwołań na podstawie identyfikatora GUID do innych projektów, które poprawia czytelność pliku.
- Plik można edytować bez zwalnianie go w programie Visual Studio:

    ![Edytuj CSPROJ menu kontekstowego w Visual Studio 2017 r.](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Zastąpienia pliku Global.asax
Platformy ASP.NET Core wprowadzono nowe mechanizm uruchamianie aplikacji. Punkt wejścia dla aplikacji ASP.NET jest *Global.asax* pliku. Zadania, takie jak Konfiguracja tras i rejestracji filtru i obszaru są obsługiwane w *Global.asax* pliku.

[!code-csharp[Main](samples/globalasax-sample.cs)]

Takie podejście couples aplikacji i serwera, na którym jest wdrożona w taki sposób, aby zakłócać implementacji. W ramach działań zmierzających do oddziel [OWIN](http://owin.org/) wprowadzono w celu umożliwiają czyszczący jednocześnie używać wielu platform. OWIN zawiera potoku można dodać tylko moduły, które są potrzebne. Pobiera środowisko macierzyste [uruchamiania](xref:fundamentals/startup) funkcja Konfigurowanie usług i aplikacji żądania potoku. `Startup`rejestruje zestaw oprogramowania pośredniczącego z aplikacją. Dla każdego żądania aplikacja wywołuje wszystkich składników oprogramowania pośredniczącego ze wskaźnikiem head połączonej listy do istniejącego zestawu programów obsługi. Do żądania obsługi potoku poszczególnych składników oprogramowania pośredniczącego można dodać jeden lub więcej programów obsługi. Jest to osiągane przez zwracanie odwołania do obsługi, która jest nowy nagłówek listy. Każdy program obsługi jest odpowiedzialny za zapamiętywanie i wywoływanie dalej obsługi na liście. Jest punkt wejścia do aplikacji platformy ASP.NET Core `Startup`, i nie ma już zależność *Global.asax*. Jeśli używasz OWIN z .NET Framework, użyj przypominać następujące jako potoku:

[!code-csharp[Main](samples/webapi-owin.cs)]

Konfiguruje trasy domyślnej i domyślnie XmlSerialization w formacie Json. Dodaj do tego potoku innym oprogramowaniu pośredniczącym, zgodnie z potrzebami (ładowania usług, ustawienia konfiguracji, pliki statyczne, itp.).

Platformy ASP.NET Core używa podejście podobne, ale nie zależą od OWIN do obsługi wpis. Zamiast tego, który odbywa się za pośrednictwem *Program.cs* `Main` metody (podobnie jak aplikacje konsoli) i `Startup` jest ładowany przez niego.

[!code-csharp[Main](samples/program.cs)]

`Startup`musi zawierać `Configure` metody. W `Configure`, Dodaj niezbędne oprogramowanie pośredniczące do potoku. W poniższym przykładzie (z domyślnego szablonu witryny sieci web) kilka metod rozszerzenia są używane do konfigurowania potoku o obsługę:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Strony błędów
* Pliki statyczne
* ASP.NET Core MVC
* Tożsamość

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Hosta i aplikacji ma została odłączona zapewniające elastyczność przechodzenia do różnych platform w przyszłości.

**Uwaga:** Aby uzyskać więcej informacji na temat odwołanie do platformy ASP.NET Core uruchamiania i oprogramowanie pośredniczące, zobacz [uruchamiania w ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Zapisywanie konfiguracji
Program ASP.NET obsługuje ustawienia przechowywania. Te ustawienia są używane, na przykład do obsługi środowiska, w której zostały wdrożone aplikacje. Popularną praktyką było przechowywać wszystkie niestandardowe pary klucz wartość w `<appSettings>` sekcji *Web.config* pliku:

[!code-xml[Main](samples/webconfig-sample.xml)]

Aplikacje odczytywać te ustawienia przy użyciu `ConfigurationManager.AppSettings` kolekcji w `System.Configuration` przestrzeni nazw:

[!code-csharp[Main](samples/read-webconfig.cs)]

Platformy ASP.NET Core można przechowywać dane konfiguracyjne dla aplikacji w żadnym pliku i załadować je jako część uruchamianie oprogramowania pośredniczącego. Domyślny plik używany w szablonach projektu jest *appsettings.json*:

[!code-json[Main](samples/appsettings-sample.json)]

Ładowanie tego pliku do wystąpienia `IConfiguration` wewnątrz aplikacji zostało wykonane w *Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

Odczytuje aplikacji z `Configuration` pobieranie ustawień:

[!code-csharp[Main](samples/read-appsettings.cs)]

Brak rozszerzenia do tej metody w celu uproszczenia procesu bardziej niezawodne, takiej jak [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane) można załadować usługi za pomocą tych wartości. Podejście Podpisane zapewnia zbiór silnie typizowanych obiektów konfiguracji.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Uwaga:** uzyskać więcej informacji na temat odwołanie do konfiguracji platformy ASP.NET Core, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Iniekcji zależności natywnego
Celem ważne podczas kompilowania dużych, skalowalnych aplikacji jest luźne powiązanie składników i usług. [Iniekcji zależności](xref:fundamentals/dependency-injection) to technika popularnych na osiągnięcie tego celu, i jest natywny składnik ASP.NET Core.

W aplikacjach ASP.NET deweloperzy korzystają z biblioteki innych firm, aby zaimplementować iniekcji zależności. Jedna takie biblioteka jest [Unity](https://github.com/unitycontainer/unity), podany przez firmę Microsoft Patterns & rozwiązań. 

Przykład konfigurowania iniekcji zależności z Unity implementuje `IDependencyResolver` który opakowuje `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Utwórz wystąpienie programu `UnityContainer`, Zarejestruj usługi i ustawienie mechanizm rozpoznawania zależności `HttpConfiguration` na nowe wystąpienie klasy `UnityResolver` z kontenera:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Wstaw `IProductRepository` w razie potrzeby:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Ponieważ iniekcji zależności jest częścią platformy ASP.NET Core, można dodać usługi w `ConfigureServices` metody *Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

Repozytorium mogą zostać dodane wszędzie, podobnie jak z Unity.

**Uwaga:** szczegółowe odwołania do iniekcji zależności w ASP.NET Core, zobacz [iniekcji zależności w ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Dostarczanie plików statycznych
Ważnym elementem projektowanie witryn sieci web jest możliwość obsługi zasoby statyczne, po stronie klienta. Najbardziej typowe przykłady pliki statyczne są HTML, CSS, Javascript i obrazów. Te pliki muszą być zapisane w lokalizacji opublikowanej aplikacji (lub CDN) i odwołuje się do, mogą być ładowane przez żądanie. Ten proces został zmieniony w ASP.NET Core.

W programie ASP.NET pliki statyczne są przechowywane w różnych katalogach i przywoływany w widokach.

W ASP.NET Core pliki statyczne są przechowywane w katalogu"web" (*&lt;zawartości głównego&gt;/wwwroot*), chyba że skonfigurowano inaczej. Pliki są ładowane do potoku żądania, wywołując `UseStaticFiles` — metoda rozszerzenia z `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Uwaga:** Jeśli przeznaczonych dla platformy .NET Framework, zainstaluj pakiet NuGet `Microsoft.AspNetCore.StaticFiles`.

Na przykład zasób obrazu w *wwwroot/obrazy* takie jak folder jest dostępny dla przeglądarki w lokalizacji `http://<app>/images/<imageFileName>`.

**Uwaga:** na pełniejsze odwołanie do dostarczania plików statycznych w ASP.NET Core, zobacz [wprowadzenie do pracy z plików statycznych w ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Eksportowanie bibliotek .NET Core](https://docs.microsoft.com/dotnet/core/porting/libraries)
