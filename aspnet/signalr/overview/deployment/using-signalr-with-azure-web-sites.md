---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Przy użyciu SignalR z aplikacjami sieci Web w usłudze Azure App Service | Dokumentacja firmy Microsoft"
author: pfletcher
description: "Ten dokument zawiera opis sposobu konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure. Wersje oprogramowania używany w samouczku Visual Studio 2013 lub Vis...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Przy użyciu SignalR z aplikacjami sieci Web w usłudze aplikacji Azure
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> Ten dokument zawiera opis sposobu konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) lub Visual Studio 2012
> - .NET 4.5
> - SignalR w wersji 2
> - Zestaw Azure SDK 2.3 dla programu Visual Studio 2013 lub 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), lub [fora Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Spis treści

- [Wprowadzenie](#introduction)
- [Wdrażanie aplikacji sieci SignalR Web w usłudze Azure App Service](#deploying)
- [Włączanie Websocket w usłudze aplikacji Azure](#websocket)
- [Przy użyciu płyty montażowej pamięci podręcznej Azure Redis](#backplane)
- [Następne kroki](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Wprowadzenie

Biblioteka SignalR platformy ASP.NET można przełączyć nowy poziom interakcji między serwerami i sieci web lub klientów platformy .NET. Gdy hostowana na platformie Azure, aplikacji SignalR można korzystać z wysokiej dostępności, skalowalności i wydajności środowisko, która działa w chmurze zapewnia.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Wdrażanie aplikacji sieci SignalR Web w usłudze Azure App Service

SignalR nie dodaje żadnych komplikacji określonego do wdrażania aplikacji na platformie Azure i wdrażanie na serwerze lokalnym. Aplikację, która używa SignalR może być hostowana na platformie Azure, bez wprowadzania żadnych zmian w konfiguracji lub inne ustawienia (mimo że obsługę protokołu WebSockets, zobacz [włączenie Websocket w usłudze Azure App Service](#websocket) poniżej.) W tym samouczku utworzone w aplikacja zostanie wdrożona [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) na platformie Azure.

**Wymagania wstępne**

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, Visual Studio 2013 Express for Web znajduje się w instalacji zestawu SDK platformy Azure.
- [Zestaw Azure SDK 2.3 dla programu Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) lub [Azure SDK 2.3 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Do ukończenia tego samouczka należy subskrypcji platformy Azure. Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), lub [Załóż subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Wdrażanie aplikacji sieci web SignalR na platformie Azure

1. Zakończenie [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać gotowego projektu z [galerii kodu](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. W programie Visual Studio, wybierz **kompilacji**, **publikowania rozmowę SignalR**.
3. W oknie dialogowym "Publikowanie w sieci Web" Wybierz "Windows Azure Web Sites".

    ![Wybierz witryny sieci Web Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Jeśli użytkownik nie jest obecnie zalogowany do konta Microsoft, kliknij przycisk **logowania...**  w oknie dialogowym "Wybierz istniejącą witrynę sieci Web" i zaloguj się.

    ![Wybierz istniejącą witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image2.png)    ![Logowanie do platformy Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. W oknie dialogowym "Wybierz istniejącą witrynę sieci Web", kliknij przycisk **nowy**.

    ![Nową witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. W oknie dialogowym "Utwórz witrynę w systemie Windows Azure" Wprowadź unikatowej nazwy aplikacji. Wybierz region znajdujący się najbliżej użytkownika na liście rozwijanej regionu. Kliknij przycisk **Utwórz**.

    ![Tworzenie witryny na platformie Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. W oknie dialogowym "Publikowanie w sieci Web", kliknij przycisk **publikowania**.

    ![Publikowanie witryny](using-signalr-with-azure-web-sites/_static/image6.png)
8. Po ukończeniu publikowania aplikacji rozmów SignalR hostowanej aplikacji w aplikacji sieci Web usługi aplikacji Azure zostanie otwarty w przeglądarce.

    ![Witryna otwierania w przeglądarce](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Włączanie Websocket aplikacji sieci Web usługi aplikacji Azure

Protokół WebSockets musi być jawnie włączone w aplikacji sieci web do użycia w aplikacji SignalR; w przeciwnym razie należy używać innych protokołów (zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports) szczegółowe informacje).

Aby można było używać Websocket aplikacji sieci Web usługi aplikacji Azure, należy ją włączyć w sekcji konfiguracji aplikacji sieci web. Aby to zrobić, Otwórz aplikację sieci web w [portalu zarządzania Azure](https://manage.windowsazure.com/)i wybierz pozycję Konfiguruj.

![Karta Konfigurowanie](using-signalr-with-azure-web-sites/_static/image8.png)

W górnej części strony konfiguracji upewnij się, że programy .NET 4.5 jest używany przez aplikację sieci web.

![Ustawienie wersji 4.5 programu .NET framework](using-signalr-with-azure-web-sites/_static/image9.png)

Na stronie konfiguracji w **Websocket** wybierz pozycję **na**.

![Ustawienie WebSockets: na](using-signalr-with-azure-web-sites/_static/image10.png)

W dolnej części strony konfiguracji, wybierz **zapisać** Aby zapisać zmiany.

![Zapisz ustawienia](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Przy użyciu płyty montażowej pamięci podręcznej Azure Redis

Jeśli używasz wielu wystąpień dla aplikacji sieci web i użytkowników z tych wystąpień konieczne współdziałać ze sobą (umożliwiając, na przykład wiadomości utworzone w jednym wystąpieniu może nawiązać połączenie użytkowników podłączonych do innych wystąpień), [pamięć podręczna Redis Azure płyty montażowej](../performance/scaleout-with-redis.md) musi zostać wdrożona w aplikacji.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących aplikacji sieci Web w usłudze Azure App Service, zobacz [Omówienie aplikacji sieci Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
