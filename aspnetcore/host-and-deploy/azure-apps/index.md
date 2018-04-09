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
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
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

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) do zapewniania platformy ASP.NET Core lightup integracji z usługi aplikacji Azure. Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.
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

* [Zainstaluj rozszerzenie lokacji wersji zapoznawczej](#site-x)
* [Wdróż aplikację samodzielnie zawarte](#self)
* [Użyj Docker z aplikacjami sieci Web dla kontenerów](#docker)

Jeśli masz problem przy użyciu rozszerzenia lokacji wersji zapoznawczej, otwórz problemu na [GitHub](https://github.com/aspnet/azureintegration/issues/new).

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a>Zainstaluj rozszerzenie lokacji wersji zapoznawczej

* W portalu Azure przejdź do bloku usługi aplikacji.
* Wprowadź "ex" w polu wyszukiwania.
* Wybierz **rozszerzenia**.
* Wybierz opcję "Dodaj".

![Azure bloku aplikacji z poprzednich krokach](index/_static/x1.png)

* Wybierz **rozszerzenia środowiska uruchomieniowego platformy ASP.NET Core**.
* Wybierz **OK** > **OK**.

Po ukończeniu operacji dodawania zainstalowano najnowszej wersji zapoznawczej .NET Core 2.1. Instalację można zweryfikować, uruchamiając `dotnet --info` w konsoli. W bloku usługi aplikacji:

* Wprowadź "con", w polu wyszukiwania.
* Wybierz **konsoli**.
* Wprowadź `dotnet --info` w konsoli.

![Azure bloku aplikacji z poprzednich krokach](index/_static/cons.png)

Obraz poprzedniego były aktualne w momencie to zostało zapisane. Może pojawić się innej wersji.

`dotnet --info` Wyświetla ścieżkę do rozszerzenia lokacji, w których została zainstalowana wersja zapoznawcza. Pokazuje, aplikacja zostanie uruchomiona z rozszerzenia lokacji zamiast z domyślnej *ProgramFiles* lokalizacji. Jeśli widzisz *ProgramFiles*, należy ponownie uruchomić witrynę i uruchomić `dotnet --info`.

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a>Rozszerzenie lokacji wersji zapoznawczej za pomocą szablonu usługi ARM

Jeśli używasz szablonu usługi ARM do tworzenia i wdrażania aplikacji można użyć `siteextensions` typu zasobów, aby dodać do lokacji rozszerzenie aplikacji sieci Web. Na przykład:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a>Wdróż aplikację samodzielnie zawarte

Można wdrożyć [niezależne aplikacji](/dotnet/core/deploying/#self-contained-deployments-scd) która prowadzi środowiska wykonawczego w wersji zapoznawczej wraz z nim podczas wdrażania. W przypadku wdrażania aplikacji samodzielną:

* Nie trzeba przygotować witryny.
* Należy opublikować aplikację inaczej niż w przypadku wdrażania aplikacji, gdy zestaw SDK jest zainstalowany na serwerze.

Samodzielne aplikacje są opcję dla wszystkich aplikacji .NET Core.

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a>Użyj Docker z aplikacjami sieci Web dla kontenerów

[Centrum Docker](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy Docker 2.1 podglądu. Można ich używać jako obrazu podstawowego i wdrażania aplikacji sieci Web dla kontenerów w zwykły sposób.

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
