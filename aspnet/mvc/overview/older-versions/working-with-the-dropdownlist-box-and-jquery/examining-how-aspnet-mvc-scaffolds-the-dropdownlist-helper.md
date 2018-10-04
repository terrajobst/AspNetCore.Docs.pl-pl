---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Badanie sposobu platformy ASP.NET MVC scaffolds pomocnika DropDownList | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577850"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="6f92f-102">Badanie sposobu scaffolds pomocnika DropDownList przez platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6f92f-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="6f92f-103">Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="6f92f-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="6f92f-104">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="6f92f-105">Nazwa kontrolera **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="6f92f-106">Ustaw opcje **Dodaj kontroler** okna dialogowego, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="6f92f-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="6f92f-107">Edytuj *StoreManager\Index.cshtml* wyświetlić i usunąć `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="6f92f-108">Usuwanie `AlbumArtUrl` spowoduje, że prezentacja bardziej czytelne.</span><span class="sxs-lookup"><span data-stu-id="6f92f-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="6f92f-109">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="6f92f-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="6f92f-110">Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="6f92f-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="6f92f-111">Dodaj `OrderBy` klauzuli tak albumów zostaną posortowane według ceny.</span><span class="sxs-lookup"><span data-stu-id="6f92f-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="6f92f-112">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="6f92f-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="6f92f-113">Sortowanie według ceny ułatwi testowanie zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6f92f-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="6f92f-114">Podczas testowania edycji, a następnie utworzyć metody, można użyć niskiej cenie, więc zapisane dane będą wyświetlane na początku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="6f92f-115">Otwórz *StoreManager\Edit.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="6f92f-116">Dodaj następujący wiersz po tagu legendy.</span><span class="sxs-lookup"><span data-stu-id="6f92f-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="6f92f-117">Poniższy kod pokazuje kontekstu tej zmiany:</span><span class="sxs-lookup"><span data-stu-id="6f92f-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="6f92f-118">`AlbumId` Jest wymagany do wprowadzania zmian w rekordzie albumu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="6f92f-119">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6f92f-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="6f92f-120">Zaznacz, aby **administratora** łącze, a następnie wybierz **Utwórz nowy** link, aby utworzyć nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="6f92f-121">Sprawdź, czy informacje albumu zostały zapisane.</span><span class="sxs-lookup"><span data-stu-id="6f92f-121">Verify the album information was saved.</span></span> <span data-ttu-id="6f92f-122">Edytuj album i sprawdź, czy dokonane zmiany są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="6f92f-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="6f92f-123">Schemat albumu</span><span class="sxs-lookup"><span data-stu-id="6f92f-123">The Album Schema</span></span>

<span data-ttu-id="6f92f-124">`StoreManager` Kontroler utworzony przez mechanizm tworzenia szkieletów MVC umożliwia dostęp CRUD (tworzenia, odczytu, Update, Delete) do albumów w bazie danych magazynu music.</span><span class="sxs-lookup"><span data-stu-id="6f92f-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="6f92f-125">Schemat albumu informacje znajdują się poniżej:</span><span class="sxs-lookup"><span data-stu-id="6f92f-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="6f92f-126">`Albums` Tabeli nie przechowuje gatunku albumu i opis, przechowuje klucz obcy, aby `Genres` tabeli.</span><span class="sxs-lookup"><span data-stu-id="6f92f-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="6f92f-127">`Genres` Tabela zawiera gatunku nazwę i opis.</span><span class="sxs-lookup"><span data-stu-id="6f92f-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="6f92f-128">Podobnie `Albums` tabela nie zawiera nazwa artystów albumu, ale klucz obcy, aby `Artists` tabeli.</span><span class="sxs-lookup"><span data-stu-id="6f92f-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="6f92f-129">`Artists` Tabela zawiera nazwę wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="6f92f-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="6f92f-130">Jeśli zbadanie danych w `Albums` tabeli, możesz zobaczyć każdy wiersz zawiera klucz obcy, aby `Genres` tabeli i klucz obcy, aby `Artists` tabeli.</span><span class="sxs-lookup"><span data-stu-id="6f92f-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="6f92f-131">Na poniższej ilustracji Pokaż dane tabeli z `Albums` tabeli.</span><span class="sxs-lookup"><span data-stu-id="6f92f-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="6f92f-132">Tag taga Select języka HTML</span><span class="sxs-lookup"><span data-stu-id="6f92f-132">The HTML Select Tag</span></span>

