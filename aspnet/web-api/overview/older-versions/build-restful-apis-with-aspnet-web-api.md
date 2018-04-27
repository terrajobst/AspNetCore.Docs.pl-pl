---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: W ostatnich latach okazało HTTP jest nie tylko dla obsługująca stron HTML. Istnieje również zaawansowane platformę do tworzenia interfejsów API sieci Web, przy użyciu o kilku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ded549109ca6e7ad806f1c3f53387766527e5a94
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="f1834-104">Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f1834-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="f1834-105">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f1834-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="f1834-106">W ostatnich latach okazało HTTP jest nie tylko dla obsługująca stron HTML.</span><span class="sxs-lookup"><span data-stu-id="f1834-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="f1834-107">Jest również zaawansowane platformę do tworzenia interfejsów API sieci Web, przy użyciu kilku poleceń (GET, POST i tak dalej) oraz kilka pojęć proste takich jak *identyfikatorów URI* i *nagłówki*.</span><span class="sxs-lookup"><span data-stu-id="f1834-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="f1834-108">Interfejs API sieci Web platformy ASP.NET to zestaw składników, które upraszczają Programowanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1834-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="f1834-109">Ponieważ jest on oparty na środowiska uruchomieniowego ASP.NET MVC, interfejsu API sieci Web obsługuje automatycznie szczegółów niskiego poziomu transportu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1834-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="f1834-110">W tym samym czasie interfejsu API sieci Web udostępnia naturalnie modelu programowania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1834-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="f1834-111">W rzeczywistości celem jednego interfejsu API sieci Web jest *nie* abstrakcyjnej optymalizacji stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1834-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="f1834-112">W związku z tym interfejs API sieci Web jest zarówno elastyczne i łatwe do rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f1834-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="f1834-113">W tym laboratorium praktycznego użyjesz interfejsu API sieci Web do tworzenia prostego interfejsu API REST kontaktów Menedżerze aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="f1834-114">Utworzysz także klientowi korzystanie z interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f1834-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="f1834-115">Styl architektury REST okazał się efektywny sposób korzystać z protokołu HTTP -, chociaż na pewno nie jest ważny tylko metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1834-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="f1834-116">Powoduje to udostępnienie kontaktów Menedżerze RESTful do wyświetlania, dodawania i usuwania kontaktów, między innymi.</span><span class="sxs-lookup"><span data-stu-id="f1834-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="f1834-117">W tym laboratorium wymaga podstawową wiedzę na temat protokołu HTTP, REST, przyjęto założenie, że masz podstawową wiedzę na temat pracy z kodu HTML, JavaScript i jQuery.</span><span class="sxs-lookup"><span data-stu-id="f1834-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f1834-118">Witryny sieci Web ASP.NET jest przeznaczona do framework interfejsu API sieci Web platformy ASP.NET w [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f1834-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="f1834-119">Ta lokacja będzie dostarczać, najnowsze informacje, przykłady i informacje związane z interfejsu API sieci Web, więc spróbuj on często Jeśli chcesz delve więcej danych w kompozycji tworzenia niestandardowych interfejsów API sieci Web dostępnych praktycznie dowolne urządzenie lub programowanie Framework.</span><span class="sxs-lookup"><span data-stu-id="f1834-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="f1834-120">ASP.NET Web API, podobnie jak ASP.NET MVC 4 ma dużą elastyczność w zakresie oddzielanie warstwy usług z kontrolerów, co umożliwia używanie kilku dostępnych platform iniekcji zależności dość proste.</span><span class="sxs-lookup"><span data-stu-id="f1834-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="f1834-121">W witrynie MSDN, która przedstawia sposób użycia Ninject iniekcji zależności w projekcie interfejsu API sieci Web platformy ASP.NET, który można pobrać z znajduje się przykład dobrej [tutaj](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="f1834-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="f1834-122">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f1834-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f1834-123">Cele</span><span class="sxs-lookup"><span data-stu-id="f1834-123">Objectives</span></span>

<span data-ttu-id="f1834-124">W tym laboratorium praktycznego przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="f1834-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f1834-125">Implementowanie RESTful interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1834-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="f1834-126">Wywołanie interfejsu API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="f1834-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f1834-127">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f1834-127">Prerequisites</span></span>

<span data-ttu-id="f1834-128">Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:</span><span class="sxs-lookup"><span data-stu-id="f1834-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="f1834-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="f1834-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f1834-130">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="f1834-130">Setup</span></span>

<span data-ttu-id="f1834-131">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="f1834-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="f1834-132">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f1834-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f1834-133">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="f1834-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f1834-134">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f1834-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f1834-135">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="f1834-135">Exercises</span></span>

