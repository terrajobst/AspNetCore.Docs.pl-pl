---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Używanie SignalR z usługą Web Apps w usłudze Azure App Service | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym dokumencie opisano sposób konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure. Wersje oprogramowania używanego w tym samouczku, Visual Studio 2013 lub Vis....
ms.author: riande
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: da69e1aba1b56d69ad8e710cddd2b492168f1255
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287771"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Używanie usługi SignalR z funkcją Web Apps w usłudze Azure App Service
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym dokumencie opisano sposób konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) lub Visual Studio 2012
> - .NET 4.5
> - SignalR w wersji 2
> - Zestaw Azure SDK 2.3 dla programu Visual Studio 2013 lub 2012
>
>
>
> ## <a name="questions-and-comments"></a>Pytania i komentarze
>
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), lub [fora Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Spis treści

- [Wprowadzenie](#introduction)
- [Wdrażanie aplikacji sieci Web SignalR w usłudze Azure App Service](#deploying)
- [Włączanie funkcji WebSockets w usłudze Azure App Service](#websocket)
- [Za pomocą systemu Backplane usługi Azure Redis Cache](#backplane)
- [Następne kroki](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Wprowadzenie

Biblioteki SignalR platformy ASP.NET można przenieść na nowy poziom interakcji między serwerami i sieci web lub klientów programu .NET. W przypadku hostowanych na platformie Azure, aplikacji SignalR korzystać wysoko dostępnych, skalowalnych i wydajnych środowiska działające w chmurze zapewniająca.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Wdrażanie aplikacji sieci Web SignalR w usłudze Azure App Service

SignalR nie dodaje żadnych konkretnej kompilacji do wdrażania aplikacji na platformie Azure i wdrażanie na serwerze lokalnym. Aplikacji korzystającej z biblioteki SignalR można hostować na platformie Azure bez konieczności wprowadzania zmian w konfiguracji lub innymi ustawieniami (mimo że obsługę funkcji WebSockets, zobacz [włączenie funkcji WebSockets w usłudze Azure App Service](#websocket) poniżej.) W tym samouczku wdrożysz aplikacja utworzona w [Samouczek wprowadzający](../getting-started/tutorial-getting-started-with-signalr.md) na platformie Azure.

**Wymagania wstępne**

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, Visual Studio 2013 Express for Web znajduje się w instalacji zestawu SDK usługi Azure.
- [Zestaw Azure SDK 2.3 dla programu Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) lub [zestaw Azure SDK 2.3 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Do ukończenia tego samouczka, potrzebujesz subskrypcji platformy Azure. Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), lub [logowania do subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Wdrażanie aplikacji sieci web SignalR na platformie Azure

1. Wykonaj [Samouczek wprowadzający](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać gotowy projekt z [galerii kodów](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. W programie Visual Studio, wybierz **kompilacji**, **publikowania SignalR Chat**.
3. W oknie dialogowym "Publikowanie w sieci Web" Wybierz "Windows Azure Web Sites".

    ![Wybierz opcję Usługa witryny sieci Web](using-signalr-with-azure-web-sites/_static/image1.png)
4. Jeśli użytkownik nie jest zalogowany do konta Microsoft, kliknij przycisk **Zaloguj...**  w oknie dialogowym "Wybieranie istniejącej witryny sieci Web" i zaloguj się.

    ![Wybierz istniejącą witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image2.png)    ![Logowanie do platformy Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. W oknie dialogowym "Wybieranie istniejącej witryny sieci Web" kliknij **New**.

    ![Nową witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. W oknie dialogowym "Tworzenie witryny na platformie Windows Azure" Wprowadź unikatową nazwę aplikacji. W menu rozwijanym Region, wybierz region najbliżej Ciebie. Kliknij przycisk **Utwórz**.

    ![Tworzenie witryny na platformie Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. W oknie dialogowym "Publikowanie w sieci Web" kliknij **Publikuj**.

    ![Publikowanie witryny](using-signalr-with-azure-web-sites/_static/image6.png)
8. Po jej zakończeniu publikowania aplikacji rozmów SignalR, hostowana w usłudze Azure App Service Web Apps zostanie otwarta w przeglądarce.

    ![Otwieranie w przeglądarce witrynę](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Włączanie funkcji WebSockets w usłudze Azure App Service Web Apps

Gniazda Websocket musi być jawnie włączone w aplikacji sieci web ma być używany w aplikacji SignalR; w przeciwnym razie używać innych protokołów (zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports) Aby uzyskać szczegółowe informacje).

Aby można było używać funkcji WebSockets w usłudze Azure App Service Web Apps, należy ją włączyć w sekcji Konfiguracja aplikacji sieci web. Aby to zrobić, Otwórz w aplikacji sieci web [portalu zarządzania systemu Azure](https://manage.windowsazure.com/)i wybierz pozycję Konfiguruj.

![Karta Konfigurowanie](using-signalr-with-azure-web-sites/_static/image8.png)

W górnej części strony konfiguracji upewnij się, że .NET 4.5 jest używana dla aplikacji sieci web.

![Ustawienia programu .NET framework w wersji 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

Na stronie konfiguracji w **WebSockets** ustawienie, wybierz **na**.

![Ustawienie funkcji WebSockets: On](using-signalr-with-azure-web-sites/_static/image10.png)

W dolnej części strony konfiguracji, wybierz **Zapisz** Aby zapisać zmiany.

![Zapisz ustawienia](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Za pomocą systemu Backplane usługi Azure Redis Cache

Jeśli wiele wystąpień jest używana dla aplikacji sieci web, a użytkownicy tych wystąpień, które muszą wchodzić w interakcje ze sobą, (umożliwiając, na przykład wiadomości na czacie utworzone w jednym wystąpieniu można dotrzeć do użytkowników podłączonych do innych wystąpień), [usługi Azure Redis Cache płyty montażowej](../performance/scaleout-with-redis.md) musi zostać wdrożona w aplikacji.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat aplikacji sieci Web w usłudze Azure App Service, zobacz [omówienia usługi Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
