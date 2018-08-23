---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Badanie metod edycji i widoku edycji | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: ea8156fc2ea334be37e7c3f02ac15d168cfc6843
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756239"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Badanie metod edycji i widoku edycji
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji omówione metod akcji wygenerowanych i wyświetlenia dla kontrolera filmu. Następnie należy dodać niestandardową stronę wyszukiwania.

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL w pasku adresu przeglądarki. Umieść wskaźnik myszy nad **Edytuj** link, aby wyświetlić adres URL, który łączy on.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Edytuj** link został wygenerowany przez `Html.ActionLink` method in Class metoda *Views\Movies\Index.cshtml* widoku:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Obiekt jest pomocnika, która jest widoczna, za pomocą właściwości na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) klasy bazowej. `ActionLink` Metody pomocnika ułatwia można dynamicznie wygenerować hiperłącza HTML, które łącze do metody akcji, na kontrolerach. Pierwszy argument `ActionLink` metodą jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument jest nazwa metody akcji do wywołania. Ostatni argument jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący dane trasy (w tym przypadku identyfikator 4).

Wygenerowane łącze na poprzednim obrazie jest `http://localhost:xxxxx/Movies/Edit/4`. Trasa domyślna (w *aplikacji\_Start\RouteConfig.cs*) zajmuje wzorzec adresu URL `{controller}/{action}/{id}`. W związku z tym, tłumaczy ASP.NET `http://localhost:xxxxx/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontroler z parametrem `ID` równa 4. Sprawdź następujący kod z *aplikacji\_Start\RouteConfig.cs* pliku.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Można również przekazać parametry metody akcji przy użyciu ciągu zapytania. Na przykład adres URL `http://localhost:xxxxx/Movies/Edit?ID=4` przekazuje parametr `ID` 4 do `Edit` metody akcji `Movies` kontrolera.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otwórz `Movies` kontrolera. Dwa `Edit` poniżej przedstawiono metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `HttpPost` atrybutu. Ten atrybut określa, że przeciążenia `Edit` metoda może być wywoływana tylko w przypadku żądania POST. Można zastosować `HttpGet` atrybutu do pierwszego Edytuj metodę, ale który nie jest konieczne, ponieważ jest ona domyślnie. (Będziemy się odwoływać do metod akcji, które są jawnie przypisane `HttpGet` atrybutu jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda przyjmuje parametr Identyfikatora filmu, wyszukuje filmu używający narzędzia Entity Framework `Find` metodę i zwraca wybrany film do widoku edycji. Parametr Identyfikatora Określa [wartość domyślna](https://msdn.microsoft.com/library/dd264739.aspx) zero, jeśli `Edit` metoda jest wywoływana bez parametrów. Jeśli nie można odnaleźć filmu, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) jest zwracana. Podczas tworzenia widoku edycji system scaffoldingu zbadane `Movie` klasy i utworzony kod do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. Poniższy przykład przedstawia widok edycji, który został wygenerowany:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku — Określa, czy widok oczekuje modelu dla widoku szablonu typu `Movie`.

Utworzony szkielet kodu wykorzystuje kilka *metody pomocnika* uprościć kod znaczników HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocnik wyświetli nazwę pola (&quot;tytuł&quot;, &quot;ReleaseDate&quot;, &quot;gatunku&quot;, lub &quot;ceny &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Pomocnika renderowanie kodu HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocnik wyświetli wszystkie komunikaty weryfikacji skojarzony z tej właściwości.

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. Poniżej przedstawiono kod HTML dla elementu form.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Elementy są w formacie HTML `<form>` elementu którego `action` ma ustawioną wartość atrybutu Opublikuj */filmy/Edytuj* adresu URL. Dane formularza zostaną opublikowane na serwerze po **Edytuj** przycisku.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniższej przedstawiono listę `HttpPost` wersję `Edit` metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[Integratora modelu programu ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) przyjmuje wartości przesłanego formularza i tworzy `Movie` obiektu, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Metoda sprawdza, czy dane dostarczone w formie może służyć do modyfikowania (edycji lub aktualizacja) `Movie` obiektu. Jeśli dane są prawidłowe, dane film zostanie zapisana w `Movies` zbiór `db(MovieDBContext` wystąpienie). Nowe dane film jest zapisywany w bazie danych przez wywołanie metody `SaveChanges` metody `MovieDBContext`. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, który wyświetla film kolekcji, w tym zmiany wprowadzone przed chwilą.

