---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="4db91-104">Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="4db91-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="4db91-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4db91-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4db91-106">Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="4db91-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4db91-107">Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="4db91-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4db91-108">Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="4db91-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4db91-109">Ten samouczek przedstawia sposób użycia wiązania modelu warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="4db91-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="4db91-110">Element członkowski OnCallingDataMethods, aby określić, który można wywoływać metod danych obiektu innych niż bieżąca strona zostanie ustawiona.</span><span class="sxs-lookup"><span data-stu-id="4db91-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="4db91-111">W tym samouczku opiera się na projekcie utworzone w [wcześniej](retrieving-data.md) części serii.</span><span class="sxs-lookup"><span data-stu-id="4db91-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="4db91-112">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="4db91-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4db91-113">Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4db91-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4db91-114">Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="4db91-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="4db91-115">Będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="4db91-115">What you'll build</span></span>

<span data-ttu-id="4db91-116">Wiązanie modelu umożliwia umieść swój kod interakcji danych, w pliku CodeBehind dla strony sieci web lub w klasie logika biznesowa oddzielne.</span><span class="sxs-lookup"><span data-stu-id="4db91-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="4db91-117">Samouczki poprzednie wykazały, jak używać plików z kodem kod interakcji danych.</span><span class="sxs-lookup"><span data-stu-id="4db91-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="4db91-118">Ta metoda działa w przypadku małych witryn, ale może to prowadzić do kodu powtarzania i większa trudności podczas obsługi dużej witryny.</span><span class="sxs-lookup"><span data-stu-id="4db91-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="4db91-119">Może również być bardzo trudne do testowania programistyczne kod, który znajduje się w kodzie plików, ponieważ nie istnieje żadne warstwy abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="4db91-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="4db91-120">Aby scentralizować dane kod interakcji, można utworzyć warstwy logiki biznesowej zawierający wszystkie logiki do interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="4db91-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="4db91-121">Następnie należy wywołać warstwę logiki biznesowej ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="4db91-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="4db91-122">W tym samouczku pokazano, jak przenieść cały kod zapisane w poprzednim samouczków do warstwy logiki biznesowej, a następnie użyj tego kodu ze stron.</span><span class="sxs-lookup"><span data-stu-id="4db91-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="4db91-123">W tym samouczku będziesz:</span><span class="sxs-lookup"><span data-stu-id="4db91-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4db91-124">Przenieś kod z plików z kodem do warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="4db91-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="4db91-125">Zmień formanty powiązane z danymi do wywołania metody warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="4db91-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="4db91-126">Tworzenie warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="4db91-126">Create business logic layer</span></span>

<span data-ttu-id="4db91-127">Teraz utworzysz klasy, która jest wywoływana ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="4db91-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="4db91-128">Metody tej klasy wyglądać podobnie do metody, które są używane w poprzednich samouczki i zawierać atrybuty dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="4db91-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="4db91-129">Najpierw Dodaj nowy folder o nazwie **logiki warstwy Biznesowej**.</span><span class="sxs-lookup"><span data-stu-id="4db91-129">First, add a new folder called **BLL**.</span></span>

![Dodaj folder](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="4db91-131">W folderze logiki warstwy Biznesowej, Utwórz nową klasę o nazwie **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="4db91-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="4db91-132">Będzie zawierać wszystkie operacje danych, które pierwotnie znajdowały się w plików z kodem.</span><span class="sxs-lookup"><span data-stu-id="4db91-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="4db91-133">Te metody są prawie takie same, jak metody w pliku związanym z kodem, ale zawiera pewne zmiany.</span><span class="sxs-lookup"><span data-stu-id="4db91-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="4db91-134">Najważniejsze zmiany, należy pamiętać, jest są kodu z wnętrza wystąpienia nie jest już wykonywane **strony** klasy.</span><span class="sxs-lookup"><span data-stu-id="4db91-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="4db91-135">Zawiera klasy strony **TryUpdateModel** — metoda i **ModelState** właściwości.</span><span class="sxs-lookup"><span data-stu-id="4db91-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="4db91-136">Gdy ten kod zostanie przeniesiony do warstwy logiki biznesowej, masz już wystąpienie klasy strony do wywołania tych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="4db91-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="4db91-137">Aby uniknąć problemów tego problemu, należy dodać **ModelMethodContext** parametru do dowolnej metody, która uzyskuje dostęp do TryUpdateModel lub ModelState.</span><span class="sxs-lookup"><span data-stu-id="4db91-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="4db91-138">Użyj tego parametru ModelMethodContext aby wywołać TryUpdateModel lub pobrać element ModelState.</span><span class="sxs-lookup"><span data-stu-id="4db91-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="4db91-139">Nie jest konieczne wprowadzanie zmian w stronie sieci web dla tego nowego parametru.</span><span class="sxs-lookup"><span data-stu-id="4db91-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="4db91-140">Zastąp kod w SchoolBL.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="4db91-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="4db91-141">Popraw istniejących stron do pobierania danych z warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="4db91-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="4db91-142">Na koniec przekonwertuje stron Students.aspx, AddStudent.aspx i Courses.aspx z za pomocą zapytań w pliku CodeBehind, aby za pomocą warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="4db91-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="4db91-143">Pliki CodeBehind dla uczniów lub studentów, AddStudent i kursów usunąć lub komentarz z następujących metod zapytania:</span><span class="sxs-lookup"><span data-stu-id="4db91-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="4db91-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="4db91-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="4db91-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="4db91-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="4db91-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="4db91-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="4db91-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="4db91-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="4db91-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="4db91-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="4db91-149">Teraz wymaga żaden kod w pliku CodeBehind, które odnoszą się do operacji danych.</span><span class="sxs-lookup"><span data-stu-id="4db91-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="4db91-150">**OnCallingDataMethods** obsługi zdarzeń można określić obiektu dla metod danych.</span><span class="sxs-lookup"><span data-stu-id="4db91-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="4db91-151">W Students.aspx należy dodać wartość dla tego programu obsługi zdarzeń i zmieniać nazwy metod danych do nazwy metod w klasie logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="4db91-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="4db91-152">W pliku CodeBehind dla Students.aspx zdefiniuj programu obsługi zdarzeń dla zdarzenia CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="4db91-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="4db91-153">Ten program obsługi zdarzeń służy do określenia klasy logiki biznesowej danych operacje.</span><span class="sxs-lookup"><span data-stu-id="4db91-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="4db91-154">W AddStudent.aspx zmiany podobne.</span><span class="sxs-lookup"><span data-stu-id="4db91-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="4db91-155">W Courses.aspx zmiany podobne.</span><span class="sxs-lookup"><span data-stu-id="4db91-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="4db91-156">Uruchom aplikację i zwróć uwagę, że wszystkie strony działać zgodnie z ich były wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4db91-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="4db91-157">Logikę weryfikacji działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="4db91-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4db91-158">Wniosek</span><span class="sxs-lookup"><span data-stu-id="4db91-158">Conclusion</span></span>

<span data-ttu-id="4db91-159">W tym samouczku ponownie strukturę aplikacji przy użyciu Warstwa dostępu do danych i warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="4db91-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="4db91-160">Należy określić, czy formanty danych użyć obiektu, który nie jest bieżącej strony danych operacje.</span><span class="sxs-lookup"><span data-stu-id="4db91-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4db91-161">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="4db91-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
