---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Za pomocą klasy TagBuilder do tworzenia pomocników HTML (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Stephen Walther przedstawiono klasy przydatne narzędzia w nazwie klasy TagBuilder platformę ASP.NET MVC. Można użyć klasy TagBuilder można łatwo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c0e8e4e3a733f2cc8690dc85e3006bce6c661d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="aecdc-104">Za pomocą klasy TagBuilder do tworzenia pomocników HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="aecdc-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="aecdc-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="aecdc-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="aecdc-106">Stephen Walther przedstawiono klasy przydatne narzędzia w nazwie klasy TagBuilder platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="aecdc-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="aecdc-107">Klasa TagBuilder umożliwia łatwe tworzenie tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="aecdc-108">Platforma ASP.NET MVC zawiera klasę przydatne narzędzie o nazwie klasy TagBuilder, której można użyć podczas kompilowania pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="aecdc-109">Klasa TagBuilder, jak nazwa klasy sugeruje, umożliwia łatwe tworzenie tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="aecdc-110">W tym samouczku krótki podano przegląd klasy TagBuilder i jak przy użyciu tej klasy, podczas tworzenia prostych pomocnika kodu HTML, który renderuje HTML &lt;img&gt; tagów.</span><span class="sxs-lookup"><span data-stu-id="aecdc-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="aecdc-111">Przegląd klasy TagBuilder</span><span class="sxs-lookup"><span data-stu-id="aecdc-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="aecdc-112">Klasa TagBuilder znajduje się w przestrzeni nazw System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="aecdc-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="aecdc-113">Składa się z pięciu metod:</span><span class="sxs-lookup"><span data-stu-id="aecdc-113">It has five methods:</span></span>

- <span data-ttu-id="aecdc-114">AddCssClass() — umożliwia dodanie nowego *klasy = ""* atrybut do tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="aecdc-115">GenerateId() — umożliwia dodanie atrybutu id znacznika.</span><span class="sxs-lookup"><span data-stu-id="aecdc-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="aecdc-116">Ta metoda automatycznie zastępuje kropki w identyfikatorze (domyślnie okresów zastępuje znaki podkreślenia)</span><span class="sxs-lookup"><span data-stu-id="aecdc-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="aecdc-117">MergeAttribute() — pozwala dodać atrybuty do tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="aecdc-118">Istnieje wiele przeciążenie tej metody.</span><span class="sxs-lookup"><span data-stu-id="aecdc-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="aecdc-119">SetInnerText() — pozwala ustawić tekst wewnętrzny tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="aecdc-120">Tekst wewnętrzny jest automatycznie kodowanie HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="aecdc-121">ToString() — umożliwia renderowania tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="aecdc-122">Można określić, czy chcesz utworzyć tag normalne, tagu początkowego, tagu końcowego lub tagu samozamykającego.</span><span class="sxs-lookup"><span data-stu-id="aecdc-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="aecdc-123">Klasa TagBuilder ma cztery ważne właściwości:</span><span class="sxs-lookup"><span data-stu-id="aecdc-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="aecdc-124">Atrybuty — reprezentuje wszystkie atrybuty tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="aecdc-125">Idatributedotreplacement ma wartość - reprezentuje znak używany przez metodę GenerateId() do zastępowania kropki (domyślnie jest to znak podkreślenia).</span><span class="sxs-lookup"><span data-stu-id="aecdc-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="aecdc-126">InnerHTML - reprezentuje wewnętrzny zawartości znacznika.</span><span class="sxs-lookup"><span data-stu-id="aecdc-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="aecdc-127">Przypisanie do tej właściwości ciąg *nie* ciąg kodowanie HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="aecdc-128">TagName - reprezentuje nazwę znacznika.</span><span class="sxs-lookup"><span data-stu-id="aecdc-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="aecdc-129">Te metody i właściwości do wszystkich podstawowe metody i właściwości, które są potrzebne do zbudowania tagu HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="aecdc-130">Naprawdę nie trzeba używać klasy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="aecdc-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="aecdc-131">Zamiast tego można użyć klasy StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="aecdc-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="aecdc-132">Jednak klasy TagBuilder ułatwia życie nieco.</span><span class="sxs-lookup"><span data-stu-id="aecdc-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="aecdc-133">Tworzenie obrazu pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="aecdc-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="aecdc-134">Podczas tworzenia wystąpienia klasy TagBuilder przekazujesz nazwa tagu, który chcesz utworzyć TagBuilder konstruktora.</span><span class="sxs-lookup"><span data-stu-id="aecdc-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="aecdc-135">Następnie można wywołać metody, takie jak AddCssClass i MergeAttribute() metody służące do modyfikowania atrybutów tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="aecdc-136">Na koniec należy wywołać metodę ToString() do renderowania tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="aecdc-137">Na przykład 1 Lista zawiera pomocnika kodu HTML z obrazu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="aecdc-138">Pomocnik obrazu jest implementowana wewnętrznie przy użyciu TagBuilder, który reprezentuje HTML &lt;img&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="aecdc-139">**Wyświetlanie listy 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="aecdc-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="aecdc-140">Klasa w 1 Lista zawiera dwa statycznego przeciążonych metod o nazwie obrazu.</span><span class="sxs-lookup"><span data-stu-id="aecdc-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="aecdc-141">Po wywołaniu metody Image() można przekazać do obiektu, który reprezentuje zestaw atrybutów HTML, czy nie.</span><span class="sxs-lookup"><span data-stu-id="aecdc-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="aecdc-142">Zwróć uwagę, jak metoda TagBuilder.MergeAttribute() jest używana do dodawania poszczególnych atrybutów, takich jak atrybutu src do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="aecdc-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="aecdc-143">Zwróć uwagę, ponadto jak metoda TagBuilder.MergeAttributes() jest używana do dodawania Kolekcja atrybutów do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="aecdc-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="aecdc-144">Metoda MergeAttributes() przyjmuje słownik&lt;string, object&gt; parametru.</span><span class="sxs-lookup"><span data-stu-id="aecdc-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="aecdc-145">Klasa RouteValueDictionary służy do konwertowania obiekt reprezentujący kolekcję atrybutów do słownika&lt;string, object&gt;.</span><span class="sxs-lookup"><span data-stu-id="aecdc-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="aecdc-146">Po utworzeniu pomocy dotyczącej obrazów, można użyć pomocnika, w sekcji Widoki ASP.NET MVC, podobnie jak inne standardowe pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="aecdc-147">Widok wyświetlania 2 używa pomocy dotyczącej obrazów do wyświetlania dwa razy ten sam obraz Xbox (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="aecdc-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="aecdc-148">Pomocnik Image() nazywa się zarówno z i bez Kolekcja atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="aecdc-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="aecdc-149">**Wyświetlanie listy 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="aecdc-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="aecdc-150">[![Okno dialogowe nowego projektu](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aecdc-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="aecdc-151">**Rysunek 01**: przy użyciu Pomocnika obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="aecdc-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="aecdc-152">Zwróć uwagę, importowania skojarzone z pomocy dotyczącej obrazów w górnej części widoku Index.aspx przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="aecdc-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="aecdc-153">Pomocnik jest importowany z dyrektywą następujące:</span><span class="sxs-lookup"><span data-stu-id="aecdc-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="aecdc-154">[Poprzednie](creating-custom-html-helpers-cs.md)
> [dalej](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="aecdc-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
