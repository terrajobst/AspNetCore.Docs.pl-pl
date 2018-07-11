---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Dodawanie sprawdzania poprawności | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 97526d908dd3b835040453b53eae76f733a01c04
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38172196"
---
<a name="adding-validation"></a>Dodawanie walidacji
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji dodasz logikę walidacji do `Movie` modelu, a będzie upewnij się, że reguły sprawdzania poprawności są wymuszane ilekroć użytkownik próbuje utworzyć lub edytować film przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Utrzymywanie susz rzeczy

Jednym z podstawowych zasadach projektowania platformy ASP.NET MVC jest [susz](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;nie Powtórz samodzielnie&quot;). ASP.NET MVC zachęca można określić funkcji lub zachowanie tylko raz, a następnie go wszędzie, gdzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, który należy napisać i sprawia, że kod, który pisanie mniej błędów, podatne i łatwiejsze w utrzymaniu.

Obsługa weryfikacji platformy ASP.NET MVC i Entity Framework Code First to świetny przykład susz zasady w akcji. Można deklaratywne określenie reguł sprawdzania poprawności w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie, gdzie w aplikacji.

Oto jak możesz korzystać z zalet tej obsługi weryfikacji w aplikacji filmu.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawania reguł sprawdzania poprawności do modelu Movie

Rozpocznie się przez dodanie niektórych logikę walidacji do `Movie` klasy.

