---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: "Przy użyciu rozszerzeń formantu ColorPicker (C#) | Dokumentacja firmy Microsoft"
author: microsoft
description: "ColorPicker jest extender ASP.NET AJAX, która udostępnia funkcje pobrania kolor po stronie klienta przy użyciu interfejsu użytkownika w formancie menu podręczne. Będzie można dołączyć do dowolnego ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3cde9552e8aecd5e7e651a825902fb79ae108c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="4f7b9-104">Przy użyciu rozszerzeń formantu ColorPicker (C#)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="4f7b9-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4f7b9-106">ColorPicker jest extender ASP.NET AJAX, która udostępnia funkcje pobrania kolor po stronie klienta przy użyciu interfejsu użytkownika w formancie menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="4f7b9-107">Może zostać dołączona do żadnego formantu ASP.NET TextBox.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="4f7b9-108">Go.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-108">It.</span></span>


<span data-ttu-id="4f7b9-109">Celem tego samouczka jest wyjaśnienie, jak używasz AJAX kontroli zestawu narzędzi ColorPicker rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="4f7b9-110">Rozszerzeń formantu ColorPicker wyświetla okno dialogowe menu podręczne, które można wybrać kolor.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="4f7b9-111">ColorPicker jest przydatne, gdy chcesz zapewnić intuicyjnego interfejsu użytkownika dla użytkownika wybrać kolor.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="4f7b9-112">Rozszerzenie kontrolki pola tekstowego z rozszerzeń formantu ColorPicker</span><span class="sxs-lookup"><span data-stu-id="4f7b9-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="4f7b9-113">Załóżmy, że chcesz utworzyć witrynę sieci Web, który umożliwia tworzenie dostosowanych kart biznesowej osoby odwiedzające.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="4f7b9-114">Osoby odwiedzające można wprowadzić tekst kart biznesowej i wybierz kolor.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="4f7b9-115">Strony ASP.NET w 1 Lista zawiera dwa kontrolki TextBox o nazwie txtCardText i txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="4f7b9-116">Po przesłaniu formularza, wybranych wartości są wyświetlane (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="4f7b9-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="4f7b9-117">[![Prosty formularz służący do tworzenia kart biznesowej](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="4f7b9-118">**Rysunek 01**: prosty formularz służący do tworzenia kart biznesowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4f7b9-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="4f7b9-119">**Wyświetlanie listy 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="4f7b9-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="4f7b9-120">Formularz działa wyświetlania 1, ale nie zapewnia obsługi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="4f7b9-121">Użytkownik będzie musiał wpisać koloru w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="4f7b9-122">Jeśli użytkownik chce specjalne kolor — na przykład tylko prawo odcień zielony pea -, a następnie użytkownik musi ustalić kod HTML koloru bez pomocy.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="4f7b9-123">Rozszerzenie kontrolki ColorPicker służy do tworzenia lepsze środowisko pracy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="4f7b9-124">ColorPicker wyświetla okno dialogowe kolorów, gdy Przenieś fokus do formantu TextBox (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="4f7b9-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="4f7b9-125">[![Rozszerzenie kontrolki ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="4f7b9-126">**Rysunek 02**: rozszerzeń formantu ColorPicker ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="4f7b9-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="4f7b9-127">Trzeba wykonać rozszerzeń formantu ColorPicker za pomocą formularza w 1 wyświetlania wykonać dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="4f7b9-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="4f7b9-128">Dodawanie formantu ScriptManager na stronie</span><span class="sxs-lookup"><span data-stu-id="4f7b9-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="4f7b9-129">Na stronie Dodaj ColorPicker rozszerzeń formantu</span><span class="sxs-lookup"><span data-stu-id="4f7b9-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="4f7b9-130">Przed użyciem ColorPicker, należy dodać element ScriptManager do strony.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="4f7b9-131">Dobrym miejscem, aby dodać element ScriptManager jest bezpośrednio pod serwerowe otwierania &lt;formularza&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="4f7b9-132">Z przybornika (ScriptManager znajduje się na karcie rozszerzenia AJAX) można przeciągnąć element ScriptManager na stronę.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="4f7b9-133">Alternatywnie możesz wpisać następującego tagu w widoku źródła poniżej otwierający tag formularza po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="4f7b9-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="4f7b9-134">&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="4f7b9-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="4f7b9-135">Najprostszym sposobem na stronie Dodaj ColorPicker rozszerzeń formantu jest w widoku Projekt.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="4f7b9-136">Jeśli umieść kursor nad txtCardColor pole tekstowe, opcja inteligentne zadań pojawi się zapewniającą można dodać moduł rozszerzający (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="4f7b9-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="4f7b9-137">W przypadku wybrania tej opcji, zostanie wyświetlony Kreator rozszerzeń, (patrz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="4f7b9-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="4f7b9-138">[![Dodawanie rozszerzeń](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="4f7b9-139">**Rysunek 03**: Dodawanie rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4f7b9-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="4f7b9-140">[![Wybieranie rozszerzeń formantu przy użyciu kreatora rozszerzeń](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="4f7b9-141">**Rysunek 04**: Wybieranie rozszerzeń formantu przy użyciu kreatora rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="4f7b9-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="4f7b9-142">Można wybrać rozszerzeń ColorPicker rozszerzenie txtCardColor pole tekstowe z rozszerzeń ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="4f7b9-143">Kliknij przycisk OK, aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="4f7b9-144">Po wprowadzeniu tych zmian źródła dla strony wygląda jak lista 2.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="4f7b9-145">Wyświetlanie listy 2 - CreateCard.aspx (z ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="4f7b9-146">Zwróć uwagę, czy strona zawiera teraz ColorPickerExtender formant, który pojawi się bezpośrednio pod txtCardColor formantu TextBox.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="4f7b9-147">Formant ColorPickerExtender rozszerza kontroli txtCardColor tak, aby go Wyświetla okno dialogowe selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="4f7b9-148">Aby uruchomić okno dialogowe selektora kolorów za pomocą przycisku</span><span class="sxs-lookup"><span data-stu-id="4f7b9-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="4f7b9-149">Rozszerzenie ColorPicker obsługuje następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="4f7b9-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="4f7b9-150">PopupButtonId — identyfikator przycisku na stronie powoduje wyświetlać okno dialogowe selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="4f7b9-151">PopupPosition - pozycji, względem formantu docelowego okna dialogowego selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="4f7b9-152">Możliwe wartości to bezwzględne, Centrum, BottomLeft, BottomRight, TopLeft, TopRight, prawo i lewej strony (wartość domyślna to BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="4f7b9-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="4f7b9-153">SampleControlId — identyfikator formantu, który wyświetla wybranego koloru.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="4f7b9-154">SelectedColor - początkowy kolor wybrany przez ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="4f7b9-155">Aby dostosować sposób wyświetlania okna dialogowego selektora kolorów i sposób wyświetlania wybranego koloru, można użyć tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="4f7b9-156">Strona wyświetlania 3 ilustruje sposób korzystania z niektóre z tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="4f7b9-157">**Wyświetlanie listy 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="4f7b9-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="4f7b9-158">Strona wyświetlania 3 zawiera wybierz kolor przycisku (patrz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="4f7b9-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="4f7b9-159">Po kliknięciu tego przycisku okna dialogowego selektora kolorów pojawia się powyżej pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="4f7b9-160">W przypadku wybrania koloru z okna dialogowego kolorów pojawia się jako kolor tła lblSample formantu etykiety.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="4f7b9-161">Właściwość ColorPicker PopupButtonID jest używana do skojarzenia z rozszerzeń ColorPicker przycisku Wybierz kolor.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="4f7b9-162">Jeśli zostanie podana wartość właściwości PopupButtonID okna dialogowego selektora kolorów nie będzie widoczny po aktywowaniu formantu docelowego.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="4f7b9-163">Musi kliknąć przycisk, aby wyświetlić okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="4f7b9-164">Właściwość SampleControlID jest używana do skojarzenia kontrolkę wyświetlającą wybranego koloru z ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="4f7b9-165">ColorPicker zmienia kolor tła tego formantu do aktualnie wybranego koloru.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="4f7b9-166">[![Wyświetlanie okna dialogowego selektora kolorów z przyciskiem](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="4f7b9-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="4f7b9-167">**Rysunek 05**: wyświetlanie okna dialogowego selektora kolorów z przyciskiem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="4f7b9-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="4f7b9-168">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="4f7b9-168">Summary</span></span>

<span data-ttu-id="4f7b9-169">W tym samouczku przedstawiono sposób rozszerzeń formantu ColorPicker umożliwia wyświetlanie podręcznego okna dialogowego selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="4f7b9-170">Po pierwsze firma Microsoft zbadane, sposób wyświetlania okna dialogowego, gdy fokus zostanie przeniesiony do kontrolki pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="4f7b9-171">Następnie przedstawiono sposób tworzenia przycisku, który powoduje wyświetlenie okna dialogowego selektora kolorów, po kliknięciu przycisku.</span><span class="sxs-lookup"><span data-stu-id="4f7b9-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4f7b9-172">Dalej</span><span class="sxs-lookup"><span data-stu-id="4f7b9-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
