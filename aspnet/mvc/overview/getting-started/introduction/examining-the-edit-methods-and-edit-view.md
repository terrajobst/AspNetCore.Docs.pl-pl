---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Badanie metod edycji i widoku edycji | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a166f6c4450c72adc23f7d36113ceba7e04f1929
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38170277"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Badanie metod edycji i widoku edycji
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji zostanie omówione wygenerowany `Edit` metody akcji i widoki dla kontrolera filmu. Ale najpierw zajmie krótki kierowaniu umożliwiają lepsze daty wydania. Otwórz *Models\Movie.cs* pliku i Dodaj wyróżnione wiersze, które przedstawiono poniżej:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Można również ustawić kulturę daty określone w następujący sposób:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Omówimy [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) w następnym samouczku. [Wyświetlić](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) atrybut określa, co będzie wyświetlone nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut określa typ danych, w tym przypadku jest data, więc nie jest wyświetlana w polu informacje o godzinie. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybut jest wymagany w przypadku błędu w przeglądarce Chrome renderujący formaty daty są niepoprawne.

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera. Umieść wskaźnik myszy nad **Edytuj** link, aby wyświetlić adres URL, który łączy on.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Edytuj** link został wygenerowany przez `Html.ActionLink` method in Class metoda *Views\Movies\Index.cshtml* widoku:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Obiekt jest pomocnika, która jest widoczna, za pomocą właściwości na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) klasy bazowej. `ActionLink` Metody pomocnika ułatwia można dynamicznie wygenerować hiperłącza HTML, które łącze do metody akcji, na kontrolerach. Pierwszy argument `ActionLink` metodą jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument jest nazwa metody akcji do wywołania (w tym przypadku `Edit` akcji). Ostatni argument jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący dane trasy (w tym przypadku identyfikator 4).

Wygenerowane łącze na poprzednim obrazie jest `http://localhost:1234/Movies/Edit/4`. Trasa domyślna (w *aplikacji\_Start\RouteConfig.cs*) zajmuje wzorzec adresu URL `{controller}/{action}/{id}`. W związku z tym, tłumaczy ASP.NET `http://localhost:1234/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontroler z parametrem `ID` równa 4. Sprawdź następujący kod z *aplikacji\_Start\RouteConfig.cs* pliku. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda jest używana do rozsyłania żądań HTTP właściwą metodę akcji i kontrolerów i podać opcjonalny parametr Identyfikatora. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda jest również używany przez [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) takich jak `ActionLink` do generowania adresów URL, biorąc pod uwagę kontrolera, metody akcji i dane trasy.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Można również przekazać parametry metody akcji przy użyciu ciągu zapytania. Na przykład adres URL `http://localhost:1234/Movies/Edit?ID=3` przekazuje parametr `ID` 3 `Edit` metody akcji `Movies` kontrolera.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otwórz `Movies` kontrolera. Dwa `Edit` poniżej przedstawiono metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `HttpPost` atrybutu. Ten atrybut określa, że przeciążenia `Edit` metoda może być wywoływana tylko w przypadku żądania POST. Można zastosować `HttpGet` atrybutu do pierwszego Edytuj metodę, ale który nie jest konieczne, ponieważ jest ona domyślnie. (Będziemy się odwoływać do metod akcji, które są jawnie przypisane `HttpGet` atrybutu jako `HttpGet` metody.) [Powiązać](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atrybutu jest inny mechanizm ważne zabezpieczeń, który zapobiega polegającymi danych do modelu blokowaniu hakerów. Właściwości powinny zawierać tylko w atrybucie powiązania, który chcesz zmienić. Informacje o overposting i atrybut wiązania w mojej [polegającymi Uwaga dotycząca zabezpieczeń](https://go.microsoft.com/fwlink/?LinkId=317598). W prosty model używany w tym samouczku firma Microsoft będzie wiążące wszystkie dane w modelu. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atrybut używany w celu zapobiegania fałszowaniu żądania i wraz z `@Html.AntiForgeryToken()` w pliku widoku edycji (*Views\Movies\Edit.cshtml*), poniżej przedstawiono fragment:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` generuje token zabezpieczający przed sfałszowaniem ukrytym, które muszą być zgodne `Edit` metody `Movies` kontrolera. Możesz dowiedzieć się więcej o Cross-site żądania sfałszowaniem (znany także jako XSRF lub CSRF) w ramach mojej samouczka [zapobiegania XSRF/CSRF WE MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Metoda przyjmuje parametr Identyfikatora filmu, wyszukuje filmu używający narzędzia Entity Framework `Find` metodę i zwraca wybrany film do widoku edycji. Jeśli nie można odnaleźć filmu, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) jest zwracana. Podczas tworzenia widoku edycji system scaffoldingu zbadane `Movie` klasy i utworzony kod do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. Poniższy przykład przedstawia widok edycji, który został wygenerowany przez system scaffoldingu programu visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku — Określa, czy widok oczekuje modelu dla widoku szablonu typu `Movie`.