Otwórz *Movie.cs* pliku. Zwróć uwagę [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw nie zawiera `System.Web`. DataAnnotations zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, które są stosowane w sposób deklaratywny do dowolnej klasy lub właściwości. (Zawiera także formatowania atrybutów, takich jak [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , ułatwić formatowanie i nie udostępniamy żadnych sprawdzania poprawności.)

Teraz zaktualizować `Movie` klasy, aby skorzystać z wbudowanych [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), i [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutów sprawdzania poprawności. Zastąp `Movie` klasy następującym kodem:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Atrybut Ustawia maksymalną długość ciągu i ustawia to ograniczenie w bazie danych, w związku z tym schemat bazy danych ulegnie zmianie. Kliknij prawym przyciskiem myszy **filmy** tabelę **Eksploratora serwera** i kliknij przycisk **Otwórz definicję tabeli**:

![](adding-validation/_static/image1.png)

Na powyższej ilustracji możesz zobaczyć wszystkie pola ciągów są ustawione na [NVARCHAR (MAKSYMALNIE)](https://technet.microsoft.com/library/ms186939.aspx). Firma Microsoft użyje migracji do zaktualizowania schematu. Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenia:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` Klasa pochodna o podanej nazwie (`DataAnnotations`), a następnie w `Up` metody zostanie wyświetlony kod, który aktualizuje ograniczenia schematu:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Pole nie jest już dopuszcza wartości null (oznacza to, wprowadź wartość). `Rating` Pole ma maksymalną długość 5 i `Title` może się składać maksymalnie 60. Minimalna długość 3 na `Title` i zakres na `Price` nie utworzyła zmiany schematu.

Zapoznać się ze schematem film:

![](adding-validation/_static/image2.png)

Pola ciągów Pokaż nowe limity długości i `Genre` już nie jest zaznaczone jako dopuszczającego wartość null.

Atrybuty weryfikacji określić zachowanie, które mają zostać wymuszone we właściwościach modelu, które są stosowane względem. `Required` i `MinimumLength` atrybuty wskazuje, że właściwość musi mieć wartość, ale nic nie uniemożliwia użytkownikowi wprowadzanie odstępów do zaspokojenia tej weryfikacji. [Wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybut jest używany do ograniczania znaków, które można danych wejściowych. W powyższym kodzie `Genre` i `Rating` należy używać tylko liter (białe miejsca, cyfry i znaki specjalne są niedozwolone). [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Atrybut ogranicza wartości do określonego zakresu. `StringLength` Atrybut pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie długości minimalnej. Typy wartości (takie jak `decimal, int, float, DateTime`) są założenia wymagane i nie ma potrzeby `Required` atrybutu.

Kod najpierw gwarantuje, że reguł sprawdzania poprawności, które określisz w klasie modelu są wymuszane, zanim aplikacja zapisuje zmiany w bazie danych. Na przykład, poniższy kod będzie zgłaszać wyjątek [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) wyjątek podczas `SaveChanges` metoda jest wywoływana, ponieważ niektóre wymagane `Movie` brakuje wartości właściwości:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Powyższy kod następujący wyjątek:

*Weryfikacja nie powiodła się dla co najmniej jednej jednostki. Zobacz właściwość "EntityValidationErrors", aby uzyskać więcej informacji.*

Posiadanie reguły sprawdzania poprawności, które automatycznie wymuszanych przez program .NET Framework ułatwia zapewnienie aplikacji bardziej niezawodne. Gwarantuje również, że nie pamiętasz do sprawdzania poprawności coś i przypadkowo umożliwiają złe dane do bazy danych.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Błąd sprawdzania poprawności UI we wzorcu ASP.NET MVC

Uruchom aplikację, a następnie przejdź do */Movies* adresu URL.

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy film. Wypełnij formularz z niektórych z nieprawidłowymi wartościami. Jak najszybciej po weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> do obsługi dotyczącą weryfikacji jQuery dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego, należy wprowadzić NuGet sprzedawać, jak opisano wcześniej w tym samouczku.


Zwróć uwagę, jak formularz został automatycznie umożliwia kolorem czerwonym obramowaniem Wyróżnij tekst zawiera nieprawidłowe dane, które ma wysyłanego komunikatu o błędzie weryfikacji odpowiednich obok każdej z nich. Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma Obsługa skryptów JavaScript wyłączona).

Korzyści z rzeczywistych jest, nie należy zmieniać jednego wiersza kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu włączenia tej weryfikacji interfejsu użytkownika. Kontrolera i widoki utworzone wcześniej w tym samouczku automatycznie wybrany w górę sprawdzania poprawności reguły określona za pomocą atrybutów weryfikacji właściwości `Movie` klasa modelu. Walidacja testu za pomocą `Edit` metody akcji i tego samego sprawdzania poprawności jest stosowana.

Dane nie są wysyłane do serwera, aż nie wystąpią żadne błędy weryfikacji po stronie klienta. Można to sprawdzić, umieszczając punkt przerwania w metodzie Post protokołu HTTP przy użyciu [narzędzie fiddler](http://fiddler2.com/fiddler2/), lub IE [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>W jaki sposób weryfikacji odbywa się w tworzenie wyświetlanie i Tworzenie metody akcji

Być może zastanawiasz się, jak sprawdzanie poprawności UI został wygenerowany bez wykonywania żadnych aktualizacji do kodu w kontrolerze lub widoków. Dalej prezentuje co `Create` metody `MovieController` jak wyglądają klasy. Są one w porównaniu z jak utworzone wcześniej w tym samouczku.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Pierwszy (HTTP GET) `Create` metody akcji Wyświetla początkowej formularza tworzenia. Drugi (`[HttpPost]`) wersja obsługuje post formularza. Drugi `Create` — metoda ( `HttpPost` wersji) wywołań `ModelState.IsValid` do sprawdzenia, czy ten film zawiera wszystkie błędy weryfikacji. Wywołanie tej metody ocenia wszelkie atrybuty weryfikacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` metoda ponownie zostanie wyświetlony formularz. Jeśli nie ma żadnych błędów, metoda zapisuje ten nowy film w bazie danych. W naszym przykładzie filmu **nie opublikowania formularza z serwerem, gdy występują błędy sprawdzania poprawności wykrywane po stronie klienta; drugi** `Create` **nigdy nie zostanie wywołana metoda**. Jeśli wyłączysz JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączona i HTTP POST `Create` wywołania metody `ModelState.IsValid` do sprawdzenia, czy ten film zawiera wszystkie błędy weryfikacji.

Możesz ustawić punkt przerwania w `HttpPost Create` metody i sprawdź, nigdy nie jest wywoływana metoda, weryfikacji po stronie klienta nie prześle dane formularza w przypadku wykrycia błędów sprawdzania poprawności. Jeśli można wyłączyć języka JavaScript w przeglądarce, a następnie Prześlij formularz z błędami, punkt przerwania zostanie osiągnięty. Będzie nadal się pojawiać pełna Walidacja bez kodu JavaScript. Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce FireFox.

![](adding-validation/_static/image7.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Chrome.

![](adding-validation/_static/image8.png)

Poniżej znajduje się *Create.cshtml* Wyświetl szablon, którego szkielet we wcześniejszej części tego samouczka. Jest on używany przez metody akcji, zarówno powyżej początkowy formularz wyświetlania i wyświetlić ją ponownie w przypadku wystąpienia błędu.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Zwróć uwagę, jak kod używa `Html.EditorFor` pomocnika służący do wypełniania wyjściowego `<input>` elementu dla każdego `Movie` właściwości. Obok tego pomocnika jest wywołaniem `Html.ValidationMessageFor` metody pomocnika. Te dwie metody pomocnika pracować obiekt modelu, który jest przekazywany przez kontrolera do widoku (w tym przypadku `Movie` obiektu). Poszukaj one automatycznie atrybutów sprawdzania poprawności, określone w modelu i wyświetlanie komunikatów o błędach zgodnie z potrzebami.

Co to jest bardzo NAS cieszy się o tego podejścia jest to, że żaden kontroler ani `Create` Wyświetl szablon wie, nic o regułach rzeczywista weryfikacja wymuszany ani o zbyt małą określone komunikaty o błędach wyświetlane. Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy. Te same zasady sprawdzania poprawności są automatycznie stosowane do `Edit` widoku i wszystkich innych widoków szablonów można utworzyć, które edytować modelu.

Jeśli chcesz zmienić logikę weryfikacji później, możesz to zrobić w dokładnie jednego miejsca przez dodanie atrybutów sprawdzania poprawności do modelu (w tym przykładzie `movie` klasy). Nie trzeba już martwić się o różnych części aplikacji jest niespójna z jak zasady są wymuszane — całą logikę weryfikacji będą zdefiniowane w jednym miejscu i użyć wszędzie. Zapewnia bardzo czystym kodzie i ułatwia utrzymanie i rozwój. Oznacza to, że użytkownik będzie można w pełni zapewniane *susz* zasady.

## <a name="using-datatype-attributes"></a>Przy użyciu atrybutów typu danych

Otwórz *Movie.cs* plików i zbadaj `Movie` klasy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji. Firma Microsoft została już zastosowana [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) wartości wyliczenia, Data wydania i pola Cena. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty zawierają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i podaj atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail. Możesz użyć [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybutu, aby sprawdzić poprawność formatu danych. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych znajdują się one ***nie*** atrybutów sprawdzania poprawności. W tym przypadku ma być uruchamiany tylko do śledzenia daty, nie daty i godziny. [Wyliczenie typu danych](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera dla wielu typów danych, takich jak *daty, godziny, numer telefonu, waluty, EmailAddress* itd. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji. Na przykład `mailto:` łącza mogą być tworzone dla [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), i można podać selektora daty [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach obsługujących [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty emituje HTML 5 [danych -](http://ejohn.org/blog/html-5-data-attributes/) (Wymowa *dash danych*) atrybutów, które może zrozumieć przeglądarki HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty nie są oferowane wszystkich sprawdzania poprawności.

`DataType.Date` Określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atrybut jest używany jawnie określić format daty:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Ustawienie określa, czy określony sposób formatowania powinien również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie może być, w przypadku niektórych pól — na przykład dla wartości waluty może nie ma symbolu waluty, w polu tekstowym do edycji.)

Możesz użyć [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu przez sam, ale zazwyczaj jest dobry pomysł, aby użyć [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również atrybutu. `DataType` Atrybutu powoduje *semantyki* danych tak jak w przeciwieństwie do sposobu renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą `DisplayFormat`:

- Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, itp.).
- Domyślnie, przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o swoje [ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybutu aby umożliwić MVC wybrać szablon po prawej stronie pola w celu przedstawienia tych danych ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Jeśli używany przez samego korzysta z szablonu ciągu). Aby uzyskać więcej informacji, zobacz Brad Wilson [szablony programu ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Chociaż napisane dla platformy MVC 2, w tym artykule nadal obowiązuje ograniczenie do bieżącej wersji platformy ASP.NET MVC.)

Jeśli używasz `DataType` atrybutu z polem daty należy określić `DisplayFormat` atrybut również w celu zapewnienia, że pole poprawnie renderowana w przeglądarkach Chrome. Aby uzyskać więcej informacji, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> nie obsługuje dotyczącą weryfikacji jQuery [zakres](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutu i [daty/godziny](https://msdn.microsoft.com/library/system.datetime.aspx). Na przykład poniższy kod zawsze będzie wyświetlała błąd weryfikacji po stronie klienta, nawet wtedy, gdy jest to data mieści się w określonym zakresie:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Należy wyłączyć sprawdzanie poprawności Data jQuery, aby użyć [zakres](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutem [daty/godziny](https://msdn.microsoft.com/library/system.datetime.aspx). Ogólnie nie jest dobrą praktyką jest kompilowanie twardych dat w ramach modeli za pomocą [zakres](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutu i [daty/godziny](https://msdn.microsoft.com/library/system.datetime.aspx) jest niezalecane.


Poniższy kod pokazuje atrybuty łączenie w jednym wierszu:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

W następnej części serii, utworzymy aplikację i wprowadzić kilka ulepszeń do automatycznie generowanego `Details` i `Delete` metody.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-new-field.md)
> [dalej](examining-the-details-and-delete-methods.md)
