---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: "Sprawdzaniu poprawności proste (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: Informacje o sposobie przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. W tym samouczku Stephen Walther wprowadza do stanu modelu i pomocnika weryfikacji HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 005872308d9d4d8ac7feb12dd5ab1fc463d0140e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="29bcd-104">Sprawdzaniu poprawności proste (C#)</span><span class="sxs-lookup"><span data-stu-id="29bcd-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="29bcd-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="29bcd-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="29bcd-106">Informacje o sposobie przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="29bcd-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="29bcd-107">W tym samouczku Stephen Walther wprowadza do stanu modelu i pomocników HTML sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="29bcd-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="29bcd-108">Celem tego samouczka jest wyjaśnienie sposobu wykonywania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="29bcd-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="29bcd-109">Na przykład zostanie przedstawiony sposób zapobiec ktoś przesyłania formularza, który nie zawiera wartości dla wymaganego pola.</span><span class="sxs-lookup"><span data-stu-id="29bcd-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="29bcd-110">Jak używać stan modelu i sprawdzania poprawności pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="29bcd-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="29bcd-111">Opis stanu modelu</span><span class="sxs-lookup"><span data-stu-id="29bcd-111">Understanding Model State</span></span>

<span data-ttu-id="29bcd-112">Używasz stan modelu — lub dokładniej ze słownika stanu modelu — do reprezentowania błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="29bcd-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="29bcd-113">Na przykład w akcji Create() wyświetlania 1 sprawdza właściwości klasy produktu przed dodaniem klasy produktu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="29bcd-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="29bcd-114">Nie mam I rekomendowania Dodaj logikę sprawdzania poprawności lub bazy danych do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="29bcd-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="29bcd-115">Kontroler powinien zawierać tylko logiki powiązane do sterowania działaniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="29bcd-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="29bcd-116">Firma Microsoft trwa skrót do zachowania prostych czynności.</span><span class="sxs-lookup"><span data-stu-id="29bcd-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="29bcd-117">**Wyświetlanie listy 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="29bcd-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="29bcd-118">Wyświetlanie listy 1 Nazwa, opis i StanMagazynu właściwości klasy produktu są weryfikowane.</span><span class="sxs-lookup"><span data-stu-id="29bcd-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="29bcd-119">Jeśli dowolne z tych właściwości test weryfikacji nie powiedzie się błędu jest dodawany do słownika stanu modelu (reprezentowane przez właściwość ModelState klasy Controller).</span><span class="sxs-lookup"><span data-stu-id="29bcd-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="29bcd-120">Jeśli wystąpią jakieś błędy w stanie modelu właściwość ModelState.IsValid zwraca wartość false.</span><span class="sxs-lookup"><span data-stu-id="29bcd-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="29bcd-121">W takim przypadku formularza HTML służący do tworzenia nowego produktu zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="29bcd-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="29bcd-122">W przeciwnym razie jeśli nie ma żadnych błędów sprawdzania poprawności, nowego produktu jest dodawany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="29bcd-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="29bcd-123">Przy użyciu pomocników sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="29bcd-123">Using the Validation Helpers</span></span>

