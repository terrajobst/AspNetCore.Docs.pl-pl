---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "Umożliwia OWIN hosta samodzielnego ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku przedstawiono sposób obsługi interfejsu API sieci Web platformy ASP.NET w aplikacji konsoli przy użyciu OWIN do hosta samodzielnego strukturę interfejsu API sieci Web. Otwórz interfejs sieci Web dla platformy .NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Umożliwia OWIN hosta samodzielnego ASP.NET Web API 2
====================
przez [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> W tym samouczku przedstawiono sposób obsługi interfejsu API sieci Web platformy ASP.NET w aplikacji konsoli przy użyciu OWIN do hosta samodzielnego strukturę interfejsu API sieci Web.
> 
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (OWIN) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN idealne rozwiązanie w przypadku samodzielnej obsługi aplikacji sieci web w własnego procesu poza usług IIS.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (współdziała również z programu Visual Studio 2012)
> - Składnik Web API 2


> [!NOTE]
> Kod źródłowy pełną można znaleźć w tym samouczku w [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Tworzenie aplikacji konsoli

Na **pliku** menu, kliknij przycisk **nowy**, następnie kliknij przycisk **projektu**. Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **Windows** , a następnie kliknij przycisk **aplikacji konsoli**. Nazwij projekt "OwinSelfhostSample", a następnie kliknij przycisk **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Dodaj składnik Web API i OWIN pakietów

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Spowoduje to zainstalowanie pakietu selfhost WebAPI OWIN i wszystkich wymaganych pakietów OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Skonfiguruj interfejs API sieci Web dla hosta samodzielnego

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Zastąp cały schematyczny kod w tym pliku następujące czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Następnie Dodaj klasę kontrolera interfejsu API sieci Web. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę. Nazwa klasy `ValuesController`.

Zastąp cały schematyczny kod w tym pliku następujące czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Rozpocznij hosta OWIN i żądanie za pomocą elementu HttpClient

Zastąp cały kod standardowy w pliku Program.cs następujące czynności:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio. Dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przegląd projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Host Web API platformy ASP.NET w roli procesu roboczego platformy Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
