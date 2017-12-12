---
title: "Omówienie wdrożenia i hostingu - platformy ASP.NET Core"
author: tdykstra
description: "Omówienie sposobu konfigurowania środowisk hostingowych i wdrażanie aplikacji platformy ASP.NET Core dla nich."
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: 0de459128426c4d027606951592b1fe3fdd24fd9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>Omówienie wdrożenia i hostingu dla aplikacji platformy ASP.NET Core

Oto główne kroki należy wykonać w celu wdrożenia aplikacji platformy ASP.NET Core do środowiska macierzystego:

* Publikowanie aplikacji do folderu na serwerze hostingu.
* Konfigurowanie Menedżera procesu, który uruchomienia aplikacji, jeśli żądania są dostępne w i ponownie go uruchamia po jego awarii lub ponownym uruchomieniu serwera.
* Konfigurowanie zwrotny serwer proxy, który przesyła dalej żądań do aplikacji.

## <a name="publish-to-a-folder"></a>Publikowanie w folderze 

[Publikowania dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) interfejsu wiersza polecenia polecenie kompiluje kod aplikacji i kopiuje pliki potrzebne do uruchomienia aplikacji w *publikowania* folderu. Podczas wdrażania w programie Visual Studio `dotnet publish` kroku jest wykonywane automatycznie przed pliki są kopiowane do lokalizacji docelowej wdrożenia.

### <a name="folder-contents"></a>Zawartości folderu

*Publikowania* folder zawiera *.exe* i *.dll* plików dla aplikacji, jego zależności i opcjonalnie środowiska uruchomieniowego .NET.

Aplikacja .NET Core mogą być publikowane jako *niezależne* lub *zależne od framework*. Jeśli aplikacja jest autonomiczna, *.dll* pliki, które zawierają środowisko uruchomieniowe platformy .NET znajdują się w *publikowania* folderu.  Jeśli aplikacja jest zależne od framework, pliki środowiska uruchomieniowego .NET nie są uwzględniane, ponieważ aplikacja odwołuje się do wersji programu .NET, który jest zainstalowany na komputerze. Domyślny model wdrożenia jest zależny od framework. Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

Oprócz *.exe* i *.dll* pliki, *publikowania* folder dla aplikacji platformy ASP.NET Core zwykle zawiera pliki konfiguracji, zasoby statyczne i widoków MVC.  Aby uzyskać więcej informacji, zobacz [struktury katalogów](xref:hosting/directory-structure).

## <a name="set-up-a-process-manager"></a>Konfigurowanie Menedżera procesu

Aplikacja platformy ASP.NET Core jest aplikacji konsoli, która ma być uruchamiana, gdy serwer jest uruchamiany i ponownie uruchomiony po awarii. Aby zautomatyzować uruchamia i ponowne uruchomienie komputera należy Menedżera procesu. Najbardziej typowe menedżerowie procesu dla platformy ASP.NET Core to [Nginx](xref:publishing/linuxproduction) i [Apache](xref:publishing/apache-proxy) w systemie Linux i [IIS](xref:publishing/iis) i [usługi systemu Windows](xref:hosting/windows-service) w systemie Windows.

## <a name="set-up-a-reverse-proxy"></a>Konfigurowanie zwrotny serwer proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web, można użyć [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), lub [IIS](xref:publishing/iis) jako zwrotny serwer proxy serwera. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi. Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli aplikacja korzysta z [Kestrel](xref:fundamentals/servers/kestrel) serwer sieci web i mają być widoczne z Internetem, możesz użyć [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), lub [IIS](xref:publishing/iis) jako reverse Serwer proxy. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi. Głównym celem przy użyciu zwrotnego serwera proxy jest zabezpieczeń. Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Przy użyciu programu Visual Studio i MSBuild do automatyzowania wdrażania

Często wdrożenie wymaga dodatkowych zadań poza kopiowanie dane wyjściowe z `dotnet publish` na serwerze. Na przykład można dołączać dodatkowe pliki w *publikowania* folderu lub wyklucz pliki z niego. Visual Studio będzie korzystać program MSBuild wdrożenia sieci web i można dostosować MSBuild do wykonywania wielu innych zadań podczas wdrażania. Aby uzyskać więcej informacji, zobacz [profilów publikowania w programie Visual Studio](xref:publishing/web-publishing-vs) i [przy użyciu programu MSBuild i Team Foundation Build](http://msbuildbook.com/) książki.

Można wdrożyć bezpośrednio z programu Visual Studio do usługi Azure App Service przy użyciu [funkcja Publikowanie w sieci Web](xref:tutorials/publish-to-azure-webapp-using-vs) lub za pomocą [wbudowana obsługa Git](xref:publishing/azure-continuous-deployment). Visual Studio Team Services obsługuje [ciągłe wdrażanie w usłudze Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).

## <a name="publishing-to-azure"></a>Publikowanie na platformie Azure

Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania tej aplikacji na platformie Azure przy użyciu programu Visual Studio.  Aplikację można także publikować na platformie Azure z [wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby dowiedzieć się, jak przy użyciu rozwiązania Docker jako środowisko macierzyste, zobacz [aplikacji hosta platformy ASP.NET Core w Docker](xref:publishing/docker).
