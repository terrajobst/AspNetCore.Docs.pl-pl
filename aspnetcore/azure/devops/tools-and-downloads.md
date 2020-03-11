---
title: Narzędzia i pliki do pobrania — metodyki DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Narzędzia i pliki do pobrania wymagane dla metodyki DevOps z platformą ASP.NET Core i platformy Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 25ce311373b0aaddfa3bc2728c39e503acbca69d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659489"
---
# <a name="tools-and-downloads"></a>Narzędzia i pliki do pobrania

Platforma Azure ma kilka interfejsów do aprowizacji zasobów i zarządzania nimi, takich jak [Azure Portal](https://portal.azure.com), [interfejsu wiersza polecenia platformy Azure](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)i programu Visual Studio. Ten przewodnik minimalistyczny podejście i korzysta z usługi Azure Cloud Shell zawsze, gdy jest to możliwe zmniejszenie kroki wymagane. Jednak należy użyć witryny Azure portal dla niektórych części.

## <a name="prerequisites"></a>Wymagania wstępne

Wymagane jest spełnienie następujących subskrypcji:

* Usługa Azure &mdash;, jeśli nie masz konta, [Uzyskaj bezpłatną wersję próbną](https://azure.microsoft.com/free/).
* Azure DevOps Services &mdash; subskrypcję usługi Azure DevOps i organizacja została utworzona w rozdziale 4.
* GitHub &mdash;, jeśli nie masz konta, [zarejestruj się bezpłatnie](https://github.com/join).

Wymagane są następujące narzędzia:

* Narzędzie [git](https://git-scm.com/downloads) &mdash; w tym przewodniku zalecamy podstawowe zrozumienie usługi git. Przejrzyj [dokumentację usługi git](https://git-scm.com/doc), w tym [narzędzia Git Remote](https://git-scm.com/docs/git-remote) i [git push](https://git-scm.com/docs/git-push).
* Do kompilowania i uruchamiania przykładowej aplikacji wymagane jest [zestaw .NET Core SDK](https://www.microsoft.com/net/download/) &mdash; w wersji 2.1.300 lub nowszej. Jeśli program Visual Studio jest zainstalowany z obciążeniem **programistycznym dla wielu platform .NET Core** , zestaw .NET Core SDK jest już zainstalowany.

    Zweryfikuj instalację zestawu .NET Core SDK. Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Zalecanych narzędzi (tylko Windows)

* Zaawansowane narzędzia platformy Azure dla [programu Visual Studio](https://visualstudio.microsoft.com)zapewniają graficzny interfejs użytkownika dla większości funkcji opisanych w tym przewodniku. W każdej wersji programu Visual Studio będzie działać, łącznie z bezpłatnego programu Visual Studio Community Edition. Samouczki są zapisywane do zademonstrowania programowania, wdrażania i metodyki DevOps z usługą i bez programu Visual Studio.

  Upewnij się, że program Visual Studio ma zainstalowane następujące [obciążenia](/visualstudio/install/modify-visual-studio) :

  * Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych
  * Tworzenie aplikacji na platformie Azure
  * Tworzenie aplikacji dla wielu platform w środowisku .NET Core
