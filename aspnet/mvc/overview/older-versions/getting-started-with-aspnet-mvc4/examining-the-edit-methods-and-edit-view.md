---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Badanie widoku edycji i Edytuj metody | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 315914056c0a666fdf23cf82a314a999e03114b6
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Badanie metody edycji i widoku edycji
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji należy zbadać metod akcji wygenerowanych i widoków dla kontrolera filmu. Następnie dodasz niestandardową stronę wyszukiwania.

Uruchom aplikację i przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL na pasku adresu przeglądarki. Umieść kursor **Edytuj** łącze, aby wyświetlić adres URL, który jest połączona.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Edytuj** łącza został wygenerowany przez `Html.ActionLink` metody w *Views\Movies\Index.cshtml* widoku:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Obiekt jest pomocnika uwidocznionego za pomocą właściwości na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) klasy podstawowej. `ActionLink` Metody pomocnika ułatwia dynamicznego generowania hiperłącza HTML, które łącze do metody akcji na kontrolerach. Pierwszy argument `ActionLink` metoda jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument jest nazwą metody akcji do wywołania. Ostatni argument jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący danych trasy (w tym przypadku identyfikator 4).

Wygenerowane łącze pokazano na poprzedniej ilustracji jest `http://localhost:xxxxx/Movies/Edit/4`. Trasa domyślna (w *aplikacji\_Start\RouteConfig.cs*) trwa wzorzec URL `{controller}/{action}/{id}`. W związku z tym tłumaczy ASP.NET `http://localhost:xxxxx/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontrolera z parametrem `ID` równa 4. Przejrzyj następujący kod z *aplikacji\_Start\RouteConfig.cs* pliku.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Można również przekazać parametry metody akcji przy użyciu ciągu zapytania. Na przykład adres URL `http://localhost:xxxxx/Movies/Edit?ID=4` przekazuje także parametr `ID` 4 do `Edit` metody akcji `Movies` kontrolera.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otwórz `Movies` kontrolera. Dwa `Edit` poniżej przedstawiono metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `HttpPost` atrybutu. Ten atrybut określa, że przeciążenia `Edit` metoda może być wywoływana tylko dla żądań POST. Można zastosować `HttpGet` pierwszy dla atrybutu Edytuj metodę, ale który nie jest konieczne, ponieważ jest to wartość domyślna. (Firma Microsoft będzie odwoływać się do metod akcji, które są przypisane niejawnie `HttpGet` atrybutu jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda przyjmuje parametr ID film, wyszukuje filmu przy użyciu programu Entity Framework `Find` metody i zwraca wybrany film do widoku edycji. Określa parametr ID [wartość domyślna](https://msdn.microsoft.com/library/dd264739.aspx) zero, jeśli `Edit` metoda jest wywoływana bez parametrów. Jeśli nie można odnaleźć film, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) jest zwracany. Podczas tworzenia widoku edycji systemu szkieletów zbadane `Movie` klasy i kodu do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. W poniższym przykładzie pokazano widok edycji został wygenerowany:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku — to ustawienie określa, czy widok oczekuje modelu widoku szablonu typu `Movie`.

Kod z utworzonym szkieletem używa kilku *metody pomocnicze* uprościć kod znaczników HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocnika Wyświetla nazwę pola (&quot;tytuł&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, lub &quot;cen &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Pomocnika renderowania kodu HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocnik wyświetli żadnych komunikatów dotyczących sprawdzania poprawności skojarzone z tą właściwością.

Uruchom aplikację i przejdź do */Movies* adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. Poniżej przedstawiono kod HTML dla elementu form.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Elementy są w formacie HTML `<form>` element których `action` ustawiono atrybut do wysłania do */filmów/Edytuj* adresu URL. Dane formularza zostaną opublikowane na serwerze po **Edytuj** kliknięciu przycisku.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniżej przedstawiono listę `HttpPost` wersji `Edit` metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[Integratora modelu platformy ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) przyjmuje wartości przesłanego formularza i tworzy `Movie` obiekt, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Metoda sprawdza, czy dane dostarczone w formie może służyć do modyfikowania (edycji lub aktualizacji) `Movie` obiektu. Jeśli dane są prawidłowe, są zapisywane dane filmu `Movies` Kolekcja `db(MovieDBContext` wystąpienie). Nowe dane film jest zapisywana w bazie danych przez wywołanie metody `SaveChanges` metody `MovieDBContext`. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, w którym są wyświetlane z kolekcji, filmów, w tym tylko zmiany.

