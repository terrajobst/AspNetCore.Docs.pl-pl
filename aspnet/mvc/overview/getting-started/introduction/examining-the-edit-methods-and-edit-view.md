---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Badanie widoku edycji i Edytuj metody | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Badanie metody edycji i widoku edycji
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

W tej sekcji będziesz Sprawdź wygenerowany `Edit` widoków dla filmu kontrolera i metody akcji. Ale najpierw potrwa krótkich kierowania dokonanie Data wydania lepsze. Otwórz *Models\Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Można również ustawić jako kultury Data określonych w następujący sposób:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Omówione zostaną następujące czynności [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) w następnym samouczku. [Wyświetlić](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) Określa atrybut, co ma być wyświetlany dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut określa typ danych, w tym przypadku jest data, więc nie jest wyświetlany w polu informacje o godzinie. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybut jest wymagany dla usterki w przeglądarce Chrome, która renderuje formaty daty niepoprawnie.

Uruchom aplikację i przejdź do `Movies` kontrolera. Umieść kursor **Edytuj** łącze, aby wyświetlić adres URL, który jest połączona.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Edytuj** łącza został wygenerowany przez `Html.ActionLink` metody w *Views\Movies\Index.cshtml* widoku:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Obiekt jest pomocnika uwidocznionego za pomocą właściwości na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) klasy podstawowej. `ActionLink` Metody pomocnika ułatwia dynamicznego generowania hiperłącza HTML, które łącze do metody akcji na kontrolerach. Pierwszy argument `ActionLink` metoda jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument jest nazwą metody akcji (w tym przypadku `Edit` akcji). Ostatni argument jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący danych trasy (w tym przypadku identyfikator 4).

Wygenerowane łącze pokazano na poprzedniej ilustracji jest `http://localhost:1234/Movies/Edit/4`. Trasa domyślna (w *aplikacji\_Start\RouteConfig.cs*) trwa wzorzec URL `{controller}/{action}/{id}`. W związku z tym tłumaczy ASP.NET `http://localhost:1234/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontrolera z parametrem `ID` równa 4. Przejrzyj następujący kod z *aplikacji\_Start\RouteConfig.cs* pliku. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda jest używana do routingu żądań HTTP właściwą metodę akcji i kontrolerów i podać parametr opcjonalny identyfikator. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda jest już używana przez [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) takich jak `ActionLink` do generowania adresów URL podany kontrolera, metody akcji i dane trasy.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Można również przekazać parametry metody akcji przy użyciu ciągu zapytania. Na przykład adres URL `http://localhost:1234/Movies/Edit?ID=3` przekazuje także parametr `ID` 3 `Edit` metody akcji `Movies` kontrolera.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otwórz `Movies` kontrolera. Dwa `Edit` poniżej przedstawiono metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `HttpPost` atrybutu. Ten atrybut określa, że przeciążenia `Edit` metoda może być wywoływana tylko dla żądań POST. Można zastosować `HttpGet` pierwszy dla atrybutu Edytuj metodę, ale który nie jest konieczne, ponieważ jest to wartość domyślna. (Firma Microsoft będzie odwoływać się do metod akcji, które są przypisane niejawnie `HttpGet` atrybutu jako `HttpGet` metody.) [Powiązać](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atrybut jest inny ważne mechanizm chroniąca przed hakerami z nadmiernie wysyłania danych do modelu. W atrybucie powiązania, które chcesz zmienić powinna zawierać tylko właściwości. Informacje o overposting i atrybut wiązania w mojej [overposting Uwaga dotycząca zabezpieczeń](https://go.microsoft.com/fwlink/?LinkId=317598). W modelu prostego używane w tym samouczku firma Microsoft będzie wiążące wszystkich danych w modelu. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atrybut jest używany w celu zapobiegania fałszowaniu żądania i wraz z `@Html.AntiForgeryToken()` w pliku widok edycji (*Views\Movies\Edit.cshtml*), fragment jest pokazany poniżej:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` generuje token zabezpieczający przed sfałszowaniem ukrytym, które muszą być zgodne `Edit` metody `Movies` kontrolera. Więcej o Cross-site żądania sfałszowaniem (znanej także jako XSRF lub CSRF) w samouczku Mój [zapobiegania XSRF/CSRF w nazwie wzorca MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Metoda przyjmuje parametr ID film, wyszukuje filmu przy użyciu programu Entity Framework `Find` metody i zwraca wybrany film do widoku edycji. Jeśli nie można odnaleźć film, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) jest zwracany. Podczas tworzenia widoku edycji systemu szkieletów zbadane `Movie` klasy i kodu do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. W poniższym przykładzie pokazano widok edycji został wygenerowany przez system szkieletów programu visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku — to ustawienie określa, czy widok oczekuje modelu widoku szablonu typu `Movie`.

