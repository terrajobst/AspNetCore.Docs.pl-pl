---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Wprowadzenie do składnika ASP.NET Web Pages — usuwanie danych bazy danych | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak usunąć wpis poszczególnych baz danych. Założono, że zostały wykonane serii za pośrednictwem aktualizacji bazy danych w sieci Web platformy ASP.NET Pa....
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020445"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="17bf1-104">Wprowadzenie do wzorca ASP.NET Web Pages — usuwanie danych bazy danych</span><span class="sxs-lookup"><span data-stu-id="17bf1-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="17bf1-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="17bf1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="17bf1-106">W tym samouczku dowiesz się, jak usunąć wpis poszczególnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="17bf1-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="17bf1-107">Przyjęto założenie, że zostały wykonane serii za pośrednictwem [aktualizowanie bazy danych w programie ASP.NET Web Pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="17bf1-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="17bf1-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="17bf1-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="17bf1-109">Jak wybrać pojedynczego rekordu z listy rekordów.</span><span class="sxs-lookup"><span data-stu-id="17bf1-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="17bf1-110">Jak usunąć pojedynczy rekord z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="17bf1-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="17bf1-111">Jak sprawdzić, czy konkretny przycisk został kliknięty w formularzu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="17bf1-112">Funkcje/technologie omówione:</span><span class="sxs-lookup"><span data-stu-id="17bf1-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="17bf1-113">`WebGrid` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="17bf1-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="17bf1-114">SQL `Delete` polecenia.</span><span class="sxs-lookup"><span data-stu-id="17bf1-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="17bf1-115">`Database.Execute` Metodę, aby uruchomić SQL `Delete` polecenia.</span><span class="sxs-lookup"><span data-stu-id="17bf1-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="17bf1-116">Jakie będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="17bf1-116">What You'll Build</span></span>

<span data-ttu-id="17bf1-117">W poprzednim samouczku przedstawiono sposób aktualizacji istniejącego rekordu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="17bf1-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="17bf1-118">W tym samouczku jest podobny, z tą różnicą, że zamiast aktualizowania rekordu, zostaną one usunięte.</span><span class="sxs-lookup"><span data-stu-id="17bf1-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="17bf1-119">Procesy są bardzo podobne do, z tą różnicą, że usunięcie jest prostsze, więc w tym samouczku będą krótki.</span><span class="sxs-lookup"><span data-stu-id="17bf1-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="17bf1-120">W *filmy* stronie będą aktualizowane `WebGrid` pomocnika, tak że wyświetla **Usuń** łącze obok każdego filmu, która ma towarzyszyć **Edytuj** łącze dodano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="17bf1-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Filmy strona, wyświetlająca łącze usuwanie dla każdego filmu](deleting-data/_static/image1.png)

<span data-ttu-id="17bf1-122">Podobnie jak w przypadku edytowania, po kliknięciu **Usuń** łącza, spowoduje to przejście do innej strony, której informacje film jest już w postaci:</span><span class="sxs-lookup"><span data-stu-id="17bf1-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Usuwanie strony filmu przy użyciu filmu wyświetlane](deleting-data/_static/image2.png)

