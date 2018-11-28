---
title: Hostowanie i wdrażanie platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować środowiskach hostingu i wdrażanie aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/index
ms.openlocfilehash: f70b05df6bf710e2ab54a1eaafb71b4f9b306cbe
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450622"
---
# <a name="host-and-deploy-aspnet-core"></a>Hostowanie i wdrażanie platformy ASP.NET Core

Ogólnie rzecz biorąc, aby wdrożyć aplikację ASP.NET Core w środowisku hostingu:

* Opublikuj aplikację w folderze na serwerze hostingu.
* Skonfiguruj menedżera procesów, które uruchamia aplikację podczas żądania, odbierania i ponownie uruchamia aplikację po jej awarii lub ponownym uruchomieniu serwera.
* W razie potrzeby konfiguracji zwrotny serwer proxy należy skonfigurować zwrotny serwer proxy, który przekazuje żądania do aplikacji.

## <a name="publish-to-a-folder"></a>Publikowanie w folderze

[Publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) interfejsu wiersza polecenia kompiluje kod aplikacji i kopiuje pliki potrzebne do uruchomienia aplikacji do *publikowania* folderu. W przypadku wdrażania w programie Visual Studio, [publikowania dotnet](/dotnet/core/tools/dotnet-publish) kroku odbywa się automatycznie, zanim pliki są kopiowane do lokalizacji docelowej wdrożenia.

### <a name="folder-contents"></a>Zawartość folderu

*Publikowania* folder zawiera *.exe* i *.dll* plików dla aplikacji, jego zależności i, opcjonalnie, środowisko uruchomieniowe platformy .NET.

Aplikacja platformy .NET Core mogą być publikowane jako *niezależna* lub *zależny od struktury* aplikacji. Jeśli aplikacja znajduje się samodzielna *.dll* pliki, które zawierają środowisko uruchomieniowe platformy .NET znajdują się w *publikowania* folderu. Jeśli aplikacja jest zależny od struktury, pliki środowiska uruchomieniowego .NET nie są uwzględniane ponieważ aplikacja odwołuje się do wersji platformy .NET, który jest zainstalowany na serwerze. Domyślny model wdrożenia jest zależny od struktury. Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji .NET Core](/dotnet/articles/core/deploying/index).

Oprócz *.exe* i *.dll* plików *publikowania* folder dla aplikacji ASP.NET Core zwykle zawiera pliki konfiguracyjne, statycznych zasobów i widoków MVC. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/directory-structure>.

## <a name="set-up-a-process-manager"></a>Konfigurowanie menedżera procesów

Aplikacja platformy ASP.NET Core to aplikacja konsolowa, która musi zostać uruchomione, gdy serwer jest uruchamiany i uruchamiane ponownie, jeśli jego awarii. Aby zautomatyzować rozpoczyna się i ponownego uruchomienia, menedżera procesów jest wymagana. Najbardziej typowe menedżerów procesu dla platformy ASP.NET Core to:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Usługa Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Konfigurowanie zwrotnego serwera proxy

::: moniker range=">= aspnetcore-2.0"

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), lub [IIS](xref:host-and-deploy/iis/index) mogą być używane jako zwrotnego serwera proxy. Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi.

Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje. Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwer sieci web i będzie łączyć się z Internetem, użyj [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), lub [IIS](xref:host-and-deploy/iis/index) jako zwrotny serwer proxy serwera. Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi. Głównym powodem przy użyciu zwrotnego serwera proxy jest zabezpieczeń. Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia. Bez dodatkowej konfiguracji aplikacji nie mieć dostępu do schemat (HTTP/HTTPS) i adres IP zdalnego skąd pochodzi żądanie. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>Umożliwia zautomatyzowanie wdrożeń programu Visual Studio i MSBuild

Wdrożenie często wymaga dodatkowych zadań, oprócz kopiowania danych wyjściowych [publikowania dotnet](/dotnet/core/tools/dotnet-publish) do serwera. Na przykład może być wymagane lub wykluczone z dodatkowych plików *publikowania* folderu. Program Visual Studio używa MSBuild dla wdrażania w Internecie i MSBuild można dostosować do wykonywania wielu innych zadań podczas wdrażania. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/visual-studio-publish-profiles> i [przy użyciu programu MSBuild i Team Foundation Build](http://msbuildbook.com/) książki.

Za pomocą [funkcji publikowania w sieci Web](xref:tutorials/publish-to-azure-webapp-using-vs) lub [wbudowana obsługa Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), aplikacje można wdrożyć bezpośrednio z programu Visual Studio w usłudze Azure App Service. Obsługuje usługi Azure DevOps [ciągłe wdrażanie w usłudze Azure App Service](/azure/devops/pipelines/targets/webapp).

## <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Zobacz <xref:tutorials/publish-to-azure-webapp-using-vs> instrukcje dotyczące sposobu publikowania aplikacji za pomocą programu Visual Studio. Można także publikować aplikacji na platformie Azure z [wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="host-in-a-web-farm"></a>Hosting w ramach farmy sieci web

Instrukcje dotyczące konfiguracji do hostowania aplikacji platformy ASP.NET Core w środowisku farmy sieci web (na przykład wdrożenie wielu wystąpień aplikacji w przypadku skalowalności), zobacz <xref:host-and-deploy/web-farm>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
