---
title: Migrowanie ASP.NET ASP.NET Core
author: isaac2004
description: Odbieranie wskazówki dotyczące migrowania istniejących aplikacji ASP.NET MVC lub Web API do ASP.NET Core.web
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/index
ms.openlocfilehash: 82f85bf2919fac1c023c0b89419a42a3ef7c402c
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a>Migrowanie ASP.NET ASP.NET Core

Przez [Isaac Levin](https://isaaclevin.com)

W tym artykule służy jako Podręcznik migrowanie aplikacji ASP.NET do platformy ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

## <a name="target-frameworks"></a>Docelowych platform

Projektów platformy ASP.NET Core oferuje deweloperom elastyczność przeznaczonych dla platformy .NET Core i .NET Framework. Zobacz [wybór między .NET Core i .NET Framework dla aplikacji serwera](/dotnet/standard/choosing-core-framework-server) do określenia, które platforma docelowa jest najbardziej odpowiednia.

Jeśli celem .NET Framework, projekty muszą odwołania się do poszczególnych pakietów NuGet.

Przeznaczonych dla platformy .NET Core pozwala wyeliminować wiele odwołań jawne pakietu, dzięki użyciu platformy ASP.NET Core [metapackage](xref:fundamentals/metapackage). Zainstaluj `Microsoft.AspNetCore.All` metapackage w projekcie:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

W przypadku metapackage żadnych pakietów, do którego odwołuje się metapackage zostały wdrożone za pomocą aplikacji. Magazynie środowiska uruchomieniowego .NET Core obejmuje te zasoby oraz są one wstępnie skompilowana do zwiększenia wydajności. Zobacz [metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x](xref:fundamentals/metapackage) uzyskać więcej szczegółowych informacji.

## <a name="project-structure-differences"></a>Różnice struktury projektu

*.Csproj* format pliku jest teraz prostszy w ASP.NET Core. Niektóre ważne zmiany obejmują:

- Dołączenie jawne plików nie jest niezbędne do uważany za część projektu. Zmniejsza ryzyko wystąpienia konfliktów scalania XML, podczas pracy w dużych zespołów.
- Brak żadnych odwołań na podstawie identyfikatora GUID do innych projektów, które poprawia czytelność pliku.
- Plik można edytować bez zwalnianie go w programie Visual Studio:

    ![Edytuj CSPROJ menu kontekstowego w Visual Studio 2017 r.](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Zastąpienia pliku Global.asax

Platformy ASP.NET Core wprowadzono nowe mechanizm uruchamianie aplikacji. Punkt wejścia dla aplikacji ASP.NET jest *Global.asax* pliku. Zadania, takie jak Konfiguracja tras i rejestracji filtru i obszaru są obsługiwane w *Global.asax* pliku.

[!code-csharp[](samples/globalasax-sample.cs)]

Takie podejście couples aplikacji i serwera, na którym jest wdrożona w taki sposób, aby zakłócać implementacji. W ramach działań zmierzających do oddziel [OWIN](http://owin.org/) wprowadzono w celu umożliwiają czyszczący jednocześnie używać wielu platform. OWIN zawiera potoku można dodać tylko moduły, które są potrzebne. Pobiera środowisko macierzyste [uruchamiania](xref:fundamentals/startup) funkcja Konfigurowanie usług i aplikacji żądania potoku. `Startup` rejestruje zestaw oprogramowania pośredniczącego z aplikacją. Dla każdego żądania aplikacja wywołuje wszystkich składników oprogramowania pośredniczącego ze wskaźnikiem head połączonej listy do istniejącego zestawu programów obsługi. Do żądania obsługi potoku poszczególnych składników oprogramowania pośredniczącego można dodać jeden lub więcej programów obsługi. Jest to osiągane przez zwracanie odwołania do obsługi, która jest nowy nagłówek listy. Każdy program obsługi jest odpowiedzialny za zapamiętywanie i wywoływanie dalej obsługi na liście. Jest punkt wejścia do aplikacji platformy ASP.NET Core `Startup`, i nie ma już zależność *Global.asax*. Jeśli używasz OWIN z .NET Framework, użyj przypominać następujące jako potoku:

[!code-csharp[](samples/webapi-owin.cs)]

Konfiguruje trasy domyślnej i domyślnie XmlSerialization w formacie Json. Dodaj do tego potoku innym oprogramowaniu pośredniczącym, zgodnie z potrzebami (ładowania usług, ustawienia konfiguracji, pliki statyczne, itp.).

Platformy ASP.NET Core używa podejście podobne, ale nie zależą od OWIN do obsługi wpis. Zamiast tego, który odbywa się za pośrednictwem *Program.cs* `Main` metody (podobnie jak aplikacje konsoli) i `Startup` jest ładowany przez niego.

[!code-csharp[](samples/program.cs)]

`Startup` musi zawierać `Configure` metody. W `Configure`, Dodaj niezbędne oprogramowanie pośredniczące do potoku. W poniższym przykładzie (z domyślnego szablonu witryny sieci web) kilka metod rozszerzenia są używane do konfigurowania potoku o obsługę:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Strony błędów
* Pliki statyczne
* ASP.NET Core MVC
* Tożsamość

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Hosta i aplikacji ma została odłączona zapewniające elastyczność przechodzenia do różnych platform w przyszłości.

> [!NOTE]
> Aby uzyskać więcej informacji na temat odwołanie do platformy ASP.NET Core uruchamiania i oprogramowanie pośredniczące, zobacz [uruchamiania w ASP.NET Core](xref:fundamentals/startup)

## <a name="store-configurations"></a>Konfiguracje magazynu

Program ASP.NET obsługuje ustawienia przechowywania. Te ustawienia są używane, na przykład do obsługi środowiska, w której zostały wdrożone aplikacje. Popularną praktyką było przechowywać wszystkie niestandardowe pary klucz wartość w `<appSettings>` sekcji *Web.config* pliku:

[!code-xml[](samples/webconfig-sample.xml)]

Aplikacje odczytywać te ustawienia przy użyciu `ConfigurationManager.AppSettings` kolekcji w `System.Configuration` przestrzeni nazw:

[!code-csharp[](samples/read-webconfig.cs)]

Platformy ASP.NET Core można przechowywać dane konfiguracyjne dla aplikacji w żadnym pliku i załadować je jako część uruchamianie oprogramowania pośredniczącego. Domyślny plik używany w szablonach projektu jest *appsettings.json*:

[!code-json[](samples/appsettings-sample.json)]

Ładowanie tego pliku do wystąpienia `IConfiguration` wewnątrz aplikacji zostało wykonane w *Startup.cs*:

[!code-csharp[](samples/startup-builder.cs)]

Odczytuje aplikacji z `Configuration` pobieranie ustawień:

[!code-csharp[](samples/read-appsettings.cs)]

Brak rozszerzenia do tej metody w celu uproszczenia procesu bardziej niezawodne, takiej jak [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane) można załadować usługi za pomocą tych wartości. Podejście Podpisane zapewnia zbiór silnie typizowanych obiektów konfiguracji.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> Więcej informacji na temat odwołanie do konfiguracji platformy ASP.NET Core, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Iniekcji zależności natywnego

Celem ważne podczas kompilowania dużych, skalowalnych aplikacji jest luźne powiązanie składników i usług. [Iniekcji zależności](xref:fundamentals/dependency-injection) to technika popularnych na osiągnięcie tego celu, i jest natywny składnik ASP.NET Core.

W aplikacjach ASP.NET deweloperzy korzystają z biblioteki innych firm, aby zaimplementować iniekcji zależności. Jedna takie biblioteka jest [Unity](https://github.com/unitycontainer/unity), podany przez firmę Microsoft Patterns & rozwiązań.

Przykład konfigurowania iniekcji zależności z Unity implementuje `IDependencyResolver` który opakowuje `UnityContainer`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Utwórz wystąpienie programu `UnityContainer`, Zarejestruj usługi i ustawienie mechanizm rozpoznawania zależności `HttpConfiguration` na nowe wystąpienie klasy `UnityResolver` z kontenera:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Wstaw `IProductRepository` w razie potrzeby:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Ponieważ iniekcji zależności jest częścią platformy ASP.NET Core, można dodać usługi w `ConfigureServices` metody *Startup.cs*:

[!code-csharp[](samples/configure-services.cs)]

Repozytorium mogą zostać dodane wszędzie, podobnie jak z Unity.

> [!NOTE]
> Aby uzyskać szczegółowe informacje do iniekcji zależności w ASP.NET Core, zobacz [iniekcji zależności w ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serve-static-files"></a>Obsługiwać pliki statyczne

Ważnym elementem projektowanie witryn sieci web jest możliwość obsługi zasoby statyczne, po stronie klienta. Najbardziej typowe przykłady pliki statyczne są HTML, CSS, Javascript i obrazów. Te pliki muszą być zapisane w lokalizacji opublikowanej aplikacji (lub CDN) i odwołuje się do, mogą być ładowane przez żądanie. Ten proces został zmieniony w ASP.NET Core.

W programie ASP.NET pliki statyczne są przechowywane w różnych katalogach i przywoływany w widokach.

W ASP.NET Core pliki statyczne są przechowywane w katalogu"web" (*&lt;zawartości głównego&gt;/wwwroot*), chyba że skonfigurowano inaczej. Pliki są ładowane do potoku żądania, wywołując `UseStaticFiles` — metoda rozszerzenia z `Startup.Configure`:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> Jeśli przeznaczonych dla platformy .NET Framework, zainstaluj pakiet NuGet `Microsoft.AspNetCore.StaticFiles`.

Na przykład zasób obrazu w *wwwroot/obrazy* takie jak folder jest dostępny dla przeglądarki w lokalizacji `http://<app>/images/<imageFileName>`.

> [!NOTE]
> Aby uzyskać więcej informacji na temat odwołanie do dostarczania plików statycznych w ASP.NET Core, zobacz [pliki statyczne](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Eksportowanie bibliotek .NET Core](/dotnet/core/porting/libraries)