<span data-ttu-id="17bf1-124">Można następnie kliknij przycisk aby trwale usunąć rekord.</span><span class="sxs-lookup"><span data-stu-id="17bf1-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="17bf1-125">Dodawanie Usuń łącze do listy filmów</span><span class="sxs-lookup"><span data-stu-id="17bf1-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="17bf1-126">Użytkownik rozpoczyna się przez dodanie **Usuń** połączyć `WebGrid` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="17bf1-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="17bf1-127">Ten link jest podobny do **Edytuj** łącze, które zostały dodane w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="17bf1-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="17bf1-128">Otwórz *Movies.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="17bf1-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="17bf1-129">Zmiana `WebGrid` znaczników w treści strony przez dodanie kolumny.</span><span class="sxs-lookup"><span data-stu-id="17bf1-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="17bf1-130">Oto znaczniki zmodyfikowane:</span><span class="sxs-lookup"><span data-stu-id="17bf1-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="17bf1-131">Nowa kolumna jest to:</span><span class="sxs-lookup"><span data-stu-id="17bf1-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="17bf1-132">Sposób siatki jest skonfigurowany, **Edytuj** kolumna jest skrajnie po lewej stronie w siatce i **Usuń** kolumna znajduje się po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="17bf1-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="17bf1-133">(Brak przecinka po `Year` teraz kolumny w przypadku, gdy nie można zauważyć, że.) Nic specjalnego nie o dokąd te kolumny łącza, a następnie równie łatwo można je umieścić obok siebie.</span><span class="sxs-lookup"><span data-stu-id="17bf1-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="17bf1-134">W tym przypadku są one utrudnić pomieszała się oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="17bf1-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Strona filmy wraz z łączami do edycji i szczegóły oznaczone, aby pokazać, że są one nie obok siebie](deleting-data/_static/image3.png)

<span data-ttu-id="17bf1-136">Nowa kolumna pokazuje łącze (`<a>` elementu) którego tekst jest wyświetlany komunikat "Delete".</span><span class="sxs-lookup"><span data-stu-id="17bf1-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="17bf1-137">Miejsce docelowe łącza (jego `href` atrybutu) jest kod, który ostatecznie jest rozpoznawany jako podobny do tego adresu URL za pomocą `id` wartość dla każdego filmu różnią się od:</span><span class="sxs-lookup"><span data-stu-id="17bf1-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="17bf1-138">Ten link spowoduje wywołanie stronę o nazwie *DeleteMovie* i przekaż go identyfikator filmu wybrano.</span><span class="sxs-lookup"><span data-stu-id="17bf1-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="17bf1-139">W tym samouczku nie przejść do szczegółów dotyczących sposobu ten link jest tworzony, ponieważ jest niemal identyczny **Edytuj** link z poprzedniego samouczka ([aktualizowanie bazy danych w programie ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="17bf1-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="17bf1-140">Tworzenie strony usuwania</span><span class="sxs-lookup"><span data-stu-id="17bf1-140">Creating the Delete Page</span></span>

