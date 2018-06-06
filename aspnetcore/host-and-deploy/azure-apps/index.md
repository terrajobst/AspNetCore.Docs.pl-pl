---
title: Host platformy ASP.NET Core w usłudze aplikacji Azure
author: guardrex
description: Dowiedzieć się, jak udostępniać aplikacje platformy ASP.NET Core w usłudze Azure App Service z łączami do przydatnych zasobów.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 4cf81a3e269500a5108f280348fbddd172df10a0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687506"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Host platformy ASP.NET Core w usłudze aplikacji Azure

[Usługa aplikacji Azure](https://azure.microsoft.com/services/app-service/) jest [przetwarzania platformy usług w chmurze firmy Microsoft](https://azure.microsoft.com/) do obsługi aplikacji sieci web, łącznie z platformy ASP.NET Core.

## <a name="useful-resources"></a>Przydatne zasoby

Azure [dokumentacji aplikacji sieci Web](/azure/app-service/) jest stroną główną dla dokumentacja, samouczki, przykłady, przewodników instruktażowych dot i innych zasobów aplikacji Azure. Są dwa samouczki zauważalne, które odnoszą się do obsługi aplikacji platformy ASP.NET Core:

[Szybki Start: Tworzenie aplikacji sieci web platformy ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Tworzenie i wdrażanie aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service w systemie Windows za pomocą programu Visual Studio.

[Szybki Start: Tworzenie aplikacji sieci web platformy .NET Core w usłudze App Service w systemie Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Użyj wiersza polecenia do tworzenia i wdrażania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service w systemie Linux.

Są dostępne w dokumentacji platformy ASP.NET Core następujące artykuły:

[Publikowanie na platformie Azure przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.

[Publikowanie na platformie Azure przy użyciu narzędzi interfejsu wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli)  
Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu klienta wiersza polecenia Git.

[Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Informacje o sposobie tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w usłudze Azure App Service przy użyciu usługi Git do ciągłego wdrażania.

[Ciągłe wdrażanie na platformie Azure za pomocą usług VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Konfigurowanie kompilacji elementu konfiguracji dla aplikacji platformy ASP.NET Core, a następnie utworzyć wersję ciągłe wdrażanie w usłudze Azure App Service.

[Piaskownica aplikacji sieci Web platformy Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Wykryj ograniczenia wykonywania środowiska uruchomieniowego usługi Azure App Service wymuszane przez platformę aplikacji Azure.

## <a name="application-configuration"></a>Konfiguracja aplikacji

Z platformy ASP.NET Core 2.0 lub nowszego oraz trzy pakiety w programie [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) udostępnia funkcje automatycznego rejestrowania w przypadku aplikacji wdrożonych w usłudze Azure App Service:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) do zapewniania platformy ASP.NET Core lightup integracji z usługi aplikacji Azure. Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) dodawać dostawców rejestrowania diagnostyki Azure App Service w `Microsoft.Extensions.Logging.AzureAppServices` pakietu.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) zawiera implementacje rejestratora do obsługi dzienników diagnostyki Azure App Service i przesyłania strumieniowego funkcje dziennika.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

IIS integracji oprogramowania pośredniczącego, który konfiguruje przekazywane oprogramowanie pośredniczące nagłówki i moduł platformy ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) oraz adres IP zdalnego, którego pochodzi żądanie. Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy dodatkowe i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitorowanie i rejestrowanie

Monitorowanie, rejestrowanie i informacje dotyczące rozwiązywania problemów zobacz następujące artykuły:

[Porady: monitorować aplikacje w usłudze aplikacji Azure](/azure/app-service/web-sites-monitor)  
Dowiedz się, jak Przejrzyj przydziałów i metryki dla aplikacji i planów usługi aplikacji.

[Włączanie rejestrowania diagnostyki dla aplikacji sieci web w usłudze aplikacji Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Dowiedzieć się, jak włączyć i dostępu do diagnostyki rejestrowanie aktywności serwera sieci web, nieudanych żądań i kodów stanu HTTP.

[Wprowadzenie do obsługi błędów w platformy ASP.NET Core](xref:fundamentals/error-handling)  
Dowiedz się, typowe appoaches do obsługi błędów w aplikacji platformy ASP.NET Core.

[Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Dowiedz się, jak diagnozować problemy z wdrożenia usługi Azure App Service przy użyciu aplikacji platformy ASP.NET Core.

[Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Zobacz typowych błędów konfiguracji wdrożenia dla aplikacji hostowanych przez Azure App Service/IIS z porady dotyczące rozwiązywania problemów.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Pierścień klucza ochrony danych i miejsca wdrożenia

[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zostały utrwalone *%HOME%\ASP.NET\DataProtection-Keys* folderu. Ten folder nie jest obsługiwana przez sieć magazynu i jest synchronizowane na wszystkich komputerach hosting aplikacji. Klucze chronione nie są w stanie spoczynku. Ten folder zawiera pierścień klucza do wszystkich wystąpień aplikacji w miejscu pojedynczego wdrożenia. Miejsc oddzielne wdrożenia, takich jak przemieszczania i produkcji, nie należy współużytkować pierścień klucza.

Podczas wymiany między miejscami wdrożenia, każdy system przy użyciu ochrony danych będzie niemożliwe do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza. Oprogramowanie pośredniczące plików Cookie ASP.NET używa ochrony danych do ochrony jego plików cookie. Prowadzi to do użytkowników podpisywany z aplikacji, która używa standardowych oprogramowaniu pośredniczącym pliku Cookie ASP.NET. Jako rozwiązanie pierścień klucza niezależny od miejsca używa dostawcy zewnętrznego pierścień klucza, takich jak:

* Azure Blob Storage
* Usługi Azure Key Vault
* Magazynu SQL
* Pamięci podręcznej redis

Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Wdrażanie wersji zapoznawczej platformy ASP.NET Core w usłudze Azure App Service

Platformy ASP.NET Core Podgląd aplikacje można wdrożyć w usłudze Azure App Service z następujących metod:

* [Zainstaluj rozszerzenie lokacji wersji zapoznawczej](#install-the-preview-site-extension)
* [Wdróż aplikację autonomiczną](#deploy-the-app-self-contained)
* [Użyj Docker z aplikacjami sieci Web dla kontenerów](#use-docker-with-web-apps-for-containers)

Jeśli wystąpi problem przy użyciu rozszerzenia lokacji wersji zapoznawczej, otwórz problemu na [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Zainstaluj rozszerzenie lokacji wersji zapoznawczej

1. W portalu Azure przejdź do bloku usługi aplikacji.
1. Wybierz aplikację sieci web.
1. W polu wyszukiwania wprowadź "ex" lub przewiń w dół listy okienka zarządzania do **narzędzi PROGRAMISTYCZNYCH**.
1. Wybierz **narzędzi PROGRAMISTYCZNYCH** > **rozszerzenia**.
1. Wybierz **dodać**.

   ![Azure bloku aplikacji z poprzednich krokach](index/_static/x1.png)

1. Wybierz **rozszerzeń platformy ASP.NET Core**.
1. Wybierz **OK** zaakceptować postanowienia prawne.
1. Wybierz **OK** do zainstalowania rozszerzenia.

Po ukończeniu operacji dodawania, jest zainstalowana w najnowszej wersji zapoznawczej .NET Core. Weryfikowanie instalacji przez uruchomienie `dotnet --info` w konsoli. Z **usługi aplikacji** bloku:

1. Wprowadź "con" w polu wyszukiwania lub przewiń w dół listę okienka zarządzania do **narzędzi PROGRAMISTYCZNYCH**.
1. Wybierz **narzędzi PROGRAMISTYCZNYCH** > **konsoli**.
1. Wprowadź `dotnet --info` w konsoli.

Jeśli wersja `2.1.300-preview1-008174` jest najnowszej wersji zapoznawczej następujące dane wyjściowe są uzyskiwane przez uruchomienie `dotnet --info` w wierszu polecenia:

![Azure bloku aplikacji z poprzednich krokach](index/_static/cons.png)

Wersja platformy ASP.NET Core pokazano na powyższej ilustracji `2.1.300-preview1-008174`, jest przykładem. Najnowszej wersji wstępnej programu ASP.NET Core w czasie skonfigurowano rozszerzenie lokacji pojawia się podczas wykonywania `dotnet --info`.

`dotnet --info` Wyświetla ścieżkę do rozszerzenia lokacji, w których została zainstalowana wersja zapoznawcza. Pokazuje, aplikacja zostanie uruchomiona z rozszerzenia lokacji zamiast z domyślnej *ProgramFiles* lokalizacji. Jeśli widzisz *ProgramFiles*, należy ponownie uruchomić witrynę i uruchomić `dotnet --info`.

**Rozszerzenie lokacji wersji zapoznawczej za pomocą szablonu usługi ARM**

Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typ zasobu można dodać do lokacji rozszerzenie aplikacji sieci web. Na przykład:

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Wdróż aplikację autonomiczną

A [niezależne aplikacji](/dotnet/core/deploying/#self-contained-deployments-scd) można wdrożyć środowiska wykonawczego w wersji zapoznawczej która prowadzi we wdrożeniu. W przypadku wdrażania aplikacji niezależne:

* Witryny nie muszą być przygotowane.
* Aplikację należy opublikować inaczej niż podczas publikowania wdrożenia zależne od framework z udostępnionego środowiska uruchomieniowego i hosta na serwerze.

Samodzielne aplikacje są opcję dla wszystkich aplikacji platformy ASP.NET Core.

### <a name="use-docker-with-web-apps-for-containers"></a>Użyj Docker z aplikacjami sieci Web dla kontenerów

[Centrum Docker](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy usługi Docker w wersji preview. Obrazy mogą służyć jako obrazu podstawowego. Użyć obrazu i wdrażania aplikacji sieci Web dla kontenerów normalnie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Przegląd usługi Web Apps (5-minutowy klip wideo z omówieniem)](/azure/app-service/app-service-web-overview)
* [Usługa aplikacji Azure: Najlepiej umieścić na hoście aplikacji .NET (55-minutowy klip wideo z omówieniem)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Piątek z Azure: Diagnostycznych usługi aplikacji Azure i rozwiązywanie problemów z doświadczenia (12-minutowy film wideo)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Omówienie diagnostyki usługi aplikacji Azure](/azure/app-service/app-service-diagnostics)

Usługa aplikacji Azure w systemie Windows Server używa [Internet Information Services (IIS)](https://www.iis.net/). Poniższe tematy odnoszą się do podstawowych technologii usług IIS:

* [Host platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index)
* [Wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Moduły usług IIS z platformą ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Bibliotece Microsoft TechNet: Serwer systemu Windows](/windows-server/windows-server-versions)