Jeśli przesłanych wartości nie są prawidłowe, są wyświetlane ponownie w formularzu. `Html.ValidationMessageFor` Obiekty pomocnicze w *Edit.cshtml* Wyświetl szablon powinien zachować ostrożność, wyświetlania odpowiedniego komunikatu o błędzie.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> do obsługi dotyczącą weryfikacji jQuery dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (&quot;,&quot;) separator dziesiętny, musi zawierać *globalize.js* i konkretne *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i języka JavaScript, aby użyć `Globalize.parseFloat`. Poniższy kod przedstawia zmiany w pliku Views\Movies\Edit.cshtml do pracy z &quot;fr-FR&quot; kultury:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Pola dziesiętnego może wymagać przecinek, nie przecinka dziesiętnego. Jako tymczasowe awarii można dodać elementu globalizacji do głównego pliku web.config projektów. Poniższy kod przedstawia element globalizacji z kulturą równa Stanów Zjednoczonych w języku angielskim.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Wszystkie `HttpGet` metody wykonaj podobny wzorzec. Staną się obiekt filmu (lub listę obiektów, w przypadku `Index`) i przekazać model widoku. `Create` Metoda przekazuje obiekt pusty film do tworzenia widoku. Wszystkie metody, które tworzenie, edytowanie, usuwanie lub inny sposób modyfikować danych, należy więc w `HttpPost` przeciążenia metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpis w blogu wpis [platformy ASP.NET MVC Porada 46 — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w przypadku metody GET również narusza HTTP najlepszych rozwiązań i architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) wzorca, który określa, czy żądania GET, nie należy zmieniać stan aplikacji. Innymi słowy wykonywanie operacji GET powinna być bezpieczne operacji, która ma żadnych efektów ubocznych i nie modyfikuje utrwalonych danych.

## <a name="adding-a-search-method-and-search-view"></a>Dodawanie metody wyszukiwania i widoku wyszukiwania

W tej sekcji dodasz `SearchIndex` metody akcji, która umożliwia wyszukiwanie filmów według gatunku lub nazwy. Jest to dostępne za pośrednictwem */filmy/SearchIndex* adresu URL. Żądanie spowoduje wyświetlenie formularza HTML, który zawiera elementy danych wejściowych, które użytkownik może wprowadzić, aby wyszukać film. Gdy użytkownik przesyła formularz, metody akcji Pobierz wartości wyszukiwania opublikowanych przez użytkownika i użyj wartości, aby wyszukać bazy danych.

## <a name="displaying-the-searchindex-form"></a>Wyświetlanie formularza SearchIndex

Rozpocznij od dodania `SearchIndex` metody akcji do istniejących `MoviesController` klasy. Metoda zwraca widok, który zawiera formularza HTML. Oto kod:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

W pierwszym wierszu `SearchIndex` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmów:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Zapytanie jest zdefiniowany w tym momencie, ale nie zostało jeszcze uruchomione względem magazynu danych.

Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania, używając następującego kodu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