Jeśli przesłanych wartości nie są prawidłowe, są wyświetlane ponownie w formularzu. `Html.ValidationMessageFor` Pomocników w *Edit.cshtml* widok szablonu zajmie się wyświetlanie odpowiednie komunikaty o błędach.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> w celu obsługi weryfikacji jQuery dla ustawień regionalnych innych niż angielskie, które użyj przecinka (&quot;,&quot;) dla dziesiętnego, należy wprowadzić *globalize.js* i konkretnej *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. Poniższy kod przedstawia zmiany w pliku Views\Movies\Edit.cshtml do pracy z &quot;fr-FR&quot; kultury:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Pole dziesiętnych mogą wymagać przecinkami, a nie punktu dziesiętnego. Jako tymczasowy poprawkę można dodać elementu globalizacji do głównego pliku web.config projektów. Poniższy kod przedstawia element globalizacji z kulturą ustawioną językiem angielskim.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Wszystkie `HttpGet` metody wykonaj podobnego wzorca. Otrzymują obiektu movie (lub listę obiektów, w przypadku `Index`) i przekazać model widoku. `Create` — Metoda przekazuje filmu pusty obiekt do tworzenia widoku. Wszystkie metody, które tworzenia, edytowania, usuwania lub modyfikację danych należy więc w `HttpPost` przeciążenia metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpis w blogu post [ASP.NET MVC Porada 46 — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w metodzie GET również narusza HTTP najlepszych rozwiązań i architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) wzorca, który określa, że żądania GET nie należy zmieniać stanu aplikacji. Innymi słowy wykonanie operacji GET powinny być bezpieczne operację, która nie ma skutków ubocznych i nie modyfikuje danych trwałych.

## <a name="adding-a-search-method-and-search-view"></a>Dodawanie metody wyszukiwania i widoku wyszukiwania

W tej sekcji dodasz `SearchIndex` metody akcji, która umożliwia wyszukiwanie filmów genre lub nazwę. Będzie to dostępne przy użyciu */filmów/SearchIndex* adresu URL. Żądanie spowoduje wyświetlenie formularza HTML, który zawiera elementy wejściowe, które użytkownik może wprowadzić w celu wyszukania filmu. Gdy użytkownik przesyła formularz, metoda akcji wartości wyszukiwania zapisane przez użytkownika, a Użyj wartości do wyszukania bazy danych.

## <a name="displaying-the-searchindex-form"></a>Wyświetlanie w formularzu SearchIndex

Rozpocznij od dodania `SearchIndex` metody akcji do istniejącej `MoviesController` klasy. Metoda zwraca widok zawierający formularza HTML. Oto kod:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

W pierwszym wierszu `SearchIndex` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmy:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Zapytanie jest zdefiniowany w tym momencie, ale nie został jeszcze uruchomiony przed magazynu danych.

Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania wartość ciągu wyszukiwania, używając następującego kodu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

