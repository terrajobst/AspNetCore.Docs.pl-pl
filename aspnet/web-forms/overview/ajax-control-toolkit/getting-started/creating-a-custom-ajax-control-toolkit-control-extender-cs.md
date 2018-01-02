---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: "Tworzenie niestandardowych AJAX formantu rozszerzeń formantu Toolkit (C#) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Niestandardowych rozszerzeń umożliwiają dostosowywanie i rozszerzanie możliwości kontrolki ASP.NET, bez konieczności tworzenia nowych klas."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 2ae03484dd1161c65b77f4718bb8cedb5abfdd82
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="1ff05-103">Tworzenie rozszerzeń formantu Toolkit kontroli AJAX niestandardowych (C#)</span><span class="sxs-lookup"><span data-stu-id="1ff05-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="1ff05-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1ff05-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1ff05-105">Niestandardowych rozszerzeń umożliwiają dostosowywanie i rozszerzanie możliwości kontrolki ASP.NET, bez konieczności tworzenia nowych klas.</span><span class="sxs-lookup"><span data-stu-id="1ff05-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="1ff05-106">W tym samouczku Dowiedz się tworzenie niestandardowych rozszerzeń formantu Toolkit kontroli AJAX.</span><span class="sxs-lookup"><span data-stu-id="1ff05-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="1ff05-107">Utworzymy prostą, ale przydatne, nowych rozszerzeń, który zmienia stan przycisku wyłączonego na włączony podczas wpisywania tekstu w pole tekstowe.</span><span class="sxs-lookup"><span data-stu-id="1ff05-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="1ff05-108">Po przeczytaniu tego samouczka, można rozszerzyć zestawie narzędzi programu ASP.NET AJAX z własnych rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="1ff05-109">Można utworzyć przy użyciu programu Visual Studio lub Visual Web Developer rozszerzeń formantu niestandardowego (Upewnij się, że masz najnowszą wersję programu Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="1ff05-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="1ff05-110">Omówienie rozszerzeń DisabledButton</span><span class="sxs-lookup"><span data-stu-id="1ff05-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="1ff05-111">Nasze nowych rozszerzeń formantu nosi nazwę DisabledButton rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="1ff05-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="1ff05-112">Tego rozszerzenia ma trzy właściwości:</span><span class="sxs-lookup"><span data-stu-id="1ff05-112">This extender will have three properties:</span></span>

- <span data-ttu-id="1ff05-113">Targetcontrolid równa - pole tekstowe, rozszerzający formantu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="1ff05-114">TargetButtonIID - przycisku, który jest wyłączone lub włączone.</span><span class="sxs-lookup"><span data-stu-id="1ff05-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="1ff05-115">DisabledText - tekst wyświetlany na przycisku.</span><span class="sxs-lookup"><span data-stu-id="1ff05-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="1ff05-116">Po rozpoczęciu wprowadzania, przycisk wyświetla wartość właściwości tekst przycisku.</span><span class="sxs-lookup"><span data-stu-id="1ff05-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="1ff05-117">Utworzenie punktu zaczepienia rozszerzeń DisabledButton do formantu TextBox i przycisk.</span><span class="sxs-lookup"><span data-stu-id="1ff05-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="1ff05-118">Przed wprowadzeniem tekst przycisku jest wyłączona, a pole tekstowe i przycisk wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="1ff05-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="1ff05-119">([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="1ff05-120">Po rozpoczęciu wpisywania tekstu, ten przycisk jest włączony i pole tekstowe i przycisk wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="1ff05-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="1ff05-121">([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="1ff05-122">Aby utworzyć naszych rozszerzeń formantu, należy utworzyć trzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="1ff05-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="1ff05-123">DisabledButtonExtender.cs — ten plik jest klasy formantu po stronie serwera, która będzie zarządzać tworzenia urządzenia extender i umożliwiają skonfigurowanie właściwości w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="1ff05-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="1ff05-124">Definiuje właściwości, które można ustawić na Twoje rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="1ff05-125">Te właściwości są dostępne za pośrednictwem kodu i w czasie projektowania i odpowiada właściwości zdefiniowane w pliku DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="1ff05-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="1ff05-126">DisabledButtonBehavior.js — Ten plik jest, gdzie zostaną dodane wszystkie logiki skryptu klienta.</span><span class="sxs-lookup"><span data-stu-id="1ff05-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="1ff05-127">DisabledButtonDesigner.cs — ta klasa umożliwia funkcjonalność czasu projektowania.</span><span class="sxs-lookup"><span data-stu-id="1ff05-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="1ff05-128">Ta klasa jest konieczne, jeśli chcesz, rozszerzający formantu do poprawnego działania z programem Visual Studio/Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="1ff05-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="1ff05-129">Dlatego rozszerzeń formantu składa się z formantu po stronie serwera, zachowanie po stronie klienta i po stronie serwera klasy projektanta.</span><span class="sxs-lookup"><span data-stu-id="1ff05-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="1ff05-130">Jak utworzyć wszystkie trzy pliki w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="1ff05-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="1ff05-131">Tworzenie rozszerzeń niestandardową witrynę sieci Web i projektu</span><span class="sxs-lookup"><span data-stu-id="1ff05-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="1ff05-132">Pierwszym krokiem jest utworzenie projektu biblioteki klas i witryny sieci Web w programie Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="1ff05-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="1ff05-133">Firma Microsoft ll tworzenia niestandardowych rozszerzeń w projektu biblioteki klas i testowania niestandardowych rozszerzeń w witrynie internetowej.</span><span class="sxs-lookup"><span data-stu-id="1ff05-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="1ff05-134">Let s uruchomić z poziomu witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1ff05-134">Let�s start with the website.</span></span> <span data-ttu-id="1ff05-135">Wykonaj następujące kroki, aby utworzyć witrynę sieci Web:</span><span class="sxs-lookup"><span data-stu-id="1ff05-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="1ff05-136">Wybierz opcję menu **plik, nowej witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="1ff05-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="1ff05-137">Wybierz **witryny sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="1ff05-138">Nazwa nowej witryny sieci Web *Website1*.</span><span class="sxs-lookup"><span data-stu-id="1ff05-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="1ff05-139">Kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="1ff05-139">Click the **OK** button.</span></span>

<span data-ttu-id="1ff05-140">Następnie należy utworzyć projektu biblioteki klas, zawierające kod dla rozszerzeń formantu:</span><span class="sxs-lookup"><span data-stu-id="1ff05-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="1ff05-141">Wybierz opcję menu **pliku, Dodaj nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ff05-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="1ff05-142">Wybierz **biblioteki klas** szablonu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="1ff05-143">Nazwa nowej biblioteki klasy o nazwie **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="1ff05-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="1ff05-144">Kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="1ff05-144">Click the **OK** button.</span></span>

<span data-ttu-id="1ff05-145">Po wykonaniu tych kroków okna Eksploratora rozwiązań powinien wyglądać rysunek 1.</span><span class="sxs-lookup"><span data-stu-id="1ff05-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="1ff05-146">[![Rozwiązania z witryny sieci Web i klasa projektu biblioteki](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="1ff05-147">**Rysunek 01**: rozwiązania z witryny sieci Web i klasa projektu biblioteki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="1ff05-148">Następnie należy dodać wszystkie niezbędne odwołania zestawów do projektu biblioteki klas:</span><span class="sxs-lookup"><span data-stu-id="1ff05-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="1ff05-149">Kliknij prawym przyciskiem myszy projekt CustomExtenders i wybierz opcję menu **Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="1ff05-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="1ff05-150">Wybierz kartę .NET.</span><span class="sxs-lookup"><span data-stu-id="1ff05-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="1ff05-151">Dodaj odwołania do następujących zestawów:</span><span class="sxs-lookup"><span data-stu-id="1ff05-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="1ff05-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="1ff05-152">System.Web.dll</span></span>
    2. <span data-ttu-id="1ff05-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="1ff05-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="1ff05-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="1ff05-154">System.Design.dll</span></span>
    4. <span data-ttu-id="1ff05-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="1ff05-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="1ff05-156">Wybierz kartę przeglądania.</span><span class="sxs-lookup"><span data-stu-id="1ff05-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="1ff05-157">Dodaj odwołanie do zestawu AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="1ff05-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="1ff05-158">Ten zestaw znajduje się w folderze, którego pobrano Toolkit kontroli AJAX.</span><span class="sxs-lookup"><span data-stu-id="1ff05-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="1ff05-159">Po wykonaniu tych kroków folderze odwołania do projektu biblioteki klas powinien wyglądać na rysunku 2.</span><span class="sxs-lookup"><span data-stu-id="1ff05-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="1ff05-160">[![Odwołania do folderu z odwołania wymagane](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="1ff05-161">**Rysunek 02**: folder odwołań z odwołania wymagane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="1ff05-162">Tworzenie rozszerzeń formantu niestandardowego</span><span class="sxs-lookup"><span data-stu-id="1ff05-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="1ff05-163">Teraz, gdy mamy naszych biblioteki klas, możemy rozpocząć tworzenie naszych formant rozszerzający.</span><span class="sxs-lookup"><span data-stu-id="1ff05-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="1ff05-164">Let s rozpoczynać kości bare niestandardowe rozszerzenie klasy formantu (patrz lista 1).</span><span class="sxs-lookup"><span data-stu-id="1ff05-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="1ff05-165">**Wyświetlanie listy 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="1ff05-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="1ff05-166">Istnieje kilka kwestii, które można zauważyć, że informacje o klasie rozszerzeń formantu w wyświetlania 1.</span><span class="sxs-lookup"><span data-stu-id="1ff05-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="1ff05-167">Najpierw należy zauważyć, że klasa dziedziczy z klasy podstawowej ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="1ff05-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="1ff05-168">Wszystkie formanty rozszerzające AJAX Toolkit formant pochodzi od tej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="1ff05-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="1ff05-169">Na przykład klasa podstawowa zawiera właściwość TargetID, która jest wymagana właściwość co rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="1ff05-170">Następnie należy zauważyć, że klasa zawiera następujące atrybuty powiązane z skrypt po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="1ff05-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="1ff05-171">Widok — powoduje, że plik do załączenia jako osadzonego zasobu w zestawie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="1ff05-172">ClientScriptResource — powoduje, że zasób Skrypt można pobrać z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="1ff05-173">Atrybut widok jest używany do osadzania plik MyControlBehavior.js JavaScript w zestawie podczas kompilowania rozszerzeń niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="1ff05-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="1ff05-174">Atrybut ClientScriptResource służy do pobierania skryptu MyControlBehavior.js z zestawu, gdy niestandardowych rozszerzeń jest używany na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="1ff05-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="1ff05-175">Aby atrybutów zasobów sieci Web i ClientScriptResource działała należy skompilować plik JavaScript jako osadzony zasób.</span><span class="sxs-lookup"><span data-stu-id="1ff05-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="1ff05-176">Wybierz plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisuje wartość *osadzonego zasobu* do **Akcja kompilacji** właściwości.</span><span class="sxs-lookup"><span data-stu-id="1ff05-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="1ff05-177">Zwróć uwagę, że formant rozszerzający obejmuje również atrybutu element TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="1ff05-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="1ff05-178">Ten atrybut służy do określania typu formantu, który zostanie przedłużony rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="1ff05-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="1ff05-179">W przypadku wyświetlania 1 rozszerzający kontroli służy do rozszerzyć pole tekstowe.</span><span class="sxs-lookup"><span data-stu-id="1ff05-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="1ff05-180">Warto zauważyć, że niestandardowe rozszerzenie zawiera właściwość o nazwie MyProperty.</span><span class="sxs-lookup"><span data-stu-id="1ff05-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="1ff05-181">Właściwość jest oznaczona atrybutem ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="1ff05-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="1ff05-182">Metody GetPropertyValue() i SetPropertyValue() są używane do przekazywania wartości właściwości z rozszerzeń po stronie serwera kontroli zachowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1ff05-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="1ff05-183">Let s Przejdź dalej i implementowania kodu dla naszych DisabledButton rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1ff05-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="1ff05-184">Kod dla tego rozszerzenia można znaleźć w wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="1ff05-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="1ff05-185">**Wyświetlanie listy 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="1ff05-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="1ff05-186">Extender DisabledButton wyświetlania 2 ma dwie właściwości o nazwie TargetButtonID i DisabledText.</span><span class="sxs-lookup"><span data-stu-id="1ff05-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="1ff05-187">IDReferenceProperty zastosować do właściwości TargetButtonID uniemożliwia innym niż identyfikator formantu przycisku przypisanie do tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="1ff05-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="1ff05-188">Atrybuty widok i ClientScriptResource skojarzyć zachowanie klienta znajduje się w pliku o nazwie DisabledButtonBehavior.js z tego rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1ff05-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="1ff05-189">Omówiono ten plik JavaScript w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1ff05-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="1ff05-190">Tworzenie niestandardowych rozszerzeń zachowanie</span><span class="sxs-lookup"><span data-stu-id="1ff05-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="1ff05-191">Składnik po stronie klienta rozszerzeń formantu jest nazywany zachowanie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="1ff05-192">Rzeczywiste logikę wyłączenie i włączenie przycisku znajduje się w zachowaniu DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1ff05-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="1ff05-193">Kod JavaScript zachowanie jest dołączona do wyświetlania 3.</span><span class="sxs-lookup"><span data-stu-id="1ff05-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="1ff05-194">**Wyświetlanie listy 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="1ff05-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="1ff05-195">Plik JavaScript w 3 lista zawiera klasę klienta o nazwie DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="1ff05-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="1ff05-196">Ta klasa, takie jak jego dwie po stronie serwera zawiera dwie właściwości o nazwie TargetButtonID i uzyskać DisabledText, którego można uzyskiwać dostęp za pomocą\_TargetButtonID/set\_TargetButtonID i uzyskać\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="1ff05-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="1ff05-197">Metodę initialize() kojarzy obsługi zdarzeń keyup z elementem docelowym zachowania.</span><span class="sxs-lookup"><span data-stu-id="1ff05-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="1ff05-198">Wykonuje obsługi keyup zawsze można wpisać w pole tekstowe skojarzone z tym działaniem literą.</span><span class="sxs-lookup"><span data-stu-id="1ff05-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="1ff05-199">Program obsługi keyup Włącza lub wyłącza przycisk w zależności od tego, czy pole tekstowe skojarzonych z zachowaniem zawiera dowolny tekst.</span><span class="sxs-lookup"><span data-stu-id="1ff05-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="1ff05-200">Należy pamiętać, że należy skompilować plik JavaScript w wyświetlania 3 jako osadzony zasób.</span><span class="sxs-lookup"><span data-stu-id="1ff05-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="1ff05-201">Wybierz plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisuje wartość *osadzonego zasobu* do **Akcja kompilacji** właściwości (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="1ff05-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="1ff05-202">Ta opcja jest dostępna w programie Visual Studio i Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="1ff05-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="1ff05-203">[![Dodawanie pliku JavaScript jako osadzony zasób](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="1ff05-204">**Rysunek 03**: Dodawanie pliku JavaScript jako osadzony zasób ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="1ff05-205">Tworzenie niestandardowych rozszerzeń projektanta</span><span class="sxs-lookup"><span data-stu-id="1ff05-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="1ff05-206">Brak jednego ostatniego klasy, która należy utworzyć w celu ukończenia naszego rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="1ff05-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="1ff05-207">Należy utworzyć designer klasy w listę 4.</span><span class="sxs-lookup"><span data-stu-id="1ff05-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="1ff05-208">Ta klasa jest wymagana do udostępnienia, rozszerzający zadziała poprawnie przy użyciu projektanta Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="1ff05-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="1ff05-209">**Wyświetlanie listy 4 - DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="1ff05-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="1ff05-210">Projektant w listę 4 należy skojarzyć z rozszerzeń DisabledButton z atrybutem projektanta. Należy zastosować atrybut projektanta do klasy DisabledButtonExtender następująco:</span><span class="sxs-lookup"><span data-stu-id="1ff05-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="1ff05-211">Przy użyciu niestandardowych rozszerzeń</span><span class="sxs-lookup"><span data-stu-id="1ff05-211">Using the Custom Extender</span></span>

<span data-ttu-id="1ff05-212">Teraz, gdy Tworzenie rozszerzeń formantu DisabledButton została zakończona, nadszedł czas na używany w naszej witrynie sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ff05-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="1ff05-213">Najpierw należy dodać niestandardowe rozszerzenie do przybornika.</span><span class="sxs-lookup"><span data-stu-id="1ff05-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="1ff05-214">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="1ff05-214">Follow these steps:</span></span>

1. <span data-ttu-id="1ff05-215">Otwórz stronę ASP.NET, klikając dwukrotnie strony w oknie Eksploratora rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="1ff05-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="1ff05-216">Kliknij prawym przyciskiem myszy przybornika i wybierz opcję menu **wybierz elementy**.</span><span class="sxs-lookup"><span data-stu-id="1ff05-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="1ff05-217">W oknie dialogowym Wybierz elementy przybornika przejdź do zestawu CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="1ff05-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="1ff05-218">Kliknij przycisk **OK** przycisk, aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="1ff05-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="1ff05-219">Po wykonaniu tych kroków rozszerzeń formantu DisabledButton powinny być wyświetlane w przyborniku (patrz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="1ff05-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="1ff05-220">[![DisabledButton w przyborniku](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="1ff05-221">**Rysunek 04**: DisabledButton w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="1ff05-222">Następnie należy utworzyć nową stronę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ff05-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="1ff05-223">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="1ff05-223">Follow these steps:</span></span>

1. <span data-ttu-id="1ff05-224">Utwórz nową stronę ASP.NET o nazwie ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="1ff05-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="1ff05-225">Przeciągnij element ScriptManager na stronie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="1ff05-226">Przeciągnij kontrolki pola tekstowego na stronie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="1ff05-227">Przeciągnij formant przycisku na stronie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="1ff05-228">W oknie Właściwości zmień wartość właściwości przycisk identyfikator na wartość *btnSave* i wartość właściwości Text *zapisać\**.</span><span class="sxs-lookup"><span data-stu-id="1ff05-228">In the Properties window, change the Button ID property to the value *btnSave* and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="1ff05-229">Utworzono stronę z formantu standardowego pola tekstowego ASP.NET i przycisk.</span><span class="sxs-lookup"><span data-stu-id="1ff05-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="1ff05-230">Następnie należy rozszerzyć formantu TextBox z rozszerzeń DisabledButton:</span><span class="sxs-lookup"><span data-stu-id="1ff05-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="1ff05-231">Wybierz **Dodawanie rozszerzeń** zadań opcję, aby otworzyć okno dialogowe Kreator Extender (patrz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="1ff05-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="1ff05-232">Zwróć uwagę, że okno dialogowe zawiera naszych niestandardowych rozszerzeń DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1ff05-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="1ff05-233">Wybierz rozszerzenie DisabledButton i kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="1ff05-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="1ff05-234">[![Okno dialogowe Kreator rozszerzeń](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="1ff05-235">**Rysunek 05**: okno dialogowe Kreator rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="1ff05-236">Firma Microsoft wreszcie, ustaw właściwości rozszerzeń DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1ff05-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="1ff05-237">Właściwości rozszerzeń DisabledButton można modyfikować, zmieniając właściwości formantu TextBox:</span><span class="sxs-lookup"><span data-stu-id="1ff05-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="1ff05-238">Wybierz pole tekstowe w projektancie.</span><span class="sxs-lookup"><span data-stu-id="1ff05-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="1ff05-239">W oknie właściwości rozwiń węzeł Extender (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="1ff05-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="1ff05-240">Przypisuje wartość *zapisać* DisabledText właściwości i wartość *btnSave* TargetButtonID właściwości.</span><span class="sxs-lookup"><span data-stu-id="1ff05-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="1ff05-241">[![Ustawianie właściwości rozszerzeń](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="1ff05-242">**Rysunek 06**: Ustawianie właściwości rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="1ff05-243">Po uruchomieniu strony (za pomocą F5) formantu przycisku początkowo jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="1ff05-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="1ff05-244">Jak w przypadku uruchomienia wprowadzanie tekstu w polu tekstowym, przycisk formant jest włączony (patrz rysunek 7).</span><span class="sxs-lookup"><span data-stu-id="1ff05-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="1ff05-245">[![Rozszerzenie DisabledButton w akcji](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="1ff05-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="1ff05-246">**Rysunek 07**: DisabledButton rozszerzeń w akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="1ff05-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="1ff05-247">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="1ff05-247">Summary</span></span>

<span data-ttu-id="1ff05-248">Celem tego samouczka było wyjaśniają, jak można rozszerzyć Toolkit kontroli AJAX z formanty rozszerzające niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="1ff05-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="1ff05-249">W tym samouczku utworzyliśmy proste rozszerzeń formantu DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1ff05-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="1ff05-250">Tworząc klasę DisabledButtonExtender, zachowanie DisabledButtonBehavior JavaScript i klasa DisabledButtonDesigner zaimplementowano tego rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1ff05-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="1ff05-251">Zbiór podobne kroki należy wykonać zawsze, gdy Tworzenie rozszerzeń kontrolki niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="1ff05-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1ff05-252">[Poprzednie](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[dalej](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1ff05-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
