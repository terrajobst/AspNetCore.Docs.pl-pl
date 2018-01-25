---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Tworzenie MVC 3 aplikacji Razor i dyskretny kod JavaScript | Dokumentacja firmy Microsoft
author: microsoft
description: "Przykładową aplikację sieci web listy użytkowników pokazano, jak łatwo jest tworzenie aplikacji ASP.NET MVC 3, za pomocą aparatu widoku Razor. Przykładowe s aplikacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 29b45c07b5498542abbf22c4c3001b1cee41edc9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="1b6ec-104">Tworzenie MVC 3 aplikacji Razor i dyskretny kod JavaScript</span><span class="sxs-lookup"><span data-stu-id="1b6ec-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="1b6ec-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1b6ec-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1b6ec-106">Przykładową aplikację sieci web listy użytkowników pokazano, jak łatwo jest tworzenie aplikacji ASP.NET MVC 3, za pomocą aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="1b6ec-107">Przykładowa aplikacja przedstawia sposób użycia nowy aparat widoku Razor z platformą ASP.NET MVC w wersji 3 i Visual Studio 2010 do utworzenia fikcyjnej listy użytkowników witryny sieci Web, która obejmuje funkcje, takie jak tworzenie, wyświetlanie, edytowanie i usuwanie użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="1b6ec-108">W tym samouczku opisano kroki, które zostały pobrane do zbudowania przykładowej aplikacji ASP.NET MVC 3 listy użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="1b6ec-109">Projektu programu Visual Studio C# i VB kod źródłowy jest dostępny powiązany z tym tematem: [Pobierz](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="1b6ec-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="1b6ec-110">Jeśli masz pytania dotyczące tego samouczka, opublikuj je, aby [MVC forum](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b6ec-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="1b6ec-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1b6ec-111">Overview</span></span>

<span data-ttu-id="1b6ec-112">Aplikację, która będzie zbudować jest witryną internetową listy proste użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="1b6ec-113">Użytkownicy mogą wprowadzać, wyświetlanie i aktualizowanie informacji o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-113">Users can enter, view, and update user information.</span></span>

