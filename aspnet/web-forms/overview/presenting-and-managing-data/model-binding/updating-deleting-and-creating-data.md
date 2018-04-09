---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizowanie, usuwanie i tworzenie danych z wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c22ae-104">Aktualizowanie, usuwanie i tworzenie danych z wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="c22ae-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="c22ae-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c22ae-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c22ae-106">Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c22ae-107">Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="c22ae-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c22ae-108">Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="c22ae-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c22ae-109">W tym samouczku przedstawiono sposób tworzenia, aktualizacji i usuwania danych z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="c22ae-110">Ustawi następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="c22ae-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="c22ae-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="c22ae-111">DeleteMethod</span></span>
> - <span data-ttu-id="c22ae-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="c22ae-112">InsertMethod</span></span>
> - <span data-ttu-id="c22ae-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="c22ae-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="c22ae-114">Te właściwości odbierać nazwę metody, która obsługuje odpowiadająca mu operacja.</span><span class="sxs-lookup"><span data-stu-id="c22ae-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="c22ae-115">W ramach tej metody musisz podać logikę do interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="c22ae-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="c22ae-116">W tym samouczku opiera się na projekcie utworzony w pierwszym [części](retrieving-data.md) serii.</span><span class="sxs-lookup"><span data-stu-id="c22ae-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="c22ae-117">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="c22ae-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c22ae-118">Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c22ae-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c22ae-119">Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="c22ae-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="c22ae-120">Będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="c22ae-120">What you'll build</span></span>

<span data-ttu-id="c22ae-121">W tym samouczku będziesz:</span><span class="sxs-lookup"><span data-stu-id="c22ae-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c22ae-122">Dodaj szablony danych dynamicznych</span><span class="sxs-lookup"><span data-stu-id="c22ae-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="c22ae-123">Włącz aktualizowania i usuwania danych za pomocą metody wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="c22ae-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="c22ae-124">Zastosowanie reguły sprawdzania poprawności danych - Włącz tworzenie nowy rekord w bazie danych</span><span class="sxs-lookup"><span data-stu-id="c22ae-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="c22ae-125">Dodaj szablony danych dynamicznych</span><span class="sxs-lookup"><span data-stu-id="c22ae-125">Add dynamic data templates</span></span>

<span data-ttu-id="c22ae-126">Aby zapewnić najlepsze środowisko użytkownika i zminimalizować powtarzania kodu, użyje szablonów danych dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="c22ae-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="c22ae-127">Można łatwo zintegrować szablonów wstępnie zbudowanych danych dynamicznych istniejącej lokacji, instalując pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="c22ae-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="c22ae-128">Z **Zarządzaj pakietami NuGet**, zainstaluj **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="c22ae-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![Szablony danych dynamicznych](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="c22ae-130">Powiadomienie, że projekt zawiera teraz folder o nazwie **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="c22ae-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="c22ae-131">W tym folderze można znaleźć szablonów, które są automatycznie stosowane do formantów dynamicznych w formularzach sieci web.</span><span class="sxs-lookup"><span data-stu-id="c22ae-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![folder danych dynamicznych](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="c22ae-133">Włącz aktualizowanie i usuwanie</span><span class="sxs-lookup"><span data-stu-id="c22ae-133">Enable updating and deleting</span></span>

<span data-ttu-id="c22ae-134">Umożliwienie użytkownikom się aktualizować i usuwać rekordy w bazie danych jest bardzo podobny do procesu pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="c22ae-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="c22ae-135">W **UpdateMethod** i **DeleteMethod** właściwości, określ nazwy metod, które wykonują te operacje.</span><span class="sxs-lookup"><span data-stu-id="c22ae-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="c22ae-136">Za pomocą formantu widoku GridView możesz określić automatyczne generowanie edycji i usuń przyciski.</span><span class="sxs-lookup"><span data-stu-id="c22ae-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="c22ae-137">Następujący wyróżniony kod pokazuje dodatki do kodu widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="c22ae-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="c22ae-138">W pliku CodeBehind dodać za pomocą instrukcji dla **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="c22ae-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="c22ae-139">Następnie dodaj następującą aktualizację i metody zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="c22ae-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="c22ae-140">**TryUpdateModel** metoda dotyczy pasujących wartości powiązane z danymi za pomocą formularza sieci web elementu danych.</span><span class="sxs-lookup"><span data-stu-id="c22ae-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="c22ae-141">Element danych jest pobierany na podstawie wartości parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="c22ae-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="c22ae-142">Wymusić wymagania weryfikacji</span><span class="sxs-lookup"><span data-stu-id="c22ae-142">Enforce validation requirements</span></span>