`s => s.Title` Kod powyżej [wyrażenia Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodę używaną w powyższym kodzie. Zapytania LINQ nie są wykonywane, gdy są one zdefiniowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`. Zamiast tego wykonywania zapytania jest opóźnione, co oznacza, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest rzeczywiście iterowane za pośrednictwem lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana. W `SearchIndex` przykładowe zapytanie jest wykonywane w widoku SearchIndex. Aby uzyskać więcej informacji na temat wykonywania zapytań odroczonych, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).

Teraz można wdrożyć `SearchIndex` widok, który będzie wyświetlany formularz dla użytkownika. Kliknij prawym przyciskiem myszy wewnątrz `SearchIndex` metody, a następnie kliknij przycisk **Dodaj widok**. W **Dodaj widok** oknie dialogowym Określ, że zamierzasz przekazać `Movie` obiekt w szablonie widoku, jak jego klasa modelu. W **szablonu szkieletu** wybierz **listy**, następnie kliknij przycisk **Dodaj**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Po kliknięciu **Dodaj** przycisku *Views\Movies\SearchIndex.cshtml* jest tworzony widok szablonu. Ponieważ wybrano **listy** w **szablonu szkieletu** listy programu Visual Studio, które są generowane automatycznie (szkieletu) niektóre domyślne znaczników w widoku. Rusztowania utworzyć formularza HTML. Ją zbadać `Movie` klasy i kodu do renderowania `<label>` elementy dla każdej właściwości klasy. Lista poniżej zawiera widok Utwórz, który został wygenerowany:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Uruchom aplikację i przejdź do */filmów/SearchIndex*. Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL. Filtrowane filmów są wyświetlane.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Jeśli zmienisz podpis `SearchIndex` metoda ma parametr o nazwie `id`, `id` parametr będzie odpowiadać `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Global.asax* pliku.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Oryginalna `SearchIndex` metody wygląda następująco:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Zmodyfikowane `SearchIndex` metoda będzie wyglądać następująco:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Tytuł wyszukiwania mogą być obecnie przekazywane jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą, aby wyszukać filmu. Tak, teraz możesz dodasz interfejsu użytkownika w celu ich filtrowania filmów. Zmiana podpisu `SearchIndex` metody do testowania sposób przekazywania parametru Identyfikatora trasy wiązaniem, zmień ją tak, że Twoje `SearchIndex` metoda przyjmuje parametr ciągu o nazwie `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Otwórz *Views\Movies\SearchIndex.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj następujący kod:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

W poniższym przykładzie przedstawiono część *Views\Movies\SearchIndex.cshtml* pliku z dodano znaczników filtrowania.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocnika tworzy otwierania `<form>` tagu. `Html.BeginForm` Pomocnika powoduje, że formularz publikowania do samej siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.

Uruchom aplikację i spróbuj wykonać wyszukiwanie filmu.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Brak nie `HttpPost` przeciążenia z `SearchIndex` metody. Nie jest konieczne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `HttpPost SearchIndex` metody. W takim przypadku element wywołujący akcji spowoduje dopasowanie `HttpPost SearchIndex` metody i `HttpPost SearchIndex` metoda może działać, jak pokazano na poniższej ilustracji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Jednak nawet jeśli dodasz to `HttpPost` wersji `SearchIndex` metody, jest to ograniczenie, w jaki sposób to wszystko zostało zaimplementowane. Załóżmy, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać łącze do znajomych, mogą kliknąć w celu wyświetlenia tego samego filtrowane listy filmów. Zwróć uwagę, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmów/SearchIndex) — nie ma żadnych informacji wyszukiwania w adresie URL, do samej siebie. Prawo teraz, wyszukaj ciąg informacje są wysyłane do serwera jako wartość pola formularza. Oznacza to, że nie można przechwycić informacje wyszukiwania zakładki lub wysyłać do znajomych w adresie URL.

Rozwiązaniem jest użycie przeciążenia `BeginForm` , który określa żądanie POST należy dodać informacje dotyczące wyszukiwania na adres URL i że powinny być kierowane do wersji HttpGet `SearchIndex` metody. Zamień istniejące bez parametrów `BeginForm` metody z następujących czynności:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet SearchIndex` metody akcji, nawet jeśli masz `HttpPost SearchIndex` metody.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według rodzaju

Jeśli dodano `HttpPost` wersji `SearchIndex` metody, usuń go teraz.

Następnie dodasz funkcji, aby umożliwić użytkownikom wyszukiwanie filmów według rodzaju. Zastąp `SearchIndex` metodę z następującym kodem:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Ta wersja `SearchIndex` metoda przyjmuje dodatkowych parametrów, a mianowicie `movieGenre`. Tworzenie pierwszego kilku wierszy kodu `List` obiektu do przechowywania genres film z bazy danych.

Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

W kodzie użyto `AddRange` metody ogólnej `List` kolekcji można dodać do listy wszystkich unikatowych genres. (Bez `Distinct` modyfikator, zostanie dodany genres zduplikowane — na przykład zostanie dodany Komedia dwa razy w naszym przykładzie). Kod następnie przechowuje listę gatunkami muzyki w `ViewBag` obiektu.

Poniższy kod przedstawia sposób sprawdzania `movieGenre` parametru. Jeśli nie jest pusta, kodu ogranicza kwerendę filmy i ograniczyć wybranego filmów do określonego rodzaju.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Dodawanie znaczników do widoku SearchIndex obsługuje wyszukiwania według rodzaju

Dodaj `Html.DropDownList` Pomocnika *Views\Movies\SearchIndex.cshtml* pliku tuż przed `TextBox` pomocnika. Poniżej przedstawiono wypełniony kod znaczników:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Uruchom aplikację i przejdź do */filmów/SearchIndex*. Spróbować przeprowadzić wyszukiwanie według rodzaju, nazwa filmu i oba kryteria.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

W tej sekcji zbadać metod akcji CRUD i widoki generowane przez platformę. Utworzono metody akcji wyszukiwania i widok, który zezwala użytkownikom na wyszukiwanie według tytuł filmu i rodzaju. W następnej sekcji, możesz wyjaśniono, jak dodać właściwości do `Movie` modelu oraz sposobu dodawania inicjatora automatycznie utworzy testowej bazy danych.

>[!div class="step-by-step"]
[Poprzednie](accessing-your-models-data-from-a-controller.md)
[dalej](adding-a-new-field-to-the-movie-model-and-table.md)
