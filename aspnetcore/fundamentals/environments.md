---
title: "Praca z wieloma środowiska w programie ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak platformy ASP.NET Core umożliwia kontrolowanie zachowania aplikacji w wielu środowiskach."
keywords: "Platformy ASP.NET Core ustawienia środowiska, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 9127c3d7180422c0e3dbd813340dd485bf360c81
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="working-with-multiple-environments"></a>Praca w środowiskach wielu

Przez [Steve Smith](https://ardalis.com/)

Platformy ASP.NET Core umożliwia kontrolowanie zachowania aplikacji w wielu środowiskach, takich jak projektowanie, tymczasową i produkcyjną. Zmienne środowiskowe są używane do wskazywania środowisko uruchomieniowe, dzięki czemu aplikacja dla tego środowiska.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Programowanie, przemieszczania, produkcji

Platformy ASP.NET Core odwołuje się do danej zmiennej środowiskowej, `ASPNETCORE_ENVIRONMENT` do opisu aplikacji jest obecnie uruchomiony w środowisku. Tę zmienną można ustawić dowolną wartość Ci się podoba, ale trzy wartości są używane przez Konwencję: `Development`, `Staging`, i `Production`. Dostępne są te wartości używane w przykładach i szablony dostarczane z platformy ASP.NET Core.

Bieżące ustawienie środowiska mogą być wykrywane programowo z wewnątrz aplikacji. Ponadto można użyć środowiska [pomocnika tagów](../mvc/views/tag-helpers/index.md) do uwzględnienia niektórych części Twojego [widoku](../mvc/views/index.md) na podstawie bieżącego środowiska aplikacji.

Uwaga: W systemach Windows i macOS nazwę określonego środowiska jest bez uwzględniania wielkości liter. Czy Ustaw zmienną na `Development` lub `development` lub `DEVELOPMENT` wyniki będą takie same. Jednak Linux jest **z uwzględnieniem wielkości liter** systemu operacyjnego domyślnie. Zmienne środowiskowe, nazwy plików i ustawienia wymagają rozróżniania wielkości liter.

### <a name="development"></a>Tworzenie

Powinno to być środowisko używany podczas tworzenia aplikacji. Zwykle jest używana do włączenia funkcji, które nie mają być dostępne, gdy aplikacja jest uruchamiana w środowisku produkcyjnym, takich jak [developer wyjątku strony](xref:fundamentals/error-handling#the-developer-exception-page).

Jeśli używasz programu Visual Studio, można skonfigurować środowisko w profilach debugowania projektu. Debugowanie profilów określ [serwera](xref:fundamentals/servers/index) na potrzeby można ustawić podczas uruchamiania aplikacji i zmiennych środowiskowych. Projekt może mieć wiele profilów debugowania, które ustawione inaczej dla zmiennych środowiskowych. Te profile są zarządzane za pomocą **debugowania** kartę projektu aplikacji sieci web **właściwości** menu. Wartości ustawione we właściwościach projektu są zachowywane w *launchSettings.json* pliku, a także można skonfigurować profile bezpośrednio edytując ten plik.

Profil dla usług IIS Express jest następujący:

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

Oto `launchSettings.json` plik zawierający profilów `Development` i `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

Zmiany wprowadzone do profilów projektu mogą nie zostać zastosowane do czasu ponownego uruchomienia serwera sieci web, użyć (w szczególności Kestrel należy ponownie uruchomić przed wykryje zmiany wprowadzone w jego środowisku).

>[!WARNING]
> Zmienne środowiskowe są przechowywane w *launchSettings.json* nie są już zabezpieczone w jakikolwiek sposób i będzie częścią repozytorium kodu źródłowego dla projektu, jeśli używany jest jeden. **Nigdy nie przechowują poświadczeń lub inne poufne dane w tym pliku.** Miejsce do przechowywania tych danych, należy użyć *Manager klucz tajny* narzędzia opisanego w [bezpiecznego magazynu kluczy tajnych aplikacji podczas tworzenia](xref:security/app-secrets).

### <a name="staging"></a>Przemieszczania

Według konwencji `Staging` środowisko jest używane do testowania końcowego przed wdrożeniem w środowisku produkcyjnym w środowisku przedprodukcyjnym. Najlepiej, jeśli jego właściwości fizyczne duplikowany, który produkcji, tak, aby wszelkie problemy, które mogą wystąpić w środowisku produkcyjnym występować jako pierwsza w środowisku przemieszczania, w którym może zostać zlikwidowane bez wpływu na użytkowników.

### <a name="production"></a>Produkcji

`Production` Środowiska to środowisko, w której aplikacja jest uruchamiana podczas działania i używane przez użytkowników końcowych. Tego środowiska należy skonfigurować w celu zwiększenia bezpieczeństwa, wydajności i niezawodności aplikacji. Niektóre typowe ustawienia, które mogą mieć w środowisku produkcyjnym różniących od projektowania obejmują:

* Włącz buforowanie

* Upewnij się, wszystkie zasoby po stronie klienta są powiązane, zminimalizowane i potencjalnie pochodzący z sieci CDN

* Wyłącz ErrorPages diagnostycznych

* Włącz stron błędów przyjazne

* Włącz produkcji rejestrowania i monitorowania (na przykład [usługi Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

To jest ale w żadnym wypadku nie można uzyskać pełną listę. Najlepiej uniknąć rozproszenia kontroli środowiska w wielu części aplikacji. Zamiast tego jest zalecane podejście do takiego sprawdzania w tej aplikacji `Startup` klasy tam gdzie to możliwe

## <a name="setting-the-environment"></a>Ustawienia środowiska

Metoda do ustawiania środowiska zależy od systemu operacyjnego.

### <a name="windows"></a>Windows
Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, jeśli aplikacja zostanie uruchomiona przy użyciu `dotnet run`, są używane następujące polecenia

**Wiersz polecenia**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**Środowiska PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Te polecenia brane pod uwagę tylko dla bieżącego okna. Po zamknięciu okna ustawienie ASPNETCORE_ENVIRONMENT Przywraca wartości domyślne ustawienie lub komputera. Aby można było ustawić wartość globalnie dla systemu Windows otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT` wartość.

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png) 

**plik Web.config**

Zobacz *ustawienia zmiennych środowiskowych* sekcji [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.

**Dla każdej puli aplikacji usług IIS**

Jeśli musisz ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w pulach aplikacji izolowanych (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentacji odwołania tematu w usługach IIS.

### <a name="macos"></a>System macOS
Ustawianie bieżącego środowiska pod kątem macOS może odbywać się w wierszu przy uruchamianiu aplikacji;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
lub przy użyciu `export` ustaw go przed uruchomieniem aplikacji.

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
Zmienne środowiskowe poziomu są ustawiane w *.bashrc* lub *.bash_profile* pliku. Przeprowadź edycję pliku za pomocą dowolnego edytora tekstu, a następnie dodaj następującą instrukcję.

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a>Linux
Dystrybucjach systemu Linux, można użyć `export` polecenie w wierszu polecenia dla zmiennej ustawieniami opartymi na sesji i *bash_profile* w pliku ustawień z poziomu środowiska maszyny.

## <a name="determining-the-environment-at-runtime"></a>Określanie środowiska w czasie wykonywania

`IHostingEnvironment` Usługa udostępnia abstrakcję core Praca w środowiskach. Ta usługa jest dostarczane przez platformę ASP.NET hosting warstwy i mogą zostać dodane do Logika uruchamiania za pomocą [iniekcji zależności](dependency-injection.md). Szablon witryny sieci web platformy ASP.NET Core w programie Visual Studio korzysta z tej metody można załadować plików konfiguracyjnych środowiska (jeśli istnieje) i dostosować ustawienia postępowania z błędami instalacji aplikacji. W obu przypadkach to zachowanie jest to osiągane przez odwołanie do aktualnie określonego środowiska przez wywołanie metody `EnvironmentName` lub `IsEnvironment` w wystąpieniu `IHostingEnvironment` przekazany do odpowiedniej metody.

> [!NOTE]
> Jeśli należy sprawdzić, czy aplikacja jest uruchomiona w określonym środowisku użyj `env.IsEnvironment("environmentname")` ponieważ poprawnie zignoruje przypadku (zamiast sprawdzania, czy `env.EnvironmentName == "Development"` na przykład).

Na przykład można użyć następujących kodu w metodę Konfiguruj można skonfigurować środowisko obsługi błędu:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Jeśli aplikacja jest uruchomiona `Development` środowiska, a następnie go włącza Obsługa środowiska uruchomieniowego koniecznych do używania tej funkcji "BrowserLink" w programie Visual Studio, stron z błędem specyficznym dla rozwoju, (które zwykle nie powinna być uruchamiana w środowisku produkcyjnym) i błąd specjalne bazy danych strony (które umożliwiają zastosowanie migracji i w związku z tym można używać tylko w rozwoju). W przeciwnym razie jeśli aplikacja nie jest uruchomiony w środowisku projektowania, strony obsługi błędów skonfigurowano mają być wyświetlane w odpowiedzi żadnych nieobsłużonych wyjątków.

Należy określić, którego zawartość do wysłania do klienta w czasie wykonywania, w zależności od bieżącego środowiska. Na przykład w środowisku projektowym można zwykle służą-zminimalizowane skrypty i arkusze stylów, dzięki czemu ułatwia debugowanie. Środowisk produkcyjnych i testowych powinien służyć zminimalizowany wersji i zwykle z sieci CDN. Można to zrobić przy użyciu środowiska [pomocnika tagów](../mvc/views/tag-helpers/intro.md). Pomocnik tag środowiska tylko renderowania jego zawartość, jeśli bieżącego środowiska pasuje do jednej z tych środowisk, określić przy użyciu `names` atrybutu.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Aby rozpocząć używanie pomocników tagów w Twojej aplikacji można znaleźć [wprowadzenie do pomocników tagów](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Konwencje uruchamiania

Platformy ASP.NET Core obsługuje podejście opartych na konwencjach skonfigurowanie uruchamiania aplikacji na podstawie bieżącego środowiska. Można również programowe sterowanie, jak aplikacja ma zachowywać się zgodnie z poszczególnych środowisk jest, co umożliwia tworzenie i zarządzanie nimi własne konwencje.

Po uruchomieniu aplikacji platformy ASP.NET Core `Startup` klasa jest używana do ładowania początkowego aplikacji, załadowanie jego ustawienia konfiguracji, itp. ([Dowiedz się więcej na temat uruchamiania ASP.NET](startup.md)). Jednak jeśli istnieje klasa o nazwie `Startup{EnvironmentName}` (na przykład `StartupDevelopment`) i `ASPNETCORE_ENVIRONMENT` odpowiada zmiennej środowiskowej tej nazwy, następnie `Startup` klasa jest używana zamiast tego. W związku z tym można skonfigurować `Startup` do tworzenia aplikacji, ale ma oddzielnej `StartupProduction` który będzie używany, gdy aplikacja jest uruchamiana w środowisku produkcyjnym. Lub na odwrót.

> [!NOTE]
> Wywoływanie `WebHostBuilder.UseStartup<TStartup>()` zastępuje sekcji konfiguracyjnych.

Oprócz przy użyciu w pełni odrębnych `Startup` MFC opartą na bieżącym środowisku, można także dokonać zmian w konfiguracji aplikacji w ramach `Startup` klasy. `Configure()` i `ConfigureServices()` metody obsługuje wersji określonego środowiska podobny do `Startup` klasy samego formularza `Configure{EnvironmentName}()` i `Configure{EnvironmentName}Services()`. W przypadku definiowania metody `ConfigureDevelopment()` będzie ona wywoływana zamiast `Configure()` gdy środowisko jest ustawiona na programowanie. Podobnie `ConfigureDevelopmentServices()` może być wywoływana zamiast `ConfigureServices()` w tym samym środowisku.

## <a name="summary"></a>Podsumowanie

Platformy ASP.NET Core oferuje następujące funkcje i konwencje, które umożliwiają deweloperom łatwe kontrolować zachowanie ich aplikacji w różnych środowiskach. Podczas publikowania aplikacji od projektowania do etapu przemieszczania do środowiska produkcyjnego, zestaw zmiennych środowiska odpowiednio dla środowiska umożliwiają optymalizacji aplikacji do użycia debugowania, testowania lub zastosowania, zależnie od potrzeb.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Konfiguracja](xref:fundamentals/configuration/index)

* [Wprowadzenie do pomocników tagów](../mvc/views/tag-helpers/intro.md)
