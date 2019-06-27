---
title: Publikowanie platformy ASP.NET Core aplikacji SignalR w usłudze Azure App Service
author: bradygaster
description: Dowiedz się, jak opublikować aplikację biblioteki SignalR platformy ASP.NET Core w usłudze Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406110"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Publikowanie platformy ASP.NET Core aplikacji SignalR w usłudze Azure App Service

Przez [Brady'ego Gastera](https://twitter.com/bradygaster)

[Usługa Azure App Service](/azure/app-service/app-service-web-overview) jest [przetwarzanie w chmurze firmy Microsoft](https://azure.microsoft.com/) usługa platformy do hostowania aplikacji sieci web, w tym platformy ASP.NET Core.

> [!NOTE]
> Ten artykuł odnosi się do publikowania aplikacji biblioteki SignalR platformy ASP.NET Core w programie Visual Studio. Aby uzyskać więcej informacji, zobacz [usługi SignalR platformy Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Publikowanie aplikacji

W tym artykule opisano publikowania za pomocą narzędzi w programie Visual Studio. Visual Studio Code użytkownicy mogą używać [wiersza polecenia platformy Azure](/cli/azure) polecenia, aby opublikować aplikacje na platformie Azure. Aby uzyskać więcej informacji, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet).

1. Kliknij prawym przyciskiem myszy nad projektem w **Eksploratora rozwiązań** i wybierz **Publikuj**.

1. Upewnij się, że **usługi App Service** i **Utwórz nową** są zaznaczone w **wybierz lokalizację docelową publikowania** okna dialogowego.

1. Wybierz **Utwórz profil** z **Publikuj** przycisk listy rozwijanej.

   Wprowadź informacje opisane w poniższej tabeli w **Tworzenie usługi App Service** okna dialogowego, a następnie wybierz **Utwórz**.

   | Element               | Opis |
   | ------------------ | ----------- |
   | **Nazwa**           | Unikatowa nazwa aplikacji. |
   | **Subskrypcja**   | Subskrypcja platformy Azure przez aplikację. |
   | **Grupa zasobów** | Grupa powiązane zasoby, do których należy aplikacja. |
   | **Plan hostingu**   | Plan cenowy dla aplikacji sieci web. |

1. Wybierz **usługi Azure SignalR Service** w **zależności** > **Dodaj** listy rozwijanej:

   ![Obszar zależności przedstawiający Wybieranie usługi Azure SignalR Service na liście rozwijanej Dodaj](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. W **usługi Azure SignalR Service** okno dialogowe, wybierz opcję **Utwórz nowe wystąpienie usługi Azure SignalR Service**.

1. Podaj **nazwa**, **grupy zasobów**, i **lokalizacji**. Wróć do **usługi Azure SignalR Service** okna dialogowego, a następnie wybierz **Dodaj**.

Program Visual Studio wykonuje następujące zadania:

* Tworzy profil publikowania zawierający ustawienia publikowania.
* Tworzy *aplikacji sieci Web platformy Azure* przy użyciu podanych szczegółów.
* Publikuje aplikację.
* Otworzy w przeglądarce, która ładuje aplikację sieci web.

Format adresu URL aplikacji jest `{APP SERVICE NAME}.azurewebsites.net`. Na przykład aplikacji o nazwie `SignalRChatApp` ma adres URL z `https://signalrchatapp.azurewebsites.net`.

Jeśli HTTP *502.2 — Zła brama* błąd występuje, gdy wdrażanie aplikacji, który jest przeznaczony dla wersji platformy .NET Core w wersji zapoznawczej, zobacz [wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) go rozwiązać.

## <a name="configure-the-app-in-azure-app-service"></a>Skonfiguruj aplikację w usłudze Azure App Service

> [!NOTE]
> *Ta sekcja dotyczy tylko aplikacji nie korzystających z usługi Azure SignalR Service.*
>
> Jeśli aplikacja korzysta z usługi Azure SignalR Service, App Service nie wymaga konfiguracji koligacji Routing żądań aplikacji (ARR) i elementy Web Socket opisane w tej sekcji. Klienci łączą się ich gniazda sieci Web do usługi Azure SignalR Service, nie są bezpośrednio do aplikacji.

W przypadku aplikacji hostowanych bez usługi Azure SignalR Service należy włączyć:

* [Koligacja ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) rozsyłanie żądań od użytkownika do tego samego wystąpienia usługi App Service. Ustawieniem domyślnym jest **na**.
* [Web Sockets](xref:fundamentals/websockets) umożliwiające transport gniazda sieci Web do funkcji. Ustawieniem domyślnym jest **poza**.

1. W witrynie Azure portal przejdź do aplikacji sieci web w **App Services**.
1. Otwórz **konfiguracji** > **ustawienia ogólne**.
1. Ustaw **Web sockets** do **na**.
1. Upewnij się, że **koligacja ARR** ustawiono **na**.

## <a name="app-service-plan-limits"></a>Limity planu usługi App Service

Gniazda sieci Web i innych rodzajów transportu są ograniczone oparte na planie usługi App Service wybrane. Aby uzyskać więcej informacji, zobacz *usług Azure Cloud Services ogranicza* i *limity usługi App Service* sekcje [subskrypcji platformy Azure i limity, przydziały i ograniczenia](/azure/azure-subscription-service-limits#app-service-limits) artykuł.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Co to jest usługa Azure SignalR Service?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet)
* [Hostowanie i wdrażanie aplikacji ASP.NET Core w wersji zapoznawczej na platformie Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
