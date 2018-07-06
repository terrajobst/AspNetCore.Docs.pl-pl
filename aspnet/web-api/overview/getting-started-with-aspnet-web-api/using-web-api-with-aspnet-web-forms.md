---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Za pomocą interfejsu API sieci Web przy użyciu wzorca ASP.NET Web Forms | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840891"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="555d9-102">Za pomocą interfejsu API sieci Web przy użyciu wzorca ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="555d9-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="555d9-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="555d9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="555d9-104">Chociaż interfejs API sieci Web platformy ASP.NET jest dostarczana z platformą ASP.NET MVC, jest łatwe do dodania interfejsu API sieci Web do tradycyjnych aplikacji formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="555d9-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="555d9-105">Ten samouczek przeprowadzi Cię przez kroki.</span><span class="sxs-lookup"><span data-stu-id="555d9-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="555d9-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="555d9-106">Overview</span></span>

<span data-ttu-id="555d9-107">Aby użyć interfejsu API sieci Web w aplikacji formularzy sieci Web, istnieją dwa podstawowe kroki:</span><span class="sxs-lookup"><span data-stu-id="555d9-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="555d9-108">Dodawanie kontrolera interfejsu API sieci Web, która pochodzi od klasy **klasy ApiController** klasy.</span><span class="sxs-lookup"><span data-stu-id="555d9-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="555d9-109">Dodaj tabelę tras w celu **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="555d9-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="555d9-110">Utwórz projekt formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="555d9-110">Create a Web Forms Project</span></span>

<span data-ttu-id="555d9-111">Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="555d9-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="555d9-112">Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="555d9-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="555d9-113">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="555d9-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="555d9-114">W obszarze **Visual C#**, wybierz opcję **Web**.</span><span class="sxs-lookup"><span data-stu-id="555d9-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="555d9-115">Na liście szablonów projektu wybierz **aplikacji formularzy sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="555d9-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="555d9-116">Wprowadź nazwę dla projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="555d9-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="555d9-117">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="555d9-117">Create the Model and Controller</span></span>

<span data-ttu-id="555d9-118">W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="555d9-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="555d9-119">Najpierw Dodaj klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="555d9-119">First, add a model class.</span></span> <span data-ttu-id="555d9-120">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**.</span><span class="sxs-lookup"><span data-stu-id="555d9-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="555d9-121">Nazwa klasy produktu i dodaj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="555d9-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="555d9-122">Następnie dodaj Kontroler interfejsu API sieci Web do projektu., A *kontrolera* jest obiektem, który obsługuje żądania HTTP dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="555d9-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="555d9-123">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="555d9-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="555d9-124">Wybierz **Dodaj nowy element**.</span><span class="sxs-lookup"><span data-stu-id="555d9-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="555d9-125">W obszarze **zainstalowane szablony**, rozwiń węzeł **Visual C#** i wybierz **Web**.</span><span class="sxs-lookup"><span data-stu-id="555d9-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="555d9-126">Z listy szablonów wybierz **klasa formantu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="555d9-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="555d9-127">Nazwa kontrolera "ProductsController", a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="555d9-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="555d9-128">**Dodaj nowy element** Kreator utworzy plik o nazwie ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="555d9-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="555d9-129">Usunięcie metod, które uwzględnione kreatora i dodaj następujące metody:</span><span class="sxs-lookup"><span data-stu-id="555d9-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="555d9-130">Aby uzyskać więcej informacji o kodzie, w tym kontrolerze, zobacz [wprowadzenie](tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="555d9-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="555d9-131">Dodawanie informacji o routingu</span><span class="sxs-lookup"><span data-stu-id="555d9-131">Add Routing Information</span></span>

<span data-ttu-id="555d9-132">Teraz, dlatego dodamy trasy identyfikator URI tego identyfikatory URI w postaci &quot;/interfejsAPI/produkty/&quot; są kierowane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="555d9-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="555d9-133">W **Eksploratora rozwiązań**, kliknij dwukrotnie plik Global.asax można otworzyć pliku związanego z kodem Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="555d9-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="555d9-134">Dodaj następujący kod **przy użyciu** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="555d9-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="555d9-135">Następnie dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="555d9-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="555d9-136">Aby uzyskać więcej informacji o tabelach routingu, zobacz [routingu ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="555d9-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="555d9-137">Dodaj AJAX po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="555d9-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="555d9-138">To wszystko, czego potrzebujesz do tworzenia internetowego interfejsu API, które klienci mogą uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="555d9-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="555d9-139">Teraz możemy dodać stronę HTML, która używa technologii jQuery do wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="555d9-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="555d9-140">Upewnić się, że strona wzorcowa (na przykład *Site.Master*) obejmuje `ContentPlaceHolder` z `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="555d9-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="555d9-141">Otwórz plik Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="555d9-141">Open the file Default.aspx.</span></span> <span data-ttu-id="555d9-142">Zastąp standardowy tekst, który znajduje się w sekcji głównej zawartości, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="555d9-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="555d9-143">Następnie dodaj odwołanie do pliku źródłowego jQuery w `HeaderContent` sekcji:</span><span class="sxs-lookup"><span data-stu-id="555d9-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="555d9-144">Uwaga: Możesz łatwo dodać odwołanie do skryptu, przeciągając i upuszczając go z **Eksploratora rozwiązań** do okna edytora kodu.</span><span class="sxs-lookup"><span data-stu-id="555d9-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="555d9-145">Poniżej jQuery tag skryptu Dodaj następujący blok skryptu:</span><span class="sxs-lookup"><span data-stu-id="555d9-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="555d9-146">Podczas ładowania dokumentu, ten skrypt tworzy żądanie AJAX &quot;interfejsu api/produktów&quot;.</span><span class="sxs-lookup"><span data-stu-id="555d9-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="555d9-147">Żądanie zwraca listę produktów, w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="555d9-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="555d9-148">Skrypt ten dodaje informacje o produkcie do tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="555d9-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="555d9-149">Po uruchomieniu aplikacji jej powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="555d9-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