`s => s.Title` Powyższy kod jest [wyrażenia Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodę użytą w kodzie powyżej. Zapytania LINQ nie są wykonywane, gdy są one definiowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`. Zamiast tego, wykonanie zapytania jest odroczone, co oznacza, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana. W `SearchIndex` przykładowe zapytanie jest wykonywane w widoku SearchIndex. Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).

Teraz można zaimplementować `SearchIndex` widok, w którym będą wyświetlane na formularzu do użytkownika. Kliknij prawym przyciskiem myszy wewnątrz `SearchIndex` metody, a następnie kliknij przycisk **Dodaj widok**. W **Dodaj widok** okna dialogowego należy określić, że zamierzasz przekazać `Movie` obiektu do szablonu widoku, jak jej klasa modelu. W **szablonu szkieletu** wybierz **listy**, następnie kliknij przycisk **Dodaj**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Po kliknięciu **Dodaj** przycisku *Views\Movies\SearchIndex.cshtml* Wyświetl szablon został utworzony. Ponieważ wybrano **listy** w **szablonu szkieletu** listy programu Visual Studio automatycznie generowane (działanie) niektóre znaczników domyślne w widoku. Szkieletu utworzony formularza HTML. Go zbadać `Movie` klasy i utworzony kod do renderowania `<label>` elementów dla każdej właściwości klasy. Lista poniżej przedstawiono tworzenie widoku, który został wygenerowany:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Uruchom aplikację, a następnie przejdź do */filmy/SearchIndex*. Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL. Wyświetlane są filtrowane filmów.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Jeśli zmienisz podpis `SearchIndex` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Global.asax* pliku.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Oryginalny `SearchIndex` metoda wygląda następująco:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Zmodyfikowanego `SearchIndex` metoda wyglądałby następująco:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów. Teraz możesz dodasz interfejs użytkownika, aby pomóc im filtrowanie filmów. Jeśli zmienisz podpis `SearchIndex` metodę, aby przetestować sposób przekazywania parametru ID powiązane z tras, zmień ją tak, że Twoje `SearchIndex` metoda przyjmuje jako parametr ciągu o nazwie `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Otwórz *Views\Movies\SearchIndex.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj następujący kod:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

W poniższym przykładzie pokazano część *Views\Movies\SearchIndex.cshtml* pliku dodano znaczników filtrowania.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocnika tworzy otwierający `<form>` tagu. `Html.BeginForm` Pomocnika powoduje, że formularz do publikowania do samego siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.

Uruchom aplikację, a następnie spróbuj wyszukać film.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Istnieje nie `HttpPost` przeciążenia `SearchIndex` metody. Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `HttpPost SearchIndex` metody. W takiej sytuacji będzie odpowiadać element wywołujący akcji `HttpPost SearchIndex` metody i `HttpPost SearchIndex` metoda może działać, jak pokazano na poniższej ilustracji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Jednak nawet w przypadku dodania to `HttpPost` wersję `SearchIndex` metody, jest to ograniczenie, w jak to wszystko została zaimplementowana. Wyobraź sobie, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać link do znajomych, mogą kliknąć umożliwiający zobaczenie tej samej listy filtrowane filmów. Należy zauważyć, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmy/SearchIndex) — Brak informacji wyszukiwania w adresie URL, sam. Po prawej stronie teraz informacje o parametrach wyszukiwania jest wysyłany do serwera jako wartość pola formularza. Oznacza to, że nie można przechwycić informacje wyszukiwania do zakładki lub wysłać znajomym w adresie URL.

Rozwiązaniem jest używanie przeciążenia `BeginForm` , który określa żądanie POST należy dodać informacje dotyczące wyszukiwania do adresu URL i że należy kierowane do wersji narzędzia HttpGet `SearchIndex` metody. Zastąp istniejące bez parametrów `BeginForm` metoda następującym kodem:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet SearchIndex` metody akcji, nawet jeśli masz `HttpPost SearchIndex` metody.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według gatunku

Jeśli dodano `HttpPost` wersję `SearchIndex` metody, usuń ją teraz.

Następnie dodasz funkcję, aby umożliwić użytkownikom wyszukiwanie filmów według gatunku. Zastąp `SearchIndex` metoda następującym kodem:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Ta wersja `SearchIndex` metoda przyjmuje dodatkowy parametr, to znaczy `movieGenre`. Tworzenie pierwszych kilka wierszy kodu `List` obiekt do przechowywania gatunki film z bazy danych.

Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Kod używa `AddRange` metody ogólnej `List` kolekcję, aby dodać różne gatunki do listy. (Bez `Distinct` modyfikator, zostaną dodane zduplikowane gatunki — na przykład, zostaną dodane Komedia dwukrotnie w naszym przykładzie). Kod następnie przechowuje listę gatunki w `ViewBag` obiektu.

Poniższy kod przedstawia sposób sprawdzić `movieGenre` parametru. Jeśli nie jest pusty, kod dodatkowo ogranicza zapytanie filmy, aby ograniczyć wybranych filmów na określonego rodzaju.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Dodawanie znaczników do widoku SearchIndex umożliwiających wyszukiwanie według gatunku

Dodaj `Html.DropDownList` element pomocniczy służący do *Views\Movies\SearchIndex.cshtml* pliku tuż przed `TextBox` pomocnika. Ukończone znaczników jest pokazany poniżej:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Uruchom aplikację, a następnie przejdź do */filmy/SearchIndex*. Spróbuj wyszukiwania według gatunku, tytuł filmu i oba kryteria.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

W tej sekcji służy do badania metod akcji CRUD i widoki generowane przez platformę. Utworzono metody akcji wyszukiwania i widoku, które umożliwiają użytkownikom wyszukiwanie tytuł filmu i gatunku. W następnej sekcji zostanie przyjrzymy się sposób dodawania właściwości do `Movie` modelu oraz sposób dodawania inicjatora, które automatycznie utworzy test bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](accessing-your-models-data-from-a-controller.md)
> [dalej](adding-a-new-field-to-the-movie-model-and-table.md)
