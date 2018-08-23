---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Tworzenie kontrolera (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak dodać kontrolera do aplikacji ASP.NET MVC.'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fe880775a5d477aefcf0fe6cedb3e719760ed90
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756333"
---
<a name="creating-a-controller-c"></a><span data-ttu-id="283db-103">Tworzenie kontrolera (C#)</span><span class="sxs-lookup"><span data-stu-id="283db-103">Creating a Controller (C#)</span></span>
====================
<span data-ttu-id="283db-104">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="283db-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="283db-105">W tym samouczku Walther Autor: Stephen pokazuje, jak dodać kontrolera do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="283db-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="283db-106">Celem tego samouczka jest wyjaśniają, jak utworzyć nowe platformy ASP.NET MVC kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="283db-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="283db-107">Dowiesz się, jak utworzyć kontrolerów, zarówno za pomocą opcji menu Visual Studio Dodaj kontroler, jak i przez utworzenie pliku klasy ręcznie.</span><span class="sxs-lookup"><span data-stu-id="283db-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="283db-108">Za pomocą Dodaj opcję Menu kontrolera</span><span class="sxs-lookup"><span data-stu-id="283db-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="283db-109">Najprostszym sposobem utworzenia nowego kontrolera jest kliknij prawym przyciskiem myszy folder kontrolerów, w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz **Dodaj, kontroler** opcji menu (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="283db-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="283db-110">Wybranie tej opcji menu otwiera **Dodaj kontroler** okna dialogowego (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="283db-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="283db-111">[![Okno dialogowe Nowy projekt](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="283db-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="283db-112">**Rysunek 01**: dodanie nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="283db-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>


<span data-ttu-id="283db-113">[![Okno dialogowe Nowy projekt](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="283db-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="283db-114">**Rysunek 02**: okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="283db-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>


<span data-ttu-id="283db-115">Należy zauważyć, że pierwsza część nazwy kontrolera jest wyróżniony na **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="283db-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="283db-116">Każda nazwa kontrolera musi kończyć się sufiksem *kontrolera*.</span><span class="sxs-lookup"><span data-stu-id="283db-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="283db-117">Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontroler o nazwie *produktu*.</span><span class="sxs-lookup"><span data-stu-id="283db-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="283db-118">Jeśli utworzysz kontroler, na których brakuje *kontrolera* sufiks, a następnie nie będzie można wywołać z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="283db-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="283db-119">Nie ma tych — mam już zmarnowany niezliczone godziny swojego życia po wprowadzeniu tego błędu.</span><span class="sxs-lookup"><span data-stu-id="283db-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="283db-120">**Wyświetlanie listy 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="283db-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="283db-121">Należy zawsze tworzyć kontrolerów w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="283db-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="283db-122">W przeciwnym razie będzie naruszenie Konwencji platformy ASP.NET MVC i inni deweloperzy mają coraz trudniejszy czasu, informacje o aplikacji.</span><span class="sxs-lookup"><span data-stu-id="283db-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="283db-123">Metody akcji tworzenia szkieletów</span><span class="sxs-lookup"><span data-stu-id="283db-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="283db-124">Podczas tworzenia kontrolera, masz możliwość automatycznego wygenerowania metod akcji tworzenia, aktualizowania lub szczegóły (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="283db-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="283db-125">Jeśli wybierzesz tę opcję, generowany jest klasy kontrolera w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="283db-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="283db-126">[![Automatyczne tworzenie metod akcji](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="283db-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="283db-127">**Rysunek 03**: automatyczne tworzenie metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="283db-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>


<span data-ttu-id="283db-128">**Wyświetlanie listy 2 - Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="283db-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="283db-129">Te wygenerowane metody są metodami klasy zastępczej.</span><span class="sxs-lookup"><span data-stu-id="283db-129">These generated methods are stub methods.</span></span> <span data-ttu-id="283db-130">Należy dodać rzeczywiste Logic Apps do tworzenia, aktualizowania i wyświetlania szczegółów dla klienta, samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="283db-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="283db-131">Jednak metod klasy zastępczej zapewnia dobre rozwiązanie punkt początkowy.</span><span class="sxs-lookup"><span data-stu-id="283db-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="283db-132">Tworzenie klasy kontrolera</span><span class="sxs-lookup"><span data-stu-id="283db-132">Creating a Controller Class</span></span>

<span data-ttu-id="283db-133">Kontroler ASP.NET MVC jest po prostu klasą.</span><span class="sxs-lookup"><span data-stu-id="283db-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="283db-134">Jeśli wolisz, możesz zignorować wygodne szkieletu kontrolera programu Visual Studio i ręcznie utworzyć klasę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="283db-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="283db-135">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="283db-135">Follow these steps:</span></span>

1. <span data-ttu-id="283db-136">Kliknij prawym przyciskiem myszy folder kontrolery i wybierz opcję menu **Dodaj, nowy element** i wybierz **klasy** szablonu (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="283db-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="283db-137">Nadaj nowej klasie PersonController.cs, a następnie kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="283db-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="283db-138">Zmodyfikuj plik wynikowy klasy, tak, aby klasa dziedziczy z klasy bazowej System.Web.Mvc.Controller (patrz lista 3).</span><span class="sxs-lookup"><span data-stu-id="283db-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="283db-139">[![Tworzenie nowej klasy](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="283db-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="283db-140">**Rysunek 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="283db-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>


<span data-ttu-id="283db-141">**Wyświetlanie listy 3 - Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="283db-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="283db-142">Kontroler w ofercie 3 udostępnia jedną akcję o nazwie indeks(), która zwraca ciąg "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="283db-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="283db-143">Można wywołać tej akcji kontrolera, uruchamiając aplikację i żąda adresu URL, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="283db-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="283db-144">ASP.NET Development Server używa numeru portu losowego (na przykład 40071).</span><span class="sxs-lookup"><span data-stu-id="283db-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="283db-145">Wprowadzenie adresu URL do wywołania z kontrolerem, należy podać numer portu w prawo.</span><span class="sxs-lookup"><span data-stu-id="283db-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="283db-146">Należy określić numer portu, ustawiając kursor myszy na ikonie dla ASP.NET Development Server, w obszarze powiadomień (prawym dolnym rogu ekranu), Windows.</span><span class="sxs-lookup"><span data-stu-id="283db-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="283db-147">[Poprzednie](adding-dynamic-content-to-a-cached-page-cs.md)
> [dalej](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="283db-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
