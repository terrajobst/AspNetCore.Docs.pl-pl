---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Badanie metod edycji i widoku edycji (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 13acaa13d5e696928fbc9834eb355c630ced7162
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752482"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Badanie metod edycji i widoku edycji (VB)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/examining-the-edit-methods-and-edit-view.md) po ukończeniu tego samouczka.


W tej sekcji omówione metod akcji wygenerowanych i wyświetlenia dla kontrolera filmu. Następnie należy dodać niestandardową stronę wyszukiwania.

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL w pasku adresu przeglądarki. Umieść wskaźnik myszy nad **Edytuj** link, aby wyświetlić adres URL, który łączy on.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Edytuj** link został wygenerowany przez `Html.ActionLink` method in Class metoda *Views\Movies\Index.vbhtml* widoku:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Obiekt jest pomocnika, która jest widoczna, za pomocą właściwości na `WebViewPage` klasy bazowej. `ActionLink` Metody pomocnika ułatwia można dynamicznie wygenerować hiperłącza HTML, które łącze do metody akcji, na kontrolerach. Pierwszy argument `ActionLink` metodą jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument jest nazwa metody akcji do wywołania. Ostatni argument jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący dane trasy (w tym przypadku identyfikator 4).

Wygenerowane łącze na poprzednim obrazie jest `http://localhost:xxxxx/Movies/Edit/4`. Trasa domyślna przyjmuje wzorzec adresu URL `{controller}/{action}/{id}`. W związku z tym, tłumaczy ASP.NET `http://localhost:xxxxx/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontroler z parametrem `ID` równa 4.

Można również przekazać parametry metody akcji przy użyciu ciągu zapytania. Na przykład adres URL `http://localhost:xxxxx/Movies/Edit?ID=4` przekazuje parametr `ID` 4 do `Edit` metody akcji `Movies` kontrolera.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Otwórz `Movies` kontrolera. Dwa `Edit` poniżej przedstawiono metody akcji.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `HttpPost` atrybutu. Ten atrybut określa, że przeciążenia `Edit` metoda może być wywoływana tylko w przypadku żądania POST. Można zastosować `HttpGet` atrybutu do pierwszego Edytuj metodę, ale który nie jest konieczne, ponieważ jest ona domyślnie. (Będziemy się odwoływać do metod akcji, które są jawnie przypisane `HttpGet` atrybutu jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda przyjmuje parametr Identyfikatora filmu, wyszukuje filmu używający narzędzia Entity Framework `Find` metodę i zwraca wybrany film do widoku edycji. Podczas tworzenia widoku edycji system scaffoldingu zbadane `Movie` klasy i utworzony kod do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. Poniższy przykład przedstawia widok edycji, który został wygenerowany:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Zwróć uwagę, jak szablon widoku ma `@ModelType MvcMovie.Models.Movie` instrukcji w górnej części pliku — Określa, czy widok oczekuje modelu dla widoku szablonu typu `Movie`.

Utworzony szkielet kodu wykorzystuje kilka *metody pomocnika* uprościć kod znaczników HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocnik wyświetli nazwę pola (&quot;tytuł&quot;, &quot;ReleaseDate&quot;, &quot;gatunku&quot;, lub &quot;ceny &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Pomocnik wyświetli HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocnik wyświetli wszystkie komunikaty weryfikacji skojarzony z tej właściwości.

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. HTML na stronie wygląda podobnie jak w poniższym przykładzie. (Znaczników menu został wykluczony, w celu uściślenia).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

`<input>` Elementy są w formacie HTML `<form>` elementu którego `action` ma ustawioną wartość atrybutu Opublikuj */filmy/Edytuj* adresu URL. Dane formularza zostaną opublikowane na serwerze po **Edytuj** przycisku.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniższej przedstawiono listę `HttpPost` wersję `Edit` metody akcji.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Integrator modelu ASP.NET framework przyjmuje wartości przesłanego formularza i tworzy `Movie` obiektu, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Zaewidencjonuj kod sprawdza, czy dane dostarczone w formie może służyć do modyfikowania `Movie` obiektu. Jeśli dane są prawidłowe, kod zapisuje dane filmu `Movies` zbiór `MovieDBContext` wystąpienia. Kod następnie zapisuje nowe dane filmów w bazie danych przez wywołanie metody `SaveChanges` metody `MovieDBContext`, której zmiany w bazie danych będzie nadal występować. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, która powoduje, że zaktualizowano film będzie wyświetlana na liście filmów.

Jeśli przesłanych wartości nie są prawidłowe, są wyświetlane ponownie w formularzu. `Html.ValidationMessageFor` Obiekty pomocnicze w *Edit.vbhtml* Wyświetl szablon powinien zachować ostrożność, wyświetlania odpowiedniego komunikatu o błędzie.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Uwaga dotycząca ustawień regionalnych** zwykle pracy z ustawień regionalnych innych niż angielski, zobacz [obsługi platformy ASP.NET MVC 3 Weryfikacja przy użyciu ustawienia regionalne inne niż angielski.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Udoskonalając metody edycji

`HttpGet` `Edit` Metody generowane przez system scaffoldingu nie sprawdza, czy identyfikator, który jest przekazywany do niego jest prawidłowy. Jeśli użytkownik usunie identyfikator segmentu z adresu URL (`http://localhost:xxxxx/Movies/Edit`), jest wyświetlany następujący błąd:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Użytkownik może również przekazać identyfikator, który nie istnieje w bazie danych, takich jak `http://localhost:xxxxx/Movies/Edit/1234`. Możesz utworzyć dwie zmiany analizy `HttpGet` `Edit` metody akcji, aby rozwiązać tego ograniczenia. Najpierw należy zmienić `ID` parametr ma wartość domyślną równą zero, jeśli identyfikator nie jest jawnie przekazano. Można również sprawdzić, który `Find` metoda faktycznie znaleziono filmu przed zwróceniem obiekt filmu do szablonu widoku. Zaktualizowany interfejs `Edit` metoda znajdują się poniżej.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Jeśli film nie zostanie znaleziony, `HttpNotFound` metoda jest wywoływana.

Wszystkie `HttpGet` metody wykonaj podobny wzorzec. Staną się obiekt filmu (lub listę obiektów, w przypadku `Index`) i przekazać model widoku. `Create` Metoda przekazuje obiekt pusty film do tworzenia widoku. Wszystkie metody, które tworzenie, edytowanie, usuwanie lub inny sposób modyfikować danych, należy więc w `HttpPost` przeciążenia metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpis w blogu wpis [platformy ASP.NET MVC Porada 46 — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w przypadku metody GET również narusza HTTP najlepszych rozwiązań i wzorców architektury REST, który określa, czy żądania GET, nie należy zmieniać stan aplikacji. Innymi słowy wykonywanie operacji GET powinna być bezpieczne operacji, która ma żadnych efektów ubocznych.

## <a name="adding-a-search-method-and-search-view"></a>Dodawanie metody wyszukiwania i widoku wyszukiwania

W tej sekcji dodasz `SearchIndex` metody akcji, która umożliwia wyszukiwanie filmów według gatunku lub nazwy. Jest to dostępne za pośrednictwem */filmy/SearchIndex* adresu URL. Żądanie spowoduje wyświetlenie formularza HTML, która zawiera elementy danych wejściowych użytkownika można wypełnić, aby wyszukać film. Gdy użytkownik przesyła formularz, metody akcji Pobierz wartości wyszukiwania opublikowanych przez użytkownika i użyj wartości, aby wyszukać bazy danych.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Wyświetlanie formularza SearchIndex

Rozpocznij od dodania `SearchIndex` metody akcji do istniejących `MoviesController` klasy. Metoda zwraca widok, który zawiera formularza HTML. Oto kod:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

W pierwszym wierszu `SearchIndex` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmów:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

Zapytanie jest zdefiniowany w tym momencie, ale nie zostało jeszcze uruchomione względem magazynu danych.

Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania, używając następującego kodu:

W przeciwnym razie String.IsNullOrEmpty(searchString) następnie   
 filmy = filmów. Gdzie (funkcji s.Title.Contains(searchString))   
 End If

Zapytania LINQ nie są wykonywane, gdy są one definiowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`. Zamiast tego, wykonanie zapytania jest odroczone, co oznacza, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana. W `SearchIndex` przykładowe zapytanie jest wykonywane w widoku SearchIndex. Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).

Teraz można zaimplementować `SearchIndex` widok, w którym będą wyświetlane na formularzu do użytkownika. Kliknij prawym przyciskiem myszy wewnątrz `SearchIndex` metody, a następnie kliknij przycisk **Dodaj widok**. W **Dodaj widok** okna dialogowego należy określić, że zamierzasz przekazać `Movie` obiektu do szablonu widoku, jak jej klasa modelu. W **szablonu szkieletu** wybierz **listy**, następnie kliknij przycisk **Dodaj**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Po kliknięciu **Dodaj** przycisku *Views\Movies\SearchIndex.vbhtml* Wyświetl szablon został utworzony. Ponieważ wybrano **listy** w **szablonu szkieletu** listy Visual Web Developer generowane automatycznie (szkielet) niektóre domyślnej zawartości w widoku. Szkieletu utworzony formularza HTML. Go zbadać `Movie` klasy i utworzony kod do renderowania `<label>` elementów dla każdej właściwości klasy. Lista poniżej przedstawiono tworzenie widoku, który został wygenerowany:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Uruchom aplikację, a następnie przejdź do */filmy/SearchIndex*. Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL. Wyświetlane są filtrowane filmów.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Jeśli zmienisz podpis `SearchIndex` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Global.asax* pliku.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Zmodyfikowanego `SearchIndex` metoda wyglądałby następująco:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów. Teraz możesz dodasz interfejs użytkownika, aby pomóc im filtrowanie filmów. Jeśli zmienisz podpis `SearchIndex` metodę, aby przetestować sposób przekazywania parametru ID powiązane z tras, zmień ją tak, że Twoje `SearchIndex` metoda przyjmuje jako parametr ciągu o nazwie `searchString`:

Otwórz *Views\Movies\SearchIndex.vbhtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj następujący kod:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

`Html.BeginForm` Pomocnika tworzy otwierający `<form>` tagu. `Html.BeginForm` Pomocnika powoduje, że formularz do publikowania do samego siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.

Uruchom aplikację, a następnie spróbuj wyszukać film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Istnieje nie `HttpPost` przeciążenia `SearchIndex` metody. Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych. Jeśli dodano następujące `HttpPost` `SearchIndex` metody, wywołujący akcji będzie odpowiadać `HttpPost` `SearchIndex` metody i `HttpPost` `SearchIndex` metoda może działać, jak pokazano na poniższej ilustracji.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według gatunku

Jeśli dodano `HttpPost` wersję `SearchIndex` metody, usuń ją teraz.

Następnie dodasz funkcję, aby umożliwić użytkownikom wyszukiwanie filmów według gatunku. Zastąp `SearchIndex` metoda następującym kodem:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Ta wersja `SearchIndex` metoda przyjmuje dodatkowy parametr, to znaczy `movieGenre`. Tworzenie pierwszych kilka wierszy kodu `List` obiekt do przechowywania gatunki film z bazy danych.

Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Kod używa `AddRange` metody ogólnej `List` kolekcję, aby dodać różne gatunki do listy. (Bez `Distinct` modyfikator, zostaną dodane zduplikowane gatunki — na przykład, zostaną dodane Komedia dwukrotnie w naszym przykładzie). Kod następnie przechowuje listę gatunki w `ViewBag` obiektu.

Poniższy kod przedstawia sposób sprawdzić `movieGenre` parametru. Jeśli nie jest pusty kod dodatkowo ogranicza zapytanie filmy, aby ograniczyć wybranych filmów na określonego rodzaju.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Dodawanie znaczników do widoku SearchIndex umożliwiających wyszukiwanie według gatunku

Dodaj `Html.DropDownList` element pomocniczy służący do *Views\Movies\SearchIndex.vbhtml* pliku tuż przed `TextBox` pomocnika. Ukończone znaczników jest pokazany poniżej:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Uruchom aplikację, a następnie przejdź do */filmy/SearchIndex*. Spróbuj wyszukiwania według gatunku, tytuł filmu i oba kryteria.

W tej sekcji służy do badania metod akcji CRUD i widoki generowane przez platformę. Utworzono metody akcji wyszukiwania i widoku, które umożliwiają użytkownikom wyszukiwanie tytuł filmu i gatunku. W następnej sekcji zostanie przyjrzymy się sposób dodawania właściwości do `Movie` modelu oraz sposób dodawania inicjatora, które automatycznie utworzy test bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](accessing-your-models-data-from-a-controller.md)
> [dalej](adding-a-new-field.md)
