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
<a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="8d9a7-104">Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu API sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d9a7-104">Use OWIN to Self-Host ASP.NET Web API</span></span> 
====================

> <span data-ttu-id="8d9a7-105">W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-105">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="8d9a7-106">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-106">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8d9a7-107">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza usług IIS.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8d9a7-108">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="8d9a7-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="8d9a7-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8d9a7-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="8d9a7-110">Internetowy interfejs API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="8d9a7-110">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="8d9a7-111">Możesz znaleźć pełnego kodu źródłowego w ramach tego samouczka w [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="8d9a7-111">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="8d9a7-112">Tworzenie aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="8d9a7-112">Create a console application</span></span>

<span data-ttu-id="8d9a7-113">Na **pliku** menu **New**, a następnie wybierz **projektu**.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-113">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="8d9a7-114">Z **zainstalowane**w obszarze **Visual C#** , wybierz opcję **pulpitu Windows** , a następnie wybierz **Aplikacja konsoli (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-114">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="8d9a7-115">Nadaj projektowi nazwę "OwinSelfhostSample", a następnie wybierz pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-115">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="8d9a7-116">Dodaj pakiety internetowego interfejsu API i OWIN</span><span class="sxs-lookup"><span data-stu-id="8d9a7-116">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="8d9a7-117">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-117">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8d9a7-118">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8d9a7-118">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="8d9a7-119">Spowoduje to zainstalowanie pakietu host własny WebAPI OWIN i wszystkich wymaganych pakietów OWIN.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-119">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="8d9a7-120">Konfigurowanie interfejsu API sieci Web dla hosta samodzielnego</span><span class="sxs-lookup"><span data-stu-id="8d9a7-120">Configure Web API for self-host</span></span>

<span data-ttu-id="8d9a7-121">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-121">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8d9a7-122">Nazwa klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-122">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="8d9a7-123">Zastąp cały kod standardowy, w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8d9a7-123">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="8d9a7-124">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="8d9a7-124">Add a Web API controller</span></span>

<span data-ttu-id="8d9a7-125">Następnie Dodaj klasę kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-125">Next, add a Web API controller class.</span></span> <span data-ttu-id="8d9a7-126">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-126">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8d9a7-127">Nazwa klasy `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-127">Name the class `ValuesController`.</span></span>

<span data-ttu-id="8d9a7-128">Zastąp cały kod standardowy, w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8d9a7-128">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="8d9a7-129">Uruchamianie hosta OWIN i wykonać żądanie za pomocą klasy HttpClient</span><span class="sxs-lookup"><span data-stu-id="8d9a7-129">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="8d9a7-130">Zastąp cały kod standardowy w pliku Program.cs następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8d9a7-130">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="8d9a7-131">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d9a7-131">Run the application</span></span>

<span data-ttu-id="8d9a7-132">Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d9a7-132">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="8d9a7-133">Dane wyjściowe powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8d9a7-133">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="8d9a7-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8d9a7-134">Additional resources</span></span>

[<span data-ttu-id="8d9a7-135">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="8d9a7-135">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="8d9a7-136">Hostowanie Web API platformy ASP.NET w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="8d9a7-136">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