![Przykład lokacji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="1b6ec-115">Można pobrać projektu zakończone VB i C# [tutaj](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="1b6ec-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="1b6ec-116">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="1b6ec-116">Creating the Web Application</span></span>

<span data-ttu-id="1b6ec-117">Aby uruchomić samouczek, Otwórz program Visual Studio 2010 i utworzyć nowy projekt, korzystając *aplikacji sieci Web programu ASP.NET MVC 3* szablonu.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="1b6ec-118">Nazwa aplikacji &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="1b6ec-119">[![Nowy projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="1b6ec-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="1b6ec-120">W **nowy projekt programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**wybierz aparatu widoku Razor, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Okno dialogowe nowego projektu programu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="1b6ec-122">W tym samouczku nie używana będzie dostawcy członkostwa ASP.NET, więc można usunąć wszystkie pliki skojarzone z logowania i członkostwa.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="1b6ec-123">W **Eksploratora rozwiązań**, Usuń następujące pliki i katalogi:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="1b6ec-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="1b6ec-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="1b6ec-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="1b6ec-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="1b6ec-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="1b6ec-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="1b6ec-127">*Views\Account* (i wszystkie pliki w tym katalogu)</span><span class="sxs-lookup"><span data-stu-id="1b6ec-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="1b6ec-129">Edytuj  *\_Layout.cshtml* plików i Zastąp znaczników wewnątrz `<div>` elementu o nazwie `logindisplay` z komunikatem  *&quot;* wyłączone logowania&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-129">Edit the *\_Layout.cshtml* file and replace the markup inside the `<div>` element named `logindisplay` with the message *&quot;*Login Disabled&quot;.</span></span> <span data-ttu-id="1b6ec-130">W poniższym przykładzie przedstawiono nowy kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="1b6ec-131">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="1b6ec-131">Adding the Model</span></span>

<span data-ttu-id="1b6ec-132">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie kliknij przycisk **klasy**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nowa klasa Mdl użytkownika](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="1b6ec-134">Nazwa klasy `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-134">Name the class `UserModel`.</span></span> <span data-ttu-id="1b6ec-135">Zastąp zawartość *UserModel* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="1b6ec-136">`UserModel` Klasa reprezentuje użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="1b6ec-137">Każdy element członkowski klasy jest oznaczony za pomocą [wymagane](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybutu z [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="1b6ec-138">Atrybuty w [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw udostępnienia weryfikacji automatyczne klienta — i po stronie serwera dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="1b6ec-139">Otwórz `HomeController` klasy i Dodaj `using` dyrektywy, dzięki czemu można uzyskać dostępu do `UserModel` i `Users` klasy:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="1b6ec-140">Tuż po `HomeController` deklaracji, Dodaj poniższy komentarz i odwołanie do `Users` klasy:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="1b6ec-141">`Users` Klasy jest magazynem danych uproszczoną, w pamięci, które będzie używane w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="1b6ec-142">W rzeczywistej aplikacji należy użyć bazy danych, aby przechowywać informacje użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="1b6ec-143">W pierwszym wierszu kilka `HomeController` w poniższym przykładzie przedstawiono plik:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="1b6ec-144">Utworzenie aplikacji, dzięki czemu modelu użytkownika będzie szkieletów kreatora w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="1b6ec-145">Tworzenie widok domyślny</span><span class="sxs-lookup"><span data-stu-id="1b6ec-145">Creating the Default View</span></span>

<span data-ttu-id="1b6ec-146">Następnym krokiem jest, aby dodać metody akcji i widok, aby wyświetlić użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="1b6ec-147">Usuń istniejącą *Views\Home\Index* pliku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="1b6ec-148">Utworzy nowy *indeksu* plik, aby wyświetlić użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="1b6ec-149">W `HomeController` klasy, Zastąp zawartość `Index` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="1b6ec-150">Kliknij prawym przyciskiem myszy wewnątrz `Index` metody, a następnie kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Dodaj widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="1b6ec-152">Wybierz **utworzyć widok jednoznacznie** opcji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="1b6ec-153">Aby uzyskać **wyświetlić klasy danych**, wybierz pozycję **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="1b6ec-154">(Jeśli nie widzisz **Mvc3Razor.Models.UserModel** w **wyświetlić klasy danych** pole, należy skompilować projekt.) Upewnij się, że aparat widoku jest ustawiona na **Razor**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="1b6ec-155">Ustaw **wyświetlania zawartości** do **listy** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-155">Set **View content** to **List** and then click **Add**.</span></span>

![Dodaj widok indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="1b6ec-157">Nowy widok automatycznie scaffolds danych użytkownika, który jest przekazywany do `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="1b6ec-158">Sprawdź nowo utworzonego *Views\Home\Index* pliku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="1b6ec-159">**Utwórz nowy**, **Edytuj**, **szczegóły**, i **usunąć** łącza nie działają, ale pozostałe strony będzie działać.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="1b6ec-160">Uruchom strony.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-160">Run the page.</span></span> <span data-ttu-id="1b6ec-161">Zostanie wyświetlona lista użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-161">You see a list of users.</span></span>

![Strona indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="1b6ec-163">Otwórz *Index.cshtml* plików i Zastąp `ActionLink` kod znaczników dla **Edytuj**, **szczegóły**, i **usunąć** następującym kodem :</span><span class="sxs-lookup"><span data-stu-id="1b6ec-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="1b6ec-164">Nazwa użytkownika jest używana jako identyfikator można znaleźć wybranego rekordu w **Edytuj**, **szczegóły**, i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="1b6ec-165">Tworzenie widoku szczegółów</span><span class="sxs-lookup"><span data-stu-id="1b6ec-165">Creating the Details View</span></span>

<span data-ttu-id="1b6ec-166">Następnym krokiem jest dodanie `Details` metody akcji i widok, aby wyświetlić szczegóły użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="1b6ec-168">Dodaj następujące `Details` metody kontrolerowi macierzystego:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="1b6ec-169">Kliknij prawym przyciskiem myszy wewnątrz `Details` metody, a następnie wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-169">Right-click inside the `Details` method and then select **Add View**.</span></span> <span data-ttu-id="1b6ec-170">Sprawdź, czy **wyświetlić klasy danych** zawiera pole **Mvc3Razor.Models.UserModel***.*</span><span class="sxs-lookup"><span data-stu-id="1b6ec-170">Verify that the **View data class** box contains **Mvc3Razor.Models.UserModel***.*</span></span> <span data-ttu-id="1b6ec-171">Ustaw **wyświetlania zawartości** do **szczegóły** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-171">Set **View content** to **Details** and then click **Add**.</span></span>

![Dodaj widok szczegółów](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="1b6ec-173">Uruchom aplikację, a następnie wybierz łącze Szczegóły.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-173">Run the application and select a details link.</span></span> <span data-ttu-id="1b6ec-174">Automatyczne szkieletów pokazuje każdej właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-174">The automatic scaffolding shows each property in the model.</span></span>

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="1b6ec-176">Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="1b6ec-176">Creating the Edit View</span></span>

<span data-ttu-id="1b6ec-177">Dodaj następujące `Edit` metody do macierzystego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="1b6ec-178">Dodaj widok, jak w poprzednich krokach, ale ustawiona **wyświetlania zawartości** do **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Dodaj widok edycji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="1b6ec-180">Uruchom aplikację i edytowanie imię i nazwisko jednego użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="1b6ec-181">Jeśli użytkownik narusza `DataAnnotation` ograniczenia, które zostały zastosowane do `UserModel` klasy, po przesłaniu formularza, zostanie wyświetlone błędy sprawdzania poprawności, które są tworzone przez kod serwera.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="1b6ec-182">Na przykład, jeśli zmienisz nazwę pierwszego &quot;pods&quot; do &quot;A&quot;po przesłaniu formularza na formularzu jest wyświetlany następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="1b6ec-183">W tym samouczku nazwa użytkownika jest traktowanie jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="1b6ec-184">W związku z tym nie można zmienić właściwości nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="1b6ec-185">W *Edit.cshtml* pliku tuż po `Html.BeginForm` ustawić instrukcji, nazwa użytkownika, który ma zostać ukryte pole.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="1b6ec-186">Powoduje to właściwości, które mają być przekazane w modelu.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="1b6ec-187">Poniższy fragment kodu przedstawia położenie `Hidden` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="1b6ec-188">Zastąp `TextBoxFor` i `ValidationMessageFor` kodu znaczników dla nazwy użytkownika z `DisplayFor` wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="1b6ec-189">`DisplayFor` Metoda Wyświetla właściwość jako element tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="1b6ec-190">W poniższym przykładzie przedstawiono ukończone znaczników.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-190">The following example shows the completed markup.</span></span> <span data-ttu-id="1b6ec-191">Oryginalna `TextBoxFor` i `ValidationMessageFor` wywołania są oznaczone jako komentarz ze znakami komentarz rozpoczęcia i zakończenia komentarza Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="1b6ec-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="1b6ec-192">Włączenie weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="1b6ec-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="1b6ec-193">Aby włączyć weryfikację po stronie klienta w programie ASP.NET MVC 3, należy ustawić dwie flagi i musi zawierać trzy pliki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="1b6ec-194">Otwórz aplikację *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="1b6ec-195">Sprawdź `that ClientValidationEnabled` i `UnobtrusiveJavaScriptEnabled` są ustawione na wartość true w ustawieniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="1b6ec-196">Poniższy fragment z katalogu głównego *Web.config* plików są wyświetlane poprawne ustawienia:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="1b6ec-197">Ustawienie `UnobtrusiveJavaScriptEnabled` na wartość true włącza dyskretny kod technologii Ajax i sprawdzania poprawności dyskretnego kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="1b6ec-198">Gdy używasz sprawdzania poprawności dyskretnego kodu reguły sprawdzania poprawności są przekształcane w atrybuty HTML5.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="1b6ec-199">Nazwy atrybutów HTML5 może zawierać tylko małe litery, cyfry i łączniki.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="1b6ec-200">Ustawienie `ClientValidationEnabled` na wartość true włącza weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="1b6ec-201">Przez ustawienie tych kluczy w aplikacji *Web.config* pliku, Włącz sprawdzanie poprawności klienta i dyskretny kod JavaScript dla całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="1b6ec-202">Można również włączyć lub wyłączyć te ustawienia w poszczególnych widoków lub metod kontrolera, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="1b6ec-203">Należy również uwzględnić w widoku renderowanym kilka plików JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="1b6ec-204">Prosty sposób obejmują JavaScript we wszystkich widokach jest dodanie ich do *Views\Shared\\_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="1b6ec-205">Zastąp `<head>` elementu  *\_Layout.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="1b6ec-206">Pierwsze dwa skrypty jQuery są obsługiwane przez Microsoft Ajax sieci dostarczania zawartości (CDN).</span><span class="sxs-lookup"><span data-stu-id="1b6ec-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="1b6ec-207">Korzystając z usługi Microsoft Ajax CDN, może znacznie poprawić wydajność trafień pierwszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="1b6ec-208">Uruchom aplikację, a następnie kliknij łącze edycji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-208">Run the application and click an edit link.</span></span> <span data-ttu-id="1b6ec-209">Wyświetl źródło strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-209">View the page's source in the browser.</span></span> <span data-ttu-id="1b6ec-210">Źródła przeglądarki zawiera wiele atrybutów w postaci `data-val` (do sprawdzania poprawności danych).</span><span class="sxs-lookup"><span data-stu-id="1b6ec-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="1b6ec-211">Gdy włączone jest sprawdzanie poprawności klienta i dyskretny kod JavaScript, zawierać pól wejściowych przy użyciu reguły weryfikacji klienta `data-val="true"` atrybut do wyzwolenia sprawdzania poprawności dyskretnego kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="1b6ec-212">Na przykład `City` pola w modelu zostało oznaczone [wymagane](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybut, który powoduje HTML pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="1b6ec-213">Dla każdej reguły weryfikacji klienta dodaje się atrybut, który ma postać `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="1b6ec-214">Przy użyciu `City` pola przykładzie wcześniej, generuje reguły weryfikacji klienta wymagane `data-val-required` atrybut i komunikat &quot;Miasto pole jest wymagane&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="1b6ec-215">Uruchom aplikację, edytować jeden z użytkowników, a następnie wyczyść `City` pola.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="1b6ec-216">Gdy karcie poza pole zobaczysz komunikat o błędzie weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Miasto wymagane](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="1b6ec-218">Podobnie, dla każdego parametru reguły weryfikacji klienta jest dodawany atrybut mający formularza `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="1b6ec-219">Na przykład `FirstName` właściwość jest oznaczona przy [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu i określa minimalną długość wynosi 3 i maksymalnie 8.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="1b6ec-220">Reguły sprawdzania poprawności danych o nazwie `length` ma nazwę parametru `max` i wartość parametru 8.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="1b6ec-221">Oto kod HTML, który zostanie wygenerowany dla `FirstName` podczas edycji jednego użytkowników:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="1b6ec-222">Aby uzyskać więcej informacji na temat sprawdzania poprawności dyskretnego kodu klienta, zobacz wpis [dyskretny kod weryfikacji klienta w programie ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) w blogu Brada Wilsona.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="1b6ec-223">W programie ASP.NET MVC 3 w wersji Beta czasami trzeba przesłać formularza, aby rozpocząć weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="1b6ec-224">To może ulec zmianie w ostatecznej wersji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="1b6ec-225">Tworzenie widoku Create</span><span class="sxs-lookup"><span data-stu-id="1b6ec-225">Creating the Create View</span></span>

<span data-ttu-id="1b6ec-226">Następnym krokiem jest dodanie `Create` metody akcji i widoku w celu umożliwienia użytkownikowi utworzenie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="1b6ec-227">Dodaj następujące `Create` metody kontrolerowi macierzystego:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="1b6ec-228">Dodaj widok, jak w poprzednich krokach, ale ustawiona **wyświetlania zawartości** do **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Tworzenie widoku](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="1b6ec-230">Uruchom aplikację, wybierz **Utwórz** łącza, a następnie dodaj nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="1b6ec-231">`Create` — Metoda korzysta automatycznie weryfikacji po stronie klienta i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="1b6ec-232">Spróbuj wprowadzić nazwę użytkownika, który zawiera biały znak, takie jak &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="1b6ec-233">Kiedy karta poza pole nazwy użytkownika, błąd weryfikacji po stronie klienta (`White space is not allowed`) jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="1b6ec-234">Dodaj metodę Delete</span><span class="sxs-lookup"><span data-stu-id="1b6ec-234">Add the Delete method</span></span>

<span data-ttu-id="1b6ec-235">Aby ukończyć samouczek, Dodaj następujący `Delete` metody kontrolerowi macierzystego:</span><span class="sxs-lookup"><span data-stu-id="1b6ec-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="1b6ec-236">Dodaj `Delete` widoku, jak w poprzednich krokach, ustawienie **wyświetlania zawartości** do **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Usuń widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="1b6ec-238">Masz teraz prostą, ale funkcjonalnej aplikacji ASP.NET MVC 3 z weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="1b6ec-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