<span data-ttu-id="17bf1-141">Teraz możesz utworzyć stronę która będzie obiektem docelowym dla **Usuń** łącze w siatce.</span><span class="sxs-lookup"><span data-stu-id="17bf1-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="17bf1-142">**Ważne** technika polega na wybraniu rekordu do usunięcia, a następnie użyć osobnej stronie, a przycisk, aby upewnić się, proces jest bardzo ważne dla bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="17bf1-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="17bf1-143">Jako przeczytane w poprzednich samouczkach, dzięki czemu *wszelkie* typu zmian do witryny sieci Web powinien *zawsze* odbywać się za pomocą formularza &mdash; oznacza to, za pomocą operację POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="17bf1-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="17bf1-144">Jeśli wprowadzono możliwość zmiany lokacji po prostu, klikając łącze, (tj. z użyciem operacji GET), osób może wysyłać żądania prostego do swojej witryny i usunąć swoje dane.</span><span class="sxs-lookup"><span data-stu-id="17bf1-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="17bf1-145">Nawet przeszukiwarką aparat wyszukiwania, która jest indeksowania witryny przypadkowo można usunąć danych wystarczy poniższe linki.</span><span class="sxs-lookup"><span data-stu-id="17bf1-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="17bf1-146">Gdy aplikacja umożliwia użytkownikom modyfikowania rekordu, masz użytkownik widzi rekordu do edytowania mimo to.</span><span class="sxs-lookup"><span data-stu-id="17bf1-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="17bf1-147">Jednak może być kuszące, aby pominąć ten krok w przypadku usuwania rekordu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="17bf1-148">Nie jednak pominąć ten krok.</span><span class="sxs-lookup"><span data-stu-id="17bf1-148">Don't skip that step, though.</span></span> <span data-ttu-id="17bf1-149">(To również przydatne w przypadku użytkowników zobaczyć rekord i upewnij się, że są usuwane rekord, który one przeznaczone.)</span><span class="sxs-lookup"><span data-stu-id="17bf1-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="17bf1-150">W kolejnych zestawie samouczków pokazano, jak dodać funkcję logowania, dzięki czemu użytkownik będzie musiał zalogować się przed usunięciem rekordu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="17bf1-151">Utwórz stronę o nazwie *DeleteMovie.cshtml* i Zamień, co znajduje się w pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="17bf1-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="17bf1-152">Ten kod znaczników jest podobna *EditMovie* stron, chyba że zamiast używania pól tekstowych (`<input type="text">`), zawiera znaczników `<span>` elementów.</span><span class="sxs-lookup"><span data-stu-id="17bf1-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="17bf1-153">Nie ma nic w tym miejscu można edytować.</span><span class="sxs-lookup"><span data-stu-id="17bf1-153">There's nothing here to edit.</span></span> <span data-ttu-id="17bf1-154">To wszystko, co należy zrobić, są wyświetlane szczegóły filmu, aby użytkownicy upewnić się, czy są usuwane prawo filmu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="17bf1-155">Znaczniki już zawiera łącze, które umożliwia użytkownikowi, wróć do strony listy filmów.</span><span class="sxs-lookup"><span data-stu-id="17bf1-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="17bf1-156">Podobnie jak w *EditMovie* strony, identyfikator wybrany film jest przechowywany w ukrytym polu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="17bf1-157">(Jest przekazywany do strony w pierwszej kolejności wartość ciągu zapytania.) Brak `Html.ValidationSummary` wywołania, które będą wyświetlane błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="17bf1-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="17bf1-158">W tym przypadku błąd może być, czy identyfikator filmu nie został przekazany do strony lub że identyfikator film jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="17bf1-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="17bf1-159">Taka sytuacja może wystąpić, jeśli ktoś uruchomiono tę stronę bez zaznaczania filmu w *filmy* strony.</span><span class="sxs-lookup"><span data-stu-id="17bf1-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="17bf1-160">Napis na przycisku jest **Usuwanie filmu**, a jej nazwa atrybutu jest ustawiona na `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="17bf1-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="17bf1-161">`name` Atrybut będzie używana w kodzie, wskaż przycisk, który formularz został przesłany.</span><span class="sxs-lookup"><span data-stu-id="17bf1-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="17bf1-162">Musisz napisać kod, 1) odczytać szczegóły filmu po wyświetleniu strony i (2) faktycznie usunąć film, gdy użytkownik kliknie przycisk.</span><span class="sxs-lookup"><span data-stu-id="17bf1-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="17bf1-163">Dodawanie kodu do odczytu pojedynczego filmu</span><span class="sxs-lookup"><span data-stu-id="17bf1-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="17bf1-164">W górnej części *DeleteMovie.cshtml* strony, należy dodać następujący blok kodu:</span><span class="sxs-lookup"><span data-stu-id="17bf1-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="17bf1-165">Ten kod znaczników jest taka sama jak odpowiedni kod w *EditMovie* strony.</span><span class="sxs-lookup"><span data-stu-id="17bf1-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="17bf1-166">Ona pobiera identyfikator film z ciągu zapytania i używa Identyfikatora odczytać rekordu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="17bf1-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="17bf1-167">Kod zawiera test weryfikacji (`IsInt()` i `row != null`) aby upewnić się, że identyfikator filmu przekazywany do strony jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="17bf1-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="17bf1-168">Należy pamiętać o tym, ten kod uruchamiać tylko przy pierwszym uruchomieniu strony.</span><span class="sxs-lookup"><span data-stu-id="17bf1-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="17bf1-169">Nie chcesz ponownie odczytywana nagrywanie filmu z bazy danych, gdy użytkownik kliknie **Usuwanie filmu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="17bf1-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="17bf1-170">W związku z tym, kod można odczytać filmu znajduje się wewnątrz test, który jest wyświetlany komunikat `if(!IsPost)` &mdash; czyli *Jeśli żądanie nie jest na operację post (przesyłania formularza)*.</span><span class="sxs-lookup"><span data-stu-id="17bf1-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="17bf1-171">Dodawanie kodu, aby usunąć wybrany film</span><span class="sxs-lookup"><span data-stu-id="17bf1-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="17bf1-172">Aby usunąć film, gdy użytkownik kliknie przycisk, Dodaj następujący kod wewnątrz zamykającym nawiasem klamrowym `@` bloku:</span><span class="sxs-lookup"><span data-stu-id="17bf1-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="17bf1-173">Ten kod jest podobny do kodu dotyczące aktualizacji istniejącego rekordu, ale jest prostsze.</span><span class="sxs-lookup"><span data-stu-id="17bf1-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="17bf1-174">Kod działa na zasadzie SQL `Delete` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="17bf1-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="17bf1-175">Podobnie jak w *EditMovie* stronie kod znajduje się w `if(IsPost)` bloku.</span><span class="sxs-lookup"><span data-stu-id="17bf1-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="17bf1-176">Tym razem `if()` warunek jest nieco bardziej skomplikowane:</span><span class="sxs-lookup"><span data-stu-id="17bf1-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="17bf1-177">Istnieją tutaj dwa warunki.</span><span class="sxs-lookup"><span data-stu-id="17bf1-177">There are two conditions here.</span></span> <span data-ttu-id="17bf1-178">Pierwsza to, że strona jest przekazywana, jak już wspomniano wcześniej &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="17bf1-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="17bf1-179">Drugi warunek ma `!Request["buttonDelete"].IsEmpty()`, co oznacza, że żądanie ma obiekt o nazwie `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="17bf1-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="17bf1-180">Niewątpliwie jest to sposób pośredni testów, który przycisk formularz został przesłany.</span><span class="sxs-lookup"><span data-stu-id="17bf1-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="17bf1-181">Jeśli formularz zawiera wiele przycisków przesyłania, tylko nazwę przycisku, który został kliknięty pojawi się w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="17bf1-182">W związku z tym, logicznie Jeśli nazwa określonego przycisku pojawia się w żądaniu &mdash; lub opisany w kodzie, jeśli ten przycisk nie jest pusty &mdash; to przycisk, który formularz został przesłany.</span><span class="sxs-lookup"><span data-stu-id="17bf1-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="17bf1-183">`&&` Oznacza, że operator "i" (operator logiczny oraz).</span><span class="sxs-lookup"><span data-stu-id="17bf1-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="17bf1-184">W związku z tym całą `if` warunek jest...</span><span class="sxs-lookup"><span data-stu-id="17bf1-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="17bf1-185">*To żądanie jest żądaniem post (nie żądanie po raz pierwszy)*</span><span class="sxs-lookup"><span data-stu-id="17bf1-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="17bf1-186">AND</span><span class="sxs-lookup"><span data-stu-id="17bf1-186">AND</span></span>  
  
