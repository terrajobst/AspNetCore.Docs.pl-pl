---
title: Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure
author: rachelappel
description: Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271921"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure

[Azure Web App](/azure/app-service/app-service-web-overview) jest [firmy Microsoft w chmurze obliczeniowych](https://azure.microsoft.com/) usługi platforma do obsługi aplikacji sieci web, w tym platformy ASP.NET Core.

> [!NOTE]
> Ten artykuł dotyczy publikowania aplikacji ASP.NET Core SignalR z programu Visual Studio. Odwiedź stronę [usługi SignalR platformy Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) Aby uzyskać więcej informacji o korzystaniu z SignalR na platformie Azure.

## <a name="publish-the-app"></a>Publikowanie aplikacji

Program Visual Studio oferuje wbudowane narzędzia do publikowania aplikacji sieci Web platformy Azure. Visual Studio Code użytkownik może użyć [interfejsu wiersza polecenia Azure](/cli/azure) poleceń do publikowania aplikacji na platformie Azure. W tym artykule omówiono publikowania za pomocą narzędzi w programie Visual Studio. Aby opublikować aplikację przy użyciu interfejsu wiersza polecenia Azure, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).

Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania**. Upewnij się, że **Utwórz nowy** zaewidencjonowania **wybierz element docelowy publikowania** okna dialogowego, a następnie wybierz **publikowania**.

![Pobranie publikowania docelowego](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Wprowadź następujące informacje w **Tworzenie usługi App Service** okno dialogowe i wybierz **Utwórz**.

| Element | Opis |
| ---- | ----------- |
| **Nazwa aplikacji** | Unikatowa nazwa aplikacji. |
| **Subskrypcji** | Subskrypcja platformy Azure, który korzysta z aplikacji. |
| **Grupy zasobów** | Grupa powiązane zasoby, do której należy aplikacja.  |
| **Plan hostingu** | Planu cenowego dla aplikacji sieci web. |

![Tworzenie usługi app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio wykonuje następujące zadania:

* Tworzy profil publikowania zawierający ustawienia publikowania.
* Tworzy lub wykorzystuje istniejące *aplikacji sieci Web Azure* z podanych szczegółów.
* Publikowanie aplikacji.
* Uruchamia przeglądarce do aplikacji opublikowanych w sieci web załadowane.

Zwróć uwagę format adresu URL aplikacji jest *{nazwa} .azurewebsites .net*. Na przykład aplikacji o nazwie `SignalRChattR` ma adres URL, który wygląda jak `https://signalrchattr.azurewebsites.net`.

Jeśli wystąpi błąd HTTP 502.2, zobacz [wersji zapoznawczej wdrażania platformy ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index) go rozwiązać.

## <a name="configure-signalr-web-app"></a>Konfigurowanie aplikacji sieci web SignalR

ASP.NET Core SignalR aplikacje, które są publikowane jako aplikacji sieci Web platformy Azure musi mieć [koligacji ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) włączone. [Protokół WebSockets](xref:fundamentals/websockets) powinno być włączone, aby umożliwić transportu Websocket do funkcji.

W portalu Azure, przejdź do **ustawień aplikacji** dla aplikacji sieci web. Ustaw **Websocket** do **na**i sprawdź **koligacji ARR** jest **na**.

![Azure ustawień aplikacji sieci Web w portalu Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Protokół WebSockets i innych transportów [są ograniczone w oparciu o Plan usługi aplikacji](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Zasoby pokrewne

* [Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure z programem Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Host i wdrażanie aplikacji platformy ASP.NET Core Preview na platformie Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
