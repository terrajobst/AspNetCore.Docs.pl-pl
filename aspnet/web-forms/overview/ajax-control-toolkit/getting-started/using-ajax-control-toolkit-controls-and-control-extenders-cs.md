---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Za pomocą technologii AJAX kontrolki zestawu narzędzi kontrolek i rozszerzeń (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać do stron ASP.NET AJAX Control Toolkit kontrolek i rozszerzeń.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 18ee6dd71fe0e84ec7628eba63aabeee0690d0b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752788"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="0725b-103">Za pomocą technologii AJAX kontrolki zestawu narzędzi kontrolek i rozszerzeń (C#)</span><span class="sxs-lookup"><span data-stu-id="0725b-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>
====================
<span data-ttu-id="0725b-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0725b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0725b-105">Dowiedz się, jak dodać do stron ASP.NET AJAX Control Toolkit kontrolek i rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="0725b-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="0725b-106">Zestawu narzędzi AJAX Control Toolkit zawiera zestaw kontrolek i rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="0725b-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="0725b-107">W tym samouczku krótki dowiesz się, jak dodać zarówno kontrolek i rozszerzeń do strony ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0725b-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="0725b-108">Aby uzyskać instrukcje dotyczące instalowania zestawu narzędzi AJAX Control Toolkit i dodawanie zestawu narzędzi AJAX Control Toolkit do przybornika Visual Studio/Visual Web Developer, zapoznaj się z samouczkiem [wprowadzenie do zestawu narzędzi AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="0725b-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="0725b-109">Korzystanie z kontrolek zestawu narzędzi kontrolka AJAX</span><span class="sxs-lookup"><span data-stu-id="0725b-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="0725b-110">Kontrolka w postaci zestawu narzędzi AJAX Control Toolkit działa tak samo jak normalne formant ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0725b-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="0725b-111">Możesz przeciągnąć kontrolki z przybornika na stronie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0725b-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="0725b-112">Można dodać kontrolki do strony w widoku projektu lub źródła.</span><span class="sxs-lookup"><span data-stu-id="0725b-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="0725b-113">Istnieje jedno wymaganie dotyczące specjalne, gdy za pomocą kontrolek z zestawu narzędzi AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0725b-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="0725b-114">Strona musi zawierać formantu ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="0725b-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="0725b-115">Formantu ScriptManager jest odpowiedzialny za oraz wszystkie niezbędne języka JavaScript, wymagane przez formanty zestawu narzędzi AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0725b-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="0725b-116">Na przykład karta zestawu narzędzi AJAX Control Toolkit zawiera kontrolki o nazwie kontrolka edytora.</span><span class="sxs-lookup"><span data-stu-id="0725b-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="0725b-117">Ta kontrolka Wyświetla Zaawansowany edytor kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="0725b-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="0725b-118">Wykonaj następujące kroki, aby dodać kontrolkę edytora do strony:</span><span class="sxs-lookup"><span data-stu-id="0725b-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="0725b-119">Tworzenie nowej strony programu ASP.NET o nazwie ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="0725b-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="0725b-120">Wybierz formantu ScriptManager from beneath na karcie rozszerzenia AJAX w przyborniku, a następnie przeciągnij go na stronę.</span><span class="sxs-lookup"><span data-stu-id="0725b-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="0725b-121">Zaznacz formant edytora from beneath kartę zestawu narzędzi AJAX Control Toolkit w przyborniku, a następnie przeciągnij formant na stronie (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="0725b-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="0725b-122">Projektant powinien wyglądać jak rysunek 2.</span><span class="sxs-lookup"><span data-stu-id="0725b-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="0725b-123">Uruchom witrynę sieci web, wybierając opcję menu **debugowania i Rozpocznij debugowanie** lub naciskając klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="0725b-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="0725b-124">Powinna zostać wyświetlona strona, na rysunku 3.</span><span class="sxs-lookup"><span data-stu-id="0725b-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="0725b-125">[![Wybieranie kontrolka edytora HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span></span>

<span data-ttu-id="0725b-126">**Rysunek 01**: Wybieranie kontrolka edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


<span data-ttu-id="0725b-127">[![Projektanta Visual Studio za pomocą formantu ScriptManager i edytowanie](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span></span>

<span data-ttu-id="0725b-128">**Rysunek 02**: projektanta Visual Studio za pomocą formantu ScriptManager i Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


<span data-ttu-id="0725b-129">[![Na stronie DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span></span>

<span data-ttu-id="0725b-130">**Rysunek 03**: strona DisplayEditor.aspx ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="0725b-131">Za pomocą rozszerzeń kontrolki zestawu narzędzi AJAX</span><span class="sxs-lookup"><span data-stu-id="0725b-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="0725b-132">Zestawu narzędzi AJAX Control Toolkit zawiera także kontrolek.</span><span class="sxs-lookup"><span data-stu-id="0725b-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="0725b-133">Jak sugeruje nazwa, rozszerzenia kontrolki zestawu narzędzi rozszerza funkcjonalność istniejącej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="0725b-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="0725b-134">Na przykład rozszerzenie kontrolki ConfirmButton rozszerza standardowe kontrolki przycisku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0725b-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="0725b-135">Urządzenia extender zmienia zachowanie s kontrolki przycisku Tak, aby przycisk powoduje wyświetlenie okna dialogowego potwierdzenia, po jego kliknięciu.</span><span class="sxs-lookup"><span data-stu-id="0725b-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="0725b-136">Rozszerzenia kontrolki zestawu narzędzi, podobnie jak kontrolka w postaci zestawu narzędzi AJAX Control Toolkit wymaga formantu ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="0725b-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="0725b-137">Przed rozpoczęciem korzystania z kontrolek na stronie, należy dodać do strony formantu ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="0725b-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="0725b-138">Wykonaj następujące kroki, aby użyć rozszerzenia kontrolki ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="0725b-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="0725b-139">Tworzenie nowej strony programu ASP.NET o nazwie ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="0725b-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="0725b-140">Dodawanie formantu ScriptManager do strony, przeciągając kontrolki na stronie from beneath na karcie rozszerzenia AJAX.</span><span class="sxs-lookup"><span data-stu-id="0725b-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="0725b-141">Dodaj kontrolkę przycisk standardowy do strony, przeciągnij przycisk from beneath standardowa karta w przyborniku na powierzchnię projektanta.</span><span class="sxs-lookup"><span data-stu-id="0725b-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="0725b-142">Kliknij przycisk **Dodaj Extender** zadań opcji (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="0725b-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="0725b-143">W oknie dialogowym Wybierz rozszerzenia, wybierz ConfirmButtonExtender (zobacz rysunek 5) i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="0725b-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="0725b-144">Zaznacz formant przycisku w projektancie, a następnie rozwiń węzeł urządzenia Extender, Button1\_ConfirmButtonExtender węzła w oknie dialogowym właściwości (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="0725b-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="0725b-145">Przypisz wartość *naprawdę?* właściwości ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="0725b-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="0725b-146">Uruchom stronę, wybierając opcję menu **debugowania i Rozpocznij debugowanie** lub naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="0725b-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="0725b-147">[![Dodawanie opcji urządzenia Extender zadania](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span></span>

<span data-ttu-id="0725b-148">**Rysunek 04**: opcja zadania dodawania rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


<span data-ttu-id="0725b-149">[![Wybierając rozszerzenie kontrolki ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span></span>

<span data-ttu-id="0725b-150">**Rysunek 05**: Wybieranie rozszerzenie kontrolki ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


<span data-ttu-id="0725b-151">[![Ustawienie właściwości kontrolki ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span></span>

<span data-ttu-id="0725b-152">**Rysunek 06**: Ustawianie właściwości kontrolki ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="0725b-153">Po otwarciu strony, powinien zostać wyświetlony przycisk.</span><span class="sxs-lookup"><span data-stu-id="0725b-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="0725b-154">Po kliknięciu przycisku, otrzymasz okno dialogowe potwierdzenia na rysunku 7.</span><span class="sxs-lookup"><span data-stu-id="0725b-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="0725b-155">[![Wyświetlanie okna dialogowego potwierdzenia](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="0725b-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span></span>

<span data-ttu-id="0725b-156">**Rysunek 07**: wyświetlanie okna dialogowego potwierdzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="0725b-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="0725b-157">Należy zauważyć, że zwykle nie przeciągniesz rozszerzenia kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="0725b-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="0725b-158">Zamiast tego należy użyć **Dodaj Extender** zadań opcję, aby dodać rozszerzenie do kontrolki, które zostało już dodane do strony.</span><span class="sxs-lookup"><span data-stu-id="0725b-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="0725b-159">Zwróć uwagę, ponadto ustawienie sterowania właściwości rozszerzeń, otwierając arkusz właściwości kontrolki zostanie przedłużony.</span><span class="sxs-lookup"><span data-stu-id="0725b-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="0725b-160">Jeden formant ASP.NET można rozszerzyć przez wiele kontrolek.</span><span class="sxs-lookup"><span data-stu-id="0725b-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="0725b-161">Arkusz właściwości kontrolki zostanie przedłużony spowoduje wyświetlenie listy wszystkich kontrolek skojarzonego z kontrolką.</span><span class="sxs-lookup"><span data-stu-id="0725b-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0725b-162">[Poprzednie](get-started-with-the-ajax-control-toolkit-cs.md)
> [dalej](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0725b-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
