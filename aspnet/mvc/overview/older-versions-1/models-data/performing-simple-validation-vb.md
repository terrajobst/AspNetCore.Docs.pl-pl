---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Wykonywanie prostej weryfikacji (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Informacje o sposobie przeprowadzania weryfikacji w aplikacji ASP.NET MVC. W tym samouczku Walther Autor: Stephen wprowadza do stanu modelu i pomocnika weryfikacji HTML...'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d0bd6917bab61b17d1cafcf0cd9eb1983275dc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753874"
---
<a name="performing-simple-validation-vb"></a><span data-ttu-id="96fed-104">Wykonywanie prostej weryfikacji (VB)</span><span class="sxs-lookup"><span data-stu-id="96fed-104">Performing Simple Validation (VB)</span></span>
====================
<span data-ttu-id="96fed-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="96fed-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="96fed-106">Informacje o sposobie przeprowadzania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="96fed-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="96fed-107">W tym samouczku Walther Autor: Stephen wprowadza do stanu modelu i pomocników HTML sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="96fed-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="96fed-108">Celem tego samouczka jest wyjaśniają, jak można wykonywać sprawdzanie poprawności w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="96fed-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="96fed-109">Na przykład dowiesz się, jak zapobiec ktoś przesyłania formularza, który nie zawiera wartości dla wymaganego pola.</span><span class="sxs-lookup"><span data-stu-id="96fed-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="96fed-110">Dowiesz się, jak używać stan modelu i sprawdzania poprawności pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="96fed-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="96fed-111">Opis stanu modelu</span><span class="sxs-lookup"><span data-stu-id="96fed-111">Understanding Model State</span></span>

<span data-ttu-id="96fed-112">Umożliwia stan modelu — lub dokładniej, ze słownika stanu modelu — reprezentują błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="96fed-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="96fed-113">Na przykład akcja Create() w ofercie 1 sprawdza właściwości klasy produktu przed dodaniem klasy produktu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="96fed-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="96fed-114">Nie jestem I zalecania dodać logikę weryfikacji lub bazy danych do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="96fed-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="96fed-115">Kontroler może zawierać tylko logiki powiązane do sterowania działaniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96fed-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="96fed-116">Podejmujemy skrótów, aby zachować ich prostotę.</span><span class="sxs-lookup"><span data-stu-id="96fed-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="96fed-117">**Wyświetlanie listy 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="96fed-117">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

<span data-ttu-id="96fed-118">W ofercie 1 Nazwa, opis i UnitsInStock właściwości klasy produktu są weryfikowane.</span><span class="sxs-lookup"><span data-stu-id="96fed-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="96fed-119">Jeśli dowolne z tych właściwości testu sprawdzania poprawności zakończy się niepowodzeniem błąd zostanie dodany do słownika stanu modelu (reprezentowane przez właściwość ModelState klasy kontrolera).</span><span class="sxs-lookup"><span data-stu-id="96fed-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="96fed-120">Jeśli istnieją błędy w stanie modelu ModelState.IsValid właściwość zwraca wartość false.</span><span class="sxs-lookup"><span data-stu-id="96fed-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="96fed-121">W takim przypadku zostanie wyświetlony ponownie formularza HTML do tworzenia nowych produktów.</span><span class="sxs-lookup"><span data-stu-id="96fed-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="96fed-122">W przeciwnym razie jeśli nie ma żadnych błędów sprawdzania poprawności, nowy produkt jest dodawany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="96fed-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="96fed-123">Za pomocą pomocników sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="96fed-123">Using the Validation Helpers</span></span>

<span data-ttu-id="96fed-124">Platforma ASP.NET MVC zawiera dwa pomocników sprawdzania poprawności: pomocnika Html.ValidationMessage() i pomocnika Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="96fed-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="96fed-125">Użyjesz tych dwóch wątków w widoku do wyświetlania komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="96fed-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="96fed-126">Pomocnicy Html.ValidationMessage() i Html.ValidationSummary() są używane w tworzenie i edytowanie widoków, które są generowane automatycznie przez szkieletu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="96fed-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="96fed-127">Wykonaj następujące kroki, aby wygenerować widok Utwórz:</span><span class="sxs-lookup"><span data-stu-id="96fed-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="96fed-128">Kliknij prawym przyciskiem myszy w akcji Create() kontroler produktu, a następnie wybierz opcję menu **Dodaj widok** (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="96fed-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="96fed-129">W **Dodaj widok** okno dialogowe, zaznacz pola wyboru **utworzyć widok silnie typizowane** (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="96fed-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="96fed-130">Z **wyświetlić klasy danych** listy rozwijanej wybierz klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="96fed-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="96fed-131">Z **wyświetlanie zawartości** listy rozwijanej, wybierz pozycję Utwórz.</span><span class="sxs-lookup"><span data-stu-id="96fed-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="96fed-132">Kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="96fed-132">Click the **Add** button.</span></span>


<span data-ttu-id="96fed-133">Upewnij się, czy kompilujesz aplikację przed dodaniem widoku.</span><span class="sxs-lookup"><span data-stu-id="96fed-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="96fed-134">W przeciwnym razie lista klas nie będzie wyświetlane w **wyświetlić klasy danych** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="96fed-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="96fed-135">[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="96fed-135">[![The New Project dialog box](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="96fed-136">**Rysunek 01**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="96fed-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="96fed-137">[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="96fed-137">[![The New Project dialog box](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span></span>

<span data-ttu-id="96fed-138">**Rysunek 02**: tworzenia silnie typizowane widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="96fed-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-vb/_static/image4.png))</span></span>


<span data-ttu-id="96fed-139">Po wykonaniu tych kroków, uzyskasz widoku Utwórz w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="96fed-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="96fed-140">**Wyświetlanie listy 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="96fed-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

<span data-ttu-id="96fed-141">W ofercie 2 pomocnika Html.ValidationSummary() jest wywoływana natychmiast powyżej formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="96fed-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="96fed-142">Tego pomocnika służy do wyświetlania listy komunikaty o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="96fed-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="96fed-143">Pomocnik Html.ValidationSummary() renderuje błędy na liście punktowanej.</span><span class="sxs-lookup"><span data-stu-id="96fed-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="96fed-144">Pomocnik Html.ValidationMessage() nazywa się obok każdego pola formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="96fed-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="96fed-145">Pomocnik ten jest używany do wyświetlania komunikatu o błędzie w prawo obok pola formularza.</span><span class="sxs-lookup"><span data-stu-id="96fed-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="96fed-146">W przypadku wyświetlania listy 2 pomocnika Html.ValidationMessage() wyświetla gwiazdkę, gdy występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="96fed-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="96fed-147">Strona na rysunku 3 przedstawiono komunikaty o błędach renderowany przez pomocników sprawdzania poprawności, gdy formularz zostanie przesłany z polami brakujących i nieprawidłowych wartości.</span><span class="sxs-lookup"><span data-stu-id="96fed-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="96fed-148">[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="96fed-148">[![The New Project dialog box](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span></span>

<span data-ttu-id="96fed-149">**Rysunek 03**: Create view, które są przesyłane z problemami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="96fed-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-vb/_static/image6.png))</span></span>


<span data-ttu-id="96fed-150">Należy zauważyć, że pola również są modyfikowane, gdy występuje błąd weryfikacji danych wejściowych wygląd elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="96fed-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="96fed-151">Renderuje pomocnika Html.TextBox() *klasy = "danych wejściowych błędzie sprawdzania poprawności"* atrybutu, gdy występuje błąd weryfikacji skojarzony z właściwością renderowany przez pomocnika Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="96fed-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="96fed-152">Istnieją trzy kaskadowych klasy arkusz stylów używany, aby sterować wyglądem błędy sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="96fed-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="96fed-153">dane wejściowe błędzie sprawdzania poprawności — są stosowane do &lt;wejściowych&gt; renderowany przez Html.TextBox() pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="96fed-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="96fed-154">pole — błędzie sprawdzania poprawności — są stosowane do &lt;span&gt; renderowany przez pomocnika Html.ValidationMessage() tagu.</span><span class="sxs-lookup"><span data-stu-id="96fed-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="96fed-155">błędy — sprawdzania poprawności podsumowanie — są stosowane do &lt;ul&gt; renderowany przez pomocnika Html.ValidationSumamry() tagu.</span><span class="sxs-lookup"><span data-stu-id="96fed-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="96fed-156">Możesz zmodyfikować te kaskadowych klasy arkusza stylów i w związku z tym zmodyfikować wygląd błędy sprawdzania poprawności, modyfikując plik Site.css znajduje się w folderze zawartości.</span><span class="sxs-lookup"><span data-stu-id="96fed-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="96fed-157">Klasa HtmlHelper obejmuje statycznych właściwości tylko do odczytu dla pobierania nazw weryfikacji powiązanych CSS klasy.</span><span class="sxs-lookup"><span data-stu-id="96fed-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="96fed-158">Te właściwości statyczne są nazywane ValidationInputCssClassName ValidationFieldCssClassName i ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="96fed-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="96fed-159">Prebinding weryfikacji i sprawdzania poprawności Postbinding</span><span class="sxs-lookup"><span data-stu-id="96fed-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="96fed-160">Jeśli przesyłanie formularza HTML do tworzenia produktu i wprowadź nieprawidłową wartość dla pola Cena i bez wartości dla pola UnitsInStock, następnie otrzymasz komunikatów weryfikacji wyświetlanych na rysunku 4.</span><span class="sxs-lookup"><span data-stu-id="96fed-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="96fed-161">Skąd pochodzą te komunikaty o błędach weryfikacji?</span><span class="sxs-lookup"><span data-stu-id="96fed-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="96fed-162">[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="96fed-162">[![The New Project dialog box](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span></span>

<span data-ttu-id="96fed-163">**Rysunek 04**: błędy sprawdzania poprawności Prebinding ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="96fed-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-vb/_static/image8.png))</span></span>


<span data-ttu-id="96fed-164">Istnieją faktycznie dwa typy komunikatów o błędach weryfikacji - wygenerowanymi przed pola formularza HTML są powiązane z klasą i te wygenerowane po pól formularza jest powiązana z tej klasy.</span><span class="sxs-lookup"><span data-stu-id="96fed-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="96fed-165">Innymi słowy, istnieją prebinding błędy sprawdzania poprawności i postbinding błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="96fed-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="96fed-166">Akcja Create() udostępnianych przez kontroler produktu w ofercie 1 przyjmuje wystąpienie klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="96fed-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="96fed-167">Podpis metody Create wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="96fed-167">The signature of the Create method looks like this:</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

<span data-ttu-id="96fed-168">Wartości pól formularza HTML w formularzu tworzenia związane są z klasą productToCreate coś, co jest nazywane integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="96fed-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="96fed-169">Domyślny integrator modelu komunikat o błędzie do stanu modelu automatycznie dodaje gdy go nie można powiązać pole formularza z właściwością formularza.</span><span class="sxs-lookup"><span data-stu-id="96fed-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="96fed-170">Ciąg "apple" nie można powiązać właściwości cena klasy produktu domyślnego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="96fed-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="96fed-171">Ciąg nie można przypisać do właściwości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="96fed-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="96fed-172">W związku z tym integratora modelu dodaje błąd do stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="96fed-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="96fed-173">Domyślny integrator modelu również nie można przypisać wartość Nothing właściwość, która nie przyjmuje wartość Nothing.</span><span class="sxs-lookup"><span data-stu-id="96fed-173">The default model binder also cannot assign the value Nothing to a property that does not accept the value Nothing.</span></span> <span data-ttu-id="96fed-174">W szczególności integratora modelu nie można przypisać wartość Nothing właściwość UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="96fed-174">In particular, the model binder cannot assign the value Nothing to the UnitsInStock property.</span></span> <span data-ttu-id="96fed-175">Jeszcze raz integratora modelu rezygnuje i dodaje komunikat o błędzie do stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="96fed-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="96fed-176">Jeśli chcesz dostosować wygląd te prebinding komunikaty o błędach następnie należy utworzyć ciągów zasobów dla tych komunikatów.</span><span class="sxs-lookup"><span data-stu-id="96fed-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="96fed-177">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="96fed-177">Summary</span></span>

<span data-ttu-id="96fed-178">Celem tego samouczka było opisują podstawowa mechanika sprawdzania poprawności w platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="96fed-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="96fed-179">Przedstawiono sposób używania stanu modelu i pomocników HTML sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="96fed-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="96fed-180">Omówiono również rozróżnienie między prebinding i postbinding sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="96fed-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="96fed-181">W innych samouczków omówimy różnych strategii przenoszenia kod sprawdzania poprawności poza kontrolerach i w klasach modeli.</span><span class="sxs-lookup"><span data-stu-id="96fed-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="96fed-182">[Poprzednie](displaying-a-table-of-database-data-vb.md)
> [dalej](validating-with-the-idataerrorinfo-interface-vb.md)</span><span class="sxs-lookup"><span data-stu-id="96fed-182">[Previous](displaying-a-table-of-database-data-vb.md)
[Next](validating-with-the-idataerrorinfo-interface-vb.md)</span></span>
