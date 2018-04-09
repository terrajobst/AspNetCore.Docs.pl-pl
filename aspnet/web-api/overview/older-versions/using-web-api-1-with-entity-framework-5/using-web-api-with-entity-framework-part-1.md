---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Część 1: Omówienie i tworzenia projektu | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="6b9ee-102">Część 1: Omówienie i tworzenia projektu</span><span class="sxs-lookup"><span data-stu-id="6b9ee-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="6b9ee-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6b9ee-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6b9ee-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="6b9ee-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="6b9ee-105">Entity Framework jest obiekt/relacyjne framework mapowania.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="6b9ee-106">Mapowania obiektów domeny w kodzie jednostek relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="6b9ee-107">W większości przypadków nie masz martwić się o warstwie bazy danych, ponieważ Entity Framework zajmuje się go dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="6b9ee-108">Kod manipuluje obiektami, a zmiany są umieszczone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="6b9ee-109">Dotyczące samouczka</span><span class="sxs-lookup"><span data-stu-id="6b9ee-109">About the Tutorial</span></span>

<span data-ttu-id="6b9ee-110">W tym samouczku utworzysz aplikacją ze sklepu proste.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="6b9ee-111">Istnieją dwie główne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-111">There are two main parts to the application.</span></span> <span data-ttu-id="6b9ee-112">Normalne użytkownicy mogą wyświetlać produktów i tworzenie zleceń:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="6b9ee-113">Administratorzy mogą tworzyć, usuwać lub edytować produktów:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="6b9ee-114">Dowiesz się umiejętności</span><span class="sxs-lookup"><span data-stu-id="6b9ee-114">Skills You'll Learn</span></span>

<span data-ttu-id="6b9ee-115">Oto dowiesz się:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="6b9ee-116">Jak korzystać z interfejsu API sieci Web platformy ASP.NET Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="6b9ee-117">Jak używać knockout.js do tworzenia dynamicznego interfejsu użytkownika klienta.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="6b9ee-118">Jak używać uwierzytelniania formularzy z interfejsu API sieci Web do uwierzytelniania użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="6b9ee-119">Mimo że w tym samouczku są niezależne, warto najpierw przeczytać następujące samouczki:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="6b9ee-120">Pierwszy ASP.NET interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="6b9ee-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="6b9ee-121">Tworzenie składnika Web API obsługującego operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="6b9ee-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="6b9ee-122">Niektóre znajomość [ASP.NET MVC](../../../../mvc/index.md) jest również przydatne.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="6b9ee-123">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6b9ee-123">Overview</span></span>

<span data-ttu-id="6b9ee-124">Na wysokim poziomie Oto architektury aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="6b9ee-125">ASP.NET MVC wygeneruje stron HTML dla klienta.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="6b9ee-126">Interfejs API sieci Web ASP.NET udostępnia operacje CRUD na danych (produktów i zamówienia).</span><span class="sxs-lookup"><span data-stu-id="6b9ee-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="6b9ee-127">Entity Framework tłumaczy modeli C# używanych przez interfejs API sieci Web do bazy danych jednostki.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="6b9ee-128">Na poniższym diagramie przedstawiono, jak obiektów domeny znajdują się w różnych warstwach aplikacji: Warstwa bazy danych, model obiektów, a na końcu format danych przesyłanych w sieci, który jest używany do przesyłania danych do klienta za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="6b9ee-129">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b9ee-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="6b9ee-130">Można utworzyć projekt samouczka przy użyciu programu Visual Web Developer Express lub pełnej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="6b9ee-131">Z **Start** kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="6b9ee-132">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="6b9ee-133">W obszarze **Visual C#**, wybierz pozycję **Web**.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="6b9ee-134">Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="6b9ee-135">Nazwij projekt "ProductStore", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="6b9ee-136">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="6b9ee-137">Szablon "Aplikacji internetowej" służy do tworzenia aplikacji platformy ASP.NET MVC, który obsługuje uwierzytelnianie formularzy.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="6b9ee-138">Jeśli uruchomisz aplikację teraz już niektóre funkcje:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="6b9ee-139">Nowi użytkownicy mogą zarejestrować, klikając łącze "Register" w prawym górnym rogu.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="6b9ee-140">Zarejestrowani użytkownicy może się zalogować, klikając łącze "Zaloguj".</span><span class="sxs-lookup"><span data-stu-id="6b9ee-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="6b9ee-141">Informacje o członkostwie jest zachowywane w bazie danych, który jest tworzony automatycznie.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="6b9ee-142">Aby uzyskać więcej informacji na temat uwierzytelniania formularzy w programie ASP.NET MVC, zobacz [wskazówki: używanie uwierzytelniania formularzy w programie ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="6b9ee-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="6b9ee-143">Aktualizowanie pliku CSS</span><span class="sxs-lookup"><span data-stu-id="6b9ee-143">Update the CSS File</span></span>

<span data-ttu-id="6b9ee-144">Ten krok jest bardzo drobny, ale spowoduje to, że strony renderowania jak wcześniej zrzutów ekranu.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="6b9ee-145">W Eksploratorze rozwiązań rozwiń folder zawartości i Otwórz plik o nazwie Site.css.</span><span class="sxs-lookup"><span data-stu-id="6b9ee-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="6b9ee-146">Dodaj następujące style CSS:</span><span class="sxs-lookup"><span data-stu-id="6b9ee-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="6b9ee-147">Next</span><span class="sxs-lookup"><span data-stu-id="6b9ee-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
