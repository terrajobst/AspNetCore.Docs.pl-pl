---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Badanie sposobu ASP.NET MVC scaffolds pomocnika DropDownList | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 09d2d7a0df5e8ffa14160b7d3c16b1e9da905fa1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="03b13-102">Badanie sposobu ASP.NET MVC scaffolds pomocnika DropDownList</span><span class="sxs-lookup"><span data-stu-id="03b13-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="03b13-103">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="03b13-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="03b13-104">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.</span><span class="sxs-lookup"><span data-stu-id="03b13-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="03b13-105">Nazwa kontrolera **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="03b13-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="03b13-106">Ustaw opcje **Dodaj kontroler** okna dialogowego, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="03b13-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="03b13-107">Edytuj *StoreManager\Index.cshtml* wyświetlić i usunąć `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="03b13-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="03b13-108">Usuwanie `AlbumArtUrl` spowoduje, że prezentacji bardziej czytelne.</span><span class="sxs-lookup"><span data-stu-id="03b13-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="03b13-109">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="03b13-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="03b13-110">Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="03b13-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="03b13-111">Dodaj `OrderBy` klauzuli, albumów będą sortowane według ceny.</span><span class="sxs-lookup"><span data-stu-id="03b13-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="03b13-112">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="03b13-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="03b13-113">Sortowanie według cen ułatwi do testowania zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03b13-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="03b13-114">Podczas testowania edycji i Utwórz metody można użyć wysokich kosztów, zapisane dane będą występować jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="03b13-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="03b13-115">Otwórz *StoreManager\Edit.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="03b13-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="03b13-116">Dodaj następujący wiersz zaraz po znacznika legendy.</span><span class="sxs-lookup"><span data-stu-id="03b13-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="03b13-117">Poniższy kod przedstawia kontekstu tej zmiany:</span><span class="sxs-lookup"><span data-stu-id="03b13-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="03b13-118">`AlbumId` Jest wymagana, aby wprowadzić zmiany w rekordzie albumu.</span><span class="sxs-lookup"><span data-stu-id="03b13-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="03b13-119">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="03b13-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="03b13-120">Zaznacz, aby **Admin** łącze, a następnie wybierz **Utwórz nowy** łącze, aby utworzyć nowy album.</span><span class="sxs-lookup"><span data-stu-id="03b13-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="03b13-121">Sprawdź, czy informacje albumu zostały zapisane.</span><span class="sxs-lookup"><span data-stu-id="03b13-121">Verify the album information was saved.</span></span> <span data-ttu-id="03b13-122">Edytuj album i sprawdź, czy dokonane zmiany są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="03b13-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="03b13-123">Album schematu</span><span class="sxs-lookup"><span data-stu-id="03b13-123">The Album Schema</span></span>

<span data-ttu-id="03b13-124">`StoreManager` Kontrolera utworzonego przez mechanizm szkieletów MVC umożliwia CRUD (tworzenia, odczytu, aktualizacji, usuwania) dostęp do albumów w bazie danych magazynu utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="03b13-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="03b13-125">Poniżej przedstawiono schematu albumu informacji:</span><span class="sxs-lookup"><span data-stu-id="03b13-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="03b13-126">`Albums` Tabeli nie przechowuje genre albumu i opis, przechowuje klucz obcy do `Genres` tabeli.</span><span class="sxs-lookup"><span data-stu-id="03b13-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="03b13-127">`Genres` Tabela zawiera genre nazwę i opis.</span><span class="sxs-lookup"><span data-stu-id="03b13-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="03b13-128">Podobnie `Albums` tabela nie zawiera nazwy wykonawcy albumu, ale klucz obcy do `Artists` tabeli.</span><span class="sxs-lookup"><span data-stu-id="03b13-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="03b13-129">`Artists` Tabela zawiera nazwę wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="03b13-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="03b13-130">Jeśli sprawdzanie danych w `Albums` tabeli widać, każdy wiersz zawiera klucz obcy do `Genres` tabeli i klucza obcego do `Artists` tabeli.</span><span class="sxs-lookup"><span data-stu-id="03b13-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="03b13-131">Na poniższym obrazie Pokaż dane tabeli z `Albums` tabeli.</span><span class="sxs-lookup"><span data-stu-id="03b13-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="03b13-132">Wybierz HTML Tag</span><span class="sxs-lookup"><span data-stu-id="03b13-132">The HTML Select Tag</span></span>

