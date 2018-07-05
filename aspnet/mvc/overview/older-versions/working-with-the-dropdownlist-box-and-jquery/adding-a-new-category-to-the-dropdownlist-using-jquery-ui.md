---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Dodawanie nowej kategorii do metody DropDownList przy użyciu interfejs użytkownika jQuery | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 5d23b1d41834d669bb8006ae0ac963fab60918dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402996"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="22ce2-102">Dodawanie nowej kategorii do metody DropDownList przy użyciu interfejs użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="22ce2-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="22ce2-103">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="22ce2-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="22ce2-104">Kod HTML `Select` tag jest idealny dla prezentowanie danych na stałych kategorii, ale często trzeba dodać nową kategorię.</span><span class="sxs-lookup"><span data-stu-id="22ce2-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="22ce2-105">Załóżmy, że chcemy dodać gatunku "Opera" do kategorii w naszej bazie danych?</span><span class="sxs-lookup"><span data-stu-id="22ce2-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="22ce2-106">W tej sekcji użyjemy interfejs użytkownika jQuery dodać okno dialogowe, które możemy użyć, aby dodać nową kategorię.</span><span class="sxs-lookup"><span data-stu-id="22ce2-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="22ce2-107">Na poniższej ilustracji przedstawiono, jak interfejs użytkownika spowoduje wyświetlenie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="22ce2-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="22ce2-108">Gdy użytkownik wybierze **Dodaj nowe gatunku** łącza, wyskakujące okno dialogowe monituje użytkownika o nazwę nowego gatunku (i opcjonalnie opis).</span><span class="sxs-lookup"><span data-stu-id="22ce2-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="22ce2-109">Obraz poniżej przedstawiono **Dodaj gatunku** wyskakującego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="22ce2-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="22ce2-110">Po wprowadzeniu nowych nazw gatunku i **Zapisz** wypchnięty przycisku, wykonywane są następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="22ce2-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="22ce2-111">Wywołania AJAX przesyła dane do metody Utwórz kontroler gatunku, który zapisuje nowe gatunku do bazy danych i zwraca nowy gatunku informacje (gatunku nazwa i identyfikator) jako dane JSON.</span><span class="sxs-lookup"><span data-stu-id="22ce2-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="22ce2-112">JavaScript dodaje nowe dane gatunku do listy wyboru.</span><span class="sxs-lookup"><span data-stu-id="22ce2-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="22ce2-113">JavaScript sprawia, że nowe gatunku wybranego elementu.</span><span class="sxs-lookup"><span data-stu-id="22ce2-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="22ce2-114">Na ilustracji poniżej **Opera** zostało dodane do bazy danych i wybrać w **gatunku** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="22ce2-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="22ce2-115">Otwórz *Views\StoreManager\Create.cshtml* plik i Zastąp kod znaczników gatunku następującym kodem następujący kod:</span><span class="sxs-lookup"><span data-stu-id="22ce2-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="22ce2-116">`_ChooseGenre` Widoku częściowego zawiera całą logikę umożliwiającą podpiąć JavaScript i jQuery, używany do implementowania Dodaj nową funkcję gatunku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="22ce2-117">Po ukończyliśmy kod będzie prosty sposób taki sam jak wykonawcy interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="22ce2-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="22ce2-118">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *Views\StoreManager* i wybierz polecenie **Dodaj**, następnie **widoku**.</span><span class="sxs-lookup"><span data-stu-id="22ce2-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="22ce2-119">W **nazwy widoku** danych wejściowych, wprowadź `_ChooseGenre` polecenie **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="22ce2-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="22ce2-120">Zastąp kod znaczników w *Views\StoreManager\\_ChooseGenre.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="22ce2-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="22ce2-121">Pierwszy wiersz deklaruje, że firma Microsoft jest przesyłany w `Album` jako nasz model dokładnie tak samo modelu instrukcji znalezionej w widoku Create.</span><span class="sxs-lookup"><span data-stu-id="22ce2-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="22ce2-122">Następnych kilka wierszy są **etykiety** pomocnika znaczników.</span><span class="sxs-lookup"><span data-stu-id="22ce2-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="22ce2-123">Następny wiersz jest **DropDownList** wywołać pomocnika, tak samo jak w oryginalnym widokiem Utwórz.</span><span class="sxs-lookup"><span data-stu-id="22ce2-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="22ce2-124">Następny wiersz dodaje link o nazwie `Add New Genre`, i style go jako przycisku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="22ce2-125">Wiersz zawierający `ValidationMessageFor` jest kopiowana bezpośrednio z widoku Create.</span><span class="sxs-lookup"><span data-stu-id="22ce2-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="22ce2-126">Następujące wiersze:</span><span class="sxs-lookup"><span data-stu-id="22ce2-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="22ce2-127">Tworzy div ukrytych, o identyfikatorze `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="22ce2-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="22ce2-128">Będziemy używać jQuery aby zaczepić naszych **Dodaj gatunku** okno dialogowe z Identyfikatorem `genreDialog` w tym div.</span><span class="sxs-lookup"><span data-stu-id="22ce2-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="22ce2-129">Ostatnie dwa tagi skryptu zawierają łącza do plików JavaScript, które zostaną użyte do zaimplementowania Dodaj nową funkcję gatunku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="22ce2-130">*/Scripts/chooseGenre.js* pliku zostanie podana dla Ciebie w projekcie, firma Microsoft będzie go sprawdzić później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="22ce2-131">Uruchom aplikację, a następnie kliknij pozycję **Dodaj nowe gatunku** przycisku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="22ce2-132">W **Dodaj gatunku** okna dialogowego wprowadź **Opera** w **nazwa** pola wejściowego.</span><span class="sxs-lookup"><span data-stu-id="22ce2-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="22ce2-133">Kliknij przycisk **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-133">Click the **Save** button.</span></span> <span data-ttu-id="22ce2-134">Wywołania AJAX tworzy kategorię Opera następnie wypełnia listę rozwijaną z Opera i ustawia Opera jako wybranego gatunku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="22ce2-135">Wprowadź wykonawcy, tytuł i ceny, a następnie wybierz **Utwórz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="22ce2-136">Jeśli wprowadzasz cenie mniej niż $8.99 nowy album pojawi się u góry widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="22ce2-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="22ce2-137">Sprawdź, czy nowy wpis albumu została zapisana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="22ce2-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="22ce2-138">Spróbuj utworzyć nowy gatunku z tylko jedną literę.</span><span class="sxs-lookup"><span data-stu-id="22ce2-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="22ce2-139">Poniższy kod w *Models\Genre.cs* pliku, Ustawia minimalną i maksymalną długość nazwy gatunku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="22ce2-140">Weryfikacji po stronie klienta zgłasza, że należy wprowadzić ciąg od 2 do 20 znaków.</span><span class="sxs-lookup"><span data-stu-id="22ce2-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="22ce2-141">Badanie sposobu gatunku nowe jest dodawany do bazy danych i listy wybierz.</span><span class="sxs-lookup"><span data-stu-id="22ce2-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="22ce2-142">Otwórz *Scripts\chooseGenre.js* pliku i poszukaj w kodzie.</span><span class="sxs-lookup"><span data-stu-id="22ce2-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="22ce2-143">Drugi wiersz używa Identyfikatora `genreDialog` utworzyć okno dialogowe na tag div w *Views\StoreManager\\_ChooseGenre.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="22ce2-144">Większość nazwanych parametrów to samodzielnie objaśnienia.</span><span class="sxs-lookup"><span data-stu-id="22ce2-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="22ce2-145">`autoOpen` Parametr ma wartość false, wybierając **tworzenie gatunku** przycisku spowoduje to otwarcie okna dialogowego jawnie (jest to opisane ostatnie na).</span><span class="sxs-lookup"><span data-stu-id="22ce2-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="22ce2-146">Okno dialogowe ma dwa przyciski o **Zapisz** i **anulować**.</span><span class="sxs-lookup"><span data-stu-id="22ce2-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="22ce2-147">**Anulować** przycisk zamyka okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="22ce2-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="22ce2-148">Poniższy kod przedstawia **Zapisz** przycisk funkcji.</span><span class="sxs-lookup"><span data-stu-id="22ce2-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="22ce2-149">`var createGenreForm` Wybrana w zaufanym `createGenreForm` identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="22ce2-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="22ce2-150">`createGenreForm` Identyfikator został ustawiony w poniższym kodzie w *Views\Genre\\_CreateGenre.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="22ce2-151">[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) przeciążenia pomocnika używany w *Views\Genre\\_CreateGenre.cshtml* generuje plik HTML z atrybutem akcji zawierającego adres URL można przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="22ce2-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="22ce2-152">Można to zobaczyć, wyświetlając strona albumu tworzenia w przeglądarce i wybranie źródła Pokaż w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="22ce2-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="22ce2-153">Następujący kod przedstawia wygenerowanego kodu HTML zawierający tag formularza.</span><span class="sxs-lookup"><span data-stu-id="22ce2-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="22ce2-154">JQuery `$.post` wiersz wykonuje wywołanie AJAX do atrybutu działanie (`/StoreManager/Create`) i przekazuje dane z **tworzenie gatunku** okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="22ce2-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="22ce2-155">Dane składają się nazwy gatunku nowy i opcjonalny opis.</span><span class="sxs-lookup"><span data-stu-id="22ce2-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="22ce2-156">W przypadku pomyślnego wywołania AJAX nowej gatunku nazwy i wartości, które są dodawane do wybierz znaczników, a nowe gatunku jest ustawiona na wybranej wartości.</span><span class="sxs-lookup"><span data-stu-id="22ce2-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="22ce2-157">Ponieważ jest to dynamicznie generowanych znaczników, nie widzisz nowej opcji wybierz opcję, wyświetlając źródła w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="22ce2-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="22ce2-158">Możesz zobaczyć nowy HTML przy użyciu narzędzi deweloperskich programu Internet Explorer 9 F12.</span><span class="sxs-lookup"><span data-stu-id="22ce2-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="22ce2-159">Aby wyświetlić nową opcję Wybierz, w programie Internet Explorer 9, naciśnij klawisz F12, aby uruchomić narzędzia programistyczne F12.</span><span class="sxs-lookup"><span data-stu-id="22ce2-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="22ce2-160">Przejdź do tworzenia strony i dodać nowe gatunku, aby nowe gatunku jest zaznaczony na liście wybierz gatunku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="22ce2-161">W narzędzia programistyczne F12:</span><span class="sxs-lookup"><span data-stu-id="22ce2-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="22ce2-162">Wybierz kartę HTML.</span><span class="sxs-lookup"><span data-stu-id="22ce2-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="22ce2-163">Naciśnij ikonę odświeżania.</span><span class="sxs-lookup"><span data-stu-id="22ce2-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="22ce2-164">W polu wyszukiwania wprowadź GenreID.</span><span class="sxs-lookup"><span data-stu-id="22ce2-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="22ce2-165">Za pomocą ikony dalej</span><span class="sxs-lookup"><span data-stu-id="22ce2-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="22ce2-166">Przejdź do następującego tagu wybierz:</span><span class="sxs-lookup"><span data-stu-id="22ce2-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="22ce2-167">Rozwiń ostatnią wartość opcji.</span><span class="sxs-lookup"><span data-stu-id="22ce2-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="22ce2-168">Poniższy kod w *Scripts\chooseGenre.js* plik pokazuje, jak **Dodaj nowe gatunku** połączeniu Zdarzenie kliknięcia przycisku i sposób, w jaki **Dodaj nowe gatunku** to okno dialogowe utworzony.</span><span class="sxs-lookup"><span data-stu-id="22ce2-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="22ce2-169">Pierwszy wiersz tworzy funkcję kliknij dołączone do **Dodaj nowe gatunku** przycisku.</span><span class="sxs-lookup"><span data-stu-id="22ce2-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="22ce2-170">Następujące znaczniki z Views\StoreManager\\_ChooseGenre.cshtml plik pokazuje sposób, w jaki **Dodaj nowe gatunku** przycisku zostanie utworzony:</span><span class="sxs-lookup"><span data-stu-id="22ce2-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="22ce2-171">Metoda load tworzy i otwiera okno dialogowe Dodawanie gatunku, a wywołuje jQuery `parse` metody, aby sprawdzanie poprawności klienta występuje na dane wprowadzone w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="22ce2-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="22ce2-172">W tej sekcji mają przedstawiono sposób tworzenia okna dialogowego, który może służyć do dodawania nowych danych kategorii do listy wyboru.</span><span class="sxs-lookup"><span data-stu-id="22ce2-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="22ce2-173">Możesz wykonać tej samej procedury, aby utworzyć interfejs użytkownika, aby dodać nowe wykonawcy do listy wyboru wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="22ce2-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="22ce2-174">W tym samouczku udzielił Omówienie pracy z Pomocnika ASP.NET MVC HTML **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="22ce2-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="22ce2-175">Aby uzyskać dodatkowe informacje na temat pracy z **DropDownList**, zobacz sekcję odwołania do dodawania poniżej.</span><span class="sxs-lookup"><span data-stu-id="22ce2-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="22ce2-176">Daj nam znać czy w tym samouczku został pomocne.</span><span class="sxs-lookup"><span data-stu-id="22ce2-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="22ce2-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="22ce2-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="22ce2-178">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="22ce2-178">Additional References</span></span>

- <span data-ttu-id="22ce2-179">[ASP.NET MVC — kaskadowych w liście rozwijanej są wyświetlane samouczek](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) przez [Radu Enuca o](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="22ce2-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="22ce2-180">[Wybrane](http://harvesthq.github.com/chosen/) wtyczki języka JavaScript, który obsługuje wielokrotnego wyboru i filtrowania.</span><span class="sxs-lookup"><span data-stu-id="22ce2-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="22ce2-181">Współautorzy</span><span class="sxs-lookup"><span data-stu-id="22ce2-181">Contributors</span></span>

- [<span data-ttu-id="22ce2-182">Radu Enuca o</span><span class="sxs-lookup"><span data-stu-id="22ce2-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="22ce2-183">Goupil Jean Sébastien</span><span class="sxs-lookup"><span data-stu-id="22ce2-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="22ce2-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="22ce2-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="22ce2-185">Recenzenci</span><span class="sxs-lookup"><span data-stu-id="22ce2-185">Reviewers</span></span>

- <span data-ttu-id="22ce2-186">Goupil Jean Sébastien</span><span class="sxs-lookup"><span data-stu-id="22ce2-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="22ce2-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="22ce2-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="22ce2-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="22ce2-188">Mike Pope</span></span>
- <span data-ttu-id="22ce2-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="22ce2-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="22ce2-190">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="22ce2-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
