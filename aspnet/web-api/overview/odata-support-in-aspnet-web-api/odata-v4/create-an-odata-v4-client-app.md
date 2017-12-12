---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Tworzenie aplikacji klienckich OData v4 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: daa39fbbb4ff17d61f71bf2a642a9c2260b353e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="40081-102">Tworzenie aplikacji klienckich OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="40081-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="40081-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="40081-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="40081-104">W poprzednich samouczka utworzono podstawowej usługi OData, która obsługuje operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="40081-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="40081-105">Teraz Utwórzmy klienta dla usługi.</span><span class="sxs-lookup"><span data-stu-id="40081-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="40081-106">Uruchom nowe wystąpienie programu Visual Studio i Utwórz nowy projekt aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="40081-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="40081-107">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowana** &gt; **szablony** &gt; **Visual C#** &gt; **Windows Desktop**i wybierz **aplikacji konsoli** szablonu.</span><span class="sxs-lookup"><span data-stu-id="40081-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="40081-108">Nazwij projekt &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="40081-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="40081-109">Można również dodać aplikację konsoli do tego samego rozwiązania Visual Studio, który zawiera usługi OData.</span><span class="sxs-lookup"><span data-stu-id="40081-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="40081-110">Zainstaluj klienta OData Generator kodu</span><span class="sxs-lookup"><span data-stu-id="40081-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="40081-111">Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="40081-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="40081-112">Wybierz **Online** &gt; **galerii programu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="40081-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="40081-113">W polu wyszukiwania, wyszukaj &quot;generatora kodu klienta OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="40081-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="40081-114">Kliknij przycisk **Pobierz** do zainstalowania pliku VSIX.</span><span class="sxs-lookup"><span data-stu-id="40081-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="40081-115">Może być wyświetlony monit o ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40081-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="40081-116">Uruchamianie usługi OData lokalnie</span><span class="sxs-lookup"><span data-stu-id="40081-116">Run the OData Service Locally</span></span>

<span data-ttu-id="40081-117">Uruchom projekt ProductService z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40081-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="40081-118">Domyślnie program Visual Studio uruchamia przeglądarki do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40081-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="40081-119">Należy pamiętać, identyfikator URI; będzie on potrzebny w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="40081-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="40081-120">Pozostaw aplikacja była uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="40081-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="40081-121">Jeśli oba projekty w tym samym rozwiązaniu, upewnij się uruchomić projekt ProductService bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="40081-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="40081-122">W następnym kroku należy zachować usługę uruchomiony podczas modyfikowania projekt aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="40081-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="40081-123">Generowanie serwera Proxy usługi</span><span class="sxs-lookup"><span data-stu-id="40081-123">Generate the Service Proxy</span></span>

<span data-ttu-id="40081-124">Serwer proxy usługi jest klasą .NET, który definiuje metody do uzyskiwania dostępu do usługi OData.</span><span class="sxs-lookup"><span data-stu-id="40081-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="40081-125">Serwer proxy tłumaczy wywołania metody do żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="40081-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="40081-126">Utworzysz klasy serwera proxy, uruchamiając [szablon T4](https://msdn.microsoft.com/en-us/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="40081-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/en-us/library/bb126445.aspx).</span></span>

<span data-ttu-id="40081-127">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="40081-127">Right-click the project.</span></span> <span data-ttu-id="40081-128">Wybierz **dodać** &gt; **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="40081-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="40081-129">W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# elementów** &gt; **kod** &gt; **klient OData**.</span><span class="sxs-lookup"><span data-stu-id="40081-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="40081-130">Nazwa szablonu &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="40081-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="40081-131">Kliknij przycisk **Dodaj** i kliknij przycisk za pomocą ostrzeżenie o zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="40081-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="40081-132">W tym momencie zostanie wyświetlony błąd, można zignorować.</span><span class="sxs-lookup"><span data-stu-id="40081-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="40081-133">Visual Studio automatycznie uruchamia szablonu, ale szablon wymaga niektóre ustawienia konfiguracji pierwszy.</span><span class="sxs-lookup"><span data-stu-id="40081-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="40081-134">Otwórz plik ProductClient.odata.config. W `Parameter` elementu, Wklej w identyfikatorze URI z projektu ProductService (w poprzednim kroku).</span><span class="sxs-lookup"><span data-stu-id="40081-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="40081-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="40081-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="40081-136">Uruchom szablon ponownie.</span><span class="sxs-lookup"><span data-stu-id="40081-136">Run the template again.</span></span> <span data-ttu-id="40081-137">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy plik ProductClient.tt i wybierz **Uruchom narzędzie niestandardowe**.</span><span class="sxs-lookup"><span data-stu-id="40081-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="40081-138">Szablon tworzy plik kodu o nazwie ProductClient.cs, który definiuje serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="40081-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="40081-139">Podczas opracowywania aplikacji, jeśli zmienisz punktu końcowego OData, uruchom szablon ponownie, aby zaktualizować serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="40081-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="40081-140">Użyj serwera Proxy usługi do wywoływania usługi OData</span><span class="sxs-lookup"><span data-stu-id="40081-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="40081-141">Otwórz plik Program.cs i Zastąp schematyczny kod poniżej.</span><span class="sxs-lookup"><span data-stu-id="40081-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="40081-142">Zastąp wartość *serviceUri* z identyfikatorem URI usługi z wcześniej.</span><span class="sxs-lookup"><span data-stu-id="40081-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="40081-143">Po uruchomieniu aplikacji powinno danych wyjściowych poniżej:</span><span class="sxs-lookup"><span data-stu-id="40081-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
