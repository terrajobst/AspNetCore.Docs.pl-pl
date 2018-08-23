---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Dodawanie weryfikacji do modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 01190b7bffc0258649373f3be0c75c41af80921d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752668"
---
<a name="adding-validation-to-the-model"></a>Dodawanie weryfikacji do modelu
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji dodasz logikę walidacji do `Movie` modelu, a będzie upewnij się, że reguły sprawdzania poprawności są wymuszane ilekroć użytkownik próbuje utworzyć lub edytować film przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Utrzymywanie susz rzeczy

Jednym z podstawowych zasadach projektowania platformy ASP.NET MVC jest PRÓBNEGO (&quot;nie Powtórz samodzielnie&quot;). ASP.NET MVC zachęca można określić funkcji lub zachowanie tylko raz, a następnie go wszędzie, gdzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, który należy napisać i sprawia, że kod, który pisanie mniej błędów, podatne i łatwiejsze w utrzymaniu.

Obsługa weryfikacji platformy ASP.NET MVC i Entity Framework Code First to świetny przykład susz zasady w akcji. Można deklaratywne określenie reguł sprawdzania poprawności w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie, gdzie w aplikacji.

Oto jak możesz korzystać z zalet tej obsługi weryfikacji w aplikacji filmu.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawania reguł sprawdzania poprawności do modelu Movie

Rozpocznie się przez dodanie niektórych logikę walidacji do `Movie` klasy.

