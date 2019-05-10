---
title: Metodyka DevOps z platformami ASP.NET Core i Azure
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
ms.custom: mvc, seodec18
uid: azure/devops/index
ms.openlocfilehash: f45bb2a5dd4b3d1a820085ede7ce3219045ed80b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898561"
---
# <a name="devops-with-aspnet-core-and-azure"></a>Metodyka DevOps z platformami ASP.NET Core i Azure

[![Obraz okładki](./media/cover-large.png)](https://aka.ms/devopsbook)

Przez [Soper kamera](https://twitter.com/camsoper) i [Scott Addie](https://twitter.com/scottaddie)

Ten przewodnik jest dostępny jako [do pobrania plików PDF Książka elektroniczna](https://aka.ms/devopsbook).

## <a name="welcome"></a>Witaj 

Przewodnik cyklu tworzenia oprogramowania platformy Azure dla platformy .NET — Zapraszamy! Ten przewodnik wprowadzenie podstawowych pojęć dotyczących tworzenia cyklu życia na platformie Azure przy użyciu platformy .NET, narzędziami i procesami. Po zakończeniu tego przewodnika, możesz czerpać korzyści dojrzała łańcucha narzędzi DevOps.

## <a name="who-this-guide-is-for"></a>Kto ten przewodnik jest przeznaczony dla

Powinno być doświadczonym deweloperom platformy ASP.NET Core (poziom 200-300). Nie trzeba nic wiedzieć o platformie Azure, jak omówimy to w ramach tego wprowadzenia. Ten przewodnik może być również przydatne w przypadku inżynierom DevOps, którzy są bardziej skupia się na operacje niż rozwoju.

Ten przewodnik jest przeznaczony dla deweloperów Windows. Jednak z systemami Linux i macOS są w pełni obsługiwane przez .NET Core. Można dostosować ten przewodnik dla systemu Linux/macOS, obserwuj objaśnienia na różnice Linux/macOS.

## <a name="what-this-guide-doesnt-cover"></a>Co ten przewodnik nie obejmuje

Ten przewodnik koncentruje się na środowisko end-to-end ciągłego wdrażania dla deweloperów platformy .NET. Nie jest wyczerpująca przewodnik wszystkimi aspektami platformy Azure, a jej nie skupić często interfejsów API platformy .NET dla usług platformy Azure. Jest nacisk na całym ciągłej integracji i wdrażania, monitorowania i debugowania. Pod koniec przewodnika są oferowane zalecenia dotyczące następnych kroków. Sugestie obejmuje usług platformy Azure, które są przydatne dla deweloperów platformy ASP.NET Core.

## <a name="whats-in-this-guide"></a>Co to jest w tym przewodniku

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Narzędzia i pliki do pobrania](xref:azure/devops/tools-and-downloads)

Dowiedz się, gdzie można uzyskać narzędzia używane w tym przewodniku.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Wdrażanie w usłudze App Service](xref:azure/devops/deploy-to-app-service)

Dowiedz się, różne metody wdrażania aplikacji platformy ASP.NET Core w usłudze Azure App Service.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Ciągła integracja i ciągłe wdrażanie](xref:azure/devops/cicd)

Twórz end-to-end ciągłej integracji i wdrażania rozwiązania dla aplikacji platformy ASP.NET Core za pomocą usługi GitHub, usługom DevOps platformy Azure i platformy Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Monitorowanie i debugowania](xref:azure/devops/monitor)

Narzędzia platformy Azure umożliwia monitorowanie, rozwiązywanie problemów i dostosowywanie aplikacji.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Następne kroki](xref:azure/devops/next-steps)

Inne ścieżki szkoleniowe dla deweloperów platformy ASP.NET Core poznawania platformy Azure.

## <a name="additional-introductory-reading"></a>Dodatkowe materiały wprowadzające

Jeśli jest to pierwszy narażenia na przetwarzanie w chmurze, artykuły te wyjaśniają zasady podstawowe.

* [Co to jest chmura obliczeniowa?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Przykłady chmury obliczeniowej](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [Co to jest IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)
* [Co to jest PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
