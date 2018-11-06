---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: ff1b17295c58d40d55bdcd4346b83121b579bb4c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020562"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="ce815-104">Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="ce815-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="ce815-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ce815-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ce815-106">W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="ce815-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ce815-107">Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="ce815-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ce815-108">Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ce815-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ce815-109">W tym samouczku przedstawiono sposób dodawania interfejsu użytkownika JQuery [widżet Datepicker](http://jqueryui.com/datepicker/) do formularza sieci Web, a model użycie powiązania do aktualizacji bazy danych przy użyciu wybranej wartości.</span><span class="sxs-lookup"><span data-stu-id="ce815-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="ce815-110">Ten samouczek opiera się na projekt utworzony w [pierwszy](retrieving-data.md) i [drugi](updating-deleting-and-creating-data.md) części tej serii.</span><span class="sxs-lookup"><span data-stu-id="ce815-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="ce815-111">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="ce815-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ce815-112">Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ce815-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ce815-113">Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="ce815-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ce815-114">Będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="ce815-114">What you'll build</span></span>

<span data-ttu-id="ce815-115">W ramach tego samouczka należy:</span><span class="sxs-lookup"><span data-stu-id="ce815-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ce815-116">Dodawanie właściwości do modelu w taki sposób, aby zarejestrować studenta Data rejestracji</span><span class="sxs-lookup"><span data-stu-id="ce815-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="ce815-117">Włącz użytkownikowi wybranie daty rejestracji za pomocą widżetu selektora daty interfejsu użytkownika JQuery</span><span class="sxs-lookup"><span data-stu-id="ce815-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="ce815-118">Wymuszanie reguł sprawdzania poprawności dla Data rejestracji</span><span class="sxs-lookup"><span data-stu-id="ce815-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="ce815-119">Element widget selektora daty interfejsu użytkownika JQuery umożliwia użytkownikom łatwe wybierz datę z kalendarza, który pojawia się po użytkownik wchodzi w interakcję z polem.</span><span class="sxs-lookup"><span data-stu-id="ce815-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="ce815-120">Za pomocą tego widżetu można wygodniejsze niż ręcznie wpisując wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="ce815-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="ce815-121">Integrowanie widżet Datepicker strona, która używa wiązania modelu dla operacji na danych wymaga tylko niewielką ilość pracy dodatkowe.</span><span class="sxs-lookup"><span data-stu-id="ce815-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="ce815-122">Dodaj nową właściwość do modelu</span><span class="sxs-lookup"><span data-stu-id="ce815-122">Add a new property to the model</span></span>

<span data-ttu-id="ce815-123">Najpierw należy dodać **daty/godziny** właściwości, aby zainteresować studentów modelu, a następnie przeprowadzić migrację tej zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ce815-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="ce815-124">Otwórz **UniversityModels.cs**i Dodaj wyróżniony kod do modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="ce815-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="ce815-125">**RangeAttribute** jest dołączony do wymuszania reguł sprawdzania poprawności dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="ce815-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="ce815-126">W tym samouczku zakładamy, że Contoso University opiera się na 1 stycznia 2013 r. i w związku z tym starszych daty rejestracji są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="ce815-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="ce815-127">W oknie Zarządzanie pakietami za pomocą polecenia Dodaj migrację **AddEnrollmentDate Dodaj migracji**.</span><span class="sxs-lookup"><span data-stu-id="ce815-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="ce815-128">Należy zauważyć, że kod migracji dodaje nową kolumnę daty/godziny do tabeli dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="ce815-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="ce815-129">Do dopasowania wartości, które określiłeś w RangeAttribute, Dodaj wartość domyślną dla nowej kolumny, jak pokazano w poniższym kodzie wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="ce815-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="ce815-130">Zapisz zmiany w pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="ce815-130">Save your change to the migration file.</span></span>

<span data-ttu-id="ce815-131">Nie trzeba ponownie umieścić początkowo dane.</span><span class="sxs-lookup"><span data-stu-id="ce815-131">You do not need to seed the data again.</span></span> <span data-ttu-id="ce815-132">W związku z tym, otwórz **Configuration.cs** w folderze migracje i usuń lub przekształcić w komentarz kod w **inicjatora** metody.</span><span class="sxs-lookup"><span data-stu-id="ce815-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="ce815-133">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="ce815-133">Save and close the file.</span></span>

<span data-ttu-id="ce815-134">Teraz uruchom polecenie **update-database**.</span><span class="sxs-lookup"><span data-stu-id="ce815-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="ce815-135">Należy zauważyć, że kolumna teraz istnieje w bazie danych, a wszystkie istniejące rekordy mają wartość domyślną dla EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="ce815-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="ce815-136">Dodawanie kontrolek dynamicznych dla Data rejestracji</span><span class="sxs-lookup"><span data-stu-id="ce815-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="ce815-137">Teraz należy dodać formanty do wyświetlania i edytowania Data rejestracji.</span><span class="sxs-lookup"><span data-stu-id="ce815-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="ce815-138">W tym momencie wartość jest edytowany za pomocą pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="ce815-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="ce815-139">W dalszej części tego samouczka zostanie zmieniony pole tekstowe z walidacją JQuery Datepicker.</span><span class="sxs-lookup"><span data-stu-id="ce815-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="ce815-140">Po pierwsze, należy pamiętać, że nie trzeba wprowadzać zmian, aby jest **AddStudent.aspx** pliku.</span><span class="sxs-lookup"><span data-stu-id="ce815-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="ce815-141">Kontrolka DynamicEntity automatycznie spowoduje, że nowej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ce815-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="ce815-142">Otwórz **Students.aspx**i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="ce815-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="ce815-143">Uruchom aplikację i zwróć uwagę, że wartość Data rejestracji można ustawić, wpisując wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="ce815-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="ce815-144">Podczas dodawania nowego studenta:</span><span class="sxs-lookup"><span data-stu-id="ce815-144">When adding a new student:</span></span>

