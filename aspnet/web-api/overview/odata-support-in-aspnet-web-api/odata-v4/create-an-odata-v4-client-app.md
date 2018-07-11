---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Tworzenie aplikacji klienckiej OData v4 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1e621988b43b4f012f113d7a52250879da56bb1c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818679"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="cec42-102">Tworzenie aplikacji klienckiej OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="cec42-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="cec42-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cec42-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cec42-104">W poprzednim samouczku utworzono podstawowej usługi OData, która obsługuje operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="cec42-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="cec42-105">Teraz Utwórzmy klienta dla usługi.</span><span class="sxs-lookup"><span data-stu-id="cec42-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="cec42-106">Uruchom nowe wystąpienie programu Visual Studio i Utwórz nowy projekt aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="cec42-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="cec42-107">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane** &gt; **szablony** &gt; **Visual C#** &gt; **Pulpitu Windows**i wybierz **aplikację Konsolową** szablonu.</span><span class="sxs-lookup"><span data-stu-id="cec42-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="cec42-108">Nadaj projektowi nazwę &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="cec42-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="cec42-109">Aplikacja konsoli można również dodać do tego samego rozwiązania programu Visual Studio, który zawiera usługę OData.</span><span class="sxs-lookup"><span data-stu-id="cec42-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="cec42-110">Zainstaluj Generator kodu klienta OData</span><span class="sxs-lookup"><span data-stu-id="cec42-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="cec42-111">Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="cec42-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="cec42-112">Wybierz **Online** &gt; **galerii Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="cec42-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="cec42-113">W polu wyszukiwania, wyszukaj &quot;generatora kodu klienta OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="cec42-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="cec42-114">Kliknij przycisk **Pobierz** zainstalować VSIX.</span><span class="sxs-lookup"><span data-stu-id="cec42-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="cec42-115">Może być wyświetlony monit o ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cec42-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="cec42-116">Uruchom usługę OData lokalnie</span><span class="sxs-lookup"><span data-stu-id="cec42-116">Run the OData Service Locally</span></span>

<span data-ttu-id="cec42-117">Uruchom projekt ProductService z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cec42-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="cec42-118">Domyślnie program Visual Studio otworzy w przeglądarce katalog główny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cec42-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="cec42-119">Należy pamiętać, identyfikator URI; będzie on potrzebny w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="cec42-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="cec42-120">Pozostaw aplikację uruchomioną.</span><span class="sxs-lookup"><span data-stu-id="cec42-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="cec42-121">Jeśli oba projekty są umieszczone w tym samym rozwiązaniu, upewnij się uruchomić projekt ProductService bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="cec42-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="cec42-122">W następnym kroku należy zachować usługi uruchomione podczas modyfikowania projekt aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="cec42-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="cec42-123">Generowanie usługi serwera Proxy</span><span class="sxs-lookup"><span data-stu-id="cec42-123">Generate the Service Proxy</span></span>

<span data-ttu-id="cec42-124">Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData.</span><span class="sxs-lookup"><span data-stu-id="cec42-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="cec42-125">Serwer proxy tłumaczy wywołania metody na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="cec42-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="cec42-126">Spowoduje utworzenie klasy serwera proxy, uruchamiając [szablon T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="cec42-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="cec42-127">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="cec42-127">Right-click the project.</span></span> <span data-ttu-id="cec42-128">Wybierz **Dodaj** &gt; **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="cec42-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="cec42-129">W **Dodaj nowy element** okno dialogowe, wybierz opcję **elementy Visual C#** &gt; **kodu** &gt; **klient OData**.</span><span class="sxs-lookup"><span data-stu-id="cec42-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="cec42-130">Nazwa szablonu &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="cec42-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="cec42-131">Kliknij przycisk **Dodaj** i klikanie ostrzeżenie o zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="cec42-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="cec42-132">W tym momencie otrzymasz błąd można zignorować.</span><span class="sxs-lookup"><span data-stu-id="cec42-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="cec42-133">Visual Studio automatycznie uruchamia szablonu, ale szablon wymaga niektóre ustawienia konfiguracji pierwszego.</span><span class="sxs-lookup"><span data-stu-id="cec42-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="cec42-134">Otwórz plik ProductClient.odata.config. W `Parameter` elementu, Wklej w identyfikatorze URI z projektu ProductService (w poprzednim kroku).</span><span class="sxs-lookup"><span data-stu-id="cec42-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="cec42-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec42-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="cec42-136">Uruchom szablon ponownie.</span><span class="sxs-lookup"><span data-stu-id="cec42-136">Run the template again.</span></span> <span data-ttu-id="cec42-137">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy plik ProductClient.tt, a następnie wybierz **Uruchom narzędzie niestandardowe**.</span><span class="sxs-lookup"><span data-stu-id="cec42-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="cec42-138">Ten szablon tworzy plik kodu o nazwie ProductClient.cs, który definiuje serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="cec42-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="cec42-139">Podczas opracowywania aplikacji, jeśli zmienisz punkt końcowy OData, uruchom szablon ponownie, aby zaktualizować serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="cec42-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="cec42-140">Użyj serwera Proxy usługi do wywołania usługi OData</span><span class="sxs-lookup"><span data-stu-id="cec42-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="cec42-141">Otwórz plik Program.cs i Zastąp standardowy kod poniżej.</span><span class="sxs-lookup"><span data-stu-id="cec42-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="cec42-142">Zastąp wartość *serviceUri* identyfikatora URI z usługą wcześniej.</span><span class="sxs-lookup"><span data-stu-id="cec42-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="cec42-143">Po uruchomieniu aplikacji powinno danych wyjściowych poniżej:</span><span class="sxs-lookup"><span data-stu-id="cec42-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
