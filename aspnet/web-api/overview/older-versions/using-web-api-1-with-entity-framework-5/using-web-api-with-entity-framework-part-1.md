---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Część 1: Omówienie i tworzenie projektu | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394064"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="602ca-102">Część 1: Omówienie i tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="602ca-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="602ca-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="602ca-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="602ca-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="602ca-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="602ca-105">Entity Framework to platforma mapowania obiektowo/relacyjny.</span><span class="sxs-lookup"><span data-stu-id="602ca-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="602ca-106">Obiektów domeny w kodzie jest on mapowany do jednostek w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="602ca-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="602ca-107">W większości przypadków nie masz już martwić się o warstwy bazy danych, ponieważ Entity Framework zajmuje się go dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="602ca-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="602ca-108">Twój kod manipuluje obiektami, a zmiany zostaną utrwalone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="602ca-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="602ca-109">Samouczek — informacje</span><span class="sxs-lookup"><span data-stu-id="602ca-109">About the Tutorial</span></span>

<span data-ttu-id="602ca-110">W tym samouczku utworzysz aplikację sklepu proste.</span><span class="sxs-lookup"><span data-stu-id="602ca-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="602ca-111">Istnieją dwie główne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="602ca-111">There are two main parts to the application.</span></span> <span data-ttu-id="602ca-112">Normalne użytkownicy mogą przeglądać produktów i tworzenie zamówień:</span><span class="sxs-lookup"><span data-stu-id="602ca-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="602ca-113">Administratorzy mogą tworzyć, usuń lub Edytuj produktów:</span><span class="sxs-lookup"><span data-stu-id="602ca-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="602ca-114">Umiejętności, których dowiesz się</span><span class="sxs-lookup"><span data-stu-id="602ca-114">Skills You'll Learn</span></span>

<span data-ttu-id="602ca-115">Oto, dowiesz się:</span><span class="sxs-lookup"><span data-stu-id="602ca-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="602ca-116">Jak używać programu Entity Framework za pomocą interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="602ca-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="602ca-117">Jak korzystać z użyciem knockout.js do tworzenia dynamicznego interfejsu użytkownika klienta.</span><span class="sxs-lookup"><span data-stu-id="602ca-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="602ca-118">Jak używać uwierzytelniania formularzy z interfejsu API sieci Web do uwierzytelniania użytkowników.</span><span class="sxs-lookup"><span data-stu-id="602ca-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="602ca-119">Mimo że w tym samouczku jest niezależny, warto najpierw przeczytać następujące samouczki:</span><span class="sxs-lookup"><span data-stu-id="602ca-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="602ca-120">Usługi pierwszego wzorca ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="602ca-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="602ca-121">Tworzenie internetowego interfejsu API obsługującego operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="602ca-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="602ca-122">Pewną wiedzę na temat [platformy ASP.NET MVC](../../../../mvc/index.md) jest również przydatna.</span><span class="sxs-lookup"><span data-stu-id="602ca-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="602ca-123">Omówienie</span><span class="sxs-lookup"><span data-stu-id="602ca-123">Overview</span></span>

<span data-ttu-id="602ca-124">Na wysokim poziomie poniżej przedstawiono architekturę aplikacji:</span><span class="sxs-lookup"><span data-stu-id="602ca-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="602ca-125">ASP.NET MVC wygeneruje stron HTML dla klienta.</span><span class="sxs-lookup"><span data-stu-id="602ca-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="602ca-126">ASP.NET Web API udostępnia operacje CRUD na danych (produkty i zamówienia).</span><span class="sxs-lookup"><span data-stu-id="602ca-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="602ca-127">Entity Framework tłumaczy modeli języka C# używane przez interfejs API sieci Web do jednostek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="602ca-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="602ca-128">Na poniższym diagramie przedstawiono, jak obiekty domeny są reprezentowane w różnych warstwach aplikacji: Warstwa bazy danych, model obiektów, a na końcu formatu o komunikacji sieciowej, który jest używany do przesyłania danych do klienta za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="602ca-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="602ca-129">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="602ca-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="602ca-130">Można utworzyć projekt samouczka przy użyciu programu Visual Web Developer Express lub pełnej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="602ca-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="602ca-131">Z **Start** kliknij **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="602ca-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="602ca-132">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="602ca-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="602ca-133">W obszarze **Visual C#**, wybierz opcję **Web**.</span><span class="sxs-lookup"><span data-stu-id="602ca-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="602ca-134">Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="602ca-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="602ca-135">Nadaj projektowi nazwę "ProductStore", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="602ca-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="602ca-136">W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="602ca-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="602ca-137">Szablon "Aplikacji internetowej" umożliwia utworzenie aplikacji platformy ASP.NET MVC, która obsługuje uwierzytelnianie formularzy.</span><span class="sxs-lookup"><span data-stu-id="602ca-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="602ca-138">Jeśli uruchomisz aplikację teraz, jest już niektóre funkcje:</span><span class="sxs-lookup"><span data-stu-id="602ca-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="602ca-139">Nowi użytkownicy mogą zarejestrować, klikając link "Register" w prawym górnym rogu.</span><span class="sxs-lookup"><span data-stu-id="602ca-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="602ca-140">Zarejestrowani użytkownicy mogą się zalogować, klikając link "Zaloguj".</span><span class="sxs-lookup"><span data-stu-id="602ca-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="602ca-141">Informacje o członkostwie są utrwalane w bazie danych, która zostanie utworzona automatycznie.</span><span class="sxs-lookup"><span data-stu-id="602ca-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="602ca-142">Aby uzyskać więcej informacji na temat uwierzytelniania formularzy we wzorcu ASP.NET MVC, zobacz [Instruktaż: używanie uwierzytelniania formularzy we wzorcu ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="602ca-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="602ca-143">Zaktualizuj plik CSS</span><span class="sxs-lookup"><span data-stu-id="602ca-143">Update the CSS File</span></span>

<span data-ttu-id="602ca-144">Ten krok jest kosmetycznych, ale spowoduje to, że strony renderowania, takich jak wcześniej zrzutów ekranu.</span><span class="sxs-lookup"><span data-stu-id="602ca-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="602ca-145">W Eksploratorze rozwiązań rozwiń folder zawartości, a następnie otwórz plik o nazwie pliku Site.css.</span><span class="sxs-lookup"><span data-stu-id="602ca-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="602ca-146">Dodaj następujące style CSS:</span><span class="sxs-lookup"><span data-stu-id="602ca-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="602ca-147">Next</span><span class="sxs-lookup"><span data-stu-id="602ca-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
