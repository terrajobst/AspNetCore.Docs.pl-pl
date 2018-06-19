---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Tworzenie kontrolera (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: W tym samouczku Stephen Walther pokazano, jak dodać kontrolera do aplikacji platformy ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 86966f1064d09419e2102542c6d14c4162d153e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868363"
---
<a name="creating-a-controller-c"></a><span data-ttu-id="4b0fe-103">Tworzenie kontrolera (C#)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-103">Creating a Controller (C#)</span></span>
====================
<span data-ttu-id="4b0fe-104">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4b0fe-105">W tym samouczku Stephen Walther pokazano, jak dodać kontrolera do aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="4b0fe-106">Celem tego samouczka jest opisano sposób tworzenia nowego ASP.NET MVC kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="4b0fe-107">Jak utworzyć kontrolerów zarówno przy użyciu opcji menu programu Visual Studio Dodaj kontroler, jak i ręcznie tworząc plik klasy.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="4b0fe-108">Przy użyciu dodać kontroler opcji Menu</span><span class="sxs-lookup"><span data-stu-id="4b0fe-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="4b0fe-109">Najprostszym sposobem tworzenia nowego kontrolera jest kliknij prawym przyciskiem myszy folder kontrolery, w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz **Dodaj, kontrolera** menu opcji (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="4b0fe-110">Wybranie tej opcji menu otwiera **Dodaj kontroler** okna dialogowego (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="4b0fe-111">[![Okno dialogowe nowego projektu](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="4b0fe-112">**Rysunek 01**: dodawania nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4b0fe-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>


<span data-ttu-id="4b0fe-113">[![Okno dialogowe nowego projektu](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="4b0fe-114">**Rysunek 02**: okno dialogowe Dodaj kontroler ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="4b0fe-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>


<span data-ttu-id="4b0fe-115">Należy zauważyć, że pierwsza część nazwy kontrolera zostanie wyróżniona w **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="4b0fe-116">Każda nazwa kontrolera musi kończyć się sufiksem *kontrolera*.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="4b0fe-117">Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontrolerze o nazwie *produktu*.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="4b0fe-118">Jeśli utworzysz kontroler brakuje *kontrolera* sufiks, a następnie nie można wywołać z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="4b0fe-119">Nie wykonuj tej — I po niewykorzystana niezliczonych godzin życiu po wprowadzeniu tego błędu.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="4b0fe-120">**Wyświetlanie listy 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="4b0fe-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="4b0fe-121">Należy zawsze tworzyć kontrolerów w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="4b0fe-122">W przeciwnym razie będzie naruszania konwencje platformy ASP.NET MVC i innymi deweloperami będzie mieć trudniejsze czas opis aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="4b0fe-123">Metody akcji szkieletów</span><span class="sxs-lookup"><span data-stu-id="4b0fe-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="4b0fe-124">Podczas tworzenia kontrolera, użytkownik może automatycznie wygenerować metod akcji tworzenia, aktualizacji i szczegółów (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="4b0fe-125">Jeśli zostanie wybrana ta opcja jest generowany klasy kontrolera wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="4b0fe-126">[![Automatyczne tworzenie metod akcji](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="4b0fe-127">**Rysunek 03**: automatycznego tworzenia metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4b0fe-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>


<span data-ttu-id="4b0fe-128">**Wyświetlanie listy 2 - Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="4b0fe-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="4b0fe-129">Te metody generowane są szkieletu metody.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-129">These generated methods are stub methods.</span></span> <span data-ttu-id="4b0fe-130">Należy dodać logikę rzeczywiste tworzenie, aktualizowanie i zawierającego szczegóły klienta samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="4b0fe-131">Jednak metody klasy zastępczej Podaj dobry punkt wyjścia.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="4b0fe-132">Tworzenie klasy kontrolera</span><span class="sxs-lookup"><span data-stu-id="4b0fe-132">Creating a Controller Class</span></span>

<span data-ttu-id="4b0fe-133">Kontroler ASP.NET MVC jest tylko klasę.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="4b0fe-134">Jeśli wolisz, można zignorować wygodny szkieletów kontrolera Visual Studio i ręcznie utworzyć klasę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="4b0fe-135">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="4b0fe-135">Follow these steps:</span></span>

1. <span data-ttu-id="4b0fe-136">Kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz opcję menu **Dodaj, nowy element** i wybierz **klasy** szablonu (patrz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="4b0fe-137">Nazwa nowej klasy PersonController.cs, a następnie kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="4b0fe-138">Zmodyfikuj plik wynikowy klasy, tak, aby klasa dziedziczy z klasy podstawowej System.Web.Mvc.Controller (patrz lista 3).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="4b0fe-139">[![Tworzenie nowej klasy](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="4b0fe-140">**Rysunek 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="4b0fe-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>


<span data-ttu-id="4b0fe-141">**Wyświetlanie listy 3 - Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="4b0fe-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="4b0fe-142">Kontroler w 3 lista przedstawia jedną akcję o nazwie indeks(), która zwraca ciąg "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="4b0fe-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="4b0fe-143">Ta akcja kontrolera można wywołać przez uruchomienie aplikacji i żądanie adresu URL podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="4b0fe-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="4b0fe-144">ASP.NET Development Server używa numeru portu losowych (na przykład 40071).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="4b0fe-145">Wprowadzenie adresu URL do wywołania kontrolera, należy podać numer prawy port.</span><span class="sxs-lookup"><span data-stu-id="4b0fe-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="4b0fe-146">Należy określić numer portu, ustawiając kursor myszy na ikonie serwera projektowego ASP.NET, w obszarze powiadomień systemu Windows (prawym dolnym rogu ekranu).</span><span class="sxs-lookup"><span data-stu-id="4b0fe-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="4b0fe-147">[Poprzednie](adding-dynamic-content-to-a-cached-page-cs.md)
> [dalej](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4b0fe-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
