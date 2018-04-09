---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="cd9ec-104">Integrowanie selektora daty interfejsu użytkownika JQuery z wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="cd9ec-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="cd9ec-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cd9ec-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cd9ec-106">Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="cd9ec-107">Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="cd9ec-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="cd9ec-108">Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="cd9ec-109">W tym samouczku przedstawiono sposób dodawania interfejsu użytkownika JQuery [widget selektora daty](http://jqueryui.com/datepicker/) do formularza sieci Web i użyj modelu powiązania do aktualizacji bazy danych przy użyciu wybranej wartości.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="cd9ec-110">W tym samouczku opiera się na projekcie utworzone w [pierwszy](retrieving-data.md) i [drugi](updating-deleting-and-creating-data.md) części serii.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="cd9ec-111">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="cd9ec-112">Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="cd9ec-113">Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="cd9ec-114">Będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="cd9ec-114">What you'll build</span></span>

<span data-ttu-id="cd9ec-115">W tym samouczku będziesz:</span><span class="sxs-lookup"><span data-stu-id="cd9ec-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="cd9ec-116">Dodawanie właściwości do modelu do rejestrowania studenta Data rejestracji</span><span class="sxs-lookup"><span data-stu-id="cd9ec-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="cd9ec-117">Włącz użytkownikowi na wybranie daty rejestracji za pomocą elementu widget selektora daty interfejsu użytkownika JQuery</span><span class="sxs-lookup"><span data-stu-id="cd9ec-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="cd9ec-118">Wymuś reguły sprawdzania poprawności dla daty rejestracji</span><span class="sxs-lookup"><span data-stu-id="cd9ec-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="cd9ec-119">Widżet selektora daty interfejsu użytkownika JQuery umożliwia użytkownikom łatwe wybierz datę z kalendarza, która będzie wyświetlana, gdy użytkownik wchodzi w interakcję z polem.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="cd9ec-120">Może być wygodniejsze niż data wpisywać ręcznie przy użyciu tego elementu widget.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="cd9ec-121">Integrowanie widget selektora daty strony, które jest używane powiązanie modelu danych operacje wymaga małej ilości dodatkowych działań.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="cd9ec-122">Dodawanie nowych właściwości do modelu</span><span class="sxs-lookup"><span data-stu-id="cd9ec-122">Add a new property to the model</span></span>

<span data-ttu-id="cd9ec-123">Najpierw należy dodać **Datetime** Twojego uczniów dla właściwości modelu i Migrowanie tej zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="cd9ec-124">Otwórz **UniversityModels.cs**i Dodaj do modelu uczniów wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="cd9ec-125">**RangeAttribute** jest dołączony do wymuszania reguł sprawdzania poprawności dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="cd9ec-126">W tym samouczku zakładamy, że czy Contoso University został opiera się na 1 stycznia 2013 i w związku z tym starszych dat rejestracji są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="cd9ec-127">W oknie zarządzania pakietami Dodaj migracji za pomocą polecenia **AddEnrollmentDate dodać migracji**.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="cd9ec-128">Zwróć uwagę, że kod migracji dodaje nową kolumnę daty i godziny do tabeli dla użytkowników domowych.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="cd9ec-129">Aby odpowiada wartości określonej w RangeAttribute, należy dodać wartość domyślną dla nowej kolumny, jak pokazano w poniższym kodzie wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="cd9ec-130">Zapisz zmiany w pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-130">Save your change to the migration file.</span></span>

<span data-ttu-id="cd9ec-131">Nie trzeba ponownie inicjatora danych.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-131">You do not need to seed the data again.</span></span> <span data-ttu-id="cd9ec-132">W związku z tym Otwórz **Configuration.cs** w folderze migracji i usuń lub komentarz kod w **inicjatora** metody.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="cd9ec-133">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-133">Save and close the file.</span></span>

<span data-ttu-id="cd9ec-134">Teraz uruchom polecenie **update-database**.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="cd9ec-135">Zauważyć, że kolumna teraz istnieje w bazie danych, a wszystkie istniejące rekordy EnrollmentDate mają wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="cd9ec-136">Dodawanie formantów dynamicznych dla Data rejestracji</span><span class="sxs-lookup"><span data-stu-id="cd9ec-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="cd9ec-137">Teraz należy dodać kontrolki do wyświetlania i edytowania Data rejestracji.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="cd9ec-138">W tym momencie wartość jest edytowana za pomocą pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="cd9ec-139">Później w samouczku spowoduje zmianę pola tekstowego na elemencie widget selektora daty JQuery.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="cd9ec-140">Po pierwsze, ważne jest, należy pamiętać, że jest konieczne wprowadzenie zmian do **AddStudent.aspx** pliku.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="cd9ec-141">Formant Dynamiccontrol automatycznie spowoduje, że nowej właściwości.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="cd9ec-142">Otwórz **Students.aspx**i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="cd9ec-143">Uruchom aplikację i zwróć uwagę, że można ustawić wartość Data rejestracji, wpisując datę.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="cd9ec-144">Podczas dodawania nowego uczniów:</span><span class="sxs-lookup"><span data-stu-id="cd9ec-144">When adding a new student:</span></span>

![Ustaw daty](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="cd9ec-146">Lub edytowanie istniejącej wartości:</span><span class="sxs-lookup"><span data-stu-id="cd9ec-146">Or, editing an existing value:</span></span>

![edytowanie daty](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="cd9ec-148">Wpisywanie działa daty, ale nie może być obsługi klienta, który możesz podać.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="cd9ec-149">W następnej sekcji spowoduje włączenie wybranie daty przy użyciu kalendarza.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="cd9ec-150">Zainstaluj pakiet NuGet do pracy z interfejsu użytkownika JQuery</span><span class="sxs-lookup"><span data-stu-id="cd9ec-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="cd9ec-151">**Interfejsu użytkownika soku** pakietu NuGet umożliwia łatwą integrację widżetów interfejsu użytkownika JQuery do aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="cd9ec-152">Aby korzystać z tego pakietu, należy go zainstalować za pośrednictwem pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-152">To use this package, install it through NuGet.</span></span>

![Dodaj soku interfejsu użytkownika](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="cd9ec-154">Wersja soku interfejsu użytkownika, który należy zainstalować może spowodować konflikt z wersją JQuery w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="cd9ec-155">Przed wykonaniem tego samouczka należy ponownie uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="cd9ec-156">Jeśli wystąpi błąd kodu JavaScript, należy uzgodnić z wersją JQuery.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="cd9ec-157">Możesz dodać oczekiwanej wersji JQuery do folderu skryptów (wersja 1.8.2 w momencie pisania tego samouczka) lub w Site.master Określ ścieżkę do pliku JQuery.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="cd9ec-158">Dostosowywanie szablonu daty/godziny mają zawierać element widget selektora daty</span><span class="sxs-lookup"><span data-stu-id="cd9ec-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="cd9ec-159">Widżet selektora daty zostaną dodane do szablonu danych dynamicznych do edycji wartości daty i godziny.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="cd9ec-160">Przez dodanie elementu widget do szablonu, jest on automatycznie renderowany w formie dodawania nowych uczniów i w widoku siatki dla uczniów lub studentów edycji.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="cd9ec-161">Otwórz **DateTime\_Edit.ascx**i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="cd9ec-162">W pliku związanym z kodem spowoduje ustawienie daty minimalną i maksymalną dla selektora daty.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="cd9ec-163">Przez ustawienie tych wartości, możesz uniemożliwi użytkownikom przechodzenie do nieprawidłowe daty.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="cd9ec-164">Pobierze minimalne i maksymalne wartości z **RangeAttribute** dla właściwości DateTime, jeśli zostało ono określone.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="cd9ec-165">Otwórz **DateTime\_Edit.ascx.cs**i Dodaj następujący wyróżniony kod do strony\_załadować metody.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="cd9ec-166">Uruchom aplikację sieci web i przejdź do strony AddStudent.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="cd9ec-167">Podaj wartości dla pól i Zauważ, że po kliknięciu w polu tekstowym dla daty rejestracji kalendarza jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Wybór daty](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="cd9ec-169">Wybierz datę, a następnie kliknij przycisk **Wstaw**.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="cd9ec-170">RangeAttribute wymusza sprawdzania poprawności na serwerze.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="cd9ec-171">Przez ustawienie właściwości minDate dla selektora daty, należy również zastosować sprawdzania poprawności na kliencie.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="cd9ec-172">Kalendarz nie zezwala na użytkownika, przejdź do daty przed wartością minDate.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="cd9ec-173">Podczas edytowania rekordu w widoku siatki kalendarza jest również wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![Selektora daty w widoku GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="cd9ec-175">Wniosek</span><span class="sxs-lookup"><span data-stu-id="cd9ec-175">Conclusion</span></span>

<span data-ttu-id="cd9ec-176">W tym samouczku przedstawiono sposób włączenia elementu widget JQuery do formularza sieci web, która używa powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="cd9ec-177">W następnej [samouczek](using-query-string-values-to-retrieve-data.md), wybierając danych użyje wartości ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="cd9ec-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cd9ec-178">[Poprzednie](sorting-paging-and-filtering-data.md)
> [dalej](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="cd9ec-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
