---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizowanie, usuwanie i tworzenie danych za pomocą wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 127543b0696b01f136b340d07f6f806b6a6fb1eb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021576"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="d87d8-104">Aktualizowanie, usuwanie i tworzenie danych za pomocą wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="d87d8-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="d87d8-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d87d8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d87d8-106">W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="d87d8-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d87d8-107">Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="d87d8-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d87d8-108">Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="d87d8-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d87d8-109">W tym samouczku pokazano, jak tworzenie, aktualizowanie i usuwanie danych przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="d87d8-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="d87d8-110">Będzie można ustawić następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="d87d8-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="d87d8-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="d87d8-111">DeleteMethod</span></span>
> - <span data-ttu-id="d87d8-112">Operacji InsertMethod</span><span class="sxs-lookup"><span data-stu-id="d87d8-112">InsertMethod</span></span>
> - <span data-ttu-id="d87d8-113">Operacji UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="d87d8-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="d87d8-114">Te właściwości otrzymują nazwę metody, która obsługuje odpowiednia operacja.</span><span class="sxs-lookup"><span data-stu-id="d87d8-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="d87d8-115">W ramach tej metody możesz podać logikę do interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="d87d8-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="d87d8-116">Ten samouczek opiera się na projekt utworzony w pierwszym [część](retrieving-data.md) serii.</span><span class="sxs-lookup"><span data-stu-id="d87d8-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="d87d8-117">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="d87d8-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d87d8-118">Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d87d8-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d87d8-119">Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="d87d8-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d87d8-120">Będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="d87d8-120">What you'll build</span></span>

<span data-ttu-id="d87d8-121">W ramach tego samouczka należy:</span><span class="sxs-lookup"><span data-stu-id="d87d8-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d87d8-122">Dodawanie szablonów danych dynamicznych</span><span class="sxs-lookup"><span data-stu-id="d87d8-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="d87d8-123">Włącz aktualizowanie i usuwanie danych za pośrednictwem metody wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="d87d8-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="d87d8-124">Zastosuj reguły sprawdzania poprawności danych — Włączanie, tworząc nowy rekord w bazie danych</span><span class="sxs-lookup"><span data-stu-id="d87d8-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="d87d8-125">Dodawanie szablonów danych dynamicznych</span><span class="sxs-lookup"><span data-stu-id="d87d8-125">Add dynamic data templates</span></span>

<span data-ttu-id="d87d8-126">Aby zapewnić najlepszą jakość obsługi i zminimalizować powtarzających się kodów, użyje danych dynamicznych szablonów.</span><span class="sxs-lookup"><span data-stu-id="d87d8-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="d87d8-127">Można łatwo zintegrować dane dynamiczne wstępnie utworzonych szablonów do istniejącej lokacji, instalując pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="d87d8-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="d87d8-128">Z **Zarządzaj pakietami NuGet**, zainstaluj **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="d87d8-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![Szablony danych dynamicznych](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="d87d8-130">Zwróć uwagę, że projekt obecnie zawiera folder o nazwie **modelu DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="d87d8-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="d87d8-131">W tym folderze znajdzie się szablony, które są automatycznie stosowane do kontrolek dynamicznych w formularzach sieci web.</span><span class="sxs-lookup"><span data-stu-id="d87d8-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![folder danych dynamicznych](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="d87d8-133">Włącz, aktualizowanie i usuwanie</span><span class="sxs-lookup"><span data-stu-id="d87d8-133">Enable updating and deleting</span></span>

<span data-ttu-id="d87d8-134">Umożliwienie użytkownikom aktualizowanie i usuwanie rekordów bazy danych jest bardzo podobny do procesu pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="d87d8-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="d87d8-135">W **operacji UpdateMethod** i **DeleteMethod** właściwościami, można określić nazwy metody, które wykonują te operacje.</span><span class="sxs-lookup"><span data-stu-id="d87d8-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="d87d8-136">Przy użyciu kontrolki GridView można również określić automatycznego generowania edycji i usuń przyciski.</span><span class="sxs-lookup"><span data-stu-id="d87d8-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="d87d8-137">Następujący wyróżniony kod pokazuje dodatki do kodu kontrolki GridView.</span><span class="sxs-lookup"><span data-stu-id="d87d8-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="d87d8-138">W pliku związanym z kodem, Dodaj nazwę za pomocą instrukcji for **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="d87d8-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="d87d8-139">Następnie należy dodać następującą aktualizację i usunięcie metod.</span><span class="sxs-lookup"><span data-stu-id="d87d8-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="d87d8-140">**TryUpdateModel** metoda stosowana pasujących wartości powiązanych z danymi za pomocą formularza sieci web do elementu danych.</span><span class="sxs-lookup"><span data-stu-id="d87d8-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="d87d8-141">Element danych jest pobierany na podstawie wartości parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="d87d8-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="d87d8-142">Wymuszanie wymagań dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="d87d8-142">Enforce validation requirements</span></span>

