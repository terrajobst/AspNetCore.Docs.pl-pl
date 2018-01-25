---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Dodawanie walidacji | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: b59965b2fab00cb64db06574d5ca3c6388daa7c8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation"></a>Dodawanie walidacji
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

W tym w tej sekcji dodasz logikę weryfikacji `Movie` modelu, a będzie wymusić reguł sprawdzania poprawności w dowolnym momencie, użytkownik próbuje do tworzenia lub edytowania filmu przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Utrzymywanie suchej rzeczy

Jednym z podstawowych zasadach projektowania platformy ASP.NET MVC jest [suchej](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;nie powtarzaj samodzielnie&quot;). ASP.NET MVC zachęca do określone funkcje lub działanie tylko raz, a następnie go wszędzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, które należy napisać i sprawia, że kod napisany mniej błędów podatnych na błędy i łatwiejsze w obsłudze.

Obsługa weryfikacji platformy ASP.NET MVC i Entity Framework Code First jest doskonałym przykładem suchej zasady w akcji. Reguły sprawdzania poprawności można określić deklaratywnie w jednym miejscu (w klasie modelu) i zasady są wymuszane wszędzie w aplikacji.

Oto jak możliwość korzystania z tej obsługi sprawdzania poprawności w aplikacji filmu.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji modelu film

Będzie rozpocząć, dodając logikę sprawdzania poprawności do `Movie` klasy.

Otwórz *Movie.cs* pliku. Powiadomienie [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) nie zawiera nazw `System.Web`. DataAnnotations zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, które można zastosować deklaratywnie do klasy lub właściwości. (Zawiera ona także formatowania atrybutów, takich jak [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) pomocy z formatowaniem i nie oferują żadnych sprawdzania poprawności.)