<span data-ttu-id="29bcd-124">Platforma ASP.NET MVC zawiera dwa pomocników sprawdzania poprawności: pomocnika Html.ValidationMessage() i pomocnika Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="29bcd-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="29bcd-125">Użyjesz tych dwóch pomocników w widoku wyświetlane komunikaty o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="29bcd-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="29bcd-126">Pomocnicy Html.ValidationMessage() i Html.ValidationSummary() są używane w widokach tworzenie i edytowanie, które są generowane automatycznie przez funkcję szkieletów ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="29bcd-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="29bcd-127">Wykonaj następujące kroki, aby wygenerować widok Utwórz:</span><span class="sxs-lookup"><span data-stu-id="29bcd-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="29bcd-128">Kliknij prawym przyciskiem myszy w akcji Create() kontroler produktu i wybierz opcję menu **Dodaj widok** (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="29bcd-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="29bcd-129">W **Dodaj widok** okno dialogowe, zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie** (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="29bcd-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="29bcd-130">Z **wyświetlić klasy danych** listy rozwijanej wybierz klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="29bcd-131">Z **wyświetlania zawartości** listy rozwijanej, wybierz opcję Utwórz.</span><span class="sxs-lookup"><span data-stu-id="29bcd-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="29bcd-132">Kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="29bcd-132">Click the **Add** button.</span></span>


<span data-ttu-id="29bcd-133">Upewnij się, że można skompilować aplikację przed dodaniem widoku.</span><span class="sxs-lookup"><span data-stu-id="29bcd-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="29bcd-134">W przeciwnym razie nie będą wyświetlane na liście klas w **wyświetlić klasy danych** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="29bcd-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="29bcd-135">[![Okno dialogowe nowego projektu](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="29bcd-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="29bcd-136">**Rysunek 01**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="29bcd-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="29bcd-137">[![Okno dialogowe nowego projektu](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="29bcd-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="29bcd-138">**Rysunek 02**: tworzenia widoku jednoznacznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="29bcd-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="29bcd-139">Po wykonaniu tych kroków w wersji 2 wyświetlania jest wyświetlany widok Utwórz.</span><span class="sxs-lookup"><span data-stu-id="29bcd-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="29bcd-140">**Wyświetlanie listy 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="29bcd-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="29bcd-141">Wyświetlanie listy 2 pomocnika Html.ValidationSummary() jest wywoływana natychmiast powyżej formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="29bcd-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="29bcd-142">Pomocnik ten służy do wyświetlania listę komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="29bcd-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="29bcd-143">Pomocnik Html.ValidationSummary() renderuje błędy na liście punktowanej.</span><span class="sxs-lookup"><span data-stu-id="29bcd-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="29bcd-144">Pomocnik Html.ValidationMessage() nazywa się obok każdego pola formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="29bcd-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="29bcd-145">Pomocnik ten służy do wyświetlania komunikat o błędzie obok pola formularza.</span><span class="sxs-lookup"><span data-stu-id="29bcd-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="29bcd-146">W przypadku wyświetlania 2 pomocnika Html.ValidationMessage() wyświetla gwiazdkę po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="29bcd-147">Strony na rysunku 3 przedstawiono komunikaty o błędach renderowana przez pomocników weryfikacji po przesłaniu formularza z brakujące pola i nieprawidłowe wartości.</span><span class="sxs-lookup"><span data-stu-id="29bcd-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="29bcd-148">[![Okno dialogowe nowego projektu](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="29bcd-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="29bcd-149">**Rysunek 03**: Tworzenie widoku przesłane z problemami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="29bcd-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="29bcd-150">Należy zauważyć, że pola są również zmodyfikowane w razie błędu sprawdzania poprawności danych wejściowych wygląd HTML.</span><span class="sxs-lookup"><span data-stu-id="29bcd-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="29bcd-151">Renderuje pomocnika Html.TextBox() *klasy = "input błędzie sprawdzania poprawności"* atrybutu, gdy występuje błąd weryfikacji skojarzony z właściwością renderowana przez pomocnika Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="29bcd-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="29bcd-152">Istnieją trzy kaskadowych klasy arkusza stylów sterować wyglądem błędy sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="29bcd-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="29bcd-153">dane wejściowe błędzie sprawdzania poprawności — są stosowane do &lt;wejściowych&gt; renderowana przez Html.TextBox() pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="29bcd-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="29bcd-154">pole — błędzie sprawdzania poprawności — są stosowane do &lt;span&gt; renderowana przez pomocnika Html.ValidationMessage() tagu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="29bcd-155">błędy — sprawdzania poprawności — podsumowanie — są stosowane do &lt;ul&gt; renderowana przez pomocnika Html.ValidationSumamry() tagu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="29bcd-156">Możesz zmodyfikować te kaskadowych klasy arkusza stylów i w związku z tym zmodyfikować wygląd błędy sprawdzania poprawności przez zmodyfikowanie pliku Site.css znajdujące się w folderze zawartości.</span><span class="sxs-lookup"><span data-stu-id="29bcd-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="29bcd-157">Klasa HtmlHelper zawiera statycznej właściwości tylko do odczytu podczas pobierania nazwy weryfikacji związanych z CSS klasy.</span><span class="sxs-lookup"><span data-stu-id="29bcd-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="29bcd-158">Te właściwości statyczne są nazywane ValidationInputCssClassName, ValidationFieldCssClassName i ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="29bcd-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="29bcd-159">Prebinding weryfikacji i sprawdzania poprawności Postbinding</span><span class="sxs-lookup"><span data-stu-id="29bcd-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="29bcd-160">Jeśli przesyłania formularza HTML służący do tworzenia produktu i wprowadź nieprawidłową wartość dla pola Cena i bez wartości dla pola StanMagazynu, następnie otrzymasz komunikatów dotyczących sprawdzania poprawności wyświetlane na rysunku 4.</span><span class="sxs-lookup"><span data-stu-id="29bcd-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="29bcd-161">Skąd pochodzą te komunikaty o błędach weryfikacji?</span><span class="sxs-lookup"><span data-stu-id="29bcd-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="29bcd-162">[![Okno dialogowe nowego projektu](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="29bcd-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="29bcd-163">**Rysunek 04**: błędy sprawdzania poprawności Prebinding ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="29bcd-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="29bcd-164">Brak faktycznie dwa typy komunikatów o błędach weryfikacji - wygenerowanymi przed pola formularza HTML są powiązane z klasą i te wygenerowane po pola formularza jest powiązana z tej klasy.</span><span class="sxs-lookup"><span data-stu-id="29bcd-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="29bcd-165">Innymi słowy, występują błędy sprawdzania poprawności prebinding i postbinding błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="29bcd-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="29bcd-166">Akcja Create() udostępnianych przez kontroler produktu w 1 Lista akceptuje wystąpienia klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="29bcd-167">Podpis metody Create wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="29bcd-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="29bcd-168">Wartości pól formularza HTML w formularzu Utwórz są powiązane z klasą productToCreate przez element zwany integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="29bcd-169">Domyślny integrator modelu dodaje komunikat o błędzie do stanu modelu automatycznie, gdy nie można powiązać pole formularza właściwości formularza.</span><span class="sxs-lookup"><span data-stu-id="29bcd-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="29bcd-170">Ciąg "apple" nie można powiązać właściwości cen klasy produktu domyślnego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="29bcd-171">Ciąg nie można przypisać do właściwości typu decimal.</span><span class="sxs-lookup"><span data-stu-id="29bcd-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="29bcd-172">W związku z tym integratora modelu dodaje błąd do stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="29bcd-173">Domyślny integrator modelu także nie można przypisać wartości null do właściwości, która nie akceptuje wartości null.</span><span class="sxs-lookup"><span data-stu-id="29bcd-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="29bcd-174">W szczególności integratora modelu nie można przypisać wartości null dla właściwości StanMagazynu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="29bcd-175">Jeszcze raz integratora modelu zrezygnuje i dodaje komunikat o błędzie do stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="29bcd-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="29bcd-176">Jeśli chcesz dostosować wygląd te komunikaty o błędach prebinding następnie musisz utworzyć ciągów zasobów dla tych wiadomości.</span><span class="sxs-lookup"><span data-stu-id="29bcd-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="29bcd-177">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="29bcd-177">Summary</span></span>

<span data-ttu-id="29bcd-178">Celem tego samouczka było do opisywania podstawowa mechanika weryfikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="29bcd-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="29bcd-179">Przedstawiono sposób użycia stanu modelu i sprawdzania poprawności pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="29bcd-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="29bcd-180">Omówiono także różnice między prebinding i postbinding sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="29bcd-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="29bcd-181">W innych samouczków omówiono różne strategie kod weryfikacji z kontrolerami poza i w klasach modeli.</span><span class="sxs-lookup"><span data-stu-id="29bcd-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="29bcd-182">[Poprzednie](displaying-a-table-of-database-data-cs.md)
[dalej](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="29bcd-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
