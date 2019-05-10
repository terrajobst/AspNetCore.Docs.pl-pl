---
title: Hostowanie i wdrażanie platformy ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować środowiskach hostingu i wdrażanie aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/index
ms.openlocfilehash: f443a8ee28a859b5075a8bb03016407af9a3ddb1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899260"
---
# <a name="host-and-deploy-aspnet-core"></a>Hostowanie i wdrażanie platformy ASP.NET Core

Ogólnie rzecz biorąc, aby wdrożyć aplikację ASP.NET Core w środowisku hostingu:

* Wdróż opublikowanej aplikacji do folderu na serwerze hostingu.
* Skonfiguruj menedżera procesów, które uruchamia aplikację podczas żądania, odbierania i ponownie uruchamia aplikację po jej awarii lub ponownym uruchomieniu serwera.
* Dla konfiguracji zwrotny serwer proxy skonfiguruj zwrotny serwer proxy do przekazywania żądań do aplikacji.

## <a name="publish-to-a-folder"></a>Publikowanie w folderze

[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie kompiluje kod aplikacji i kopiuje pliki wymagane do uruchamiania aplikacji w *publikowania* folderu. W przypadku wdrażania w programie Visual Studio, `dotnet publish` kroku odbywa się automatycznie przed pliki są kopiowane do lokalizacji docelowej wdrożenia.

### <a name="folder-contents"></a>Zawartość folderu

*Publikowania* folder zawiera jeden lub więcej plików zestawu aplikacji, zależności i, opcjonalnie, środowisko uruchomieniowe platformy .NET.

Aplikację platformy .NET Core mogą być publikowane jako *niezależna wdrożenia* lub *wdrożenia zależny od struktury*. Jeśli aplikacja jest niezależny, plików zestawów, które zawierają środowisko uruchomieniowe platformy .NET są uwzględnione w *publikowania* folderu. Jeśli aplikacja jest zależny od struktury, pliki środowiska uruchomieniowego .NET nie są uwzględniane ponieważ aplikacja odwołuje się do wersji platformy .NET, który jest zainstalowany na serwerze. Domyślny model wdrożenia jest zależny od struktury. Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).

Oprócz *.exe* i *.dll* plików *publikowania* folder dla aplikacji ASP.NET Core zwykle zawiera pliki konfiguracyjne, statycznych zasobów i widoków MVC. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/directory-structure>.

## <a name="set-up-a-process-manager"></a>Konfigurowanie menedżera procesów

Aplikacja platformy ASP.NET Core to aplikacja konsolowa, która musi zostać uruchomione, gdy serwer jest uruchamiany i uruchamiane ponownie, jeśli jego awarii. Aby zautomatyzować rozpoczyna się i ponownego uruchomienia, menedżera procesów jest wymagana. Najbardziej typowe menedżerów procesu dla platformy ASP.NET Core to:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows Service](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Konfigurowanie zwrotnego serwera proxy

::: moniker range=">= aspnetcore-2.0"

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwera [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), lub [IIS](xref:host-and-deploy/iis/index) mogą być używane jako zwrotnego serwera proxy. Serwer proxy odwrotnej odbiera żądania HTTP z Internetu i przekazuje je do Kestrel.

Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest obsługiwana konfiguracja hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje. Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwer i będą łączyć się z Internetem, użyj [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), lub [IIS](xref:host-and-deploy/iis/index) jako zwrotny serwer proxy serwera. Serwer proxy odwrotnej odbiera żądania HTTP z Internetu i przekazuje je do Kestrel. Głównym powodem przy użyciu zwrotnego serwera proxy jest zabezpieczeń. Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia. Bez dodatkowej konfiguracji aplikacji nie mieć dostępu do schemat (HTTP/HTTPS) i adres IP zdalnego skąd pochodzi żądanie. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>Umożliwia zautomatyzowanie wdrożeń programu Visual Studio i MSBuild

Wdrożenie często wymaga dodatkowych zadań, oprócz kopiowania danych wyjściowych [publikowania dotnet](/dotnet/core/tools/dotnet-publish) do serwera. Na przykład może być wymagane lub wykluczone z dodatkowych plików *publikowania* folderu. Program Visual Studio używa MSBuild dla wdrażania w Internecie i MSBuild można dostosować do wykonywania wielu innych zadań podczas wdrażania. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/visual-studio-publish-profiles> i [przy użyciu programu MSBuild i Team Foundation Build](http://msbuildbook.com/) książki.

Za pomocą [funkcji publikowania w sieci Web](xref:tutorials/publish-to-azure-webapp-using-vs) lub [wbudowana obsługa Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), aplikacje można wdrożyć bezpośrednio z programu Visual Studio w usłudze Azure App Service. Obsługuje usługi Azure DevOps [ciągłe wdrażanie w usłudze Azure App Service](/azure/devops/pipelines/targets/webapp). Aby uzyskać więcej informacji, zobacz [DevOps z platformą ASP.NET Core i platformy Azure](xref:azure/devops/index).

## <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Zobacz <xref:tutorials/publish-to-azure-webapp-using-vs> instrukcje dotyczące sposobu publikowania aplikacji za pomocą programu Visual Studio. Dodatkowe przykład odbywa się przy [tworzenie aplikacji internetowej platformy ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="publish-with-msdeploy-on-windows"></a>Publikowanie za pomocą narzędzia MSDeploy na Windows

Zobacz <xref:host-and-deploy/visual-studio-publish-profiles> dla profilu publikowania instrukcje dotyczące sposobu publikowania aplikacji za pomocą programu Visual Studio, łącznie z wiersza polecenia Windows przy użyciu [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) polecenia.

## <a name="host-in-a-web-farm"></a>Hosting w farmie internetowej

Instrukcje dotyczące konfiguracji do hostowania aplikacji platformy ASP.NET Core w środowisku farmy sieci web (na przykład wdrożenie wielu wystąpień aplikacji w przypadku skalowalności), zobacz <xref:host-and-deploy/web-farm>.

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a>Kontrole kondycji

Użyj kondycji Sprawdź w oprogramowaniu pośredniczącym, aby sprawdzać kondycję aplikacji i jego zależności. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/health-checks>.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