Utworzony szkielet kodu wykorzystuje kilka *metody pomocnika* uprościć kod znaczników HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocnik wyświetli nazwę pola (&quot;tytuł&quot;, &quot;ReleaseDate&quot;, &quot;gatunku&quot;, lub &quot;ceny &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Pomocnika renderowanie kodu HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocnik wyświetli wszystkie komunikaty weryfikacji skojarzony z tej właściwości.

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. Poniżej przedstawiono kod HTML dla elementu form.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Elementy są w formacie HTML `<form>` elementu którego `action` ma ustawioną wartość atrybutu Opublikuj */filmy/Edytuj* adresu URL. Dane formularza zostaną opublikowane na serwerze po **Zapisz** przycisku. Drugi wiersz zawiera ukryte [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generowane przez `@Html.AntiForgeryToken()` wywołania.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniższej przedstawiono listę `HttpPost` wersję `Edit` metody akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) weryfikuje atrybut [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generowane przez `@Html.AntiForgeryToken()` wywołań w widoku.

[Integratora modelu programu ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) przyjmuje wartości przesłanego formularza i tworzy `Movie` obiektu, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Metoda sprawdza, czy dane dostarczone w formie może służyć do modyfikowania (edycji lub aktualizacja) `Movie` obiektu. Jeśli dane są prawidłowe, dane film zostanie zapisana w `Movies` zbiór `db(MovieDBContext` wystąpienie). Nowe dane film jest zapisywany w bazie danych przez wywołanie metody `SaveChanges` metody `MovieDBContext`. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, która wyświetla kolekcji film, w tym zmiany wprowadzone przed chwilą.

Jak najszybciej po weryfikacji po stronie klienta okaże się, że wartości pola nie są prawidłowe, jest wyświetlany komunikat o błędzie. Jeśli wyłączysz JavaScript, nie będziesz mieć weryfikacji po stronie klienta, ale serwer wykryje przesłanych wartości nie są prawidłowe, a wartości formularza zostanie wyświetlony ponownie, z komunikatów o błędach. W dalszej części tego samouczka omówiony weryfikacji bardziej szczegółowo.

`Html.ValidationMessageFor` Obiekty pomocnicze w *Edit.cshtml* Wyświetl szablon powinien zachować ostrożność, wyświetlania odpowiedniego komunikatu o błędzie.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Wszystkie `HttpGet` metody wykonaj podobny wzorzec. Staną się obiekt filmu (lub listę obiektów, w przypadku `Index`) i przekazać model widoku. `Create` Metoda przekazuje obiekt pusty film do tworzenia widoku. Wszystkie metody, które tworzenie, edytowanie, usuwanie lub inny sposób modyfikować danych, należy więc w `HttpPost` przeciążenia metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpis w blogu wpis [platformy ASP.NET MVC Porada 46 — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w przypadku metody GET również narusza HTTP najlepszych rozwiązań i architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) wzorca, który określa, czy żądania GET, nie należy zmieniać stan aplikacji. Innymi słowy wykonywanie operacji GET powinna być bezpieczne operacji, która ma żadnych efektów ubocznych i nie modyfikuje utrwalonych danych.

Jeśli używasz komputera angielski, można pominąć tę sekcję i przejdź do następnego samouczka. Możesz pobrać wersję Globalize po ukończeniu tego samouczka [tutaj](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Doskonałe samouczek dwie części na internacjonalizacji, zobacz [firmy Nadeem platformy ASP.NET MVC 5 internacjonalizacji](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> Do obsługi dotyczącą weryfikacji jQuery dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (&quot;,&quot;) dla punktu dziesiętnego i formaty daty inne niż angielski, należy wprowadzić *globalize.js* i konkretne  *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i języka JavaScript, aby użyć `Globalize.parseFloat`. Możesz uzyskać weryfikacji innej niż angielska jQuery z pakietów NuGet. (Nie należy instalować Globalize Jeśli używasz angielskie ustawienia regionalne.)


1. Z **narzędzia** kliknij menu **Menedżera pakietów NuGetLibrary**, a następnie kliknij przycisk **Zarządzaj pakietami NuGet dla rozwiązania**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. W okienku po lewej stronie wybierz <strong>Przeglądaj *.</strong>* (Zobacz poniższy obraz).
3. W polu wejściowym wpisz * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Wybierz `jQuery.Validation.Globalize`, wybierz `MvcMovie` i kliknij przycisk **zainstalować**. *Scripts\jquery.globalize\globalize.js* plik zostanie dodany do projektu. *Scripts\jquery.globalize\cultures\* folder będzie zawierać wiele plików JavaScript kultury. Należy pamiętać, że może potrwać 5 minut, aby zainstalować ten pakiet.

   Poniższy kod pokazuje zmiany w pliku Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Aby uniknąć powtarzania ten kod w każdym widoku edycji, można przenieść je do pliku układu. W celu zoptymalizowania pobieranie skryptu, zobacz samouczek Moje [tworzenie pakietów i minimalizowanie](../../performance/bundling-and-minification.md).

Aby uzyskać więcej informacji, zobacz [platformy ASP.NET MVC 3 internacjonalizacji](http://afana.me/post/aspnet-mvc-internationalization.aspx) i [platformy ASP.NET MVC 3 internacjonalizacji — część 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Jako tymczasowe awarii jeśli nie można pobrać sprawdzania poprawności, Praca z ustawieniami regionalnymi, można wymusić komputer do używania US English, lub można wyłączyć języka JavaScript w przeglądarce. Aby wymusić na komputerze użycie US English, można dodać elementu globalizacji w katalogu głównym projektów *web.config* pliku. Poniższy kod przedstawia element globalizacji z kulturą równa Stanów Zjednoczonych w języku angielskim.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> W następnym samouczku będziesz wdrażamy funkcji wyszukiwania.

> [!div class="step-by-step"]
> [Poprzednie](accessing-your-models-data-from-a-controller.md)
> [dalej](adding-search.md)
