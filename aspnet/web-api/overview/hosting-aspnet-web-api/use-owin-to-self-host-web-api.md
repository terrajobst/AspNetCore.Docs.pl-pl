---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu Web API 2 platformy ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web. Otwórz interfejs sieci Web dla platformy .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0d16498e94ac0a66c117ed057db398c14080beaa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755650"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu Web API 2 platformy ASP.NET
====================
przez [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.
> 
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza usług IIS.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (współpracuje również z programu Visual Studio 2012)
> - Składnik Web API 2


> [!NOTE]
> Możesz znaleźć pełnego kodu źródłowego w ramach tego samouczka w [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Tworzenie aplikacji konsoli

Na **pliku** menu, kliknij przycisk **New**, następnie kliknij przycisk **projektu**. Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **Windows** a następnie kliknij przycisk **aplikację Konsolową**. Nadaj projektowi nazwę "OwinSelfhostSample", a następnie kliknij przycisk **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Dodawanie interfejsu API sieci Web i pakietów OWIN

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Spowoduje to zainstalowanie pakietu host własny WebAPI OWIN i wszystkich wymaganych pakietów OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurowanie internetowego interfejsu API dla hosta samodzielnego

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Zastąp cały kod standardowy, w tym pliku następujących czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Dodaj Kontroler interfejsu API sieci Web

Następnie Dodaj klasę kontrolera interfejsu API sieci Web. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `ValuesController`.

Zastąp cały kod standardowy, w tym pliku następujących czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Uruchamianie hosta OWIN i wykonać żądanie za pomocą elementu HttpClient

Zastąp cały kod standardowy w pliku Program.cs następujących czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio. Dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hostowanie Web API platformy ASP.NET w roli procesu roboczego platformy Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
