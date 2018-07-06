---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 3 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27bbc4e1df6e26eed66680d596d13863dfab5db0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812273"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="5e00f-103">Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 3</span><span class="sxs-lookup"><span data-stu-id="5e00f-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="5e00f-104">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5e00f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="5e00f-105">Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5e00f-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="5e00f-106">Praca z typów złożonych</span><span class="sxs-lookup"><span data-stu-id="5e00f-106">Working with Complex Types</span></span>

<span data-ttu-id="5e00f-107">W tej sekcji możesz utworzyć klasę adres i Dowiedz się, jak utworzyć szablon, aby go wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="5e00f-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="5e00f-108">W *modeli* folderu, Utwórz nowy plik klasy o nazwie *osoba.cs* którym zostanie umieszczony dwa typy: `Person` klasy i `Address` klasy.</span><span class="sxs-lookup"><span data-stu-id="5e00f-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="5e00f-109">`Person` Klasa będzie zawierać właściwości, która jest `Address`.</span><span class="sxs-lookup"><span data-stu-id="5e00f-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="5e00f-110">`Address` Typ to typ złożony, co oznacza, nie jest jedną z wbudowanych typów, takich jak `int`, `string`, lub `double`.</span><span class="sxs-lookup"><span data-stu-id="5e00f-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="5e00f-111">Zamiast tego ma kilka właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e00f-111">Instead, it has several properties.</span></span> <span data-ttu-id="5e00f-112">Kod dla nowych klas wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="5e00f-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="5e00f-113">W `Movie` kontroler, Dodaj następujący kod `PersonDetail` akcji w celu wyświetlenia wystąpienia osobę:</span><span class="sxs-lookup"><span data-stu-id="5e00f-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="5e00f-114">Następnie dodaj następujący kod do `Movie` kontrolera, aby wypełnić `Person` modelu z pewnymi przykładowymi danymi:</span><span class="sxs-lookup"><span data-stu-id="5e00f-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="5e00f-115">Otwórz *Views\Movies\PersonDetail.cshtml* pliku i Dodaj następujący kod znaczników dla `PersonDetail` widoku.</span><span class="sxs-lookup"><span data-stu-id="5e00f-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="5e00f-116">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację, a następnie przejdź do *filmy/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="5e00f-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="5e00f-117">`PersonDetail` Widok nie zawiera `Address` typu złożonego, jak można sprawdzić, w tym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="5e00f-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="5e00f-118">(Brak adresu znajduje się).</span><span class="sxs-lookup"><span data-stu-id="5e00f-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="5e00f-119">`Address` Modelu danych nie jest wyświetlana, ponieważ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="5e00f-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="5e00f-120">Aby wyświetlić informacje o adresach, otwórz *Views\Movies\PersonDetail.cshtml* ponownie i Dodaj następujący kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="5e00f-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="5e00f-121">Kompletny kod znaczników dla `PersonDetail` teraz widoku wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="5e00f-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="5e00f-122">Uruchom ponownie aplikację i wyświetlić `PersonDetail` widoku.</span><span class="sxs-lookup"><span data-stu-id="5e00f-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="5e00f-123">Informacje o adresie jest teraz wyświetlany:</span><span class="sxs-lookup"><span data-stu-id="5e00f-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="5e00f-124">Tworzenie szablonu dla typu złożonego</span><span class="sxs-lookup"><span data-stu-id="5e00f-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="5e00f-125">W tej sekcji utworzysz szablon, który zostanie użyty do renderowania `Address` typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="5e00f-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="5e00f-126">Po utworzeniu szablonu dla `Address` typu, ASP.NET MVC automatycznie służy do formatowania modelu adresu w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e00f-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="5e00f-127">Daje to możliwość kontrolowania renderowania `Address` typu na podstawie tylko w jednym miejscu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e00f-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="5e00f-128">W *folder Views\Shared\DisplayTemplates* folderze utwórz silnie typizowanego widoku częściowego, o nazwie **adres**:</span><span class="sxs-lookup"><span data-stu-id="5e00f-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="5e00f-129">Kliknij przycisk **Dodaj**, a następnie otwórz nowy *Views\Shared\DisplayTemplates\Address.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="5e00f-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="5e00f-130">Nowy widok zawiera następujące wygenerowane znaczniki:</span><span class="sxs-lookup"><span data-stu-id="5e00f-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="5e00f-131">Uruchom aplikację i wyświetlić `PersonDetail` widoku.</span><span class="sxs-lookup"><span data-stu-id="5e00f-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="5e00f-132">Tym razem `Address` właśnie utworzony szablon służy do wyświetlania `Address` typu złożonego, aby ekran wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="5e00f-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="5e00f-133">Podsumowanie: Sposoby, aby określić Format wyświetlania modelu i szablonu</span><span class="sxs-lookup"><span data-stu-id="5e00f-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="5e00f-134">W tym samouczku można określić format lub szablon dla właściwości modelu w przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="5e00f-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="5e00f-135">Stosowanie `DisplayFormat` atrybutu do właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="5e00f-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="5e00f-136">Na przykład poniższy kod powoduje, że data jest wyświetlana bez czasu:</span><span class="sxs-lookup"><span data-stu-id="5e00f-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="5e00f-137">Stosowanie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu do właściwości w modelu i określania typu danych.</span><span class="sxs-lookup"><span data-stu-id="5e00f-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="5e00f-138">Na przykład poniższy kod powoduje, że data jest wyświetlana bez czasu.</span><span class="sxs-lookup"><span data-stu-id="5e00f-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="5e00f-139">Jeśli aplikacja zawiera *date.cshtml* szablonu w *folder Views\Shared\DisplayTemplates* folderu lub *Views\Movies\DisplayTemplates* folder, w tym szablonie będzie używany do renderowania `DateTime` właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e00f-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="5e00f-140">W przeciwnym razie wbudowany system szablonów ASP.NET Wyświetla właściwość jako wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="5e00f-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="5e00f-141">Tworzenie szablonu ekranu w *folder Views\Shared\DisplayTemplates* folderu lub *Views\Movies\DisplayTemplates* folder, którego nazwa odpowiada typ danych, który chcesz sformatować.</span><span class="sxs-lookup"><span data-stu-id="5e00f-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="5e00f-142">Na przykład, że widoczny *Views\Shared\DisplayTemplates\DateTime.cshtml* został użyty do renderowania `DateTime` właściwości w modelu, bez konieczności dodawania atrybutów do modelu i bez dodawania żadnych znaczników do widoków.</span><span class="sxs-lookup"><span data-stu-id="5e00f-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="5e00f-143">Za pomocą [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu w modelu, aby określić szablon, aby wyświetlić właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="5e00f-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="5e00f-144">Jawne dodanie nazwę wyświetlaną szablonu w celu [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) wywołań w widoku.</span><span class="sxs-lookup"><span data-stu-id="5e00f-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="5e00f-145">Podejścia, którego używasz, zależy od tego, co musisz zrobić w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e00f-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="5e00f-146">Nie jest niczym niezwykłym kombinację obu rozwiązań, aby uzyskać dokładnie rodzaj formatowania, które należy.</span><span class="sxs-lookup"><span data-stu-id="5e00f-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="5e00f-147">W następnej sekcji można przełączać gears nieco i Przenieś możliwości dostosowywania wyświetlania danych umożliwiające dostosowanie sposobu wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="5e00f-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="5e00f-148">Będziesz podpiąć datepicker jQuery z widokami Edytuj w aplikacji w celu zapewnienia sprawny sposób, aby określić daty.</span><span class="sxs-lookup"><span data-stu-id="5e00f-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5e00f-149">[Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="5e00f-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
