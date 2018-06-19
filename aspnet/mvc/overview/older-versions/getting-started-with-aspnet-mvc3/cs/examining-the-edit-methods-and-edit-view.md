---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: Badanie metody edycji i widoku edycji (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 581138bb25b95ef9002a2bd97d1fa28797d96bfa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875815"
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Badanie metody edycji i widoku edycji (C#)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.


W tej sekcji należy zbadać metod akcji wygenerowanych i widoków dla kontrolera filmu. Następnie dodasz niestandardową stronę wyszukiwania.

Uruchom aplikację i przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL na pasku adresu przeglądarki. Umieść kursor **Edytuj** łącze, aby wyświetlić adres URL, który jest połączona.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Edytuj** łącza został wygenerowany przez `Html.ActionLink` metody w *Views\Movies\Index.cshtml* widoku:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

`Html` Obiekt jest pomocnika uwidocznionego za pomocą właściwości na `WebViewPage` klasy podstawowej. `ActionLink` Metody pomocnika ułatwia dynamicznego generowania hiperłącza HTML, które łącze do metody akcji na kontrolerach. Pierwszy argument `ActionLink` metoda jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument jest nazwą metody akcji do wywołania. Ostatni argument jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący danych trasy (w tym przypadku identyfikator 4).

Wygenerowane łącze pokazano na poprzedniej ilustracji jest `http://localhost:xxxxx/Movies/Edit/4`. Trasa domyślna trwa wzorzec URL `{controller}/{action}/{id}`. W związku z tym tłumaczy ASP.NET `http://localhost:xxxxx/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontrolera z parametrem `ID` równa 4.

Można również przekazać parametry metody akcji przy użyciu ciągu zapytania. Na przykład adres URL `http://localhost:xxxxx/Movies/Edit?ID=4` przekazuje także parametr `ID` 4 do `Edit` metody akcji `Movies` kontrolera.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Otwórz `Movies` kontrolera. Dwa `Edit` poniżej przedstawiono metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `HttpPost` atrybutu. Ten atrybut określa, że przeciążenia `Edit` metoda może być wywoływana tylko dla żądań POST. Można zastosować `HttpGet` pierwszy dla atrybutu Edytuj metodę, ale który nie jest konieczne, ponieważ jest to wartość domyślna. (Firma Microsoft będzie odwoływać się do metod akcji, które są przypisane niejawnie `HttpGet` atrybutu jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda przyjmuje parametr ID film, wyszukuje filmu przy użyciu programu Entity Framework `Find` metody i zwraca wybrany film do widoku edycji. Podczas tworzenia widoku edycji systemu szkieletów zbadane `Movie` klasy i kodu do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. W poniższym przykładzie pokazano widok edycji został wygenerowany:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku — to ustawienie określa, czy widok oczekuje modelu widoku szablonu typu `Movie`.

Kod z utworzonym szkieletem używa kilku *metody pomocnicze* uprościć kod znaczników HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocnika Wyświetla nazwę pola ("Title", "ReleaseDate", "Rodzaju" lub "Price"). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Pomocnik wyświetli HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocnik wyświetli żadnych komunikatów dotyczących sprawdzania poprawności skojarzone z tą właściwością.

Uruchom aplikację i przejdź do */Movies* adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. HTML na stronie wygląda jak w następującym przykładzie. (Znaczników menu został wykluczony, aby były bardziej zrozumiałe).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

`<input>` Elementy są w formacie HTML `<form>` element których `action` ustawiono atrybut do wysłania do */filmów/Edytuj* adresu URL. Dane formularza zostaną opublikowane na serwerze po **Edytuj** kliknięciu przycisku.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniżej przedstawiono listę `HttpPost` wersji `Edit` metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

Integrator modelu framework ASP.NET przyjmuje wartości przesłanego formularza i tworzy `Movie` obiekt, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Wyboru w kodzie sprawdza, czy dane dostarczone w formie może służyć do modyfikowania `Movie` obiektu. Jeśli dane są prawidłowe, kod zapisuje dane filmu `Movies` Kolekcja `MovieDBContext` wystąpienia. Kod zapisze nowe dane filmu do bazy danych przez wywołanie metody `SaveChanges` metoda `MovieDBContext`, której będzie się powtarzał zmiany w bazie danych. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, która powoduje, że zaktualizowano film będzie wyświetlana na liście filmów.

Jeśli przesłanych wartości nie są prawidłowe, są wyświetlane ponownie w formularzu. `Html.ValidationMessageFor` Pomocników w *Edit.cshtml* widok szablonu zajmie się wyświetlanie odpowiednie komunikaty o błędach.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Uwaga dotycząca ustawień regionalnych** zwykle pracy z ustawień regionalnych innych niż angielski, zobacz [obsługi platformy ASP.NET MVC 3 weryfikacji z innym niż angielski.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Tworzenie bardziej niezawodna metoda edycji

`HttpGet` `Edit` Metody generowany przez system szkieletów nie sprawdza, czy identyfikator, który jest przekazywany do niego jest prawidłowa. Jeśli użytkownik usunie identyfikator segmentu z adresu URL (`http://localhost:xxxxx/Movies/Edit`), zostanie wyświetlony następujący błąd:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Użytkownik może także podać identyfikator, który nie istnieje w bazie danych, takich jak `http://localhost:xxxxx/Movies/Edit/1234`. Można utworzyć dwie zmiany `HttpGet` `Edit` metody akcji w celu rozwiązania tego ograniczenia. Najpierw należy zmienić `ID` parametr ma wartość domyślną równą zero, jeśli identyfikator nie jest jawnie przekazany. Można również sprawdzić, które `Find` metody faktycznie znaleziono filmu przed zwróceniem obiekt filmu do szablonu widoku. Zaktualizowany interfejs `Edit` metody są wyświetlane poniżej.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Jeśli film nie zostanie znaleziony, `HttpNotFound` metoda jest wywoływana.

Wszystkie `HttpGet` metody wykonaj podobnego wzorca. Otrzymują obiektu movie (lub listę obiektów, w przypadku `Index`) i przekazać model widoku. `Create` — Metoda przekazuje filmu pusty obiekt do tworzenia widoku. Wszystkie metody, które tworzenia, edytowania, usuwania lub modyfikację danych należy więc w `HttpPost` przeciążenia metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpis w blogu post [ASP.NET MVC Porada 46 — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w metodzie GET również narusza HTTP najlepszych rozwiązań i wzorzec architektury REST, który określa, że żądania GET nie należy zmieniać stanu aplikacji. Innymi słowy wykonanie operacji GET powinny być bezpieczne operację, która nie ma skutków ubocznych.

## <a name="adding-a-search-method-and-search-view"></a>Dodawanie metody wyszukiwania i widoku wyszukiwania

W tej sekcji dodasz `SearchIndex` metody akcji, która umożliwia wyszukiwanie filmów genre lub nazwę. Będzie to dostępne przy użyciu */filmów/SearchIndex* adresu URL. Żądanie spowoduje wyświetlenie formularza HTML, który zawiera elementy wejściowe użytkownika można wprowadzić w celu wyszukania filmu. Gdy użytkownik przesyła formularz, metoda akcji wartości wyszukiwania zapisane przez użytkownika, a Użyj wartości do wyszukania bazy danych.

## <a name="displaying-the-searchindex-form"></a>Wyświetlanie w formularzu SearchIndex

Rozpocznij od dodania `SearchIndex` metody akcji do istniejącej `MoviesController` klasy. Metoda zwraca widok zawierający formularza HTML. Oto kod:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

W pierwszym wierszu `SearchIndex` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmy:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

Zapytanie jest zdefiniowany w tym momencie, ale nie został jeszcze uruchomiony przed magazynu danych.

Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania wartość ciągu wyszukiwania, używając następującego kodu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Zapytania LINQ nie są wykonywane, gdy są one zdefiniowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`. Zamiast tego wykonywania zapytania jest opóźnione, co oznacza, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest rzeczywiście iterowane za pośrednictwem lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana. W `SearchIndex` przykładowe zapytanie jest wykonywane w widoku SearchIndex. Aby uzyskać więcej informacji na temat wykonywania zapytań odroczonych, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).

Teraz można wdrożyć `SearchIndex` widok, który będzie wyświetlany formularz dla użytkownika. Kliknij prawym przyciskiem myszy wewnątrz `SearchIndex` metody, a następnie kliknij przycisk **Dodaj widok**. W **Dodaj widok** oknie dialogowym Określ, że zamierzasz przekazać `Movie` obiekt w szablonie widoku, jak jego klasa modelu. W **szablonu szkieletu** wybierz **listy**, następnie kliknij przycisk **Dodaj**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Po kliknięciu **Dodaj** przycisku *Views\Movies\SearchIndex.cshtml* jest tworzony widok szablonu. Ponieważ wybrano **listy** w **szablonu szkieletu** listy Visual Web Developer generowane automatycznie (szkieletu) niektórych domyślnej zawartości w widoku. Rusztowania utworzyć formularza HTML. Ją zbadać `Movie` klasy i kodu do renderowania `<label>` elementy dla każdej właściwości klasy. Lista poniżej zawiera widok Utwórz, który został wygenerowany:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Uruchom aplikację i przejdź do */filmów/SearchIndex*. Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL. Filtrowane filmów są wyświetlane.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Jeśli zmienisz podpis `SearchIndex` metoda ma parametr o nazwie `id`, `id` parametr będzie odpowiadać `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Global.asax* pliku.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Zmodyfikowane `SearchIndex` metoda będzie wyglądać następująco:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

Tytuł wyszukiwania mogą być obecnie przekazywane jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą, aby wyszukać filmu. Tak, teraz możesz dodasz interfejsu użytkownika w celu ich filtrowania filmów. Zmiana podpisu `SearchIndex` metody do testowania sposób przekazywania parametru Identyfikatora trasy wiązaniem, zmień ją tak, że Twoje `SearchIndex` metoda przyjmuje parametr ciągu o nazwie `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Otwórz *Views\Movies\SearchIndex.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj następujący kod:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

W poniższym przykładzie przedstawiono część *Views\Movies\SearchIndex.cshtml* pliku z dodano znaczników filtrowania.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

`Html.BeginForm` Pomocnika tworzy otwierania `<form>` tagu. `Html.BeginForm` Pomocnika powoduje, że formularz publikowania do samej siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.

Uruchom aplikację i spróbuj wykonać wyszukiwanie filmu.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Brak nie `HttpPost` przeciążenia z `SearchIndex` metody. Nie jest konieczne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `HttpPost SearchIndex` metody. W takim przypadku element wywołujący akcji spowoduje dopasowanie `HttpPost SearchIndex` metody i `HttpPost SearchIndex` metoda może działać, jak pokazano na poniższej ilustracji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Jednak nawet jeśli dodasz to `HttpPost` wersji `SearchIndex` metody, jest to ograniczenie, w jaki sposób to wszystko zostało zaimplementowane. Załóżmy, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać łącze do znajomych, mogą kliknąć w celu wyświetlenia tego samego filtrowane listy filmów. Zwróć uwagę, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmów/SearchIndex) — nie ma żadnych informacji wyszukiwania w adresie URL, do samej siebie. Prawo teraz, wyszukaj ciąg informacje są wysyłane do serwera jako wartość pola formularza. Oznacza to, że nie można przechwycić informacje wyszukiwania zakładki lub wysyłać do znajomych w adresie URL.

Rozwiązaniem jest użycie przeciążenia `BeginForm` Określa, czy żądanie POST, należy dodać informacje dotyczące wyszukiwania na adres URL i należy to kierowane do wersji HttpGet `SearchIndex` metody. Zamień istniejące bez parametrów `BeginForm` metody z następujących czynności:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet SearchIndex` metody akcji, nawet jeśli masz `HttpPost SearchIndex` metody.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według rodzaju

Jeśli dodano `HttpPost` wersji `SearchIndex` metody, usuń go teraz.

Następnie dodasz funkcji, aby umożliwić użytkownikom wyszukiwanie filmów według rodzaju. Zastąp `SearchIndex` metodę z następującym kodem:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Ta wersja `SearchIndex` metoda przyjmuje dodatkowych parametrów, a mianowicie `movieGenre`. Tworzenie pierwszego kilku wierszy kodu `List` obiektu do przechowywania genres film z bazy danych.

Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

W kodzie użyto `AddRange` metody ogólnej `List` kolekcji można dodać do listy wszystkich unikatowych genres. (Bez `Distinct` modyfikator, zostanie dodany genres zduplikowane — na przykład zostanie dodany Komedia dwa razy w naszym przykładzie). Kod następnie przechowuje listę gatunkami muzyki w `ViewBag` obiektu.

Poniższy kod przedstawia sposób sprawdzania `movieGenre` parametru. Jeśli nie jest on pusty kodu ogranicza kwerendę filmy i ograniczyć wybranego filmów do określonego rodzaju.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Dodawanie znaczników do widoku SearchIndex obsługuje wyszukiwania według rodzaju

Dodaj `Html.DropDownList` Pomocnika *Views\Movies\SearchIndex.cshtml* pliku tuż przed `TextBox` pomocnika. Poniżej przedstawiono wypełniony kod znaczników:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Uruchom aplikację i przejdź do */filmów/SearchIndex*. Spróbować przeprowadzić wyszukiwanie według rodzaju, nazwa filmu i oba kryteria.

W tej sekcji zbadać metod akcji CRUD i widoki generowane przez platformę. Utworzono metody akcji wyszukiwania i widok, który zezwala użytkownikom na wyszukiwanie według tytuł filmu i rodzaju. W następnej sekcji, możesz wyjaśniono, jak dodać właściwości do `Movie` modelu oraz sposobu dodawania inicjatora automatycznie utworzy testowej bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](accessing-your-models-data-from-a-controller.md)
> [dalej](adding-a-new-field.md)