<span data-ttu-id="17bf1-187">*`buttonDelete`Przycisk został przycisku,* *który formularz został przesłany.*</span><span class="sxs-lookup"><span data-stu-id="17bf1-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="17bf1-188">Ten formularz (w rzeczywistości ta strona) zawiera tylko jeden przycisk, więc dodatkowy test na `buttonDelete` technicznie nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="17bf1-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="17bf1-189">Nadal którą zamierzasz wykonać operacji, która spowoduje to trwałe usunięcie danych.</span><span class="sxs-lookup"><span data-stu-id="17bf1-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="17bf1-190">Dlatego należy się, jak to możliwe, że wykonujesz operację tylko wtedy, gdy użytkownik jawnie zgłosił żądanie.</span><span class="sxs-lookup"><span data-stu-id="17bf1-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="17bf1-191">Załóżmy na przykład, możesz później rozszerzyć tę stronę i do niej dodać inny przycisk.</span><span class="sxs-lookup"><span data-stu-id="17bf1-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="17bf1-192">Nawet, kod, który usuwa film zostanie uruchomione tylko wtedy, gdy `buttonDelete` kliknięcia przycisku.</span><span class="sxs-lookup"><span data-stu-id="17bf1-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="17bf1-193">Podobnie jak w *EditMovie* strony, możesz uzyskać identyfikator ukryte pola, a następnie uruchom polecenie SQL.</span><span class="sxs-lookup"><span data-stu-id="17bf1-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="17bf1-194">Składnia `Delete` instrukcja jest:</span><span class="sxs-lookup"><span data-stu-id="17bf1-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="17bf1-195">Należy koniecznie obejmują `WHERE` klauzuli i identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="17bf1-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="17bf1-196">Jeśli pozostawisz klauzuli WHERE *zostaną usunięte wszystkie rekordy w tabeli*.</span><span class="sxs-lookup"><span data-stu-id="17bf1-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="17bf1-197">Jak wiesz już, przekażesz wartości Identyfikatora polecenia SQL przy użyciu symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="17bf1-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="17bf1-198">Testowanie proces usuwania filmu</span><span class="sxs-lookup"><span data-stu-id="17bf1-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="17bf1-199">Teraz możesz przetestować.</span><span class="sxs-lookup"><span data-stu-id="17bf1-199">Now you can test.</span></span> <span data-ttu-id="17bf1-200">Uruchom *filmy* strony, a następnie kliknij przycisk **Usuń** obok filmu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="17bf1-201">Gdy *DeleteMovie* zostanie wyświetlona strona, kliknij przycisk **Usuwanie filmu**.</span><span class="sxs-lookup"><span data-stu-id="17bf1-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Usuń stronę film z wyróżnionym przyciskiem Usuń filmu](deleting-data/_static/image4.png)

