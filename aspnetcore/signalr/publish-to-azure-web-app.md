---
title: Publikowanie platformy ASP.NET Core app SignalR do aplikacji sieci Web platformy Azure
author: bradygaster
description: Publikowanie platformy ASP.NET Core app SignalR do aplikacji sieci Web platformy Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 1c472711a86edae8dc6e207734aa54e48c02d47d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087699"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publikowanie platformy ASP.NET Core aplikacji SignalR na aplikację internetową platformy Azure

[Usługa Azure Web Apps](/azure/app-service/app-service-web-overview) jest [przetwarzanie w chmurze firmy Microsoft](https://azure.microsoft.com/) usługa platformy do hostowania aplikacji sieci web, w tym platformy ASP.NET Core.

> [!NOTE]
> Ten artykuł odnosi się do publikowania aplikacji biblioteki SignalR platformy ASP.NET Core w programie Visual Studio. Odwiedź stronę [usługi SignalR platformy Azure](https://azure.microsoft.com/services/signalr-service) Aby uzyskać więcej informacji na temat przy użyciu biblioteki SignalR na platformie Azure.

## <a name="publish-the-app"></a>Publikowanie aplikacji

Visual Studio udostępnia wbudowane narzędzia do publikowania aplikacji sieci Web platformy Azure. Visual Studio Code użytkownik może używać [wiersza polecenia platformy Azure](/cli/azure) polecenia, aby opublikować aplikacje na platformie Azure. W tym artykule opisano publikowania za pomocą narzędzi w programie Visual Studio. Aby opublikować aplikację przy użyciu wiersza polecenia platformy Azure, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet).

Kliknij prawym przyciskiem myszy nad projektem w **Eksploratora rozwiązań** i wybierz **Publikuj**. Upewnij się, że **Utwórz nową** jest zaewidencjonowany **wybierz lokalizację docelową publikowania** okna dialogowego, a następnie wybierz **Publikuj**.

![Pobranie publikowania docelowej](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Wprowadź następujące informacje w **Tworzenie usługi App Service** okna dialogowego, a następnie wybierz **Utwórz**.

| Element | Opis |
| ---- | ----------- |
| **Nazwa aplikacji** | Unikatowa nazwa aplikacji. |
| **Subskrypcja** | Subskrypcja platformy Azure, przez aplikację. |
| **Grupa zasobów** | Grupa powiązane zasoby, do których należy aplikacja.  |
| **Plan hostingu** | Plan taryfowy aplikacji sieci web. |

![Utwórz usługę app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Program Visual Studio wykonuje następujące zadania:

* Tworzy profil publikowania zawierający ustawienia publikowania.
* Tworzy i wykorzystuje istniejące *aplikacji sieci Web platformy Azure* przy użyciu podanych szczegółów.
* Publikuje aplikację.
* Otworzy w przeglądarce, za pomocą aplikacji sieci web opublikowanych załadowane.

Zwróć uwagę format adresu URL aplikacji jest *{Nazwa aplikacji} .azurewebsites .net*. Na przykład aplikacji o nazwie `SignalRChattR` ma adres URL, który wygląda jak `https://signalrchattr.azurewebsites.net`.

Jeśli wystąpi błąd HTTP 502.2, zobacz [wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index) go rozwiązać.

## <a name="configure-signalr-web-app"></a>Konfigurowanie aplikacji sieci web SignalR

ASP.NET Core SignalR aplikacje, które są publikowane jako aplikację internetową platformy Azure musi mieć [koligacja ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) włączone. [Gniazda Websocket](xref:fundamentals/websockets) powinna mieć możliwość, Zezwalaj na transport gniazda Websocket funkcji.

W witrynie Azure portal przejdź do **ustawienia aplikacji** dla aplikacji sieci web. Ustaw **WebSockets** do **na**i sprawdź **koligacja ARR** jest **na**.

![Ustawienia aplikacji sieci Web usługi Azure w witrynie Azure portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Gniazda Websocket i innych rodzajów transportu [są ograniczone w oparciu o Plan usługi App Service](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Powiązane zasoby

* [Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet)
* [Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hostowanie i wdrażanie aplikacji ASP.NET Core w wersji zapoznawczej na platformie Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
