---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu Web API 2 platformy ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web. Otwórz interfejs sieci Web dla platformy .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 73757b50c15c6c65dbde4b61179b2d453673cfad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389563"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="f939d-104">Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu Web API 2 platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f939d-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f939d-105">przez [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="f939d-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="f939d-106">W tym samouczku pokazano, jak hostować interfejs API sieci Web platformy ASP.NET w aplikacji konsoli, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f939d-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="f939d-107">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f939d-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f939d-108">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f939d-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f939d-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="f939d-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f939d-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (współpracuje również z programu Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="f939d-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="f939d-111">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="f939d-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="f939d-112">Możesz znaleźć pełnego kodu źródłowego w ramach tego samouczka w [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="f939d-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="f939d-113">Tworzenie aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="f939d-113">Create a Console Application</span></span>

<span data-ttu-id="f939d-114">Na **pliku** menu, kliknij przycisk **New**, następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f939d-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="f939d-115">Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **Windows** a następnie kliknij przycisk **aplikację Konsolową**.</span><span class="sxs-lookup"><span data-stu-id="f939d-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="f939d-116">Nadaj projektowi nazwę "OwinSelfhostSample", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f939d-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="f939d-117">Dodawanie interfejsu API sieci Web i pakietów OWIN</span><span class="sxs-lookup"><span data-stu-id="f939d-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="f939d-118">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="f939d-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="f939d-119">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f939d-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="f939d-120">Spowoduje to zainstalowanie pakietu host własny WebAPI OWIN i wszystkich wymaganych pakietów OWIN.</span><span class="sxs-lookup"><span data-stu-id="f939d-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="f939d-121">Konfigurowanie internetowego interfejsu API dla hosta samodzielnego</span><span class="sxs-lookup"><span data-stu-id="f939d-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="f939d-122">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="f939d-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f939d-123">Nazwa klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f939d-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="f939d-124">Zastąp cały kod standardowy, w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f939d-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="f939d-125">Dodaj Kontroler interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f939d-125">Add a Web API Controller</span></span>

<span data-ttu-id="f939d-126">Następnie Dodaj klasę kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f939d-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="f939d-127">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="f939d-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f939d-128">Nazwa klasy `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="f939d-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="f939d-129">Zastąp cały kod standardowy, w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f939d-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="f939d-130">Uruchamianie hosta OWIN i wykonać żądanie za pomocą elementu HttpClient</span><span class="sxs-lookup"><span data-stu-id="f939d-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="f939d-131">Zastąp cały kod standardowy w pliku Program.cs następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f939d-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="f939d-132">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f939d-132">Running the Application</span></span>

<span data-ttu-id="f939d-133">Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f939d-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="f939d-134">Dane wyjściowe powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="f939d-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="f939d-135">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f939d-135">Additional Resources</span></span>

[<span data-ttu-id="f939d-136">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="f939d-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="f939d-137">Hostowanie Web API platformy ASP.NET w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="f939d-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