![Ustaw datę](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="ce815-146">Lub edytowania istniejącej wartości:</span><span class="sxs-lookup"><span data-stu-id="ce815-146">Or, editing an existing value:</span></span>

![Edytuj datę](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="ce815-148">Wpisywanie działa daty, ale może nie być środowiska klienta, które chcą udostępnić.</span><span class="sxs-lookup"><span data-stu-id="ce815-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="ce815-149">W następnej sekcji spowoduje włączenie wybranie daty przy użyciu kalendarza.</span><span class="sxs-lookup"><span data-stu-id="ce815-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="ce815-150">Zainstaluj pakiet NuGet do pracy za pomocą interfejsu użytkownika JQuery</span><span class="sxs-lookup"><span data-stu-id="ce815-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="ce815-151">**UI soku** pakietu NuGet umożliwia łatwą integrację widżetów interfejsu użytkownika JQuery w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ce815-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="ce815-152">Aby użyć tego pakietu, należy go zainstalować za pośrednictwem pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="ce815-152">To use this package, install it through NuGet.</span></span>

![Dodaj soku interfejsu użytkownika](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="ce815-154">Wersja soku interfejsu użytkownika, które zainstalujesz może spowodować konflikt z wersją JQuery w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce815-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="ce815-155">Przed kontynuowaniem za pomocą tego samouczka spróbuj uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce815-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="ce815-156">Jeśli wystąpi błąd kodu JavaScript, należy uzgodnić z wersją JQuery.</span><span class="sxs-lookup"><span data-stu-id="ce815-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="ce815-157">Możesz dodać oczekiwanej wersji JQuery do folderu skryptów (wersja 1.8.2 w momencie pisania tego samouczka) lub w Site.master Określ ścieżkę do pliku JQuery.</span><span class="sxs-lookup"><span data-stu-id="ce815-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="ce815-158">Dostosowywanie szablonu daty/godziny do uwzględnienia Datepicker widżetu</span><span class="sxs-lookup"><span data-stu-id="ce815-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="ce815-159">Element widget Datepicker doda do szablonu danych dynamicznych do edycji wartości daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="ce815-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="ce815-160">Przez dodawanie elementu widget do szablonu, jest on automatycznie renderowany w formie dodawania nowego studenta i w widoku siatki dla uczniów i studentów edycji.</span><span class="sxs-lookup"><span data-stu-id="ce815-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="ce815-161">Otwórz **daty/godziny\_Edit.ascx**i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="ce815-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="ce815-162">W pliku związanym z kodem ustawi daty minimalne i maksymalne DatePicker.</span><span class="sxs-lookup"><span data-stu-id="ce815-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="ce815-163">Ustawiając następujące wartości, możesz uniemożliwi użytkownikom przechodzenia do nieprawidłowe daty.</span><span class="sxs-lookup"><span data-stu-id="ce815-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="ce815-164">Pobierze minimalne i maksymalne wartości z **RangeAttribute** we właściwości daty/godziny, jeśli zostało ono określone.</span><span class="sxs-lookup"><span data-stu-id="ce815-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="ce815-165">Otwórz **daty/godziny\_Edit.ascx.cs**i Dodaj następujący wyróżniony kod do strony\_metoda obciążenia.</span><span class="sxs-lookup"><span data-stu-id="ce815-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="ce815-166">Uruchom aplikację sieci web i przejdź do strony AddStudent.</span><span class="sxs-lookup"><span data-stu-id="ce815-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="ce815-167">Podaj wartości dla pól i Zauważ, że po kliknięciu pola tekstowego dla daty rejestracji kalendarz jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="ce815-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Wybór daty](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="ce815-169">Wybierz datę, a następnie kliknij przycisk **Wstaw**.</span><span class="sxs-lookup"><span data-stu-id="ce815-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="ce815-170">RangeAttribute wymusza sprawdzania poprawności na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ce815-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="ce815-171">Przez ustawienie właściwości minDate dla selektora daty, również zastosować sprawdzania poprawności na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ce815-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="ce815-172">Kalendarz nie zezwala na użytkownika, przejdź do daty przed wartość minDate.</span><span class="sxs-lookup"><span data-stu-id="ce815-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="ce815-173">Podczas edytowania rekordu w widoku siatki, kalendarz jest również wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="ce815-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker w widoku GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="ce815-175">Wniosek</span><span class="sxs-lookup"><span data-stu-id="ce815-175">Conclusion</span></span>

<span data-ttu-id="ce815-176">W tym samouczku przedstawiono sposób włączenia elementu widget JQuery do formularza sieci web, który używa wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="ce815-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="ce815-177">W ciągu następnych [samouczek](using-query-string-values-to-retrieve-data.md), będzie używać wartości ciągu zapytania, podczas wybierania danych.</span><span class="sxs-lookup"><span data-stu-id="ce815-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ce815-178">[Poprzednie](sorting-paging-and-filtering-data.md)
> [dalej](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="ce815-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
