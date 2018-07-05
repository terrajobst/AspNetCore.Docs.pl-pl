---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Za pomocą klasa TagBuilder do tworzenia pomocników HTML (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther wprowadza do klasy przydatne narzędzia w platformę ASP.NET MVC, nazwana klasa TagBuilder. Klasa TagBuilder do mogą używać łatwo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ec50fca1d65d95aaf2baf00c84c080ba98bf333
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383871"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="dabf3-104">Za pomocą klasa TagBuilder do tworzenia pomocników HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="dabf3-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="dabf3-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="dabf3-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="dabf3-106">Autor: Stephen Walther wprowadza do klasy przydatne narzędzia w platformę ASP.NET MVC, nazwana klasa TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="dabf3-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="dabf3-107">Klasa TagBuilder umożliwia łatwe tworzenie tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="dabf3-108">Platforma ASP.NET MVC zawiera klasę przydatne narzędzie nazwana klasa TagBuilder, używanego podczas tworzenia pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="dabf3-109">Klasa TagBuilder jak sugeruje nazwa klasy, można w łatwy sposób tworzyć tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="dabf3-110">W tym samouczku krótki podano klasa TagBuilder do przeglądu i dowiesz się, jak używać tej klasy, podczas tworzenia prostego pomocnika kodu HTML, który renderuje HTML &lt;img&gt; tagów.</span><span class="sxs-lookup"><span data-stu-id="dabf3-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="dabf3-111">Omówienie klasa TagBuilder</span><span class="sxs-lookup"><span data-stu-id="dabf3-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="dabf3-112">Klasa TagBuilder znajduje się w przestrzeni nazw System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="dabf3-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="dabf3-113">Ma pięć metod:</span><span class="sxs-lookup"><span data-stu-id="dabf3-113">It has five methods:</span></span>

- <span data-ttu-id="dabf3-114">AddCssClass() — umożliwia dodanie nowego *klasy = ""* atrybut w tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="dabf3-115">GenerateId() — umożliwia dodanie atrybutu identyfikator tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="dabf3-116">Ta metoda automatycznie zastępuje kropki w identyfikatorze (domyślnie parametr kropki są zastępowane przez znaki podkreślenia)</span><span class="sxs-lookup"><span data-stu-id="dabf3-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="dabf3-117">MergeAttribute() — umożliwia dodanie atrybutów tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="dabf3-118">Istnieje wiele przeciążeń z tej metody.</span><span class="sxs-lookup"><span data-stu-id="dabf3-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="dabf3-119">SetInnerText() — umożliwia ustawianie tekst zawarty wewnątrz tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="dabf3-120">Tekst wewnętrzny jest automatycznie kodowanie HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="dabf3-121">ToString() — umożliwia renderowania tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="dabf3-122">Można określić, czy chcesz utworzyć tag normalne, tagu początkowego, tagu końcowego lub tagu samozamykającego.</span><span class="sxs-lookup"><span data-stu-id="dabf3-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="dabf3-123">Klasa TagBuilder ma cztery właściwości ważne:</span><span class="sxs-lookup"><span data-stu-id="dabf3-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="dabf3-124">Atrybuty — reprezentuje wszystkie atrybuty znacznika.</span><span class="sxs-lookup"><span data-stu-id="dabf3-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="dabf3-125">Idatributedotreplacement — reprezentuje znak używany przez metodę GenerateId() do zastępowania kropki (wartość domyślna to znaku podkreślenia).</span><span class="sxs-lookup"><span data-stu-id="dabf3-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="dabf3-126">InnerHTML — reprezentuje wewnętrzny zawartości znacznika.</span><span class="sxs-lookup"><span data-stu-id="dabf3-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="dabf3-127">Przypisywanie ciąg do tej właściwości *nie* ciąg kodowanie HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="dabf3-128">TagName — reprezentuje nazwę tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="dabf3-129">Te metody i właściwości zapewniają wszystkie podstawowe metody i właściwości, które są potrzebne do zbudowania potraktowane jak tag HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="dabf3-130">Tak naprawdę nie trzeba używać klasa TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="dabf3-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="dabf3-131">Można zamiast tego użyj klasy StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="dabf3-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="dabf3-132">Jednak klasa TagBuilder sprawia, że Twoja praca była nieco łatwiejsza.</span><span class="sxs-lookup"><span data-stu-id="dabf3-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="dabf3-133">Tworzenie obrazu pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="dabf3-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="dabf3-134">Podczas tworzenia wystąpienia klasy TagBuilder możesz przekazywać nazwę jednostki tag, który chcesz skompilować do konstruktora TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="dabf3-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="dabf3-135">Następnie możesz wywołać metod, takich jak metody AddCssClass i MergeAttribute(), aby zmodyfikować atrybuty znacznika.</span><span class="sxs-lookup"><span data-stu-id="dabf3-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="dabf3-136">Na koniec możesz wywołać metodę ToString() do renderowania tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="dabf3-137">Na przykład wyświetlanie listy 1 zawiera pomocnika kodu HTML z obrazu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="dabf3-138">Pomocnik obrazu jest implementowane wewnętrznie z TagBuilder, który reprezentuje HTML &lt;img&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="dabf3-139">**Wyświetlanie listy 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="dabf3-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="dabf3-140">Klasa w ofercie 1 zawiera dwa statyczne przeciążone metody o nazwie obrazu.</span><span class="sxs-lookup"><span data-stu-id="dabf3-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="dabf3-141">Po wywołaniu metody Image(), można przekazać do obiektu, który reprezentuje zestaw atrybutów HTML, czy nie.</span><span class="sxs-lookup"><span data-stu-id="dabf3-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="dabf3-142">Zwróć uwagę, jak metoda TagBuilder.MergeAttribute() służy do Dodaj poszczególne atrybuty, takie jak atrybut src do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="dabf3-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="dabf3-143">Zwróć uwagę, co więcej, jak metoda TagBuilder.MergeAttributes() służy do dodawania Kolekcja atrybutów do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="dabf3-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="dabf3-144">Metoda MergeAttributes() akceptuje słownik&lt;ciąg, obiekt&gt; parametru.</span><span class="sxs-lookup"><span data-stu-id="dabf3-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="dabf3-145">Klasa RouteValueDictionary jest używana do konwersji obiekt reprezentujący kolekcję atrybutów do słownika&lt;ciąg, obiekt&gt;.</span><span class="sxs-lookup"><span data-stu-id="dabf3-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="dabf3-146">Po utworzeniu Pomocnik obrazu, można użyć pomocnika, w sekcji Widoki ASP.NET MVC, podobnie jak inne standardowe pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="dabf3-147">Wyświetl w ofercie 2 użyto Pomocnik obrazu, aby wyświetlić dwa razy ten sam obraz Xbox (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="dabf3-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="dabf3-148">Pomocnik Image() nazywa się zarówno z i bez Kolekcja atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="dabf3-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="dabf3-149">**Wyświetlanie listy 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="dabf3-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="dabf3-150">[![Okno dialogowe Nowy projekt](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dabf3-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="dabf3-151">**Rysunek 01**: przy użyciu Pomocnika obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="dabf3-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="dabf3-152">Zwróć uwagę, należy zaimportować skojarzone z elementem pomocniczym obrazu w górnej części widoku Index.aspx przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="dabf3-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="dabf3-153">Pomocnik jest importowany z następującą dyrektywę:</span><span class="sxs-lookup"><span data-stu-id="dabf3-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="dabf3-154">[Poprzednie](creating-custom-html-helpers-cs.md)
> [dalej](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="dabf3-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