<span data-ttu-id="f1834-136">To laboratorium praktycznego obejmuje wykonywanie następujących:</span><span class="sxs-lookup"><span data-stu-id="f1834-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="f1834-137">Ćwiczenie 1: Tworzenie tylko do odczytu interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1834-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="f1834-138">Ćwiczenie 2: Tworzenie składnika Web API odczytu/zapisu</span><span class="sxs-lookup"><span data-stu-id="f1834-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="f1834-139">Ćwiczenie 3: Korzystanie z interfejsu API sieci Web z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="f1834-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="f1834-140">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="f1834-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f1834-141">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="f1834-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="f1834-142">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="f1834-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="f1834-143">Ćwiczenie 1: Tworzenie tylko do odczytu interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1834-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="f1834-144">W tym ćwiczeniu zostanie implementuje metody GET trybie tylko do odczytu do kontaktów Menedżerze.</span><span class="sxs-lookup"><span data-stu-id="f1834-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="f1834-145">Zadanie 1 — Tworzenie projektu interfejsu API</span><span class="sxs-lookup"><span data-stu-id="f1834-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="f1834-146">Nowe szablony projektów sieci web platformy ASP.NET to zadanie umożliwia tworzenie aplikacji sieci web interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="f1834-147">Uruchom **programu Visual Studio 2012 Express for Web**, aby zrobić to przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f1834-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f1834-148">Z **pliku** menu, wybierz opcję **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="f1834-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="f1834-149">Wybierz **Visual C# | Web** typ z widoku drzewa projektu typ projektu, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 4** typu projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="f1834-150">Ustaw projekt **nazwa** do *ContactManager* i **Nazwa rozwiązania** do *rozpocząć*, następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1834-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="f1834-151">![Tworzenie nowego programu ASP.NET MVC 4.0 projektu sieci Web aplikacji](build-restful-apis-with-aspnet-web-api/_static/image1.png "tworzenie nowych ASP.NET MVC 4.0 projektu sieci Web aplikacji")</span><span class="sxs-lookup"><span data-stu-id="f1834-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="f1834-152">*Tworzenie nowego programu ASP.NET MVC 4.0 projektu sieci Web aplikacji*</span><span class="sxs-lookup"><span data-stu-id="f1834-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="f1834-153">W oknie dialogowym Typ projektu programu ASP.NET MVC 4, wybierz **interfejsu API sieci Web** typu projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="f1834-154">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1834-154">Click **OK**.</span></span>

    <span data-ttu-id="f1834-155">![Określanie typu projektu interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "określenie typu projektu interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="f1834-156">*Określanie typu projektu interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="f1834-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="f1834-157">Zadanie 2 — Tworzenie kontrolerów interfejsu API kontaktów Menedżerze</span><span class="sxs-lookup"><span data-stu-id="f1834-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="f1834-158">W ramach tego zadania spowoduje utworzenie klasy kontrolera, w których będą znajdować się metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f1834-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f1834-159">Usuń plik o nazwie **ValuesController.cs** w **kontrolerów** folder z projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="f1834-160">Kliknij prawym przyciskiem myszy **kontrolerów** folderu projektu i wybierz **Dodaj | Kontroler** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="f1834-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="f1834-161">![Dodawanie nowego kontrolera do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "dodawania nowego kontrolera do projektu")</span><span class="sxs-lookup"><span data-stu-id="f1834-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="f1834-162">*Dodawanie nowego kontrolera do projektu*</span><span class="sxs-lookup"><span data-stu-id="f1834-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="f1834-163">W **Dodaj kontroler** okno dialogowe zostanie wyświetlone, wybierz **pusty Kontroler interfejsu API** z menu szablon.</span><span class="sxs-lookup"><span data-stu-id="f1834-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="f1834-164">Nazwa klasy kontrolera **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f1834-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="f1834-165">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="f1834-165">Then, click **Add.**</span></span>

    <span data-ttu-id="f1834-166">![Tworzenie nowego kontrolera interfejsu API sieci Web przy użyciu okna dialogowego Dodawanie kontrolera](build-restful-apis-with-aspnet-web-api/_static/image4.png "za pomocą okna dialogowego Dodawanie kontrolera do utworzenia nowego kontrolera interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="f1834-167">*Tworzenie nowego kontrolera interfejsu API sieci Web przy użyciu okna dialogowego Dodawanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="f1834-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="f1834-168">Dodaj następujący kod do **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f1834-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="f1834-169">(Fragment - kodu *laboratorium interfejsu API sieci Web - Ex01 - Get, metoda interfejsu API*)</span><span class="sxs-lookup"><span data-stu-id="f1834-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="f1834-170">Naciśnij klawisz **F5** do debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="f1834-171">Domyślną stronę główną dla projektu interfejsu API sieci Web powinny być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="f1834-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="f1834-172">![Domyślna strona główna aplikacji interfejsu API sieci Web platformy ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "domyślną stronę główną aplikacji interfejsu API sieci Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="f1834-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="f1834-173">*Domyślna strona główna aplikacji interfejsu API sieci Web platformy ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="f1834-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="f1834-174">W oknie programu Internet Explorer, naciśnij klawisz **F12** klawisz, aby otworzyć **Developer Tools** okna.</span><span class="sxs-lookup"><span data-stu-id="f1834-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="f1834-175">Kliknij przycisk **sieci** , a następnie kliknij pozycję **Start Przechwytywanie** przycisk, aby rozpocząć przechwytywanie ruchu sieciowego do okna.</span><span class="sxs-lookup"><span data-stu-id="f1834-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="f1834-176">![Otwarcie karty sieciowej i Inicjowanie sieci przechwytywania](build-restful-apis-with-aspnet-web-api/_static/image6.png "otwarcie karty sieciowej i zainicjowaniu sieciowej przechwytywania")</span><span class="sxs-lookup"><span data-stu-id="f1834-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="f1834-177">*Otwarcie karty sieciowej i zainicjowaniu przechwytywania ruchu sieciowego*</span><span class="sxs-lookup"><span data-stu-id="f1834-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="f1834-178">Dołącz adres URL na pasku adresu przeglądarki z **/api/kontaktu** i naciśnij klawisz enter.</span><span class="sxs-lookup"><span data-stu-id="f1834-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="f1834-179">W oknie Przechwytywanie sieci są wyświetlane szczegóły transmisji.</span><span class="sxs-lookup"><span data-stu-id="f1834-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="f1834-180">Należy zauważyć, że typ MIME odpowiedzi **application/json**.</span><span class="sxs-lookup"><span data-stu-id="f1834-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="f1834-181">Oznacza to, jak domyślny format danych wyjściowych jest JSON.</span><span class="sxs-lookup"><span data-stu-id="f1834-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="f1834-182">![Wynik żądania interfejsu API sieci Web jest wyświetlany w widoku sieci](build-restful-apis-with-aspnet-web-api/_static/image7.png "wynik żądania interfejsu API sieci Web jest wyświetlany w widoku sieci")</span><span class="sxs-lookup"><span data-stu-id="f1834-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="f1834-183">*Wynik żądania interfejsu API sieci Web jest wyświetlany w widoku sieci*</span><span class="sxs-lookup"><span data-stu-id="f1834-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1834-184">Internet Explorer 10 domyślne zachowanie będzie w tym momencie do żądania, jeśli użytkownik chce zapisać lub otworzyć strumienia, w wyniku wywołania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="f1834-185">Dane wyjściowe będą pliku tekstowego zawierającego JSON wynikiem wywołania adres URL interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="f1834-186">Okno dialogowe nie Anuluj aby można było oglądać zawartości odpowiedzi, za pomocą okna narzędzia dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="f1834-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="f1834-187">Kliknij przycisk **przejdź do widoku szczegółowe** przycisk, aby zobaczyć bardziej szczegółowe informacje o odpowiedzi to wywołanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f1834-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="f1834-188">![Przełącz na widok szczegółowy](build-restful-apis-with-aspnet-web-api/_static/image8.png "Przełącz do widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="f1834-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="f1834-189">*Przełącz na widok szczegółowy*</span><span class="sxs-lookup"><span data-stu-id="f1834-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="f1834-190">Kliknij przycisk **treść odpowiedzi** kartę, aby wyświetlić rzeczywiste tekstu JSON w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f1834-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="f1834-191">![Wyświetlanie kodu JSON output tekstu w Monitorze sieci](build-restful-apis-with-aspnet-web-api/_static/image9.png "wyświetlanie JSON output tekstu w Monitorze sieci")</span><span class="sxs-lookup"><span data-stu-id="f1834-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="f1834-192">*Wyświetlanie tekstu wyjściowego JSON w Monitorze sieci*</span><span class="sxs-lookup"><span data-stu-id="f1834-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="f1834-193">Zadanie 3. tworzenie modeli kontaktów i rozszerzyć kontrolera kontaktów</span><span class="sxs-lookup"><span data-stu-id="f1834-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="f1834-194">W ramach tego zadania spowoduje utworzenie klasy kontrolera, w których będą znajdować się metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f1834-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f1834-195">Kliknij prawym przyciskiem myszy **modele** i wybierz polecenie **Dodaj | Klasy...**  z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="f1834-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f1834-196">![Dodawanie nowego modelu aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image10.png "dodanie nowego modelu aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="f1834-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="f1834-197">*Dodawanie nowego modelu aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="f1834-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="f1834-198">W **Dodaj nowy element** okna dialogowego, nazwę nowego pliku **Contact.cs** i kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="f1834-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="f1834-199">![Tworzenie nowego pliku klasy skontaktuj się z](build-restful-apis-with-aspnet-web-api/_static/image11.png "Tworzenie nowego pliku klasy kontaktu")</span><span class="sxs-lookup"><span data-stu-id="f1834-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="f1834-200">*Tworzenie nowego pliku klasy kontaktu*</span><span class="sxs-lookup"><span data-stu-id="f1834-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="f1834-201">Dodaj następujący wyróżniony kod, aby **skontaktuj się z** klasy.</span><span class="sxs-lookup"><span data-stu-id="f1834-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="f1834-202">(Fragment - kodu *sieci Web, skontaktuj się z pomocą klasy interfejsu API laboratorium - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="f1834-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. <span data-ttu-id="f1834-203">W **ContactController** klasy, zaznacz słowo **ciąg** w definicji metody **uzyskać** metody i typu wyrazu *skontaktuj się z*.</span><span class="sxs-lookup"><span data-stu-id="f1834-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="f1834-204">Po słowie jest wpisany, wskaźnik będą wyświetlane na początku słowo **skontaktuj się z**.</span><span class="sxs-lookup"><span data-stu-id="f1834-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="f1834-205">Albo przytrzymaj **Ctrl** klucza i naciśnij klawisz kropki (.) lub kliknij ikonę, aby otworzyć okno dialogowe Pomoc w edytorze kodu, aby automatycznie wypełnić przy użyciu myszy **przy użyciu** dyrektywy dla modeli przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="f1834-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Przy użyciu Pomocy Intellisense dla deklaracji przestrzeni nazw](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="f1834-207">*Przy użyciu Pomocy Intellisense dla deklaracji przestrzeni nazw*</span><span class="sxs-lookup"><span data-stu-id="f1834-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="f1834-208">Zmodyfikuj kod **uzyskać** metodę, tak że zwraca tablicę wystąpień modelu kontaktu.</span><span class="sxs-lookup"><span data-stu-id="f1834-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="f1834-209">(Fragment - kodu *laboratorium interfejsu API sieci Web - Ex01 - zwracanie listy kontaktów*)</span><span class="sxs-lookup"><span data-stu-id="f1834-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="f1834-210">Naciśnij klawisz **F5** do debugowania aplikacji sieci web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f1834-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="f1834-211">Aby wyświetlić zmiany wprowadzone w danych wyjściowych odpowiedzi interfejsu API, wykonaj następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="f1834-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="f1834-212">Po otwarciu w przeglądarce naciśnij **F12** Jeśli narzędzi deweloperskich nie są jeszcze dostępne.</span><span class="sxs-lookup"><span data-stu-id="f1834-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="f1834-213">Kliknij przycisk **sieci** kartę.</span><span class="sxs-lookup"><span data-stu-id="f1834-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="f1834-214">Naciśnij klawisz **Start Przechwytywanie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1834-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="f1834-215">Dodaj sufiks adresu URL **/api/kontaktu** do adresu URL na pasku adresu i naciśnij klawisz **Enter** klucza.</span><span class="sxs-lookup"><span data-stu-id="f1834-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="f1834-216">Naciśnij klawisz **przejdź do widoku szczegółowe** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1834-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="f1834-217">Wybierz **treść odpowiedzi** kartę. Powinny pojawić się reprezentujący Zserializowany formę tablicę wystąpień skontaktuj się z ciągu JSON.</span><span class="sxs-lookup"><span data-stu-id="f1834-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="f1834-218">![Dane wyjściowe złożonych wywołanie metody interfejsu API sieci Web serializacji JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializowane dane wyjściowe złożonych wywołanie metody interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="f1834-219">*Dane wyjściowe JSON serializować złożona wywołanie metody interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="f1834-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="f1834-220">Zadanie 4 — wyodrębnianie funkcji do warstwy usług</span><span class="sxs-lookup"><span data-stu-id="f1834-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="f1834-221">To zadanie będzie pokazują, jak można wyodrębnić funkcji do warstwy usług, aby ułatwić deweloperom oddzielić ich funkcji usługi z warstwy kontrolera, umożliwiając możliwość ponownego wykorzystania usług, które faktycznie pracę.</span><span class="sxs-lookup"><span data-stu-id="f1834-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="f1834-222">Utwórz nowy folder w katalogu głównym rozwiązania i nadaj jej nazwę **usług**.</span><span class="sxs-lookup"><span data-stu-id="f1834-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="f1834-223">Aby to zrobić, kliknij prawym przyciskiem myszy **ContactManager** projektu, zaznacz **Dodaj** | **nowy Folder**, nadaj jej nazwę *usług*.</span><span class="sxs-lookup"><span data-stu-id="f1834-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="f1834-224">![Tworzenie folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image14.png "usługi tworzenia folderu")</span><span class="sxs-lookup"><span data-stu-id="f1834-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="f1834-225">*Tworzenie folderu usługi*</span><span class="sxs-lookup"><span data-stu-id="f1834-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="f1834-226">Kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj | Klasy...**  z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="f1834-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f1834-227">![Dodawanie nowej klasy do folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image15.png "Dodawanie nowej klasy do folderu usługi")</span><span class="sxs-lookup"><span data-stu-id="f1834-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="f1834-228">*Dodawanie nowej klasy do folderu usługi*</span><span class="sxs-lookup"><span data-stu-id="f1834-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="f1834-229">Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe nowej klasie nazwę **ContactRepository** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f1834-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="f1834-230">![Tworzenie pliku klasy zawiera kod dla warstwy usług skontaktuj się z repozytorium](build-restful-apis-with-aspnet-web-api/_static/image16.png "tworzenia pliku klasy zawiera kod dla warstwy usług skontaktuj się z repozytorium")</span><span class="sxs-lookup"><span data-stu-id="f1834-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="f1834-231">*Tworzenie pliku klasy zawiera kod dla warstwy usług skontaktuj się z repozytorium*</span><span class="sxs-lookup"><span data-stu-id="f1834-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="f1834-232">Dodaj using dyrektywy do **ContactRepository.cs** pliku, aby uwzględnić modeli przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f1834-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. <span data-ttu-id="f1834-233">Dodaj następujący wyróżniony kod, aby **ContactRepository.cs** plik, aby zaimplementować metodę GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="f1834-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="f1834-234">(Fragment - kodu *sieci Web interfejsu API laboratorium - Ex01 - kontaktu repozytorium*)</span><span class="sxs-lookup"><span data-stu-id="f1834-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="f1834-235">Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="f1834-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f1834-236">Dodaj następującą instrukcję using do sekcji deklaracji przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="f1834-236">Add the following using statement to the namespace declaration section of the file.</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. <span data-ttu-id="f1834-237">Dodaj następujący wyróżniony kod, aby **ContactController.cs** klasa do dodania pole prywatne reprezentujący wystąpienie repozytorium, dzięki czemu rest klasy członkowie mogą wprowadzać implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="f1834-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="f1834-238">(Fragment - kodu *kontaktu Kontroler interfejsu API laboratorium - Ex01 - Web*)</span><span class="sxs-lookup"><span data-stu-id="f1834-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="f1834-239">Zmiany **uzyskać** metodę, tak że sprawia, że korzystanie z usługi kontaktów repozytorium.</span><span class="sxs-lookup"><span data-stu-id="f1834-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="f1834-240">(Fragment - kodu *laboratorium interfejsu API sieci Web - Ex01 - zwracanie listy kontaktów za pośrednictwem repozytorium*)</span><span class="sxs-lookup"><span data-stu-id="f1834-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="f1834-241">Umieść punkt przerwania **ContactController**w **uzyskać** definicję metody.</span><span class="sxs-lookup"><span data-stu-id="f1834-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="f1834-242">![Dodawanie punktów przerwania do kontaktów kontrolera](build-restful-apis-with-aspnet-web-api/_static/image17.png "Dodawanie punktów przerwania do kontrolera kontaktów")</span><span class="sxs-lookup"><span data-stu-id="f1834-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="f1834-243">*Dodawanie punktów przerwania do kontrolera kontaktów*</span><span class="sxs-lookup"><span data-stu-id="f1834-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="f1834-244">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="f1834-245">Po otwarciu przeglądarki, naciśnij klawisz **F12** można otworzyć narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="f1834-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="f1834-246">Kliknij przycisk **sieci** kartę.</span><span class="sxs-lookup"><span data-stu-id="f1834-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="f1834-247">Kliknij przycisk **Start Przechwytywanie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1834-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="f1834-248">Dołącz adres URL na pasku adresu z sufiksem **/api/kontaktu** i naciśnij klawisz **Enter** załadować Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f1834-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="f1834-249">Program Visual Studio 2012 należy przerywaj raz **uzyskać** metody rozpoczyna się wykonanie.</span><span class="sxs-lookup"><span data-stu-id="f1834-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="f1834-250">![Przerwanie w obrębie metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "krytyczne w obrębie metody Get")</span><span class="sxs-lookup"><span data-stu-id="f1834-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="f1834-251">*Podziału wewnątrz metody Get*</span><span class="sxs-lookup"><span data-stu-id="f1834-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="f1834-252">Naciśnij klawisz **F5** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="f1834-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="f1834-253">Wróć do programu Internet Explorer, jeśli nie jest już fokusu.</span><span class="sxs-lookup"><span data-stu-id="f1834-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="f1834-254">Należy pamiętać, okno przechwytywania sieci.</span><span class="sxs-lookup"><span data-stu-id="f1834-254">Note the network capture window.</span></span>

    <span data-ttu-id="f1834-255">![Sieci widoku w programie Internet Explorer wyświetlanie wyników wywołania interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "sieci widoku w programie Internet Explorer wyświetlanie wyników wywołania interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="f1834-256">*Widok sieci w programie Internet Explorer wyświetlanie wyników wywołania interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="f1834-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="f1834-257">Kliknij przycisk **przejdź do widoku szczegółowe** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1834-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="f1834-258">Kliknij przycisk **treść odpowiedzi** kartę. Należy pamiętać, dane wyjściowe JSON wywołania interfejsu API i jak reprezentuje dwa kontakty pobierane przez warstwę usług.</span><span class="sxs-lookup"><span data-stu-id="f1834-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="f1834-259">![Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia developer](build-restful-apis-with-aspnet-web-api/_static/image20.png "wyświetlania danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia Projektant")</span><span class="sxs-lookup"><span data-stu-id="f1834-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="f1834-260">*Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia Projektant*</span><span class="sxs-lookup"><span data-stu-id="f1834-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="f1834-261">Ćwiczenie 2: Tworzenie składnika Web API odczytu/zapisu</span><span class="sxs-lookup"><span data-stu-id="f1834-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="f1834-262">W tym ćwiczeniu będzie implementowany POST i PUT metody kontaktu Menedżera ją włączyć funkcje edycji danych.</span><span class="sxs-lookup"><span data-stu-id="f1834-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="f1834-263">Zadanie 1 - otwierania projektu interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1834-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="f1834-264">W tym zadaniu przygotujesz się do zwiększenia projektu interfejsu API sieci Web utworzony w ćwiczenie 1, aby umożliwić akceptowanie danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f1834-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="f1834-265">Uruchom **programu Visual Studio 2012 Express for Web**, aby zrobić to przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f1834-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f1834-266">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex02-ReadWriteWebAPI/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="f1834-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f1834-267">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="f1834-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f1834-268">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="f1834-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f1834-269">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f1834-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f1834-270">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="f1834-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f1834-271">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="f1834-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f1834-272">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f1834-273">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f1834-274">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="f1834-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f1834-275">Otwórz **Services/ContactRepository.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="f1834-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="f1834-276">Zadanie 2 — Dodawanie funkcji trwałości danych do implementacji repozytorium kontaktów</span><span class="sxs-lookup"><span data-stu-id="f1834-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="f1834-277">W tym zadaniu zostanie rozszerzyć klasy ContactRepository projektu interfejsu API sieci Web utworzone w 1 wykonywania tak, aby można utrwalić i akceptuje dane wejściowe użytkownika i nowe wystąpienia kontaktu.</span><span class="sxs-lookup"><span data-stu-id="f1834-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="f1834-278">Dodaj następujące stała się **ContactRepository** klasy do reprezentowania nazwy sieci web serwera pamięci podręcznej elementu nazwę klucza później w tym ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="f1834-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="f1834-279">Dodaj Konstruktor do **ContactRepository** zawierający następujący kod.</span><span class="sxs-lookup"><span data-stu-id="f1834-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="f1834-280">(Fragment - kodu *sieci Web interfejsu API laboratorium - Ex02 - kontaktu repozytorium konstruktora*)</span><span class="sxs-lookup"><span data-stu-id="f1834-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="f1834-281">Zmodyfikuj kod **GetAllContacts** — metoda, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f1834-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="f1834-282">(Fragment - kodu *laboratorium interfejsu API sieci Web - Ex02 - pobrania wszystkich kontaktów*)</span><span class="sxs-lookup"><span data-stu-id="f1834-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="f1834-283">W tym przykładzie jest dla celów demonstracyjnych i użyje pamięci podręcznej serwera sieci web jako nośniku, dzięki czemu wartości będzie być dostępne dla wielu klientów jednocześnie, a nie za pomocą mechanizmu magazynowania sesji lub istnienia magazynu żądania.</span><span class="sxs-lookup"><span data-stu-id="f1834-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="f1834-284">Co można użyć programu Entity Framework, magazynu XML lub innych różnych zamiast pamięci podręcznej serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="f1834-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="f1834-285">Zaimplementuj nową metodę o nazwie **SaveContact** do **ContactRepository** klasy pracy zapisywania kontaktu.</span><span class="sxs-lookup"><span data-stu-id="f1834-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="f1834-286">**SaveContact** metoda powinna przyjmować jeden **skontaktuj się z** parametr i zwracany Boolean wartość wskazuje powodzenie lub niepowodzenie.</span><span class="sxs-lookup"><span data-stu-id="f1834-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="f1834-287">(Fragment - kodu *sieci Web interfejsu API laboratorium - Ex02 - implementacja metody SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="f1834-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="f1834-288">Ćwiczenie 3: Korzystanie z interfejsu API sieci Web z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="f1834-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="f1834-289">W tym ćwiczeniu utworzysz klienta HTML do wywołania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="f1834-290">Ten klient ułatwi wymiana danych z interfejsu API sieci Web przy użyciu języka JavaScript i wyświetli wyniki w przeglądarce sieci web przy użyciu znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="f1834-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="f1834-291">Zadanie 1 - zmodyfikowanie widoku indeksu zapewnienie graficznego interfejsu użytkownika do wyświetlania kontaktów</span><span class="sxs-lookup"><span data-stu-id="f1834-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="f1834-292">To zadanie należy zmodyfikować domyślny widok indeksu aplikacji sieci web do obsługi wymagań wyświetlania listy istniejących w przeglądarce HTML.</span><span class="sxs-lookup"><span data-stu-id="f1834-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="f1834-293">Otwórz **programu Visual Studio 2012 Express for Web** Jeśli nie jest już otwarty.</span><span class="sxs-lookup"><span data-stu-id="f1834-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="f1834-294">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex03-ConsumingWebAPI/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="f1834-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f1834-295">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="f1834-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f1834-296">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="f1834-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f1834-297">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f1834-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f1834-298">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="f1834-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f1834-299">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="f1834-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f1834-300">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f1834-301">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="f1834-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f1834-302">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="f1834-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f1834-303">Otwórz **Index.cshtml** znajdującym się w **widoków domowych** folderu.</span><span class="sxs-lookup"><span data-stu-id="f1834-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="f1834-304">Zastąp kod HTML w elemencie div o identyfikatorze **treści** tak, aby wygląda podobnie do następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="f1834-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. <span data-ttu-id="f1834-305">Dodaj następujący kod Javascript w dolnej części pliku do wykonania żądania HTTP do interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. <span data-ttu-id="f1834-306">Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="f1834-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f1834-307">Umieść punkt przerwania na **uzyskać** metody **ContactController** klasy.</span><span class="sxs-lookup"><span data-stu-id="f1834-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="f1834-308">![Umieszczenie punkt przerwania w metodzie Get kontrolera interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image21.png "umieszczenie punkt przerwania w metodzie Get kontrolera interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="f1834-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="f1834-309">*Umieszczenie punkt przerwania w metodzie Get kontrolera interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="f1834-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="f1834-310">Naciśnij klawisz **F5** uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="f1834-310">Press **F5** to run the project.</span></span> <span data-ttu-id="f1834-311">Przeglądarka pobierze dokumentu HTML.</span><span class="sxs-lookup"><span data-stu-id="f1834-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1834-312">Upewnij się, że przeglądania do głównego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="f1834-313">Po pobraniu strony i wykonuje kod JavaScript, punkt przerwania zostanie uruchomiona i wykonywanie kodu zostanie wstrzymane w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="f1834-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="f1834-314">![Debugowanie w wywołaniach interfejsu API sieci Web programu VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debugowania do wywołania interfejsu API sieci Web programu VS Express for Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="f1834-315">*Debugowanie w wywołaniu interfejsu API sieci Web programu Visual Studio 2012 Express for Web*</span><span class="sxs-lookup"><span data-stu-id="f1834-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="f1834-316">Usuń punkt przerwania i naciśnij klawisz **F5** lub narzędzi debugowania **Kontynuuj** przycisk, aby kontynuować ładowanie widoku w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f1834-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="f1834-317">Po zakończeniu wywołania interfejsu API sieci Web powinna zostać wyświetlona kontaktów zwrócony z interfejsu API sieci Web wywołać wyświetlane jako elementy listy w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f1834-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="f1834-318">![Wyniki wywołania interfejsu API w przeglądarce było wyświetlane jako elementy listy](build-restful-apis-with-aspnet-web-api/_static/image23.png "wyników wywołania interfejsu API w przeglądarce było wyświetlane jako elementy listy")</span><span class="sxs-lookup"><span data-stu-id="f1834-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="f1834-319">*Wyniki wywołania interfejsu API w przeglądarce było wyświetlane jako elementy listy*</span><span class="sxs-lookup"><span data-stu-id="f1834-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="f1834-320">Zatrzymaj debugowanie.</span><span class="sxs-lookup"><span data-stu-id="f1834-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="f1834-321">Zadanie 2 - zmodyfikowanie widoku indeksu zapewnienie graficzny interfejs użytkownika służący do tworzenia kontaktów</span><span class="sxs-lookup"><span data-stu-id="f1834-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="f1834-322">To zadanie będzie do modyfikowania widoku indeksu aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="f1834-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="f1834-323">Formularz zostanie dodany do strony HTML, który będzie przechwytywanie danych wejściowych użytkownika, a następnie wysłać je do interfejsu API sieci Web, aby utworzyć nowy kontakt i nowa metoda kontrolera interfejsu API sieci Web zostanie utworzone w celu zbierania daty z graficznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f1834-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="f1834-324">Otwórz **ContactController.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="f1834-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="f1834-325">Dodaj nową metodę do klasy kontrolera o nazwie **Post** jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="f1834-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="f1834-326">(Fragment - kodu *sieci Web interfejsu API laboratorium - Ex03 - metody Post*)</span><span class="sxs-lookup"><span data-stu-id="f1834-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. <span data-ttu-id="f1834-327">Otwórz **Index.cshtml** plik w programie Visual Studio, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="f1834-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="f1834-328">Dodaj poniższy kod HTML do pliku zaraz po nieuporządkowaną listę, dodanym w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="f1834-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. <span data-ttu-id="f1834-329">W elemencie skryptu w dolnej części dokumentu Dodaj następujący kod wyróżnione do obsługi zdarzeń kliknięcia przycisków, których zostanie wysłany danych do interfejsu API sieci Web przy użyciu wywołania protokołu HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f1834-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="f1834-330">W **ContactController.cs**, umieść punkt przerwania na **Post** metody.</span><span class="sxs-lookup"><span data-stu-id="f1834-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="f1834-331">Naciśnij klawisz **F5** Aby uruchomić aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f1834-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="f1834-332">Po załadowaniu w przeglądarce stronę wpisz nową nazwę kontaktu i identyfikator i kliknij przycisk **zapisać** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1834-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="f1834-333">![Klient HTML dokument załadowaniu w przeglądarce](build-restful-apis-with-aspnet-web-api/_static/image24.png "dokument klienta HTML załadowaniu w przeglądarce")</span><span class="sxs-lookup"><span data-stu-id="f1834-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="f1834-334">*Dokument HTML klienta załadowaniu w przeglądarce*</span><span class="sxs-lookup"><span data-stu-id="f1834-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="f1834-335">Gdy Dzielenie okna debugera w **Post** metody, podejmij Zobacz we właściwościach **skontaktuj się z** parametru.</span><span class="sxs-lookup"><span data-stu-id="f1834-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="f1834-336">Wartości powinna być zgodna wprowadzonych w formularzu.</span><span class="sxs-lookup"><span data-stu-id="f1834-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="f1834-337">![Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "obiektu kontaktu są wysyłane do interfejsu API sieci Web z klienta")</span><span class="sxs-lookup"><span data-stu-id="f1834-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="f1834-338">*Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta*</span><span class="sxs-lookup"><span data-stu-id="f1834-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="f1834-339">Krok za pomocą metody w debugerze do **odpowiedzi** zmiennej został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f1834-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="f1834-340">W trakcie kontroli w **zmiennych lokalnych** okna w debugerze, zobaczysz, że wszystkie właściwości ustawione.</span><span class="sxs-lookup"><span data-stu-id="f1834-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="f1834-341">![Po utworzeniu w debugerze odpowiedzi](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpowiedzi po utworzeniu w debugerze")</span><span class="sxs-lookup"><span data-stu-id="f1834-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="f1834-342">*Odpowiedź po utworzeniu w debugerze*</span><span class="sxs-lookup"><span data-stu-id="f1834-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="f1834-343">Jeśli naciśniesz **F5** lub kliknij przycisk **Kontynuuj** w debugerze żądanie zostanie ukończone.</span><span class="sxs-lookup"><span data-stu-id="f1834-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="f1834-344">Po przełączeniu do przeglądarki, dodano nowy kontakt do listy kontaktów przechowywanych przez **ContactRepository** implementacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="f1834-345">![Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia skontaktuj się z pomocą](build-restful-apis-with-aspnet-web-api/_static/image27.png "przeglądarki odzwierciedla pomyślnym utworzeniu nowego wystąpienia kontaktów")</span><span class="sxs-lookup"><span data-stu-id="f1834-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="f1834-346">*Przeglądarka odzwierciedla pomyślnym utworzeniu nowego wystąpienia kontaktów*</span><span class="sxs-lookup"><span data-stu-id="f1834-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="f1834-347">Ponadto można wdrożyć tę aplikację Azure następujących [dodatek C: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="f1834-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f1834-348">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f1834-348">Summary</span></span>

<span data-ttu-id="f1834-349">To laboratorium ma wprowadzone do nowej struktury ASP.NET Web API i wykonania RESTful interfejsów API sieci Web za pomocą środowiska.</span><span class="sxs-lookup"><span data-stu-id="f1834-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="f1834-350">W tym miejscu możesz utworzyć nowe repozytorium, który ułatwia trwałości danych przy użyciu dowolnej liczby mechanizmów i okablować usługi zapasową zamiast istnieje tylko jedna użyte jako przykłady w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="f1834-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="f1834-351">Interfejs API sieci Web obsługuje szereg dodatkowych funkcji, takich jak umożliwiające komunikację z klientami z systemem innym niż HTML zapisane w dowolnym języku obsługującego protokół HTTP i JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="f1834-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="f1834-352">Może obsługiwać interfejs API sieci Web poza aplikacją sieci web typowe możliwe jest również, jak również jest możliwość tworzenia własnych formatów serializacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="f1834-353">Witryny sieci Web ASP.NET jest przeznaczona do framework interfejsu API sieci Web platformy ASP.NET w [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f1834-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="f1834-354">Ta lokacja będzie dostarczać, najnowsze informacje, przykłady i informacje związane z interfejsu API sieci Web, więc spróbuj on często Jeśli chcesz delve więcej danych w kompozycji tworzenia niestandardowych interfejsów API sieci Web dostępnych praktycznie dowolne urządzenie lub programowanie Framework.</span><span class="sxs-lookup"><span data-stu-id="f1834-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="f1834-355">Dodatek A: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="f1834-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="f1834-356">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="f1834-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f1834-357">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="f1834-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f1834-358">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](build-restful-apis-with-aspnet-web-api/_static/image28.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="f1834-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f1834-359">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="f1834-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="f1834-360">Aby dodać fragment kodu za pomocą klawiatury (C# tylko)</span><span class="sxs-lookup"><span data-stu-id="f1834-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="f1834-361">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="f1834-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f1834-362">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="f1834-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f1834-363">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="f1834-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f1834-364">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="f1834-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f1834-365">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="f1834-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="f1834-366">![Rozpocznij wpisywanie nazwy fragment](build-restful-apis-with-aspnet-web-api/_static/image29.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="f1834-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="f1834-367">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="f1834-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="f1834-368">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](build-restful-apis-with-aspnet-web-api/_static/image30.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="f1834-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="f1834-369">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="f1834-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="f1834-370">![Ponownie naciśnij klawisz Tab i fragment rozwinie](build-restful-apis-with-aspnet-web-api/_static/image31.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="f1834-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="f1834-371">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="f1834-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="f1834-372">Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)</span><span class="sxs-lookup"><span data-stu-id="f1834-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="f1834-373">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="f1834-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="f1834-374">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="f1834-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="f1834-375">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="f1834-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="f1834-376">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="f1834-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="f1834-377">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="f1834-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="f1834-378">![Wybierz odpowiedni fragment z listy, klikając go](build-restful-apis-with-aspnet-web-api/_static/image33.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="f1834-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="f1834-379">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="f1834-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f1834-380">Dodatek B: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="f1834-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f1834-381">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f1834-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f1834-382">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="f1834-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f1834-383">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="f1834-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f1834-384">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f1834-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f1834-385">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="f1834-385">Click on **Install Now**.</span></span> <span data-ttu-id="f1834-386">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="f1834-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f1834-387">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="f1834-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f1834-388">![Instalowanie programu Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f1834-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f1834-389">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f1834-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f1834-390">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="f1834-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="f1834-392">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="f1834-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f1834-393">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-393">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="f1834-395">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="f1834-395">*Installation progress*</span></span>
6. <span data-ttu-id="f1834-396">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="f1834-396">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="f1834-398">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="f1834-398">*Installation completed*</span></span>
7. <span data-ttu-id="f1834-399">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f1834-400">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="f1834-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="f1834-402">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="f1834-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f1834-403">Dodatek C: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f1834-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f1834-404">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu Azure i opublikować aplikację, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczany przez platformę Azure.</span><span class="sxs-lookup"><span data-stu-id="f1834-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="f1834-405">Zadanie 1 — Tworzenie nowej witryny sieci Web w portalu Azure</span><span class="sxs-lookup"><span data-stu-id="f1834-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="f1834-406">Przejdź do [portalu zarządzania Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="f1834-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1834-407">Przy użyciu platformy Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="f1834-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f1834-408">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="f1834-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f1834-409">![Zaloguj się do portalu Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f1834-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f1834-410">*Zaloguj się do portalu*</span><span class="sxs-lookup"><span data-stu-id="f1834-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="f1834-411">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="f1834-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f1834-412">![Tworzenie nowej witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f1834-413">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="f1834-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f1834-414">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="f1834-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f1834-415">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="f1834-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="f1834-416">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="f1834-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1834-417">Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="f1834-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f1834-418">Opcja szybkie tworzenie umożliwia wdrażanie aplikacji sieci web ukończone do platformy Azure z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="f1834-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="f1834-419">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1834-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f1834-420">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](build-restful-apis-with-aspnet-web-api/_static/image41.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="f1834-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f1834-421">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="f1834-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f1834-422">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="f1834-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f1834-423">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="f1834-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f1834-424">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1834-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f1834-425">![Przeglądanie do nowej witryny sieci web](build-restful-apis-with-aspnet-web-api/_static/image42.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="f1834-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f1834-426">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="f1834-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f1834-427">![Witryna sieci Web działa](build-restful-apis-with-aspnet-web-api/_static/image43.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="f1834-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="f1834-428">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="f1834-428">*Web site running*</span></span>
6. <span data-ttu-id="f1834-429">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="f1834-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f1834-430">![Otwieranie stron witryny sieci web zarządzania](build-restful-apis-with-aspnet-web-api/_static/image44.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="f1834-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f1834-431">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="f1834-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f1834-432">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="f1834-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1834-433">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="f1834-434">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f1834-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="f1834-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="f1834-436">![Pobieranie witryny sieci web profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image45.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="f1834-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f1834-437">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="f1834-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f1834-438">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f1834-439">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web na platformie Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="f1834-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="f1834-440">![Zapisywanie pliku profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image46.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="f1834-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f1834-441">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="f1834-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f1834-442">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="f1834-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f1834-443">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="f1834-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f1834-444">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="f1834-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f1834-445">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1834-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f1834-446">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **pulpitu nawigacyjnego serwera**.</span><span class="sxs-lookup"><span data-stu-id="f1834-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f1834-447">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="f1834-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f1834-448">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="f1834-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f1834-449">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="f1834-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f1834-450">![Pulpit nawigacyjny serwera bazy danych SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="f1834-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f1834-451">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="f1834-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f1834-452">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="f1834-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f1834-453">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1834-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Dodawanie adresu IP klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="f1834-455">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="f1834-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f1834-456">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="f1834-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="f1834-458">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="f1834-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f1834-459">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f1834-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f1834-460">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f1834-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f1834-461">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="f1834-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f1834-462">![Publikowanie aplikacji](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="f1834-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="f1834-463">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="f1834-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="f1834-464">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="f1834-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f1834-465">![Importowanie profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="f1834-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f1834-466">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="f1834-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="f1834-467">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="f1834-467">Click **Validate Connection**.</span></span> <span data-ttu-id="f1834-468">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="f1834-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1834-469">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="f1834-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f1834-470">![Sprawdzanie poprawności połączenia](build-restful-apis-with-aspnet-web-api/_static/image53.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="f1834-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="f1834-471">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="f1834-471">*Validating connection*</span></span>
4. <span data-ttu-id="f1834-472">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="f1834-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f1834-473">![Konfiguracja narzędzia Web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="f1834-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f1834-474">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="f1834-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f1834-475">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f1834-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f1834-476">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="f1834-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f1834-477">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="f1834-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f1834-478">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="f1834-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f1834-479">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="f1834-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="f1834-480">![Konfigurowanie parametrów połączenia z lokalizacją docelową](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="f1834-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f1834-481">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="f1834-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f1834-482">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1834-482">Then click **OK**.</span></span> <span data-ttu-id="f1834-483">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="f1834-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f1834-484">![Tworzenie bazy danych](build-restful-apis-with-aspnet-web-api/_static/image56.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="f1834-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="f1834-485">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="f1834-485">*Creating the database*</span></span>
7. <span data-ttu-id="f1834-486">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="f1834-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f1834-487">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="f1834-487">Then click **Next**.</span></span>

    <span data-ttu-id="f1834-488">![Parametry połączenia wskazujące bazę danych SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="f1834-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f1834-489">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="f1834-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f1834-490">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="f1834-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f1834-491">![Publikowanie aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="f1834-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="f1834-492">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="f1834-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="f1834-493">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="f1834-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="f1834-494">![Aplikacja opublikowana w systemie Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f1834-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="f1834-495">*Aplikacja opublikowana na platformie Azure*</span><span class="sxs-lookup"><span data-stu-id="f1834-495">*Application published to Azure*</span></span>
