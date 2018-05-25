---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Używanie składnika Web API z formularzami sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="aec3b-102">Używanie składnika Web API z formularzami sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aec3b-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="aec3b-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aec3b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="aec3b-104">Chociaż interfejsu API sieci Web platformy ASP.NET jest dostarczana z platformą ASP.NET MVC, jest łatwe dodawanie interfejsu API sieci Web do tradycyjnych aplikacji formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aec3b-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="aec3b-105">Ten samouczek przedstawia kroki.</span><span class="sxs-lookup"><span data-stu-id="aec3b-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="aec3b-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="aec3b-106">Overview</span></span>

<span data-ttu-id="aec3b-107">Aby użyć interfejsu API sieci Web w aplikacji formularzy sieci Web, istnieją dwa podstawowe kroki:</span><span class="sxs-lookup"><span data-stu-id="aec3b-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="aec3b-108">Dodawanie kontrolera interfejsu API sieci Web, która jest pochodną **klasy ApiController** klasy.</span><span class="sxs-lookup"><span data-stu-id="aec3b-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="aec3b-109">Dodaj do tabeli tras **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="aec3b-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="aec3b-110">Tworzenie projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="aec3b-110">Create a Web Forms Project</span></span>

<span data-ttu-id="aec3b-111">Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="aec3b-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="aec3b-112">Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="aec3b-113">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="aec3b-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="aec3b-114">W obszarze **Visual C#**, wybierz pozycję **Web**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="aec3b-115">Na liście szablony projektów, wybierz **aplikacji formularzy sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="aec3b-116">Wprowadź nazwę dla projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="aec3b-117">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="aec3b-117">Create the Model and Controller</span></span>

<span data-ttu-id="aec3b-118">W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="aec3b-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="aec3b-119">Najpierw Dodaj klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="aec3b-119">First, add a model class.</span></span> <span data-ttu-id="aec3b-120">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="aec3b-121">Nazwa klasy produktu, a następnie dodaj następujące wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="aec3b-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="aec3b-122">Następnie dodaj do projektu., A kontroler Web API *kontrolera* jest obiekt, który obsługuje żądania HTTP dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="aec3b-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="aec3b-123">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="aec3b-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="aec3b-124">Wybierz **Dodaj nowy element**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="aec3b-125">W obszarze **zainstalowane szablony**, rozwiń węzeł **Visual C#** i wybierz **Web**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="aec3b-126">Następnie wybierz z listy szablonów, **klasy kontrolera interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="aec3b-127">Nazwa kontrolera "ProductsController", a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="aec3b-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="aec3b-128">**Dodaj nowy element** Kreator utworzy plik o nazwie ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="aec3b-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="aec3b-129">Usunięcie metod, które uwzględnione kreatora i dodaj następujące metody:</span><span class="sxs-lookup"><span data-stu-id="aec3b-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="aec3b-130">Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz [wprowadzenie](tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="aec3b-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="aec3b-131">Dodawanie informacji o routingu</span><span class="sxs-lookup"><span data-stu-id="aec3b-131">Add Routing Information</span></span>

<span data-ttu-id="aec3b-132">Następnie dodamy trasy URI tak tego identyfikatorów URI w postaci &quot;/api/produkty/&quot; są kierowane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="aec3b-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="aec3b-133">W **Eksploratora rozwiązań**, kliknij dwukrotnie plik Global.asax można otworzyć pliku CodeBehind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="aec3b-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="aec3b-134">Dodaj następujące **przy użyciu** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aec3b-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="aec3b-135">Następnie dodaj następujący kod, aby **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="aec3b-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="aec3b-136">Aby uzyskać więcej informacji o tabelach routingu, zobacz [routingu na platformie ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="aec3b-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="aec3b-137">Dodawanie strony klienta AJAX</span><span class="sxs-lookup"><span data-stu-id="aec3b-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="aec3b-138">To wszystko, czego potrzebujesz do tworzenia interfejsu API, których klienci mogą uzyskiwać dostęp w sieci web.</span><span class="sxs-lookup"><span data-stu-id="aec3b-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="aec3b-139">Teraz Dodajmy strona HTML, która używa technologii jQuery do wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="aec3b-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="aec3b-140">Upewnij się, że strona wzorcowa (na przykład *Site.Master*) zawiera `ContentPlaceHolder` z `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="aec3b-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="aec3b-141">Otwórz plik Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="aec3b-141">Open the file Default.aspx.</span></span> <span data-ttu-id="aec3b-142">Zastąp tekstu standardowego, który znajduje się w sekcji głównej zawartości, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="aec3b-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="aec3b-143">Następnie dodaj odwołanie do pliku źródłowego jQuery w `HeaderContent` sekcji:</span><span class="sxs-lookup"><span data-stu-id="aec3b-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="aec3b-144">Uwaga: Możesz łatwo dodać odwołanie do skryptu przez przeciąganie i upuszczanie plików z **Eksploratora rozwiązań** do okna edytora kodu.</span><span class="sxs-lookup"><span data-stu-id="aec3b-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="aec3b-145">Poniżej jQuery tag skryptu Dodaj następujący blok skryptu:</span><span class="sxs-lookup"><span data-stu-id="aec3b-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="aec3b-146">Podczas ładowania dokumentu, ten skrypt zgłasza żądanie AJAX do &quot;interfejsu api/produkty&quot;.</span><span class="sxs-lookup"><span data-stu-id="aec3b-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="aec3b-147">Żądanie zwraca listę produktów w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="aec3b-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="aec3b-148">Skrypt ten dodaje informacji o produkcie do tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="aec3b-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="aec3b-149">Po uruchomieniu aplikacji powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="aec3b-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