<span data-ttu-id="c22ae-143">Atrybuty weryfikacji, które zostały zastosowane do właściwości FirstName, LastName i rok w klasie uczniów automatycznie są wymuszane podczas aktualizowania danych.</span><span class="sxs-lookup"><span data-stu-id="c22ae-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="c22ae-144">Formanty DynamicField Dodaj moduły klienta i serwera, na podstawie atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="c22ae-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="c22ae-145">Właściwości imię i nazwisko są wymagane.</span><span class="sxs-lookup"><span data-stu-id="c22ae-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="c22ae-146">Imię nie może przekraczać 20 znaków długości i nazwisko nie może przekraczać 40 znaków.</span><span class="sxs-lookup"><span data-stu-id="c22ae-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="c22ae-147">Rok musi być prawidłową wartością wyliczenia AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="c22ae-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="c22ae-148">Jeśli użytkownik narusza jeden wymagań sprawdzania poprawności, nie kontynuować aktualizację.</span><span class="sxs-lookup"><span data-stu-id="c22ae-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="c22ae-149">Aby wyświetlić komunikat o błędzie, Dodaj formant ValidationSummary powyżej widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="c22ae-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="c22ae-150">Aby wyświetlić błędy sprawdzania poprawności z wiązania modelu, należy ustawić **ShowModelStateErrors** ustawioną właściwość **true**.</span><span class="sxs-lookup"><span data-stu-id="c22ae-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="c22ae-151">Uruchom aplikację sieci web i Usuń wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="c22ae-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualizowanie danych](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="c22ae-153">Zwróć uwagę, że podczas w trybie edycji wartości dla właściwości roku jest automatycznie renderowane jako listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="c22ae-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="c22ae-154">Właściwość roku jest wartością wyliczenia, a szablon danych dynamicznych dla wartości wyliczenia określa rozwijanej listy do edycji.</span><span class="sxs-lookup"><span data-stu-id="c22ae-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="c22ae-155">Tego szablonu można znaleźć, otwierając **wyliczenie\_Edit.ascx** w pliku **DynamicData**/**FieldTemplates** folderu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="c22ae-156">Jeśli podasz prawidłowe wartości aktualizacji zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="c22ae-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="c22ae-157">Jeśli narusza jest jednym z wymagań sprawdzania poprawności, nie kontynuować aktualizację i powyżej siatki jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="c22ae-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Komunikat o błędzie](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="c22ae-159">Dodaj nowe rekordy</span><span class="sxs-lookup"><span data-stu-id="c22ae-159">Add new records</span></span>

<span data-ttu-id="c22ae-160">Kontrolki widoku siatki nie zawiera **InsertMethod** właściwości i dlatego nie można użyć do dodawania nowego rekordu z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="c22ae-161">Można znaleźć właściwości InsertMethod w **FormView**, **widoku DetailsView**, lub **ListView** kontrolki.</span><span class="sxs-lookup"><span data-stu-id="c22ae-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="c22ae-162">W tym samouczku użyjesz formancie FormView do dodawania nowego rekordu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="c22ae-163">Najpierw dodaj łącze do nowej strony, będzie tworzone na potrzeby dodawania nowego rekordu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="c22ae-164">Powyżej ValidationSummary należy dodać:</span><span class="sxs-lookup"><span data-stu-id="c22ae-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="c22ae-165">W górnej części zawartości dla strony studentów pojawi się nowy link.</span><span class="sxs-lookup"><span data-stu-id="c22ae-165">The new link will appear at the top of the content for the Students page.</span></span>

![Nowy link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="c22ae-167">Następnie należy dodać nowy formularz sieci web za pomocą strony wzorcowej i nadaj jej nazwę **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="c22ae-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="c22ae-168">Wybierz Site.Master jako strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="c22ae-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="c22ae-169">Będą zawierały pola do dodawania nowych uczniów za pomocą **Dynamiccontrol** formantu.</span><span class="sxs-lookup"><span data-stu-id="c22ae-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="c22ae-170">Formant Dynamiccontrol renderuje tej właściwości można edytować w klasie określony we właściwości ItemType.</span><span class="sxs-lookup"><span data-stu-id="c22ae-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="c22ae-171">Właściwość StudentID została oznaczona atrybutem **[ScaffoldColumn(false)]** atrybutu tak nie jest on renderowany.</span><span class="sxs-lookup"><span data-stu-id="c22ae-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="c22ae-172">W zastępczym znacznika strony AddStudent Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="c22ae-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="c22ae-173">W pliku CodeBehind (AddStudent.aspx.cs), Dodaj **przy użyciu** instrukcji dla **ContosoUniversityModelBinding.Models** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c22ae-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="c22ae-174">Następnie należy dodać następujące metody do określenia sposobu wstawić nowy rekord i program obsługi zdarzeń dla przycisku Anuluj.</span><span class="sxs-lookup"><span data-stu-id="c22ae-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="c22ae-175">Zapisz wszystkie zmiany.</span><span class="sxs-lookup"><span data-stu-id="c22ae-175">Save all of the changes.</span></span>

<span data-ttu-id="c22ae-176">Uruchom aplikację sieci web i Utwórz nowe uczniów.</span><span class="sxs-lookup"><span data-stu-id="c22ae-176">Run the web application and create a new student.</span></span>

![Dodaj nowe studentów](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="c22ae-178">Kliknij przycisk **Wstaw** i zwróć uwagę, nowe uczniów został utworzony.</span><span class="sxs-lookup"><span data-stu-id="c22ae-178">Click **Insert** and notice the new student has been created.</span></span>

![Wyświetlanie nowych studentów](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="c22ae-180">Wniosek</span><span class="sxs-lookup"><span data-stu-id="c22ae-180">Conclusion</span></span>

<span data-ttu-id="c22ae-181">W tym samouczku włączone aktualizowanie, usuwanie i tworzenie danych.</span><span class="sxs-lookup"><span data-stu-id="c22ae-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="c22ae-182">Należy zapewnić reguł sprawdzania poprawności są stosowane podczas interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="c22ae-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="c22ae-183">W następnej [samouczek](sorting-paging-and-filtering-data.md) w tej serii spowoduje włączenie sortowania, stronicowania i filtrowanie danych.</span><span class="sxs-lookup"><span data-stu-id="c22ae-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c22ae-184">[Poprzednie](retrieving-data.md)
> [dalej](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="c22ae-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