Teraz zaktualizować `Movie` klasy, aby móc korzystać z wbudowanych [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), i [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutów sprawdzania poprawności. Zastąp `Movie` klasy następującym kodem:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Atrybut Ustawia maksymalną długość ciągu i ustawia tego ograniczenia w bazie danych, w związku z tym spowoduje zmianę schematu bazy danych. Kliknij prawym przyciskiem myszy **filmy** tabeli w **Eksploratora serwera** i kliknij przycisk **Otwórz definicję tabeli**:

![](adding-validation/_static/image1.png)

Na powyższej ilustracji, można wyświetlić wszystkie pola ciągu są ustawione na [NVARCHAR (maks.)](https://technet.microsoft.com/library/ms186939.aspx). Migracja zostanie wykorzystany do aktualizacji schematu. Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenia:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` pochodnej klasy o podanej nazwie (`DataAnnotations`) i w `Up` metody można wyświetlić kod, który aktualizuje ograniczeń schematu:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Pola nie są już dopuszczające wartości zerowe (to znaczy, należy wprowadzić wartość). `Rating` Pole ma maksymalną długość 5 i `Title` ma maksymalną długość wynoszącą 60. Minimalna długość 3 na `Title` i zakres na `Price` nie może utworzyć zmiany schematu.

Sprawdź schemat film:

![](adding-validation/_static/image2.png)

W polach ciąg wyświetlane nowe limity długość i `Genre` nie jest zaznaczone jako wartości null.

Atrybuty weryfikacji Określ zachowanie, które mają zostać wymuszone we właściwościach modelu, które są stosowane do. `Required` i `MinimumLength` atrybuty wskazuje, że właściwość musi mieć wartość, ale nic nie uniemożliwia wprowadzanie biały znak do zaspokojenia tej weryfikacji przez użytkownika. [Wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybut służy do ograniczania znaków, które można wprowadzić. W powyższym kodzie `Genre` i `Rating` należy używać tylko liter (białe miejsca, cyfry i znaki specjalne są niedozwolone). [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Atrybut ogranicza wartość do określonego zakresu. `StringLength` Atrybut pozwala określić maksymalną długość ciągu właściwości oraz opcjonalnie długości minimalnej. Typy wartości (takie jak `decimal, int, float, DateTime`) są z założenia wymagane i nie wymagają `Required` atrybutu.

Kod najpierw gwarantuje, że reguły sprawdzania poprawności, wybrane na klasę modelu są wymuszane, zanim aplikacja zapisuje zmiany w bazie danych. Na przykład poniższy kod zgłosi [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) wyjątek podczas `SaveChanges` metoda jest wywoływana, ponieważ niektóre wymagane `Movie` brakuje wartości właściwości:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Powyższy kod zwraca następujący wyjątek:

*Weryfikacja nie powiodła się dla co najmniej jedna jednostka. Zobacz właściwości "EntityValidationErrors", aby uzyskać więcej informacji.*

O reguł sprawdzania poprawności, automatycznie są wymuszane przez program .NET Framework pomaga zwiększyć bezpieczeństwo aplikacji bardziej niezawodne. Gwarantuje również, że nie zapomnisz do sprawdzania poprawności coś i przypadkowo let złe dane do bazy danych.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Błąd sprawdzania poprawności interfejsu użytkownika na platformie ASP.NET MVC

Uruchom aplikację i przejdź do */Movies* adresu URL.

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu. Wypełnij formularz z niektórych z nieprawidłowymi wartościami. Jak weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> w celu obsługi weryfikacji jQuery dla ustawień regionalnych innych niż angielskie, które użyj przecinka (",") dla dziesiętnego, należy wprowadzić NuGet globalize, jak opisano wcześniej w tym samouczku.


Zwróć uwagę, jak formularz automatycznie został użyty kolorem czerwonym obramowaniem aby zaznaczyć wszystkie pola, które zawierają nieprawidłowe dane i ma wysyłanego odpowiedni komunikat o błędzie weryfikacji obok każdego z nich. Błędy są wymuszane zarówno po stronie klienta (przy użyciu języka JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma JavaScript wyłączone).

Rzeczywiste korzyści jest nie potrzebuję zmienić pojedynczy wiersz kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu umożliwienia tej weryfikacji interfejsu użytkownika. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierane up sprawdzania poprawności reguły, określona za pomocą atrybutów weryfikacji właściwości `Movie` klasa modelu. Test weryfikacji za pomocą `Edit` metody akcji, a tym samym sprawdzania poprawności jest stosowany.

Dane nie są wysyłane do serwera, dopóki nie ma żadnych błędów weryfikacji po stronie klienta. Można to sprawdzić, umieszczając punkt przerwania w metodzie HTTP Post, za pomocą [narzędzie fiddler](http://fiddler2.com/fiddler2/), lub IE [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>W jaki sposób sprawdzanie poprawności jest wykonywane w tworzenia, wyświetlania i tworzenia metody akcji

Może zastanawiasz się, jak weryfikacji interfejsu użytkownika został wygenerowany bez żadnych aktualizacji do kodu w kontrolerze lub widoków. Zawiera listę dalej, co `Create` metod w `MovieController` wygląd klasy. Są one takie same jak w sposób tworzenia we wcześniejszej części tego samouczka.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Pierwszy (HTTP GET) `Create` formularza początkowego Utwórz Wyświetla metody akcji. Druga (`[HttpPost]`) wersja obsługuje post formularza. Drugi `Create` — metoda ( `HttpPost` wersji) wywołań `ModelState.IsValid` do sprawdzenia, czy film ma jakieś błędy sprawdzania poprawności. Wywołanie tej metody ocenia wszystkie atrybuty weryfikacji, które zostały zastosowane do tego obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` — metoda zostanie ponownie wyświetlony formularz. Jeśli nie ma żadnych błędów, metoda zapisuje nowe filmu w bazie danych. W naszym przykładzie filmu **formularza nie jest opublikować na serwerze, gdy występują błędy sprawdzania poprawności wykryto po stronie klienta; druga** `Create` **nigdy wywoływana jest metoda**. Jeśli wyłączysz JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączony i HTTP POST `Create` wywołania metody `ModelState.IsValid` do sprawdzenia, czy film ma jakieś błędy sprawdzania poprawności.

Można ustawić punktu przerwania w `HttpPost Create` — metoda i sprawdź nigdy nie jest wywoływana metoda, weryfikacji po stronie klienta nie prześle dane formularza w przypadku wykrycia błędów sprawdzania poprawności. Jeśli musisz wyłączyć JavaScript w przeglądarce, a następnie przesłać formularza z błędami, nastąpi trafienie punktu przerwania. Nadal otrzymywać pełne sprawdzanie poprawności bez JavaScript. Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce FireFox.

![](adding-validation/_static/image7.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Chrome.

![](adding-validation/_static/image8.png)

Poniżej znajduje się *Create.cshtml* Wyświetl szablon, który szkieletu wcześniej w samouczku. Jest on używany przez metody akcji pokazanym powyżej zarówno do wyświetlania formularza początkowego i wyświetl ją ponownie w przypadku wystąpienia błędu.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Zwróć uwagę, jak kod używa `Html.EditorFor` pomocnika do wyjściowego `<input>` elementu dla każdego `Movie` właściwości. Obok tego pomocnika jest wywołanie `Html.ValidationMessageFor` metody pomocnika. Te dwie metody pomocnika pracować z obiektu modelu, który jest przekazywany przez kontrolera do widoku (w tym przypadku `Movie` obiektu). Poszukaj one automatycznie sprawdzania poprawności atrybutów określonych dla modelu i wyświetlanie komunikatów o błędach zależnie od potrzeb.

Co to jest naprawdę nieuprzywilejowany o tej metody oznacza, że żaden kontroler ani `Create` Wyświetl szablon zna niczego dotyczące reguł rzeczywista weryfikacja wymuszany lub określone komunikaty o błędach wyświetlane. Reguły sprawdzania poprawności i ciągi błąd są określane tylko w `Movie` klasy. Te tej samej reguły sprawdzania poprawności są automatycznie stosowane do `Edit` widoku i wszystkich innych widoków szablonów można tworzyć które edytować model.

Jeśli chcesz później zmienić logikę weryfikacji, możesz to zrobić w dokładnie jednego miejsca przez dodanie atrybutów sprawdzania poprawności modelu (w tym przykładzie `movie` klasy). Nie musisz martwić się o różnych częściach aplikacji jest niespójna z jak zasady są wymuszane — całą logikę sprawdzania poprawności zostanie zdefiniowana w jednym miejscu i używany wszędzie. Przechowuje kod bardzo czystą i ułatwia utrzymanie i rozwijać. I oznacza, że użytkownik będzie można całkowicie ramach *suchej* zasady.

## <a name="using-datatype-attributes"></a>Przy użyciu atrybutów typu danych

Otwórz *Movie.cs* pliku i sprawdź, czy `Movie` klasy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Przestrzeń nazw zawiera atrybuty formatowania oprócz wbudowanych zestaw atrybutów weryfikacji. Zastosowaliśmy już [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) wartość wyliczenia Data wydania i pola cen. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty zapewniają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i podaj atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail. Można użyć [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybut do zweryfikowania formatu danych. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych znajdują się one ***nie*** atrybutów sprawdzania poprawności. W takim przypadku tylko chcemy śledzić data nie Data i godzina. [Wyliczenie DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera wiele typów danych, takich jak *dat, czasu, numer telefonu, waluty, EmailAddress* i inne. `DataType` Atrybut można również włączyć aplikacji w celu umożliwienia automatycznie funkcji specyficznych dla typu. Na przykład `mailto:` można tworzyć łącza [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), i może zostać dostarczony selektora daty [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach obsługujących [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty emituje HTML 5 [danych -](http://ejohn.org/blog/html-5-data-attributes/) (Wymowa *kreska danych*) atrybutów, które byłyby zrozumiałe dla przeglądarki HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybutów nie dostarcza żadnych sprawdzania poprawności.

`DataType.Date`Określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atrybut służy do jawnie określić format daty:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Ustawienie określa, czy określony sposób formatowania powinny również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie może być który dla niektórych pól — na przykład dla wartości waluty, może nie ma symbolu waluty w polu tekstowym do edycji.)

Można użyć [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu przez sam, ale jest zwykle warto użyć [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również atrybutu. `DataType` Przekazuje atrybutu *semantyki* danych w przeciwieństwie do jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać z `DisplayFormat`:

- Przeglądarki, można włączyć funkcje HTML5 (na przykład pokazać formant kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, itp.).
- Domyślnie, przeglądarka wyświetli danych przy użyciu właściwego formatu na podstawie Twojej[ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut można włączyć MVC wybrać szablon pola prawo do renderowania danych ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Jeśli używany przez samego używa szablonu ciągu). Aby uzyskać więcej informacji, zobacz Brad Wilson[ASP.NET MVC 2 szablony](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Chociaż przeznaczone dla platformy MVC 2, w tym artykule nadal dotyczy bieżącej wersji programu ASP.NET MVC.)

Jeśli używasz `DataType` atrybutu z polem daty należy określić `DisplayFormat` atrybutu również w celu zapewnienia, że pole poprawnie renderuje przeglądarki Chrome. Aby uzyskać więcej informacji, zobacz [tego wątku StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> nie obsługuje weryfikacji jQuery[zakres](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutu i[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Na przykład następujący kod zawsze wyświetli błąd sprawdzania poprawności po stronie klienta, nawet wtedy, gdy data jest w określonym zakresie:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Należy wyłączyć sprawdzanie poprawności daty jQuery do użycia [zakres](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutem[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Zazwyczaj nie jest dobrym rozwiązaniem do skompilowania w modelach przy użyciu twardych daty[zakres](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutu i[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) jest niezalecane.


Poniższy kod przedstawia łączenie atrybutów w jednym wierszu:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

W następnej części serii, firma Microsoft będzie Przejrzyj aplikacji i poprawiają do automatycznie generowanego `Details` i `Delete` metody.

>[!div class="step-by-step"]
[Poprzednie](adding-a-new-field.md)
[dalej](examining-the-details-and-delete-methods.md)