Otwórz *Movie.cs* pliku. Dodaj `using` instrukcji w górnej części pliku, który odwołuje się do [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Zwróć uwagę, przestrzeń nazw zawiera `System.Web`. DataAnnotations zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, które są stosowane w sposób deklaratywny do dowolnej klasy lub właściwości.

Teraz zaktualizować `Movie` klasy, aby skorzystać z wbudowanych [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), i [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutów sprawdzania poprawności . Użyj poniższego kodu, na przykład gdzie można zastosować atrybuty.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Uruchom aplikację i ponownie zostanie wyświetlony następujący błąd w czasie wykonywania:

***Model kopii kontekstu "MovieDBContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First w aktualizacji bazy danych ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Firma Microsoft użyje migracji do zaktualizowania schematu. Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenia:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` Klasa pochodna o podanej nazwie (*AddDataAnnotationsMig*), a następnie w `Up` metody zostanie wyświetlony kod, który aktualizuje ograniczenia schematu. `Title` i `Genre` pola nie są już dopuszcza wartości null (oznacza to, wprowadź wartość) i `Rating` pole ma maksymalną długość 5.

Atrybuty weryfikacji określić zachowanie, które mają zostać wymuszone we właściwościach modelu, które są stosowane względem. `Required` Atrybut wskazuje, że właściwość musi mieć wartość; w tym przykładzie filmu musi mieć wartości `Title`, `ReleaseDate`, `Genre`, i `Price` właściwości, aby był prawidłowy. `Range` Atrybut ogranicza wartości do określonego zakresu. `StringLength` Atrybut pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie długości minimalnej. Typy wewnętrzne (takie jak `decimal, int, float, DateTime`) są wymagane domyślnie i nie ma potrzeby `Required` atrybutu.

Kod najpierw gwarantuje, że reguł sprawdzania poprawności, które określisz w klasie modelu są wymuszane, zanim aplikacja zapisuje zmiany w bazie danych. Na przykład, poniższy kod spowoduje zgłoszenie wyjątku podczas `SaveChanges` metoda jest wywoływana, ponieważ niektóre wymagane `Movie` brakuje wartości właściwości, a cena jest równa zero, (która jest poza prawidłowym zakresem).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Posiadanie reguły sprawdzania poprawności, które automatycznie wymuszanych przez program .NET Framework ułatwia zapewnienie aplikacji bardziej niezawodne. Gwarantuje również, że nie pamiętasz do sprawdzania poprawności coś i przypadkowo umożliwiają złe dane do bazy danych.

Oto kompletny kod dla zaktualizowanego *Movie.cs* pliku:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Błąd sprawdzania poprawności UI we wzorcu ASP.NET MVC

Uruchom ponownie aplikację i przejdź do */Movies* adresu URL.

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film. Wypełnij formularz z niektórych z nieprawidłowymi wartościami, a następnie kliknij przycisk **Utwórz** przycisku.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> do obsługi dotyczącą weryfikacji jQuery dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (&quot;,&quot;) separator dziesiętny, musi zawierać *globalize.js* i konkretne *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i języka JavaScript, aby użyć `Globalize.parseFloat`. Poniższy kod przedstawia zmiany w pliku Views\Movies\Edit.cshtml do pracy z &quot;fr-FR&quot; kultury:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Zwróć uwagę, jak formularz został automatycznie umożliwia kolorem czerwonym obramowaniem Wyróżnij tekst zawiera nieprawidłowe dane, które ma wysyłanego komunikatu o błędzie weryfikacji odpowiednich obok każdej z nich. Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma Obsługa skryptów JavaScript wyłączona).

Korzyści z rzeczywistych jest, nie należy zmieniać jednego wiersza kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu włączenia tej weryfikacji interfejsu użytkownika. Kontrolera i widoki utworzone wcześniej w tym samouczku automatycznie wybrany w górę sprawdzania poprawności reguły określona za pomocą atrybutów weryfikacji właściwości `Movie` klasa modelu.

Być może Zauważyłeś, właściwości `Title` i `Genre`, wymaganego atrybutu nie jest wymuszana, dopóki nie można przesłać formularza (trafień **Utwórz** przycisku), lub wprowadź tekst do pola wejściowego, a on usunięty. Dla pola, które jest początkowo pusta (na przykład pola w widoku Create) i który ma wymaganego atrybutu i innych atrybutów sprawdzania poprawności, można wykonać następujące polecenie, aby wyzwolić sprawdzania poprawności:

1. Karta do pola.
2. Wprowadź jakiś tekst.
3. Karta.
4. Karta do pola.
5. Usuń tekst.
6. Karta.

Powyższe sekwencji wyzwoli wymaganej weryfikacji bez naciśnięcie przycisku Prześlij. Po prostu naciśnięcie przycisku Prześlij bez żadnego pola wprowadzania wyzwoli weryfikacji po stronie klienta. Dane nie są wysyłane do serwera, aż nie wystąpią żadne błędy weryfikacji po stronie klienta. Można to sprawdzić przez umieszczenie punkt przerwania w metodzie Post protokołu HTTP lub przy użyciu [narzędzie fiddler](http://fiddler2.com/fiddler2/) lub programu Internet Explorer 9 [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>W jaki sposób weryfikacji odbywa się w tworzenie wyświetlanie i Tworzenie metody akcji

Być może zastanawiasz się, jak sprawdzanie poprawności UI został wygenerowany bez wykonywania żadnych aktualizacji do kodu w kontrolerze lub widoków. Dalej prezentuje co `Create` metody `MovieController` jak wyglądają klasy. Są one w porównaniu z jak utworzone wcześniej w tym samouczku.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Pierwszy (HTTP GET) `Create` metody akcji Wyświetla początkowej formularza tworzenia. Drugi (`[HttpPost]`) wersja obsługuje post formularza. Drugi `Create` — metoda ( `HttpPost` wersji) wywołań `ModelState.IsValid` do sprawdzenia, czy ten film zawiera wszystkie błędy weryfikacji. Wywołanie tej metody ocenia wszelkie atrybuty weryfikacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` metoda ponownie zostanie wyświetlony formularz. Jeśli nie ma żadnych błędów, metoda zapisuje ten nowy film w bazie danych. W naszym przykładzie filmu użyto **nie opublikowania formularza z serwerem, gdy występują błędy sprawdzania poprawności wykrywane po stronie klienta; drugi** `Create` **nigdy nie zostanie wywołana metoda**. Jeśli wyłączysz JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączona i HTTP POST `Create` wywołania metody `ModelState.IsValid` do sprawdzenia, czy ten film zawiera wszystkie błędy weryfikacji.

Możesz ustawić punkt przerwania w `HttpPost Create` metody i sprawdź, nigdy nie jest wywoływana metoda, weryfikacji po stronie klienta nie prześle dane formularza w przypadku wykrycia błędów sprawdzania poprawności. Jeśli można wyłączyć języka JavaScript w przeglądarce, a następnie Prześlij formularz z błędami, punkt przerwania zostanie osiągnięty. Będzie nadal się pojawiać pełna Walidacja bez kodu JavaScript. Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce FireFox.

![](adding-validation-to-the-model/_static/image5.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Poniżej znajduje się *Create.cshtml* Wyświetl szablon, którego szkielet we wcześniejszej części tego samouczka. Jest on używany przez metody akcji, zarówno powyżej początkowy formularz wyświetlania i wyświetlić ją ponownie w przypadku wystąpienia błędu.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Zwróć uwagę, jak kod używa `Html.EditorFor` pomocnika służący do wypełniania wyjściowego `<input>` elementu dla każdego `Movie` właściwości. Obok tego pomocnika jest wywołaniem `Html.ValidationMessageFor` metody pomocnika. Te dwie metody pomocnika pracować obiekt modelu, który jest przekazywany przez kontrolera do widoku (w tym przypadku `Movie` obiektu). Poszukaj one automatycznie atrybutów sprawdzania poprawności, określone w modelu i wyświetlanie komunikatów o błędach zgodnie z potrzebami.

Co to jest bardzo NAS cieszy się o to podejście jest, czy kontroler ani Utwórz szablon widoku nie wie, nic o regułach rzeczywista weryfikacja wymuszany ani o zbyt małą określone komunikaty o błędach wyświetlane. Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy. Te same zasady sprawdzania poprawności są automatycznie stosowane do widoku edycji i wszelkich innych widoków szablonach, które można utworzyć, które edytować model.

Jeśli chcesz zmienić logikę weryfikacji później, możesz to zrobić w dokładnie jednego miejsca przez dodanie atrybutów sprawdzania poprawności do modelu (w tym przykładzie `movie` klasy). Nie trzeba już martwić się o różnych części aplikacji jest niespójna z jak zasady są wymuszane — całą logikę weryfikacji będą zdefiniowane w jednym miejscu i użyć wszędzie. Zapewnia bardzo czystym kodzie i ułatwia utrzymanie i rozwój. I oznacza, że można będzie można w pełni zapewniane susz zasady.

## <a name="adding-formatting-to-the-movie-model"></a>Dodanie formatowania do modelu Movie

Otwórz *Movie.cs* plików i zbadaj `Movie` klasy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji. Firma Microsoft została już zastosowana [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) wartości wyliczenia, Data wydania i pola Cena. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Atrybuty nie są atrybutów sprawdzania poprawności, są one używane do Poinformuj aparat widoku w sposób renderowania kodu HTML. W powyższym przykładzie `DataType.Date` atrybut Wyświetla daty filmu jako tylko do daty bez godziny. Na przykład następująca [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutów nie sprawdzania poprawności formatu danych:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Atrybuty wymienione powyżej zapewniają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i podaj atrybutów, takich jak &lt;&gt; dla adresu URL i &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; do obsługi poczty e-mail. Możesz użyć [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybutu, aby sprawdzić poprawność formatu danych.

Innym sposobem przy użyciu `DataType` atrybutów, można jawnie ustawić [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) wartości. Poniższy kod pokazuje właściwości daty wydania z ciągiem formatu daty (to znaczy, &quot;d&quot;). Będzie to użyć, aby określić, że nie chcesz czas jako część daty wydania.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Pełne `Movie` klasy znajdują się poniżej.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera. Data wydania i ceny są dobrze sformatowane. Na poniższej ilustracji przedstawiono, Data wydania i cenę za pomocą &quot;fr-FR&quot; kultury.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Na poniższej ilustracji przedstawiono te same dane, które są wyświetlane przy użyciu domyślnej kultury (angielskie US).

![](adding-validation-to-the-model/_static/image8.png)

W następnej części serii, utworzymy aplikację i wprowadzić kilka ulepszeń do automatycznie generowanego `Details` i `Delete` metody.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-new-field-to-the-movie-model-and-table.md)
> [dalej](examining-the-details-and-delete-methods.md)