<span data-ttu-id="6f92f-133">Kod HTML `<select>` — element (utworzone przez HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocnika) jest używany, aby wyświetlić pełną listę wartości (takie jak lista gatunki).</span><span class="sxs-lookup"><span data-stu-id="6f92f-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="6f92f-134">Na Edycja formularzy gdy znana jest bieżąca wartość, listy wyboru można wyświetlić bieżącą wartość.</span><span class="sxs-lookup"><span data-stu-id="6f92f-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="6f92f-135">Widzieliśmy tym wcześniej podczas ustawimy wybranej wartości **Komedia**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="6f92f-136">Lista wyboru doskonale nadaje się do wyświetlania kategorii lub danych klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6f92f-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="6f92f-137">`<select>` Element klucza obcego gatunku Wyświetla listę nazw gatunku możliwe, ale po zapisaniu formularza właściwość gatunku jest aktualizowana gatunku wartość klucza obcego, nie nazwy wyświetlane gatunku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="6f92f-138">Na poniższej ilustracji jest gatunku wybrane **Najdywania** i artystów **lato Donną**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="6f92f-139">Badanie platformy ASP.NET MVC szkieletu kodu</span><span class="sxs-lookup"><span data-stu-id="6f92f-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="6f92f-140">Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `HTTP GET Create` metody.</span><span class="sxs-lookup"><span data-stu-id="6f92f-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="6f92f-141">`Create` Metoda dodaje dwa [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) obiekty do `ViewBag`, do zawierają informacje gatunku, a drugi zawiera informacji o wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="6f92f-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="6f92f-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) przeciążenia konstruktora powyżej przyjmuje trzy argumenty:</span><span class="sxs-lookup"><span data-stu-id="6f92f-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="6f92f-143">*elementy*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierający elementy na liście.</span><span class="sxs-lookup"><span data-stu-id="6f92f-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="6f92f-144">W powyższym przykładzie lista gatunki zwrócone przez `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="6f92f-145">*dataValueField*: Nazwa właściwości w **IEnumerable** listę zawierającą wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="6f92f-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="6f92f-146">W powyższym przykładzie `GenreId` i `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="6f92f-147">*dataTextField*: Nazwa właściwości w **IEnumerable** listę zawierającą informacje do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="6f92f-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="6f92f-148">Zarówno artyści, jak i tabela gatunku `name` pole jest używane.</span><span class="sxs-lookup"><span data-stu-id="6f92f-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="6f92f-149">Otwórz *Views\StoreManager\Create.cshtml* plików i zbadaj `Html.DropDownList` znaczników pomocy dla pola gatunku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="6f92f-150">Pierwszy wiersz pokazuje, że trwa tworzenie widoku `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="6f92f-151">W `Create` metoda powyżej, model nie został przekazany, więc pobiera widoku **null** `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="6f92f-152">W tym momencie tworzymy nowy album więc nie ma żadnych `Album` danych dla niego.</span><span class="sxs-lookup"><span data-stu-id="6f92f-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="6f92f-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) przeciążenia powyżej przyjmuje nazwę pola, aby powiązać modelu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="6f92f-154">Również używa tej nazwy do wyszukania **obiekt ViewBag** obiekt zawierający [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) obiektu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="6f92f-155">Korzystając z tego przeciążenia, wymagane jest nazwa **SelectList obiekt ViewBag** obiektu `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="6f92f-156">Drugi parametr (`String.Empty`) jest tekst do wyświetlenia, gdy nie wybrano elementu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="6f92f-157">Jest to dokładnie, co chcemy, aby podczas tworzenia nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="6f92f-158">Jeśli usunięte drugi parametr i użyć następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="6f92f-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="6f92f-159">Domyślnie lista wyboru do pierwszego elementu lub Rock w naszym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="6f92f-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="6f92f-160">Badanie `HTTP POST Create` metody.</span><span class="sxs-lookup"><span data-stu-id="6f92f-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="6f92f-161">To przeciążenie `Create` metoda przyjmuje `album` obiekt utworzony przez system powiązań modelu ASP.NET MVC z opublikowanych wartości formularza.</span><span class="sxs-lookup"><span data-stu-id="6f92f-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="6f92f-162">Po przesłaniu nowego albumu, jeśli stan modelu jest nieprawidłowy i nie ma żadnych błędów bazy danych, nowy album dodaniu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6f92f-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="6f92f-163">Na poniższej ilustracji przedstawiono tworzenie nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="6f92f-164">Możesz użyć [narzędzie fiddler](http://www.fiddler2.com/fiddler2/) umożliwiającej sprawdzenie wartości przesłanego formularza że wiązanie modelu programu ASP.NET MVC używa w celu utworzenia obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="6f92f-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="6f92f-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="6f92f-166">Tworzenie elementów ViewBag SelectList refaktoryzacji</span><span class="sxs-lookup"><span data-stu-id="6f92f-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="6f92f-167">Zarówno `Edit` metod i `HTTP POST Create` metoda ma identyczny kod, aby skonfigurować **SelectList** w **obiekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="6f92f-168">W ducha [susz](http://en.wikipedia.org/wiki/Don't_repeat_yourself), ten kod będzie refaktoryzacji.</span><span class="sxs-lookup"><span data-stu-id="6f92f-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="6f92f-169">My sprawiamy, żeby to użycie zaprojektowane od nowa kodu później.</span><span class="sxs-lookup"><span data-stu-id="6f92f-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="6f92f-170">Utwórz nową metodę, aby dodać gatunku, wykonawcy i **SelectList** do **obiekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="6f92f-171">Zastąp dwa wiersze ustawienie `ViewBag` we wszystkich `Create` i `Edit` metody z wywołaniem `SetGenreArtistViewBag` metody.</span><span class="sxs-lookup"><span data-stu-id="6f92f-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="6f92f-172">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="6f92f-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="6f92f-173">Utwórz nowy album i Edytuj album, aby sprawdzić, czy zmiany działają.</span><span class="sxs-lookup"><span data-stu-id="6f92f-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="6f92f-174">Jawne przekazywanie SelectList do metody DropDownList</span><span class="sxs-lookup"><span data-stu-id="6f92f-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="6f92f-175">Widoki tworzyć i edytować utworzone przy użyciu tworzenia szkieletu ASP.NET MVC następujące **DropDownList** przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="6f92f-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="6f92f-176">`DropDownList` Znaczników widoku Utwórz znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="6f92f-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="6f92f-177">Ponieważ `ViewBag` właściwość `SelectList` nosi nazwę `GenreId`, **DropDownList** użyje Pomocnika `GenreId` **SelectList** w **obiekt ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="6f92f-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="6f92f-178">W następującym **DropDownList** przeciążenia, `SelectList` jawnie jest przekazywany w.</span><span class="sxs-lookup"><span data-stu-id="6f92f-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="6f92f-179">Otwórz *Views\StoreManager\Edit.cshtml* pliku, a następnie zmień **DropDownList** wywołania jawne przekazywanie w **SelectList**, za pomocą przeciążenia powyżej.</span><span class="sxs-lookup"><span data-stu-id="6f92f-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="6f92f-180">W tym dla kategorii gatunku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-180">Do this for the Genre category.</span></span> <span data-ttu-id="6f92f-181">Kompletny kod jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="6f92f-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="6f92f-182">Uruchom aplikację, a następnie kliknij przycisk **administratora** połączyć, a następnie przejdź do albumów Jazz i wybierz **Edytuj** łącza.</span><span class="sxs-lookup"><span data-stu-id="6f92f-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="6f92f-183">Zamiast wyświetlania Jazz jako aktualnie wybranego gatunku, Rock jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="6f92f-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="6f92f-184">Gdy argument ciągu (właściwość można powiązać) i **SelectList** obiektu mają taką samą nazwę, wybranej wartości nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="6f92f-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="6f92f-185">W przypadku nie podano żadnej wartości wybranej przeglądarki domyślne do pierwszego elementu w **SelectList**(czyli **Rock** w powyższym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="6f92f-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="6f92f-186">Jest to znane ograniczenie **DropDownList** pomocnika.</span><span class="sxs-lookup"><span data-stu-id="6f92f-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="6f92f-187">Otwórz *Controllers\StoreManagerController.cs* pliku, a następnie zmień **SelectList** nazwy do obiektów `Genres` i `Artists`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="6f92f-188">Kompletny kod jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="6f92f-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="6f92f-189">Gatunki i artystów są lepsze nazwy kategorii, ponieważ zawierają one więcej niż tylko identyfikator wystąpienia każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="6f92f-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="6f92f-190">Refaktoryzacja, które były wykonywane wcześniej opłaciło.</span><span class="sxs-lookup"><span data-stu-id="6f92f-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="6f92f-191">Zamiast zmieniać **obiekt ViewBag** w cztery metody były izolowane do zmian `SetGenreArtistViewBag` metody.</span><span class="sxs-lookup"><span data-stu-id="6f92f-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="6f92f-192">Zmiana **DropDownList** wywołania tworzenia i edytowania widoków, aby korzystać z nowych **SelectList** nazwy.</span><span class="sxs-lookup"><span data-stu-id="6f92f-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="6f92f-193">Poniżej przedstawiono nowy kod znaczników dla widoku edycji:</span><span class="sxs-lookup"><span data-stu-id="6f92f-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="6f92f-194">Utwórz widok wymaga pusty ciąg, aby uniemożliwić wyświetlenie pierwszy element SelectList.</span><span class="sxs-lookup"><span data-stu-id="6f92f-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="6f92f-195">Utwórz nowy album i Edytuj album, aby sprawdzić, czy zmiany działają.</span><span class="sxs-lookup"><span data-stu-id="6f92f-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="6f92f-196">Przetestować kod edycji, wybierając album z określonego rodzaju inne niż skale.</span><span class="sxs-lookup"><span data-stu-id="6f92f-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="6f92f-197">Przy użyciu Pomocnika DropDownList przy użyciu modelu widoku</span><span class="sxs-lookup"><span data-stu-id="6f92f-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="6f92f-198">Utwórz nową klasę w folderze modele widoków o nazwie `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="6f92f-199">Zastąp kod w `AlbumSelectListViewModel` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6f92f-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="6f92f-200">`AlbumSelectListViewModel` Konstruktor przyjmuje albumu, listę artystów i gatunki i tworzy obiekt zawierający album i `SelectList` gatunki i artystów.</span><span class="sxs-lookup"><span data-stu-id="6f92f-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="6f92f-201">Skompiluj projekt, więc `AlbumSelectListViewModel` jest dostępna podczas tworzenia widoku w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="6f92f-202">Dodaj `EditVM` metody `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="6f92f-203">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="6f92f-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="6f92f-204">Kliknij prawym przyciskiem myszy `AlbumSelectListViewModel`, wybierz opcję **rozwiązać**, następnie **przy użyciu MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="6f92f-205">Alternatywnie, można dodać następujące instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="6f92f-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="6f92f-206">Kliknij prawym przyciskiem myszy `EditVM` i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="6f92f-207">Za pomocą opcji poniżej.</span><span class="sxs-lookup"><span data-stu-id="6f92f-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="6f92f-208">Wybierz **Dodaj**, następnie zastąp zawartość *Views\StoreManager\EditVM.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6f92f-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="6f92f-209">`EditVM` Znaczników jest bardzo podobny do oryginalnego `Edit` znaczników z następującymi wyjątkami.</span><span class="sxs-lookup"><span data-stu-id="6f92f-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="6f92f-210">Właściwości w modelu `Edit` widoku mają postać `model.property`(na przykład `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="6f92f-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="6f92f-211">Właściwości w modelu `EditVm` widoku mają postać `model.Album.property`(na przykład `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="6f92f-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="6f92f-212">To dlatego, że `EditVM` widok jest przekazywany kontener `Album`, a nie `Album` jak `Edit` widoku.</span><span class="sxs-lookup"><span data-stu-id="6f92f-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="6f92f-213">**DropDownList** drugi parametr pochodzi z modelu widoku nie **obiekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="6f92f-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="6f92f-214">**BeginForm** pomocnika w `EditVM` Wyświetl jawnie wpisy do `Edit` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="6f92f-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="6f92f-215">Publikując z powrotem do `Edit` akcji, firma Microsoft nie ma konieczności zapisywania `HTTP POST EditVM` akcji i ponownie użyć `HTTP POST` `Edit` akcji.</span><span class="sxs-lookup"><span data-stu-id="6f92f-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="6f92f-216">Uruchom aplikację, a następnie edytuj albumu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-216">Run the application and edit an album.</span></span> <span data-ttu-id="6f92f-217">Zmień adres URL, aby użyć `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="6f92f-218">Zmienianie pola, a następnie kliknij przycisk **Zapisz** przycisk, aby sprawdzić kod działa.</span><span class="sxs-lookup"><span data-stu-id="6f92f-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="6f92f-219">Które rozwiązanie należy użyć?</span><span class="sxs-lookup"><span data-stu-id="6f92f-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="6f92f-220">Wszystkie trzy metody wyświetlane są akceptowalne.</span><span class="sxs-lookup"><span data-stu-id="6f92f-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="6f92f-221">Wielu programistów chce explictily — dostęp próbny `SelectList` do `DropDownList` przy użyciu `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="6f92f-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="6f92f-222">Takie podejście ma dodatkową zaletę, zapewniając elastyczność przy użyciu bardziej odpowiednie nazwy dla kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6f92f-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="6f92f-223">Jedno zastrzeżenie: to nie nazwa `ViewBag SelectList` obiekt o nazwie identycznej z nazwą właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="6f92f-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="6f92f-224">Niektórzy deweloperzy preferowane podejście ViewModel.</span><span class="sxs-lookup"><span data-stu-id="6f92f-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="6f92f-225">Inne należy wziąć pod uwagę pełniejszy znaczników i wygenerowany kod HTML podejście ViewModel uprzywilejowanych.</span><span class="sxs-lookup"><span data-stu-id="6f92f-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="6f92f-226">W tej sekcji nauczyliśmy się przy użyciu na trzy sposoby **DropDownList** kategorii danych.</span><span class="sxs-lookup"><span data-stu-id="6f92f-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="6f92f-227">W następnej sekcji pokażemy sposób dodać nową kategorię.</span><span class="sxs-lookup"><span data-stu-id="6f92f-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f92f-228">[Poprzednie](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [dalej](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="6f92f-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
