---
title: Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Ten artykuł zawiera linki do hosta platformy Azure i wdrażanie zasobów.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: a70ca46da633161083c1860aca92242d47d5e7c7
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454768"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service

[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) jest [chmury obliczeniowej platformy usługi firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, łącznie z platformą ASP.NET Core.

## <a name="useful-resources"></a>Przydatne zasoby

Azure [dokumentację usługi Web Apps](/azure/app-service/) to miejsce, dokumentacja, samouczki, przykłady, przewodniki z instrukcjami i inne zasoby aplikacji platformy Azure. Są dwa istotne samouczków, które odnoszą się do hostowania aplikacji platformy ASP.NET Core:

[Szybki Start: Tworzenie aplikacji internetowej platformy ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Visual Studio umożliwia tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w Windows.

[Szybki Start: Tworzenie aplikacji sieci web platformy .NET Core w usłudze App Service w systemie Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w systemie Linux, należy użyć wiersza polecenia.

Następujące artykuły są dostępne w dokumentacji platformy ASP.NET Core:

[Publikowanie na platformie Azure przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.

[Publikowanie na platformie Azure przy użyciu narzędzi interfejsu wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli)  
Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service za pomocą klienta wiersza polecenia usługi Git.

[Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Dowiedz się, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio, a następnie wdrożyć ją w usłudze Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.

[Tworzenie pierwszego potoku za pomocą potoków usługi Azure](/azure/devops/pipelines/get-started-yaml)  
Skonfiguruj kompilację o ciągłej integracji dla aplikacji ASP.NET Core, a następnie utworzyć wydanie ciągłe wdrażanie w usłudze Azure App Service.

[Usługa Azure piaskownica aplikacji sieci Web](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Dowiedz się, wykonywanie ograniczenia środowiska uruchomieniowego usługi Azure App Service, wymuszane przez platformę Azure Apps.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Konfiguracja aplikacji

W programie ASP.NET Core 2.0 lub nowszej następujące pakiety NuGet zapewniają funkcji automatycznego rejestrowania w przypadku aplikacji wdrożonych w usłudze Azure App Service:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) nad umożliwieniem integracji światła w górę platformy ASP.NET Core w usłudze Azure App Service. Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) dodać dostawców rejestrowania diagnostycznego usługi Azure App Service w `Microsoft.Extensions.Logging.AzureAppServices` pakietu.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) zawiera implementacje rejestratora do obsługi dzienników diagnostycznych usługi Azure App Service i przesyłanie strumieniowe funkcji dzienników.

Jeśli przeznaczonych dla platformy .NET Core i odwoływanie się do [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage), pakiety są już uwzględniane. Pakiety nie są spełnione z nowszego [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, odwołania się do pakietów indywidualne rejestrowanie.

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a>Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal

**Ustawienia aplikacji** obszaru **ustawienia aplikacji** bloku pozwala na Ustawianie zmiennych środowiskowych dla aplikacji. Zmienne środowiskowe mogą być używane przez [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Jeśli aplikacja używa [hosta sieci Web](xref:fundamentals/host/web-host) i kompilacje hosta, za pomocą [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), użyj zmiennych środowiskowych, które skonfigurować hosta `ASPNETCORE_` prefiks. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host> i [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Jeśli aplikacja używa [ogólnego hosta](xref:fundamentals/host/generic-host), zmienne środowiskowe nie są domyślnie ładowany do konfiguracji aplikacji i dostawcy konfiguracji muszą zostać dodane przez dewelopera. Deweloper Określa prefiks zmiennej środowiska, po dodaniu dostawcy konfiguracji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi IIS oprogramowania pośredniczącego integracji, który konfiguruje przekazany oprogramowania pośredniczącego nagłówków i modułu ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) i zdalny adres IP, skąd pochodzi żądanie. Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitorowanie i rejestrowanie

Monitorowanie, rejestrowanie i informacje dotyczące rozwiązywania problemów zobacz następujące artykuły:

[Porady: monitorowanie aplikacji w usłudze Azure App Service](/azure/app-service/web-sites-monitor)  
Dowiedz się, jak przeglądać limity przydziału i metryki, aby aplikacje i plany usługi App Service.

[Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Dowiedz się, jak włączyć i dostęp do rejestrowania diagnostycznego dla kodów stanu HTTP, żądań zakończonych niepowodzeniem i aktywności serwera sieci web.

[Wprowadzenie do obsługi błędów w platformy ASP.NET Core](xref:fundamentals/error-handling)  
Poznaj typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.

[Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Dowiedz się, jak diagnozować problemy z wdrożeniami w usłudze Azure App Service za pomocą aplikacji platformy ASP.NET Core.

[Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Zobacz typowych błędów konfiguracji wdrażania dla aplikacji hostowanych przez Azure App Service/IIS za pomocą poradę dotyczącą rozwiązywania problemów.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Pierścień klucz ochrony danych i miejsca wdrożenia

[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zostaną utrwalone w *%HOME%\ASP.NET\DataProtection-Keys* folderu. Ten folder jest wspierana przez sieć, Magazyn, które jest synchronizowane na wszystkich maszynach hostingu aplikacji. Klucze nie są chronione w stanie spoczynku. Ten folder zawiera pierścień klucza do wszystkich wystąpień aplikacji w gnieździe pojedynczego wdrożenia. Gniazda wdrażane pojedynczo, takich jak przejściowe i produkcyjne, nie udostępniaj klucza pierścień.

Po zamianie między miejscami wdrożenia, każdy system przy użyciu ochrony danych będzie możliwe do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza. Oprogramowaniu pośredniczącym pliku Cookie ASP.NET używa ochrony danych, aby chronić swoje pliki cookie. Prowadzi to do użytkowników, trwa wylogowanie z aplikacji, która używa standardowych oprogramowaniu pośredniczącym pliku Cookie ASP.NET. Jako rozwiązanie pierścień klucz niezależnie od miejsca należy użyć dostawcy zewnętrznego pierścienia klucz takiego jak:

* Azure Blob Storage
* Usługa Azure Key Vault
* Magazyn SQL
* Pamięć podręczna redis

Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service

Platforma ASP.NET Core w wersji zapoznawczej aplikacji można wdrożyć w usłudze Azure App Service przy użyciu następujących metod:

* [Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [Używać platformy Docker z funkcją Web Apps for containers](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a>Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)

Jeśli wystąpi problem, za pomocą rozszerzenia witryny (wersja zapoznawcza), otwórz problem w [GitHub](https://github.com/aspnet/azureintegration/issues/new).

1. W witrynie Azure portal przejdź do bloku usługi App Service.
1. Wybierz aplikację sieci web.
1. Typ "ex" w polu wyszukiwania lub przewiń w dół na liście zarządzania sekcji, aby **narzędzia PROGRAMISTYCZNE**.
1. Wybierz **narzędzia PROGRAMISTYCZNE** > **rozszerzenia**.
1. Wybierz **Dodaj**.
1. Wybierz **platformy ASP.NET Core &lt;x.y&gt; (x86) środowiska uruchomieniowego** rozszerzenia z listy, gdzie `<x.y>` jest w wersji zapoznawczej platformy ASP.NET Core (na przykład **środowiska uruchomieniowego platformy ASP.NET Core 2.2 (x86)**). X86 środowiska uruchomieniowego jest odpowiednia dla [wdrożeń zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) opierają się na spoza procesu hostingu przez modułu ASP.NET Core.
1. Wybierz **OK** aby zaakceptować warunki prawne.
1. Wybierz **OK** można zainstalować rozszerzenia.

Po zakończeniu tej operacji jest zainstalowana najnowsza wersja zapoznawcza platformy .NET Core. Zweryfikuj instalację:

1. Wybierz **zaawansowane narzędzia** w obszarze **narzędzia PROGRAMISTYCZNE**.
1. Wybierz **Przejdź** na **Narzędzia zaawansowane** bloku.
1. Wybierz **konsoli debugowania** > **PowerShell** elementu menu.
1. W wierszu polecenia programu PowerShell uruchom następujące polecenie. Zastąpiono wersję środowiska uruchomieniowego platformy ASP.NET Core dla `<x.y>` w poleceniu:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   Jeśli jest zainstalowany w wersji zapoznawczej środowiska uruchomieniowego dla platformy ASP.NET Core 2.2, polecenie to:
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   Polecenie zwraca `True` podczas x64 czas wykonywania (wersja zapoznawcza) jest zainstalowany.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Architektura platformy — x86/x64 64 aplikację usługi App Services znajduje się w **ustawienia aplikacji** bloku w obszarze **ustawienia ogólne** warstwy hostowanym w obliczeniowej serii A i lepsze hostingu aplikacji. Jeśli aplikacja jest uruchamiana w trybie w procesie, architektura platformy jest skonfigurowany dla 64-bitowych (x64) modułu ASP.NET Core używa środowiska uruchomieniowego 64-bitowych (wersja zapoznawcza), jeśli jest obecny. Zainstaluj **platformy ASP.NET Core &lt;x.y&gt; (x64) środowiska uruchomieniowego** rozszerzenia (na przykład **środowiska uruchomieniowego platformy ASP.NET Core 2.2 (x64)**).
>
> Po zainstalowaniu x64 podglądu środowiska uruchomieniowego, uruchom następujące polecenie w oknie wiersza polecenia programu PowerShell programu Kudu, aby zweryfikować instalację. Zastąpiono wersję środowiska uruchomieniowego platformy ASP.NET Core dla `<x.y>` w poleceniu:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> Jeśli jest zainstalowany w wersji zapoznawczej środowiska uruchomieniowego dla platformy ASP.NET Core 2.2, polecenie to:
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> Polecenie zwraca `True` podczas x64 czas wykonywania (wersja zapoznawcza) jest zainstalowany.

::: moniker-end

> [!NOTE]
> **ASP.NET Core rozszerzenia** zapewnia dodatkowe funkcje dla platformy ASP.NET Core w usłudze Azure App Services, takich jak włączenie rejestrowania platformy Azure. Rozszerzenie jest instalowana automatycznie podczas wdrażania w programie Visual Studio. Jeśli rozszerzenie nie jest zainstalowany, zainstaluj go dla aplikacji.

**Rozszerzenie witryny (wersja zapoznawcza) za pomocą szablonu usługi ARM**

Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typu zasobu może służyć do dodawania rozszerzenia witryny aplikacji sieci web. Na przykład:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>Używać platformy Docker z funkcją Web Apps for containers

[Usługi Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy platformy Docker w wersji zapoznawczej. Obrazy może służyć jako obrazu podstawowego. Przy użyciu obrazu i wdrażanie w usłudze Web Apps for Containers normalnie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Przegląd usługi Web Apps (5-minutowy klip wideo z omówieniem)](/azure/app-service/app-service-web-overview)
* [Usługa Azure App Service: Najlepsze miejsce do hostowania aplikacji .NET (55-minutowy klip wideo z omówieniem)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Diagnostyki usługi aplikacji Azure i środowisko rozwiązywania problemów (12-minutowy klip wideo)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Omówienie diagnostyki w usłudze Azure App Service](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Usługa Azure App Service w systemie Windows Server [Internet Information Services (IIS)](https://www.iis.net/). Poniższe tematy odnoszą się do podstawowych technologii usług IIS:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Biblioteki Microsoft TechNet: Systemu Windows Server](/windows-server/windows-server-versions)
