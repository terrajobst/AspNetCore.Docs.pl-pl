---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web. Otwórz interfejs sieci Web dla platformy .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667391"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a>Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu API sieci Web platformy ASP.NET 
====================

> W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.
>
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza usług IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - Internetowy interfejs API 5.2.7


> [!NOTE]
> Możesz znaleźć pełnego kodu źródłowego w ramach tego samouczka w [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

Na **pliku** menu **New**, a następnie wybierz **projektu**. Z **zainstalowane**w obszarze **Visual C#** , wybierz opcję **pulpitu Windows** , a następnie wybierz **Aplikacja konsoli (.Net Framework)**. Nadaj projektowi nazwę "OwinSelfhostSample", a następnie wybierz pozycję **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Dodaj pakiety internetowego interfejsu API i OWIN

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Spowoduje to zainstalowanie pakietu host własny WebAPI OWIN i wszystkich wymaganych pakietów OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurowanie interfejsu API sieci Web dla hosta samodzielnego

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Zastąp cały kod standardowy, w tym pliku następujących czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Następnie Dodaj klasę kontrolera interfejsu API sieci Web. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `ValuesController`.

Zastąp cały kod standardowy, w tym pliku następujących czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Uruchamianie hosta OWIN i wykonać żądanie za pomocą klasy HttpClient

Zastąp cały kod standardowy w pliku Program.cs następujących czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio. Dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hostowanie Web API platformy ASP.NET w roli procesu roboczego platformy Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
