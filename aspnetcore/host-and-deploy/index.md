---
title: Host i wdrażania platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować środowiskach hostingu i wdrażanie aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 1ffc7f9f2dc2a06dddb629d2d2553964b56cec05
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Host i wdrażania platformy ASP.NET Core

W ogólności, aby wdrożyć aplikację ASP.NET Core do środowiska macierzystego:

* Publikowanie aplikacji do folderu na serwerze hostingu.
* Konfigurowanie Menedżera proces uruchamiany aplikacji, podczas żądania odbierania i ponowne uruchomienie aplikacji po jej awarii lub ponownym uruchomieniu serwera.
* W razie potrzeby konfiguracji zwrotny serwer proxy należy skonfigurować zwrotny serwer proxy, który przesyła dalej żądań do aplikacji.

## <a name="publish-to-a-folder"></a>Publikowanie w folderze

[Publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) interfejsu wiersza polecenia polecenie kompiluje kod aplikacji i kopiuje pliki potrzebne do uruchomienia aplikacji w *publikowania* folderu. W przypadku wdrażania w programie Visual Studio, [publikowania dotnet](/dotnet/core/tools/dotnet-publish) krok odbywa się automatycznie, zanim pliki są kopiowane do lokalizacji docelowej wdrożenia.

### <a name="folder-contents"></a>Zawartości folderu

*Publikowania* folder zawiera *.exe* i *.dll* plików dla aplikacji, jego zależności i opcjonalnie środowiska uruchomieniowego .NET.

Aplikacja .NET Core mogą być publikowane jako *niezależne* lub *zależne od framework* aplikacji. Jeśli aplikacja jest autonomiczna, *.dll* pliki, które zawierają środowisko uruchomieniowe platformy .NET znajdują się w *publikowania* folderu. Jeśli aplikacja jest zależne od framework, pliki środowiska uruchomieniowego .NET nie są uwzględniane ponieważ aplikacja odwołuje się do wersji programu .NET, który jest zainstalowany na serwerze. Domyślny model wdrożenia jest zależny od framework. Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji .NET Core](/dotnet/articles/core/deploying/index).

Oprócz *.exe* i *.dll* pliki, *publikowania* folder dla aplikacji platformy ASP.NET Core zwykle zawiera pliki konfiguracji, zasoby statyczne i widoków MVC. Aby uzyskać więcej informacji, zobacz [struktury katalogów](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Konfigurowanie Menedżera procesu

Aplikacja platformy ASP.NET Core jest aplikacji konsoli, która musi być uruchamiana, gdy serwer jest uruchamiany i w razie jego awarii uruchomione ponownie. Aby zautomatyzować uruchamia i ponowne uruchomienie komputera, Menedżer procesu jest wymagana. Najbardziej typowe menedżerów procesu ASP.NET Core są:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Usługa systemu Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Konfigurowanie zwrotny serwer proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), lub [IIS](xref:host-and-deploy/iis/index) mogą być używane jako zwrotnego serwera proxy. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

Albo konfiguracji&mdash;z lub bez zwrotnego serwera proxy&mdash;jest prawidłowa i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core w wersji 2.0 lub nowszej aplikacji. Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwer sieci web i będzie łączyć się z Internetem, użyj [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), lub [IIS](xref:host-and-deploy/iis/index) jako zwrotny serwer proxy serwera. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi. Głównym celem przy użyciu zwrotnego serwera proxy jest zabezpieczeń. Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy i moduły równoważenia obciążenia. Bez dodatkowej konfiguracji aplikacji może nie mieć dostępu do schemat (HTTP/HTTPS) i adres IP zdalnego którego pochodzi żądanie. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Przy użyciu programu Visual Studio i MSBuild do automatyzowania wdrażania

Często wdrożenie wymaga dodatkowych zadań poza kopiowanie dane wyjściowe z [publikowania dotnet](/dotnet/core/tools/dotnet-publish) na serwerze. Na przykład może być wymagane lub wykluczone z dodatkowych plików *publikowania* folderu. Visual Studio będzie korzystać program MSBuild wdrożenia sieci web i MSBuild można dostosować do wykonywania wielu innych zadań podczas wdrażania. Aby uzyskać więcej informacji, zobacz [profilów publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) i [przy użyciu programu MSBuild i Team Foundation Build](http://msbuildbook.com/) książki.

Za pomocą [funkcja Publikowanie w sieci Web](xref:tutorials/publish-to-azure-webapp-using-vs) lub [wbudowana obsługa Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), aplikacje można wdrożyć bezpośrednio z programu Visual Studio w usłudze Azure App Service. Visual Studio Team Services obsługuje [ciągłe wdrażanie w usłudze Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Publikowanie na platformie Azure

Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania aplikacji na platformie Azure przy użyciu programu Visual Studio. Aplikację można także publikować na platformie Azure z [wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Dodatkowe zasoby

Uzyskać przy użyciu rozwiązania Docker jako środowisko macierzyste, zobacz [aplikacji hosta platformy ASP.NET Core w Docker](xref:host-and-deploy/docker/index).