<span data-ttu-id="03b13-133">Kod HTML `<select>` elementu (utworzony przez HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocnika) służy do wyświetlania pełną listę wartości (takie jak lista gatunkami muzyki).</span><span class="sxs-lookup"><span data-stu-id="03b13-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="03b13-134">Dla formularzy edycji gdy bieżąca wartość jest znana, lista wyboru można wyświetlić bieżącą wartość.</span><span class="sxs-lookup"><span data-stu-id="03b13-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="03b13-135">Widzieliśmy tym wcześniej podczas umieszczania wybranej wartości **komedii**.</span><span class="sxs-lookup"><span data-stu-id="03b13-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="03b13-136">Lista wyboru jest idealny dla wyświetlanie kategorii lub dane klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="03b13-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="03b13-137">`<select>` Element klucza obcego Genre Wyświetla listę nazw możliwe genre, ale po zapisaniu formularza właściwość Genre został zaktualizowany o rodzaju wartości klucza obcego, nie nazwy wyświetlane rodzaju.</span><span class="sxs-lookup"><span data-stu-id="03b13-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="03b13-138">Na poniższej ilustracji jest genre wybrane **Disco** i wykonawcy **lato Donna**.</span><span class="sxs-lookup"><span data-stu-id="03b13-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="03b13-139">Badanie platformy ASP.NET MVC szkieletu kodu</span><span class="sxs-lookup"><span data-stu-id="03b13-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="03b13-140">Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `HTTP GET Create` metody.</span><span class="sxs-lookup"><span data-stu-id="03b13-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="03b13-141">`Create` Metoda dodaje dwa [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) obiekty do `ViewBag`, co ma zawierać informacje o rodzaju, a drugi zawiera informacje o wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="03b13-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="03b13-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) przeładowania konstruktora użyta powyżej przyjmuje trzy argumenty:</span><span class="sxs-lookup"><span data-stu-id="03b13-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="03b13-143">*elementy*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierających elementy na liście.</span><span class="sxs-lookup"><span data-stu-id="03b13-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="03b13-144">W powyższym przykładzie listy gatunkami muzyki zwrócone przez `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="03b13-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="03b13-145">*dataValueField*: Nazwa właściwości w **IEnumerable** listę zawierającą wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="03b13-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="03b13-146">W powyższym przykładzie `GenreId` i `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="03b13-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="03b13-147">*dataTextField*: Nazwa właściwości w **IEnumerable** listy, który zawiera informacje do wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="03b13-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="03b13-148">W artystów i tabeli genre `name` pole jest używane.</span><span class="sxs-lookup"><span data-stu-id="03b13-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="03b13-149">Otwórz *Views\StoreManager\Create.cshtml* pliku i sprawdź, czy `Html.DropDownList` znaczników pomocnika dla pola rodzaju.</span><span class="sxs-lookup"><span data-stu-id="03b13-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="03b13-150">Pierwszy wiersz pokazuje, że trwa tworzenie widoku `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="03b13-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="03b13-151">W `Create` metody powyższym, żaden model został przekazany, więc pobiera widoku **null** `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="03b13-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="03b13-152">W tym momencie Tworzenie nowego albumu, firma Microsoft nie ma żadnych `Album` danych dla niego.</span><span class="sxs-lookup"><span data-stu-id="03b13-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="03b13-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) przeciążenia pokazano powyżej przyjmuje nazwę pola, które można powiązać z modelem.</span><span class="sxs-lookup"><span data-stu-id="03b13-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="03b13-154">Również używa tej nazwy do wyszukania **obiekt ViewBag** obiekt zawierający [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) obiektu.</span><span class="sxs-lookup"><span data-stu-id="03b13-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="03b13-155">Za pomocą tego przeciążenia, konieczne jest nazwa **SelectList obiekt ViewBag** obiektu `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="03b13-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="03b13-156">Drugi parametr (`String.Empty`) jest wyświetlany tekst wyświetlany, gdy nie wybrano elementu.</span><span class="sxs-lookup"><span data-stu-id="03b13-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="03b13-157">Jest to dokładnie, co chcemy podczas tworzenia nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="03b13-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="03b13-158">Jeśli drugi parametr usunięte i użyć poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="03b13-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="03b13-159">Lista wyboru domyślnie pierwszym elementem lub skale w naszym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="03b13-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="03b13-160">Badanie `HTTP POST Create` metody.</span><span class="sxs-lookup"><span data-stu-id="03b13-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="03b13-161">To przeciążenie metody `Create` ma metodę `album` obiekt utworzony przez system powiązanie modelu platformy ASP.NET MVC z wartości formularza opublikowane.</span><span class="sxs-lookup"><span data-stu-id="03b13-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="03b13-162">Po przesłaniu nowego albumu, jeśli stan modelu jest nieprawidłowy i nie ma żadnych błędów bazy danych, baza danych jest dodana nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="03b13-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="03b13-163">Na poniższej ilustracji przedstawiono tworzenie nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="03b13-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="03b13-164">Można użyć [narzędzie fiddler](http://www.fiddler2.com/fiddler2/) do sprawdzenia wartości przesłanego formularza że wiązanie modelu ASP.NET MVC używa do utworzenia obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="03b13-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="03b13-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="03b13-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="03b13-166">Tworzenie elementów ViewBag SelectList refaktoryzacji</span><span class="sxs-lookup"><span data-stu-id="03b13-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="03b13-167">Zarówno `Edit` metod i `HTTP POST Create` metoda ma taki sam kod, aby skonfigurować **SelectList** w **obiekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="03b13-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="03b13-168">W w rozumieniu [suchej](http://en.wikipedia.org/wiki/Don't_repeat_yourself), firma Microsoft będzie Refaktoryzuj tego kodu.</span><span class="sxs-lookup"><span data-stu-id="03b13-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="03b13-169">Firma Microsoft będzie wprowadzać później refaktoryzowane Użyj tego kodu.</span><span class="sxs-lookup"><span data-stu-id="03b13-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="03b13-170">Utworzenie nowej metody, aby dodać wykonawcy i genre **SelectList** do **obiekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="03b13-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="03b13-171">Zastąp dwa wiersze, ustawienie `ViewBag` w każdym `Create` i `Edit` metody wywołaniem `SetGenreArtistViewBag` — metoda.</span><span class="sxs-lookup"><span data-stu-id="03b13-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="03b13-172">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="03b13-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="03b13-173">Tworzenie nowego albumu i edytowanie album, aby sprawdzić, czy działa zmiany.</span><span class="sxs-lookup"><span data-stu-id="03b13-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="03b13-174">Jawnie przekazywanie SelectList do DropDownList</span><span class="sxs-lookup"><span data-stu-id="03b13-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="03b13-175">Tworzenie i edytowanie widoków utworzone przy użyciu szkieletów ASP.NET MVC następujące **DropDownList** przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="03b13-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="03b13-176">`DropDownList` Znaczników Tworzenie widoku są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="03b13-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="03b13-177">Ponieważ `ViewBag` właściwość `SelectList` nosi nazwę `GenreId`, **DropDownList** użyje Pomocnika `GenreId` **SelectList** w **obiekt ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="03b13-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="03b13-178">W następujących **DropDownList** przeciążenia, `SelectList` jawnie przekazano.</span><span class="sxs-lookup"><span data-stu-id="03b13-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="03b13-179">Otwórz *Views\StoreManager\Edit.cshtml* pliku, a następnie zmień **DropDownList** jawnie należy przekazać w wywołaniu **SelectList**, za pomocą przeciążenia powyżej.</span><span class="sxs-lookup"><span data-stu-id="03b13-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="03b13-180">W tym rodzaju kategorii.</span><span class="sxs-lookup"><span data-stu-id="03b13-180">Do this for the Genre category.</span></span> <span data-ttu-id="03b13-181">Kompletny kod jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="03b13-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="03b13-182">Uruchom aplikację, a następnie kliknij przycisk **Admin** łącza, a następnie przejdź do albumów Jazz i wybierz **Edytuj** łącza.</span><span class="sxs-lookup"><span data-stu-id="03b13-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="03b13-183">Zamiast przedstawiający Jazz jako aktualnie zaznaczonego genre, zostanie wyświetlony skale.</span><span class="sxs-lookup"><span data-stu-id="03b13-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="03b13-184">Jeśli argument ciągu (właściwości do powiązania) i **SelectList** obiektu mają taką samą nazwę, wybranej wartości nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="03b13-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="03b13-185">Gdy nie mają żadnej wartości wybranych pod warunkiem, przeglądarek domyślnie pierwszym elementem w **SelectList**(czyli **skale** w powyższym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="03b13-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="03b13-186">Jest to znane ograniczenie **DropDownList** pomocnika.</span><span class="sxs-lookup"><span data-stu-id="03b13-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="03b13-187">Otwórz *Controllers\StoreManagerController.cs* plików i zmień **SelectList** nazwy do obiektów `Genres` i `Artists`.</span><span class="sxs-lookup"><span data-stu-id="03b13-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="03b13-188">Kompletny kod jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="03b13-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="03b13-189">Nazwy Genres i artystów są lepiej nazwy kategorii, ponieważ zawierają one tylko identyfikator każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="03b13-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="03b13-190">Refaktoryzacja, który wcześniej robiliśmy płatnej.</span><span class="sxs-lookup"><span data-stu-id="03b13-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="03b13-191">Zamiast zmiany **obiekt ViewBag** w czterech metod naszych zmiany zostały odizolowaną w celu `SetGenreArtistViewBag` metody.</span><span class="sxs-lookup"><span data-stu-id="03b13-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="03b13-192">Zmień **DropDownList** wywołań tworzenia i edytowania widoków do używania nowych **SelectList** nazwy.</span><span class="sxs-lookup"><span data-stu-id="03b13-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="03b13-193">Nowy kod znaczników dla widoku edycji jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="03b13-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="03b13-194">Utwórz widok wymaga pusty ciąg, aby zapobiec wyświetlaniu pierwszy element SelectList.</span><span class="sxs-lookup"><span data-stu-id="03b13-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="03b13-195">Tworzenie nowego albumu i edytowanie album, aby sprawdzić, czy działa zmiany.</span><span class="sxs-lookup"><span data-stu-id="03b13-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="03b13-196">Testowania kodu edycji, wybierając album z określonego rodzaju niż skale.</span><span class="sxs-lookup"><span data-stu-id="03b13-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="03b13-197">Z elementem pomocniczym DropDownList za pomocą modelu widoku</span><span class="sxs-lookup"><span data-stu-id="03b13-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="03b13-198">Utwórz nową klasę w folderze ViewModels o nazwie `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="03b13-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="03b13-199">Zastąp kod w `AlbumSelectListViewModel` klasy następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="03b13-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="03b13-200">`AlbumSelectListViewModel` Konstruktor ma albumu, listę artystów i genres i tworzy obiekt zawierający album i `SelectList` genres i artystów.</span><span class="sxs-lookup"><span data-stu-id="03b13-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="03b13-201">Skompiluj projekt tak `AlbumSelectListViewModel` jest dostępna w przypadku możemy utworzyć widok w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="03b13-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="03b13-202">Dodaj `EditVM` metodę `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="03b13-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="03b13-203">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="03b13-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="03b13-204">Kliknij prawym przyciskiem myszy `AlbumSelectListViewModel`, wybierz pozycję **rozwiązać**, następnie **przy użyciu MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="03b13-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="03b13-205">Alternatywnie można dodać następujące instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="03b13-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="03b13-206">Kliknij prawym przyciskiem myszy `EditVM` i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="03b13-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="03b13-207">Użyj opcji wymienionych poniżej.</span><span class="sxs-lookup"><span data-stu-id="03b13-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="03b13-208">Wybierz **Dodaj**, następnie zamienić zawartość *Views\StoreManager\EditVM.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="03b13-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="03b13-209">`EditVM` Znaczników jest bardzo podobny do oryginalnej `Edit` znaczników z następującymi wyjątkami.</span><span class="sxs-lookup"><span data-stu-id="03b13-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="03b13-210">Właściwości w modelu `Edit` widoku są w formie `model.property`(na przykład `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="03b13-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="03b13-211">Właściwości w modelu `EditVm` widoku są w formie `model.Album.property`(na przykład `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="03b13-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="03b13-212">Jest to spowodowane tym `EditVM` do widoku została przekazana kontener `Album`, a nie `Album` jako w `Edit` widoku.</span><span class="sxs-lookup"><span data-stu-id="03b13-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="03b13-213">**DropDownList** drugi parametr pochodzi z modelu widoku nie **obiekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="03b13-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="03b13-214">**BeginForm** pomocnika w `EditVM` widoku jawnie dokonuje ogłoszenia `Edit` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="03b13-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="03b13-215">Publikując z powrotem do `Edit` akcji, nie trzeba było zapisać `HTTP POST EditVM` akcji można wykorzystywać `HTTP POST` `Edit` akcji.</span><span class="sxs-lookup"><span data-stu-id="03b13-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="03b13-216">Uruchom aplikację i edytowanie albumu.</span><span class="sxs-lookup"><span data-stu-id="03b13-216">Run the application and edit an album.</span></span> <span data-ttu-id="03b13-217">Zmień adres URL do użycia `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="03b13-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="03b13-218">Zmień pola i kliknij przycisk **zapisać** przycisk, aby zweryfikować kodu.</span><span class="sxs-lookup"><span data-stu-id="03b13-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="03b13-219">Rozwiązania należy używać?</span><span class="sxs-lookup"><span data-stu-id="03b13-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="03b13-220">Wszystkie trzy podejścia, wyświetlane są acceptible.</span><span class="sxs-lookup"><span data-stu-id="03b13-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="03b13-221">Wielu deweloperów wolą przebiegu explictily `SelectList` do `DropDownList` przy użyciu `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="03b13-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="03b13-222">Takie podejście charakteryzuje się dodatkową zaletę, zapewniając elastyczność przy użyciu nazwy bardziej odpowiednie dla kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03b13-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="03b13-223">Jest jedno zastrzeżenie: nie można nazwać `ViewBag SelectList` obiekt o takiej samej nazwie jak właściwość modelu.</span><span class="sxs-lookup"><span data-stu-id="03b13-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="03b13-224">Niektórzy deweloperzy preferowane podejście ViewModel.</span><span class="sxs-lookup"><span data-stu-id="03b13-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="03b13-225">Inne osoby należy wziąć pod uwagę na pełniejsze znaczników i generowane HTML podejścia ViewModel uprzywilejowanych.</span><span class="sxs-lookup"><span data-stu-id="03b13-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="03b13-226">W tej sekcji możemy uzyskanych przy użyciu na trzy sposoby **DropDownList** z danymi kategorii.</span><span class="sxs-lookup"><span data-stu-id="03b13-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="03b13-227">W następnej sekcji pokażemy jak dodać nową kategorię.</span><span class="sxs-lookup"><span data-stu-id="03b13-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="03b13-228">[Poprzednie](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [dalej](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="03b13-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