<span data-ttu-id="d87d8-143">Atrybuty weryfikacji, które został zastosowany do właściwości FirstName, LastName i rok w klasie ucznia jest automatycznie wymuszanych podczas aktualizowania danych.</span><span class="sxs-lookup"><span data-stu-id="d87d8-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="d87d8-144">Formanty DynamicField Dodaj moduły klienta i serwera, na podstawie atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="d87d8-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="d87d8-145">Właściwości imię i nazwisko są wymagane.</span><span class="sxs-lookup"><span data-stu-id="d87d8-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="d87d8-146">Imię nie może przekraczać 20 znaków i nazwisko nie może przekraczać 40 znaków.</span><span class="sxs-lookup"><span data-stu-id="d87d8-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="d87d8-147">Rok musi być prawidłową wartością wyliczenia AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="d87d8-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="d87d8-148">Jeśli użytkownik narusza jedno z wymagań sprawdzania poprawności, aktualizacja nie kontynuować.</span><span class="sxs-lookup"><span data-stu-id="d87d8-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="d87d8-149">Aby wyświetlić komunikat o błędzie, należy dodać kontrolki podsumowania walidacji powyżej widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="d87d8-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="d87d8-150">Aby wyświetlić błędy sprawdzania poprawności z wiązania modelu, należy ustawić **ShowModelStateErrors** właściwością **true**.</span><span class="sxs-lookup"><span data-stu-id="d87d8-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="d87d8-151">Uruchom aplikację sieci web i Usuń wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="d87d8-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualizowanie danych](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="d87d8-153">Należy zauważyć, że jeśli w trybie edycji wartości dla właściwości roku automatycznie jest renderowane jako listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="d87d8-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="d87d8-154">Właściwość Year jest wartością wyliczenia i dane dynamiczne szablon wyliczenia wartości służy do listy rozwijanej listy do edycji.</span><span class="sxs-lookup"><span data-stu-id="d87d8-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="d87d8-155">Tego szablonu można znaleźć, otwierając **wyliczenie\_Edit.ascx** w pliku **modelu DynamicData**/**FieldTemplates** folderu.</span><span class="sxs-lookup"><span data-stu-id="d87d8-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="d87d8-156">Jeśli podano prawidłowe wartości aktualizacji zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="d87d8-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="d87d8-157">Jeśli naruszają jest jedno z wymagań sprawdzania poprawności, będzie można kontynuować aktualizacji i powyżej siatki jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="d87d8-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![komunikat o błędzie](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="d87d8-159">Dodawanie nowych rekordów</span><span class="sxs-lookup"><span data-stu-id="d87d8-159">Add new records</span></span>

<span data-ttu-id="d87d8-160">Nie ma w kontrolce GridView **operacji InsertMethod** właściwości i dlatego nie można używać do dodawania nowego rekordu przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="d87d8-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="d87d8-161">Możesz znaleźć właściwość operacji InsertMethod **FormView**, **DetailsView**, lub **ListView** kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d87d8-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="d87d8-162">W tym samouczku użyjesz kontrolce FormView do dodawania nowego rekordu.</span><span class="sxs-lookup"><span data-stu-id="d87d8-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="d87d8-163">Najpierw dodaj łącze do nowej strony, która zostanie utworzona dla dodawania nowego rekordu.</span><span class="sxs-lookup"><span data-stu-id="d87d8-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="d87d8-164">Powyżej podsumowania walidacji należy dodać:</span><span class="sxs-lookup"><span data-stu-id="d87d8-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="d87d8-165">Nowe łącze pojawi się w górnej części zawartości dla strony studentów.</span><span class="sxs-lookup"><span data-stu-id="d87d8-165">The new link will appear at the top of the content for the Students page.</span></span>

![Nowy link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="d87d8-167">Następnie dodaj nowy formularz sieci web za pomocą strony wzorcowej i nadaj mu nazwę **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="d87d8-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="d87d8-168">Wybierz Site.Master jako strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="d87d8-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="d87d8-169">Renderowanie zostanie przeprowadzone pola, aby dodać nowego studenta przy użyciu **DynamicEntity** kontroli.</span><span class="sxs-lookup"><span data-stu-id="d87d8-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="d87d8-170">Kontrolka DynamicEntity renderuje tej właściwości można edytować w klasie określony we właściwości ItemType.</span><span class="sxs-lookup"><span data-stu-id="d87d8-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="d87d8-171">Właściwość StudentID została oznaczona **[ScaffoldColumn(false)]** atrybutu, dzięki czemu nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="d87d8-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="d87d8-172">W zastępczym MainContent strony AddStudent Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="d87d8-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="d87d8-173">W pliku związanym z kodem (AddStudent.aspx.cs) Dodaj **przy użyciu** poufności informacji dotyczące **ContosoUniversityModelBinding.Models** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d87d8-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="d87d8-174">Następnie dodaj następujące metody, aby określić sposób wstawiania nowego rekordu i program obsługi zdarzeń dla przycisku Anuluj.</span><span class="sxs-lookup"><span data-stu-id="d87d8-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="d87d8-175">Zapisz wszystkie zmiany.</span><span class="sxs-lookup"><span data-stu-id="d87d8-175">Save all of the changes.</span></span>

<span data-ttu-id="d87d8-176">Uruchom aplikację sieci web i utworzyć nowego studenta.</span><span class="sxs-lookup"><span data-stu-id="d87d8-176">Run the web application and create a new student.</span></span>

![dodać nowego studenta](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="d87d8-178">Kliknij przycisk **Wstaw** i zwróć uwagę, utworzono nowego studenta.</span><span class="sxs-lookup"><span data-stu-id="d87d8-178">Click **Insert** and notice the new student has been created.</span></span>

![Wyświetlanie nowego studenta](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="d87d8-180">Wniosek</span><span class="sxs-lookup"><span data-stu-id="d87d8-180">Conclusion</span></span>

<span data-ttu-id="d87d8-181">W tym samouczku włączone aktualizowanie, usuwanie i tworzenie danych.</span><span class="sxs-lookup"><span data-stu-id="d87d8-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="d87d8-182">Możesz zapewnić reguł sprawdzania poprawności są stosowane podczas interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="d87d8-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="d87d8-183">W ciągu następnych [samouczek](sorting-paging-and-filtering-data.md) w tej serii umożliwi sortowanie, stronicowanie i filtrowanie danych.</span><span class="sxs-lookup"><span data-stu-id="d87d8-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d87d8-184">[Poprzednie](retrieving-data.md)
> [dalej](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="d87d8-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