<span data-ttu-id="17bf1-203">Po kliknięciu przycisku, kod usuwa filmy i zwraca listę filmu.</span><span class="sxs-lookup"><span data-stu-id="17bf1-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="17bf1-204">Można wyszukać usunięte film i upewnij się, że jest ona zostać usunięta.</span><span class="sxs-lookup"><span data-stu-id="17bf1-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="17bf1-205">Pojawi się dalej</span><span class="sxs-lookup"><span data-stu-id="17bf1-205">Coming Up Next</span></span>

<span data-ttu-id="17bf1-206">Następny samouczek pokazuje, jak zapewnić wszystkich stron w witrynie, wspólnej układ i wygląd.</span><span class="sxs-lookup"><span data-stu-id="17bf1-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="17bf1-207">Kompletna lista strony filmu (aktualizowane wraz z łączami Delete)</span><span class="sxs-lookup"><span data-stu-id="17bf1-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="17bf1-208">Kompletna lista DeleteMovie strony</span><span class="sxs-lookup"><span data-stu-id="17bf1-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="17bf1-209">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="17bf1-209">Additional Resources</span></span>

- [<span data-ttu-id="17bf1-210">Wprowadzenie do programowania dla sieci Web platformy ASP.NET przy użyciu składni Razor</span><span class="sxs-lookup"><span data-stu-id="17bf1-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="17bf1-211">[Usuń instrukcję SQL w](http://www.w3schools.com/sql/sql_delete.asp) witrynie W3Schools</span><span class="sxs-lookup"><span data-stu-id="17bf1-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="17bf1-212">[Poprzednie](updating-data.md)
> [dalej](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="17bf1-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
