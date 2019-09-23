---
title: Wdróż aplikacje ASP.NET Core w Azure App Service
author: guardrex
description: Ten artykuł zawiera linki do hosta platformy Azure i wdrażania zasobów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 4dc150ff4534e42e1995a185f650cea9df70ccc4
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187047"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>Wdróż aplikacje ASP.NET Core w Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) to [usługa platformy obliczeniowej w chmurze firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, w tym ASP.NET Core.

## <a name="useful-resources"></a>Przydatne zasoby

[Dokumentacja App Service](/azure/app-service/) jest domem dla dokumentacji usługi Azure Apps, samouczków, przykładów, przewodników i innych zasobów. Dwa istotne samouczki odnoszące się do hostingu aplikacji ASP.NET Core są następujące:

[Tworzenie aplikacji sieci Web ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Użyj programu Visual Studio, aby utworzyć i wdrożyć aplikację sieci Web ASP.NET Core do Azure App Service w systemie Windows.

[Tworzenie aplikacji ASP.NET Core w usłudze App Service w systemie Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Użyj wiersza polecenia, aby utworzyć i wdrożyć aplikację sieci Web ASP.NET Core do Azure App Service w systemie Linux.

Następujące artykuły są dostępne w dokumentacji ASP.NET Core:

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
Dowiedz się, jak utworzyć aplikację internetową ASP.NET Core przy użyciu programu Visual Studio i wdrożyć ją do Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.

[Tworzenie pierwszego potoku](/azure/devops/pipelines/get-started-yaml)  
Skonfiguruj kompilację CI dla aplikacji ASP.NET Core, a następnie Utwórz wersję ciągłego wdrażania do Azure App Service.

[Piaskownica aplikacji sieci Web platformy Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Odkryj ograniczenia wykonywania w środowisku uruchomieniowym Azure App Service wymuszane przez platformę Azure Apps.

<xref:test/troubleshoot>  
Zrozumienie i rozwiązywanie problemów z ostrzeżeniami i błędami w projektach ASP.NET Core.

## <a name="application-configuration"></a>Konfiguracja aplikacji

### <a name="platform"></a>Platforma

::: moniker range=">= aspnetcore-2.2"

W Azure App Service są obecne środowiska uruchomieniowe dla aplikacji 64-bitowych (x64) i 32-bitowych (x86). [Zestaw .NET Core SDK](/dotnet/core/sdk) dostępne w App Service to 32-bitowe, ale można wdrożyć aplikacje 64-bitowe utworzone lokalnie za pomocą konsoli [kudu](https://github.com/projectkudu/kudu/wiki) lub procesu publikowania w programie Visual Studio. Aby uzyskać więcej informacji, zobacz sekcję [Publikowanie i wdrażanie aplikacji](#publish-and-deploy-the-app) .

::: moniker-end

::: moniker range="< aspnetcore-2.2"

W przypadku aplikacji z natywnymi zależnościami w Azure App Service są obecne środowiska uruchomieniowe dla 32-bitowych (x86) aplikacji. [Zestaw .NET Core SDK](/dotnet/core/sdk) dostępne w App Service to 32-bitowe.

::: moniker-end

Aby uzyskać więcej informacji na temat składników .NET Core i metod dystrybucji, takich jak informacje o środowisku uruchomieniowym .NET Core i zestaw .NET Core SDK, [Zobacz informacje o programie .NET Core: Kompozycja](/dotnet/core/about#composition).

### <a name="packages"></a>Pakiety

Dołącz następujące pakiety NuGet, aby udostępnić funkcje automatycznego rejestrowania dla aplikacji wdrożonych w Azure App Service:

* [Microsoft. AspNetCore. AzureAppServices. HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) , aby zapewnić ASP.NET Coreą integrację z Azure App Service. Dodane funkcje rejestrowania są udostępniane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakiet.
* [Microsoft. AspNetCore. AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) , aby dodać Azure App Service `Microsoft.Extensions.Logging.AzureAppServices` dostawców rejestrowania diagnostyki w pakiecie.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) udostępnia implementacje rejestratorów umożliwiające obsługę dzienników diagnostyki Azure App Service i funkcji przesyłania strumieniowego dzienników.

Poprzednie pakiety nie są dostępne w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Aplikacje, które są przeznaczone dla elementu `Microsoft.AspNetCore.App` .NET Framework lub odwołują się do pakietu, muszą jawnie odwoływać się do poszczególnych pakietów w pliku projektu aplikacji.

## <a name="override-app-configuration-using-the-azure-portal"></a>Przesłoń konfigurację aplikacji przy użyciu witryny Azure Portal

Ustawienia aplikacji w witrynie Azure Portal umożliwiają ustawianie zmiennych środowiskowych dla aplikacji. Zmienne środowiskowe mogą być używane przez [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Po utworzeniu lub zmodyfikowaniu ustawienia aplikacji w witrynie Azure Portal i wybraniu przycisku **Zapisz** aplikacja platformy Azure zostanie uruchomiona ponownie. Zmienna środowiskowa jest dostępna dla aplikacji po ponownym uruchomieniu usługi.

::: moniker range=">= aspnetcore-3.0"

Gdy aplikacja korzysta z [hosta ogólnego](xref:fundamentals/host/generic-host), zmienne środowiskowe nie są domyślnie ładowane do konfiguracji aplikacji, a dostawca konfiguracji musi zostać dodany przez dewelopera. Deweloper Określa prefiks zmiennej środowiskowej podczas dodawania dostawcy konfiguracji. Aby uzyskać więcej informacji, <xref:fundamentals/host/generic-host> Zobacz i [dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Gdy aplikacja kompiluje hosta za pomocą elementu [webhost. CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), zmienne środowiskowe, które konfigurują `ASPNETCORE_` hosta, używają prefiksu. Aby uzyskać więcej informacji, <xref:fundamentals/host/web-host> Zobacz i [dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

[Oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), które konfiguruje przekazane nagłówki oprogramowania pośredniczącego podczas hostingu [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), a moduł ASP.NET Core jest skonfigurowany do przesyłania dalej schematu (http/https) i zdalnego adresu IP, z którego pochodzi żądanie . Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitorowanie i rejestrowanie

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core aplikacje wdrożone do App Service automatycznie otrzymują rozszerzenie App Service **ASP.NET Core integrację rejestrowania**. Rozszerzenie włącza integrację rejestrowania dla aplikacji ASP.NET Core w Azure App Service.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core aplikacje wdrożone do App Service automatycznie otrzymują rozszerzenia App Service **ASP.NET Core rejestrowania rozszerzeń**. Rozszerzenie włącza integrację rejestrowania dla aplikacji ASP.NET Core w Azure App Service.

::: moniker-end

Informacje o monitorowaniu, rejestrowaniu i rozwiązywaniu problemów znajdują się w następujących artykułach:

[Monitorowanie aplikacji w Azure App Service](/azure/app-service/web-sites-monitor)  
Dowiedz się, jak przeglądać limity przydziału i metryki dla aplikacji i planów App Service.

[Włączanie rejestrowania diagnostycznego dla aplikacji w Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Odkryj, jak włączyć i uzyskać dostęp do rejestrowania diagnostycznego dla kodów stanu HTTP, żądań zakończonych niepowodzeniem i aktywności serwera sieci Web.

<xref:fundamentals/error-handling>  
Poznaj typowe podejścia do obsługi błędów w aplikacjach ASP.NET Core.

<xref:test/troubleshoot-azure-iis>  
Dowiedz się, jak zdiagnozować problemy z wdrożeniami Azure App Service przy użyciu aplikacji ASP.NET Core.

<xref:host-and-deploy/azure-iis-errors-reference>  
Zapoznaj się z informacjami o typowych problemach z konfiguracją wdrażania dla aplikacji hostowanych przez Azure App Service/usług IIS.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Pierścień i miejsca wdrożenia klucza ochrony danych

[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) są utrwalane w folderze *%Home%\ASP.NET\DataProtection-Keys* . Ten folder jest obsługiwany przez magazyn sieciowy i synchronizowany na wszystkich komputerach obsługujących aplikację. Klucze nie są chronione w stanie spoczynku. Ten folder dostarcza pierścień kluczy do wszystkich wystąpień aplikacji w jednym miejscu wdrożenia. Oddzielne miejsca wdrożenia, takie jak przygotowanie i środowisko produkcyjne, nie dzielą się z kluczem.

W przypadku wymiany między miejscami wdrożenia każdy system korzystający z ochrony danych nie będzie w stanie odszyfrować przechowywanych danych przy użyciu dzwonka klucza w poprzednim gnieździe. Oprogramowanie pośredniczące ASP.NET plików cookie używa ochrony danych do ochrony plików cookie. Prowadzi to do użytkowników wylogowanych z aplikacji korzystającej ze standardowego oprogramowania ASP.NET cookie. W przypadku rozwiązania z niezależnym dzwonkiem, należy użyć zewnętrznego dostawcy usługi Key dystynktywnego, takiego jak:

* Azure Blob Storage
* W usłudze Azure Key Vault
* Sklep SQL
* Pamięć podręczna Redis

Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/implementation/key-storage-providers>.
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>
<!-- revert this after 3.0 supported
## Deploy ASP.NET Core preview release to Azure App Service

Use one of the following approaches if the app relies on a preview release of .NET Core:

* [Install the preview site extension](#install-the-preview-site-extension).
* [Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).
* [Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).
-->
## <a name="deploy-aspnet-core-30-to-azure-app-service"></a>Wdróż ASP.NET Core 3,0 do Azure App Service

Mamy nadzieję, że na Azure App Service wkrótce będzie dostępna ASP.NET Core 3,0.

Jeśli aplikacja korzysta z platformy .NET Core 3,0, należy użyć jednej z następujących metod:

* [Zainstaluj rozszerzenie witryny w wersji](#install-the-preview-site-extension)zapoznawczej.
* [Wdróż samodzielną aplikację w wersji](#deploy-a-self-contained-preview-app)zapoznawczej.
* [Użyj platformy Docker z Web Apps dla kontenerów](#use-docker-with-web-apps-for-containers).

### <a name="install-the-preview-site-extension"></a>Zainstaluj rozszerzenie witryny w wersji zapoznawczej

Jeśli wystąpi problem przy użyciu rozszerzenia witryny w wersji zapoznawczej, Otwórz [problem ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/issues).

1. W witrynie Azure Portal przejdź do App Service.
1. Wybierz aplikację sieci Web.
1. Wpisz "ex" w polu wyszukiwania, aby odfiltrować "rozszerzenia", lub przewiń w dół listy narzędzi do zarządzania.
1. Wybierz pozycję **rozszerzenia**.
1. Wybierz pozycję **Dodaj**.
1. ASP.NET Core wybierz z listy rozszerzenie **środowiska uruchomieniowego {X. Y} ({x64 | x86})** , na `{X.Y}` którym jest ASP.NET Core wersja zapoznawcza i `{x64|x86}` Określa platformę.
1. Wybierz **przycisk OK** , aby zaakceptować postanowienia prawne.
1. Wybierz **przycisk OK** , aby zainstalować rozszerzenie.

Po zakończeniu operacji zostanie zainstalowana najnowsza wersja programu .NET Core Preview. Sprawdź instalację:

1. Wybierz pozycję **Narzędzia zaawansowane**.
1. Wybierz pozycję **Przejdź** do **zaawansowanych narzędzi**.
1. Wybierz element menu**programu PowerShell** **konsoli** > debugowania.
1. W wierszu polecenia programu PowerShell wykonaj następujące polecenie. Zastąp wersję środowiska uruchomieniowego `{X.Y}` ASP.NET Core dla programu i `{PLATFORM}` platformy dla programu w ramach polecenia:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   Polecenie zwraca wartość `True` , gdy jest zainstalowane środowisko uruchomieniowe x64 w wersji zapoznawczej.

> [!NOTE]
> Architektura platformy (x86/x64) aplikacji App Services jest ustawiana w ustawieniach aplikacji w witrynie Azure Portal dla aplikacji hostowanych w ramach obliczeń serii A lub lepszej warstwy hostingu. Jeśli aplikacja jest uruchamiana w trybie w procesie i architektura platformy jest skonfigurowana pod kątem 64-bitowej (x64), moduł ASP.NET Core używa 64-bitowego środowiska uruchomieniowego w wersji zapoznawczej, jeśli jest obecny. Zainstaluj rozszerzenie **środowiska uruchomieniowego ASP.NET Core {X. Y} (x64)** .
>
> Po zainstalowaniu środowiska uruchomieniowego w wersji zapoznawczej x64 Uruchom następujące polecenie w oknie poleceń programu PowerShell kudu, aby zweryfikować instalację. Zastąp wersję środowiska uruchomieniowego `{X.Y}` ASP.NET Core dla polecenia:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> Polecenie zwraca wartość `True` , gdy jest zainstalowane środowisko uruchomieniowe x64 w wersji zapoznawczej.

> [!NOTE]
> **Rozszerzenia ASP.NET Core** umożliwiają korzystanie z dodatkowych funkcji ASP.NET Core na platformie Azure App Services, takich jak Włączanie rejestrowania na platformie Azure. Rozszerzenie jest instalowane automatycznie podczas wdrażania z programu Visual Studio. Jeśli rozszerzenie nie jest zainstalowane, zainstaluj je dla aplikacji.

**Używanie rozszerzenia witryny w wersji zapoznawczej z szablonem ARM**

Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typ zasobu może służyć do dodawania rozszerzenia witryny do aplikacji sieci Web. Na przykład:

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a>Wdrażanie samodzielnej aplikacji w wersji zapoznawczej

[Wdrożenie samodzielne (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) , które jest przeznaczone dla środowiska uruchomieniowego w wersji zapoznawczej, przenosi środowisko uruchomieniowe w wersji zapoznawczej w ramach wdrożenia.

Podczas wdrażania aplikacji samodzielnej:

* Lokacja w Azure App Service nie wymaga [rozszerzenia witryny w wersji](#install-the-preview-site-extension)zapoznawczej.
* Aplikacja musi zostać opublikowana przy użyciu innego podejścia niż w przypadku publikowania dla [wdrożenia zależnego od platformy (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).

Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Wdróż aplikację](#deploy-the-app-self-contained) samodzielną.

### <a name="use-docker-with-web-apps-for-containers"></a>Korzystanie z platformy Docker z Web Apps dla kontenerów

[Centrum platformy Docker](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy platformy Docker w wersji zapoznawczej. Obrazy mogą być używane jako obraz podstawowy. Użyj obrazu i Wdróż go w celu zapewnienia normalnej Web Apps kontenerów.

## <a name="publish-and-deploy-the-app"></a>Publikowanie i wdrażanie aplikacji

### <a name="deploy-the-app-framework-dependent"></a>Wdróż aplikację zależną od platformy

::: moniker range=">= aspnetcore-2.2"

W przypadku wdrożenia 64-bitowego [zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

* Aby utworzyć aplikację 64-bitową, użyj 64-bitowej zestaw .NET Core SDK.
* Ustaw **platformę** na **64 bit** w**ustawieniach ogólnych** **konfiguracji** > App Service. Aby umożliwić wybór liczby bitów platformy, aplikacja musi korzystać z planu usługi w warstwie Podstawowa lub wyższa.

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz pozycję **kompilacja** > **Opublikuj {nazwa aplikacji}** na pasku narzędzi programu Visual Studio lub kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.
1. W oknie dialogowym **Wybieranie elementu docelowego publikowania** upewnij się, że **App Service** jest zaznaczone.
1. Wybierz pozycję **Zaawansowane**. Zostanie otwarte okno dialogowe **Publikowanie** .
1. W **Publikuj** okno dialogowe:
   * Upewnij się, że wybrano konfigurację **wydania** .
   * Otwórz listę rozwijaną **tryb wdrażania** i wybierz pozycję **zależne od struktury**.
   * Wybierz **przenośne** jako **docelowe środowisko uruchomieniowe**.
   * Jeśli konieczne jest usunięcie dodatkowych plików po wdrożeniu, Otwórz **Opcje publikowania plików** i zaznacz pole wyboru w celu usunięcia dodatkowych plików w miejscu docelowym.
   * Wybierz pozycję **Zapisz**.
1. Utwórz nową lokację lub zaktualizuj istniejącą witrynę, postępując zgodnie z pozostałymi instrukcjami Kreatora publikacji.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. W pliku projektu nie określaj [identyfikatora środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog).

1. W powłoce poleceń Opublikuj aplikację w konfiguracji wydania przy użyciu polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) . W poniższym przykładzie aplikacja jest publikowana jako aplikacja zależna od platformy:

   ```console
   dotnet publish --configuration Release
   ```

1. Przenieś zawartość katalogu *bin/Release/{Target Framework}/Publish* do lokacji programu w App Service. Jeśli przeciągasz zawartość folderu *publikowania* z lokalnego dysku twardego lub udziału sieciowego bezpośrednio do App Service w konsoli [kudu](https://github.com/projectkudu/kudu/wiki) , przeciągnij `D:\home\site\wwwroot` pliki do folderu w konsoli kudu.

---

### <a name="deploy-the-app-self-contained"></a>Wdróż aplikację samodzielną

Użyj programu Visual Studio lub narzędzi interfejsu wiersza polecenia (CLI) dla samodzielnego [wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz pozycję **kompilacja** > **Opublikuj {nazwa aplikacji}** na pasku narzędzi programu Visual Studio lub kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.
1. W oknie dialogowym **Wybieranie elementu docelowego publikowania** upewnij się, że **App Service** jest zaznaczone.
1. Wybierz pozycję **Zaawansowane**. Zostanie otwarte okno dialogowe **Publikowanie** .
1. W **Publikuj** okno dialogowe:
   * Upewnij się, że wybrano konfigurację **wydania** .
   * Otwórz listę rozwijaną **tryb wdrażania** i wybierz pozycję samodzielny.
   * Wybierz docelowe środowisko uruchomieniowe z listy rozwijanej **docelowy środowisko uruchomieniowe** . Wartość domyślna to `win-x86`.
   * Jeśli konieczne jest usunięcie dodatkowych plików po wdrożeniu, Otwórz **Opcje publikowania plików** i zaznacz pole wyboru w celu usunięcia dodatkowych plików w miejscu docelowym.
   * Wybierz pozycję **Zapisz**.
1. Utwórz nową lokację lub zaktualizuj istniejącą witrynę, postępując zgodnie z pozostałymi instrukcjami Kreatora publikacji.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. W pliku projektu Określ jeden lub więcej [identyfikatorów środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog). Użyj `<RuntimeIdentifier>` (pojedyncze) dla pojedynczego identyfikatora RID lub Użyj `<RuntimeIdentifiers>` (plural), aby podać listę identyfikatorów RID rozdzielonych średnikami. W poniższym przykładzie `win-x86` określono identyfikator RID:

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. W powłoce poleceń Opublikuj aplikację w konfiguracji wydania dla środowiska uruchomieniowego hosta za pomocą polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) . W poniższym przykładzie aplikacja jest publikowana dla `win-x86` identyfikatora RID. Identyfikator RID dostarczony do `--runtime` opcji musi być podany `<RuntimeIdentifier>` we właściwości (lub `<RuntimeIdentifiers>`) w pliku projektu.

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. Przenieś zawartość katalogu *bin/Release/{Target Framework}/{Runtime identyfikator}/Publish* do lokacji w App Service. Jeśli przeciągasz zawartość folderu *publikowania* z lokalnego dysku twardego lub udziału sieciowego bezpośrednio do App Service w konsoli kudu, przeciągnij pliki do `D:\home\site\wwwroot` folderu w konsoli kudu.

---

## <a name="protocol-settings-https"></a>Ustawienia protokołu (HTTPS)

Bezpieczne powiązania protokołów pozwalają określić certyfikat, który ma być używany podczas odpowiadania na żądania za pośrednictwem protokołu HTTPS. Powiązanie wymaga prawidłowego certyfikatu prywatnego (*PFX*) dla określonej nazwy hosta. Aby uzyskać więcej informacji, [zobacz Samouczek: Powiąż istniejący niestandardowy certyfikat SSL z Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).

## <a name="transform-webconfig"></a>Przekształcanie pliku web.config

Jeśli musisz przekształcić *plik Web. config* przy publikowaniu (na przykład ustawić zmienne środowiskowe na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Przegląd App Service](/azure/app-service/app-service-web-overview)
* [Azure App Service: Najlepsze miejsce do hostowania aplikacji .NET (wideo z omówieniem 55 minut)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Piątek Azure: Azure App Service środowisko diagnostyczne i rozwiązywania problemów (wideo 12-minutowy)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Omówienie diagnostyki Azure App Service](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Azure App Service w systemie Windows Server używa [Internet Information Services (IIS)](https://www.iis.net/). Poniższe tematy dotyczą podstawowej technologii usług IIS:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [System Windows Server — zawartość administratora IT dla bieżących i poprzednich wersji](/windows-server/windows-server-versions)
