---
title: Migrowanie z programu ASP.NET do ASP.NET Core 2.0
author: isaac2004
description: Odbieranie wskazówki dotyczące migrowania istniejących aplikacji ASP.NET MVC lub Web API platformy ASP.NET Core 2.0.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/mvc2
ms.openlocfilehash: 7f048f2f95f1a51a0b6ce3d36665420ff28ec26f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208476"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a>Migrowanie z programu ASP.NET do ASP.NET Core 2.0

Przez [Levin Tomasz](https://isaaclevin.com)

Ten artykuł służy jako przewodnik odwołania do migrowania aplikacji ASP.NET do ASP.NET Core 2.0.

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj **jeden** z następujących czynności z [.NET pliki do pobrania: Windows](https://www.microsoft.com/net/download/windows):

* Zestaw SDK dla platformy .NET core
* Visual Studio for Windows
  * **ASP.NET i tworzenie aplikacji internetowych** obciążenia
  * **Programowanie dla wielu platform .NET core** obciążenia

## <a name="target-frameworks"></a>Platformy docelowe

Projekty ASP.NET Core 2.0 oferują deweloperom elastyczność określania wartości docelowej platformy .NET Core i .NET Framework. Zobacz [Wybieranie między programami .NET Core i .NET Framework dla aplikacji serwerowych](/dotnet/standard/choosing-core-framework-server) ustalenie, której lokalizacja docelowa jest najbardziej odpowiednie.

Podczas przeznaczonych dla platformy .NET Framework, projekty muszą odwoływać się do poszczególnych pakietów NuGet.

Przeznaczone dla platformy .NET Core pozwala wyeliminować wiele odwołań pakietu jawne, dzięki ASP.NET Core 2.0 [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage). Zainstaluj `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all w projekcie:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
</ItemGroup>
```

W przypadku meta Microsoft.aspnetcore.all żadnych pakietów, do którego odwołuje się meta Microsoft.aspnetcore.all są wdrażane przy użyciu aplikacji. Store środowiska uruchomieniowego programu .NET Core obejmuje te zasoby, i są one wstępnie skompilowany do zwiększenia wydajności. Zobacz <xref:fundamentals/metapackage> Aby uzyskać więcej szczegółów.

## <a name="project-structure-differences"></a>Różnice struktura projektu

*.Csproj* format pliku została uproszczona w programie ASP.NET Core. Pewne istotne zmiany obejmują:

* Dołączenie jawne plików nie jest niezbędne zostały uznane za częścią projektu. Zmniejsza to ryzyko konfliktów scalania XML, pracując w dużych zespołach.
* Nie ma żadnych identyfikatora GUID na podstawie odwołań do innych projektów, co zwiększa czytelność pliku.
* Plik można edytować bez rozładowywania go w programie Visual Studio:

  ![Edytowanie pliku CSPROJ menu kontekstowego w programie Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Zastąpienie pliku Global.asax

Platforma ASP.NET Core wprowadzono nowy mechanizm do uruchomienia aplikacji. Punkt wejścia dla aplikacji platformy ASP.NET to *Global.asax* pliku. Zadania, takie jak Konfiguracja tras i filtrowania i obszaru rejestracje są obsługiwane w *Global.asax* pliku.

[!code-csharp[](samples/globalasax-sample.cs)]

To podejście couples aplikacji i serwera, na którym jest wdrożona w taki sposób, że koliduje z wykonywaniem. W ramach działań zmierzających do oddzielenia [OWIN](http://owin.org/) została wprowadzona w celu zapewnienia bardziej przejrzysty sposób na korzystanie z wielu platform ze sobą. OWIN zawiera potoku można dodać tylko do modułów, które są potrzebne. Środowisko hostingu zajmuje [uruchamiania](xref:fundamentals/startup) funkcja Konfigurowanie usług i potok żądań aplikacji. `Startup` rejestruje zestaw oprogramowania pośredniczącego z aplikacją. Dla każdego żądania aplikacja wywołuje wszystkich składników oprogramowania pośredniczącego przy użyciu głównego wskaźnika połączonej listy do istniejącego zestawu obsługi. Każdy składnik oprogramowania pośredniczącego można dodać jeden lub więcej programów obsługi żądania obsługi potoku. Jest to realizowane przez zwracanie odwołania do programu obsługi, który jest nowy nagłówek listy. Każdy program obsługi jest odpowiedzialny za zapamiętywania i wywoływanie kolejna procedura obsługi na liście. Platforma ASP.NET Core jest punkt wejścia do aplikacji `Startup`, i nie ma już zależności *Global.asax*. Korzystając z OWIN za pomocą .NET Framework, należy użyć podobną do następującej jako potok:

[!code-csharp[](samples/webapi-owin.cs)]

Konfiguruje trasy domyślne, a wartość domyślna to XmlSerialization w formacie JSON. Dodaj do tego potoku innym oprogramowaniu pośredniczącym, zgodnie z potrzebami (Trwa ładowanie usług, ustawienia konfiguracji, pliki statyczne itp.).

Platforma ASP.NET Core korzysta z podobnego podejścia, ale nie jest zależny od OWIN do obsługi wpis. Zamiast tego zostanie to zrobione za pośrednictwem *Program.cs* `Main` — metoda (podobne do aplikacji konsoli) i `Startup` jest załadowany poprzez miejsca.

[!code-csharp[](samples/program.cs)]

`Startup` musi zawierać `Configure` metody. W `Configure`, dodać niezbędne oprogramowanie pośredniczące do potoku. W poniższym przykładzie (z domyślnego szablonu witryny sieci web) kilka metod rozszerzenia są używane do konfigurowania potoku obsługę:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Strony błędów
* Pliki statyczne
* ASP.NET Core MVC
* Tożsamość

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Host i aplikacji ma zostały odłączone zapewniającą elastyczność przechodzenia do różnych platform w przyszłości.

Aby uzyskać więcej informacji na temat odwołania do platformy ASP.NET Core uruchamianie i oprogramowanie pośredniczące, zobacz <xref:fundamentals/startup>.

## <a name="storing-configurations"></a>Zapisywanie konfiguracji

Obsługuje platformy ASP.NET z przechowywaniem ustawień. Te ustawienia są używane, na przykład do obsługi środowiska, w której zostały wdrożone aplikacje. Powszechną praktyką było przechowywać wszystkie niestandardowe pary klucz wartość w `<appSettings>` części *Web.config* pliku:

[!code-xml[](samples/webconfig-sample.xml)]

Aplikacje odczytywać te ustawienia przy użyciu `ConfigurationManager.AppSettings` kolekcji w `System.Configuration` przestrzeni nazw:

[!code-csharp[](samples/read-webconfig.cs)]

Platforma ASP.NET Core można przechowywać dane konfiguracji dla aplikacji w dowolnym pliku i załadować je jako część uruchamianie oprogramowania pośredniczącego. Domyślny plik używany w szablonach projektu jest *appsettings.json*:

[!code-json[](samples/appsettings-sample.json)]

Załadowanie pliku jako wystąpienia elementu `IConfiguration` wewnątrz aplikacji odbywa się w *Startup.cs*:

[!code-csharp[](samples/startup-builder.cs)]

Aplikacja odczytuje z `Configuration` pobieranie ustawień:

[!code-csharp[](samples/read-appsettings.cs)]

Istnieją rozszerzenia tego podejścia w celu uproszczenia procesu bardziej niezawodne, takiej jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI), aby obciążenia usługi za pomocą tych wartości. Podejście DI zawiera zestaw silnie typizowanych obiektów konfiguracji.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Uwaga:** Aby uzyskać więcej informacji na temat odwołania do konfiguracji platformy ASP.NET Core, zobacz <xref:fundamentals/configuration/index>.

## <a name="native-dependency-injection"></a>Wstrzykiwanie zależności natywnych

Ważne podczas kompilowania dużych, skalowalnych aplikacji ma luźne powiązanie składników i usług. [Wstrzykiwanie zależności](xref:fundamentals/dependency-injection) to technika popularnych służące osiągnięciu tego celu i jest to natywny składnik ASP.NET Core.

W aplikacjach ASP.NET deweloperzy zależy od innej biblioteki, aby zaimplementować iniekcji zależności. Jednego takiego biblioteka jest [Unity](https://github.com/unitycontainer/unity), podana przez Microsoft Patterns & rozwiązania.

Przykład konfigurowania wstrzykiwanie zależności za pomocą aparatu Unity implementuje `IDependencyResolver` to opakowuje `UnityContainer`:

[!code-csharp[](samples/sample8.cs)]

Utwórz wystąpienie usługi `UnityContainer`, a następnie zarejestrować usługę i ustawić mechanizm rozpoznawania zależności `HttpConfiguration` do nowego wystąpienia programu `UnityResolver` kontenera:

[!code-csharp[](samples/sample9.cs)]

Wstrzykiwanie `IProductRepository` w razie potrzeby:

[!code-csharp[](samples/sample5.cs)]

Wstrzykiwanie zależności jest częścią platformy ASP.NET Core, dlatego można dodać usługi `Startup.ConfigureServices`:

[!code-csharp[](samples/configure-services.cs)]

Repozytorium może być wprowadzony dowolnego miejsca, podobnie jak przy użyciu aparatu Unity.

Aby uzyskać więcej informacji na temat wstrzykiwanie zależności w programie ASP.NET Core, zobacz <xref:fundamentals/dependency-injection>.

## <a name="serving-static-files"></a>Dostarczania plików statycznych.

Ważnym elementem programowania dla sieci web jest możliwość obsługi zasoby statyczne, po stronie klienta. Najbardziej typowe przykłady plików statycznych są HTML, CSS, Javascript i obrazy. Te pliki muszą być zapisane w lokalizacji opublikowany aplikacji (lub sieci CDN) i odwołania, dzięki czemu mogą być ładowane przez żądanie. Ten proces został zmieniony w programie ASP.NET Core.

W programie ASP.NET: pliki statyczne są przechowywane w różnych katalogach i przywoływany w widokach.

W programie ASP.NET Core, pliki statyczne są przechowywane w "root sieci web" (*&lt;zawartości głównego&gt;/wwwroot*), chyba że skonfigurowano inaczej. Pliki są ładowane do potoku żądania za pomocą wywołania `UseStaticFiles` metoda rozszerzenia z `Startup.Configure`:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Uwaga:** Jeśli przeznaczony dla .NET Framework, należy zainstalować pakiet NuGet `Microsoft.AspNetCore.StaticFiles`.

Na przykład zasób obrazu w *wwwroot/obrazów* folder jest dostępny w przeglądarce w lokalizacji, takich jak `http://<app>/images/<imageFileName>`.

**Uwaga:** Aby uzyskać więcej informacji na temat odwołania do dostarczania plików statycznych z platformy ASP.NET Core, zobacz <xref:fundamentals/static-files>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Przenoszenie bibliotek .NET Core](/dotnet/core/porting/libraries)