Kod z utworzonym szkieletem używa kilku *metody pomocnicze* uprościć kod znaczników HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocnika Wyświetla nazwę pola (&quot;tytuł&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, lub &quot;cen &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Pomocnika renderowania kodu HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocnik wyświetli żadnych komunikatów dotyczących sprawdzania poprawności skojarzone z tą właściwością.

Uruchom aplikację i przejdź do */Movies* adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. Poniżej przedstawiono kod HTML dla elementu form.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Elementy są w formacie HTML `<form>` element których `action` ustawiono atrybut do wysłania do */filmów/Edytuj* adresu URL. Dane formularza zostaną opublikowane na serwerze po **zapisać** kliknięciu przycisku. Drugi wiersz zawiera ukrytego [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generowane przez `@Html.AntiForgeryToken()` wywołania.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniżej przedstawiono listę `HttpPost` wersji `Edit` metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) sprawdza poprawność atrybutu [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generowane przez `@Html.AntiForgeryToken()` wywołań w widoku.

[Integratora modelu platformy ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) przyjmuje wartości przesłanego formularza i tworzy `Movie` obiekt, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Metoda sprawdza, czy dane dostarczone w formie może służyć do modyfikowania (edycji lub aktualizacji) `Movie` obiektu. Jeśli dane są prawidłowe, są zapisywane dane filmu `Movies` Kolekcja `db(MovieDBContext` wystąpienie). Nowe dane film jest zapisywana w bazie danych przez wywołanie metody `SaveChanges` metody `MovieDBContext`. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, która wyświetla kolekcję film, w tym tylko zmiany.

Jak weryfikacji po stronie klienta określa, czy wartości pola nie są prawidłowe, wyświetlany jest komunikat o błędzie. Jeśli wyłączysz JavaScript, nie będziesz mieć weryfikacji po stronie klienta, ale serwer wykryje przesłanych wartości nie są prawidłowe, a wartości formularza zostanie wyświetlony ponownie, z komunikatów o błędach. Sprawdzanie poprawności bardziej szczegółowo omówione w dalszej części tego samouczka.

`Html.ValidationMessageFor` Pomocników w *Edit.cshtml* widok szablonu zajmie się wyświetlanie odpowiednie komunikaty o błędach.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Wszystkie `HttpGet` metody wykonaj podobnego wzorca. Otrzymują obiektu movie (lub listę obiektów, w przypadku `Index`) i przekazać model widoku. `Create` — Metoda przekazuje filmu pusty obiekt do tworzenia widoku. Wszystkie metody, które tworzenia, edytowania, usuwania lub modyfikację danych należy więc w `HttpPost` przeciążenia metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpis w blogu post [ASP.NET MVC Porada 46 — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w metodzie GET również narusza HTTP najlepszych rozwiązań i architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) wzorca, który określa, że żądania GET nie należy zmieniać stanu aplikacji. Innymi słowy wykonanie operacji GET powinny być bezpieczne operację, która nie ma skutków ubocznych i nie modyfikuje danych trwałych.

Jeśli używasz komputera angielski, można pominąć tę sekcję i przejdź do następnego samouczka. Możesz pobrać wersję Globalize tego samouczka [tutaj](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Uzyskać doskonałe dwie części samouczka na internacjonalizacji [Nadeem dla platformy ASP.NET MVC 5 internacjonalizacji](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> w celu obsługi weryfikacji jQuery dla ustawień regionalnych innych niż angielskie, które użyj przecinka (&quot;,&quot;) dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wprowadzić *globalize.js* i konkretnej  *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. Możesz pobrać ze strony NuGet weryfikacji jQuery innej niż angielska. (Nie należy instalować Globalize Jeśli używasz angielskiej wersji językowej ustawień regionalnych.)


1. Z **narzędzia** kliknij menu **Menedżera pakietów NuGetLibrary**, a następnie kliknij przycisk **Zarządzaj pakietami NuGet dla rozwiązania**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. W lewym okienku wybierz **Przeglądaj*. *** (zobacz obraz poniżej).
3. W polu wejściowym, wprowadź * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Wybierz `jQuery.Validation.Globalize`, wybierz `MvcMovie` i kliknij przycisk **zainstalować**. *Scripts\jquery.globalize\globalize.js* plik zostanie dodany do projektu. *Scripts\jquery.globalize\cultures\* folder będzie zawierać wiele plików JavaScript kultury. Uwaga: może zająć 5 minut, aby zainstalować ten pakiet.

 Poniższy kod przedstawia zmiany w pliku Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Aby uniknąć powtarzania ten kod w każdym widoku edycji, można przenieść go do pliku układu. Aby zoptymalizować pobieranie skryptu, zobacz samouczek Mój [tworzenie pakietów i minimalizowanie](../../performance/bundling-and-minification.md).

Aby uzyskać więcej informacji, zobacz [ASP.NET MVC 3 internacjonalizacji](http://afana.me/post/aspnet-mvc-internationalization.aspx) i [ASP.NET MVC 3 internacjonalizacji — część 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Jako tymczasowy poprawkę Jeśli nie można pobrać weryfikacji pracy w ustawień regionalnych użytkownika, możesz wymusić komputera do US English albo można wyłączyć JavaScript w przeglądarce. Aby wymusić US English na komputerze, można dodać elementu globalizacji w katalogu głównym projekty *web.config* pliku. Poniższy kod przedstawia element globalizacji z kulturą ustawioną językiem angielskim.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> W następnym samouczku będzie wprowadzania funkcji wyszukiwania.

>[!div class="step-by-step"]
[Poprzednie](accessing-your-models-data-from-a-controller.md)
[dalej](adding-search.md)
