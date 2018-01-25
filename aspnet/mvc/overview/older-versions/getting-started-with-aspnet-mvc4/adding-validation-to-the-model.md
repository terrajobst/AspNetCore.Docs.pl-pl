---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Dodawanie walidacji do modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 93b4df5fcbde8d87866d00dffda8a241d0dd596b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model"></a>Dodawanie walidacji do modelu
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tym w tej sekcji dodasz logikę weryfikacji `Movie` modelu, a będzie wymusić reguł sprawdzania poprawności w dowolnym momencie, użytkownik próbuje do tworzenia lub edytowania filmu przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Utrzymywanie suchej rzeczy

Jednym z podstawowych zasadach projektowania platformy ASP.NET MVC jest suchej (&quot;nie powtarzaj samodzielnie&quot;). ASP.NET MVC zachęca do określone funkcje lub działanie tylko raz, a następnie go wszędzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, które należy napisać i sprawia, że kod napisany mniej błędów podatnych na błędy i łatwiejsze w obsłudze.

Obsługa weryfikacji platformy ASP.NET MVC i Entity Framework Code First jest doskonałym przykładem suchej zasady w akcji. Reguły sprawdzania poprawności można określić deklaratywnie w jednym miejscu (w klasie modelu) i zasady są wymuszane wszędzie w aplikacji.

Oto jak możliwość korzystania z tej obsługi sprawdzania poprawności w aplikacji filmu.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji modelu film

Będzie rozpocząć, dodając logikę sprawdzania poprawności do `Movie` klasy.

Otwórz *Movie.cs* pliku. Dodaj `using` instrukcji w górnej części pliku, który odwołuje się do [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Zwróć uwagę, przestrzeń nazw nie zawiera `System.Web`. DataAnnotations zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, które można zastosować deklaratywnie do klasy lub właściwości.

Teraz zaktualizować `Movie` klasy, aby móc korzystać z wbudowanych [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), i [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutów sprawdzania poprawności . Użyć poniższego kodu, na przykład gdzie stosować atrybutów.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Uruchom aplikację i ponownie zostanie wyświetlony następujący błąd czasu wykonywania:

***Model kopii kontekstu "MovieDBContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First aktualizacji bazy danych ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Migracja zostanie wykorzystany do aktualizacji schematu. Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna i wprowadź następujące polecenia:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` pochodnej klasy o podanej nazwie (*AddDataAnnotationsMig*), a następnie w `Up` — metoda zostanie wyświetlony kod, który aktualizuje ograniczenia schematu. `Title` i `Genre` pola nie są już dopuszczające wartości zerowe (to znaczy, wprowadź wartość) i `Rating` pole ma maksymalną długość 5.

Atrybuty weryfikacji Określ zachowanie, które mają zostać wymuszone we właściwościach modelu, które są stosowane do. `Required` Atrybut wskazuje, że właściwość musi mieć wartość; w tym przykładzie filmu musi mieć wartości `Title`, `ReleaseDate`, `Genre`, i `Price` właściwości, aby był prawidłowy. `Range` Atrybut ogranicza wartość do określonego zakresu. `StringLength` Atrybut pozwala określić maksymalną długość ciągu właściwości oraz opcjonalnie długości minimalnej. Typy wewnętrzne (takich jak `decimal, int, float, DateTime`) są wymagane domyślnie i nie wymagają `Required` atrybutu.

Kod najpierw gwarantuje, że reguły sprawdzania poprawności, wybrane na klasę modelu są wymuszane, zanim aplikacja zapisuje zmiany w bazie danych. Na przykład poniższy kod spowoduje zgłoszenie wyjątku podczas `SaveChanges` metoda jest wywoływana, ponieważ niektóre wymagane `Movie` brakuje wartości właściwości i cena wynosi zero (która jest poza prawidłowym zakresem).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

O reguł sprawdzania poprawności, automatycznie są wymuszane przez program .NET Framework pomaga zwiększyć bezpieczeństwo aplikacji bardziej niezawodne. Gwarantuje również, że nie zapomnisz do sprawdzania poprawności coś i przypadkowo let złe dane do bazy danych.

W tym miejscu jest kompletny kod dla zaktualizowanego *Movie.cs* pliku:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Błąd sprawdzania poprawności interfejsu użytkownika na platformie ASP.NET MVC

Ponownie uruchom aplikację i przejdź do */Movies* adresu URL.

Kliknij przycisk **Utwórz nowy** łącze, aby dodać nowy filmu. Wypełnij formularz z niektóre nieprawidłowe wartości, a następnie kliknij przycisk **Utwórz** przycisku.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> w celu obsługi weryfikacji jQuery dla ustawień regionalnych innych niż angielskie, które użyj przecinka (&quot;,&quot;) dla dziesiętnego, należy wprowadzić *globalize.js* i konkretnej *cultures/globalize.cultures.js* pliku (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. Poniższy kod przedstawia zmiany w pliku Views\Movies\Edit.cshtml do pracy z &quot;fr-FR&quot; kultury:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Zwróć uwagę, jak formularz automatycznie został użyty kolorem czerwonym obramowaniem aby zaznaczyć wszystkie pola, które zawierają nieprawidłowe dane i ma wysyłanego odpowiedni komunikat o błędzie weryfikacji obok każdego z nich. Błędy są wymuszane zarówno po stronie klienta (przy użyciu języka JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma JavaScript wyłączone).

Rzeczywiste korzyści jest nie potrzebuję zmienić pojedynczy wiersz kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu umożliwienia tej weryfikacji interfejsu użytkownika. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierane up sprawdzania poprawności reguły, określona za pomocą atrybutów weryfikacji właściwości `Movie` klasa modelu.

Zwróć uwagę na właściwości `Title` i `Genre`, wymaganego atrybutu nie są wymuszane, dopóki nie można przesłać formularza (trafień **Utwórz** przycisk), lub wprowadź tekst do pola wejściowego i usunąć go. Dla pola które jest początkowo pusta (np. pola w widoku Create) i który ma wymaganego atrybutu i innych atrybutów sprawdzania poprawności, można wykonać następujące polecenie, aby wyzwolić sprawdzania poprawności:

1. Karta do pola.
2. Wprowadź tekst.
3. Karta wychodzących.
4. Karta wrócić do pola.
5. Usuń tekst.
6. Karta wychodzących.

Sekwencja powyżej wyzwoli wymaganej weryfikacji bez naciśnięcie przycisku Prześlij. Po prostu naciśnięcie przycisku Prześlij bez wprowadzania żadnego pola spowodują uruchomienie weryfikacji po stronie klienta. Dane nie są wysyłane do serwera, dopóki nie ma żadnych błędów weryfikacji po stronie klienta. Można to sprawdzić przez umieszczenie punktu przerwania w metodzie Post protokołu HTTP lub przy użyciu [narzędzie fiddler](http://fiddler2.com/fiddler2/) lub programu Internet Explorer 9 [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>W jaki sposób sprawdzanie poprawności jest wykonywane w tworzenia, wyświetlania i tworzenia metody akcji

Może zastanawiasz się, jak weryfikacji interfejsu użytkownika został wygenerowany bez żadnych aktualizacji do kodu w kontrolerze lub widoków. Zawiera listę dalej, co `Create` metod w `MovieController` wygląd klasy. Są one takie same jak w sposób tworzenia we wcześniejszej części tego samouczka.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Pierwszy (HTTP GET) `Create` formularza początkowego Utwórz Wyświetla metody akcji. Druga (`[HttpPost]`) wersja obsługuje post formularza. Drugi `Create` — metoda ( `HttpPost` wersji) wywołań `ModelState.IsValid` do sprawdzenia, czy film ma jakieś błędy sprawdzania poprawności. Wywołanie tej metody ocenia wszystkie atrybuty weryfikacji, które zostały zastosowane do tego obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` — metoda zostanie ponownie wyświetlony formularz. Jeśli nie ma żadnych błędów, metoda zapisuje nowe filmu w bazie danych. W naszym przykładzie filmu używamy **formularza nie jest opublikować na serwerze, gdy występują błędy sprawdzania poprawności wykryto po stronie klienta; druga** `Create` **nigdy wywoływana jest metoda**. Jeśli wyłączysz JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączony i HTTP POST `Create` wywołania metody `ModelState.IsValid` do sprawdzenia, czy film ma jakieś błędy sprawdzania poprawności.

Można ustawić punktu przerwania w `HttpPost Create` — metoda i sprawdź nigdy nie jest wywoływana metoda, weryfikacji po stronie klienta nie prześle dane formularza w przypadku wykrycia błędów sprawdzania poprawności. Jeśli musisz wyłączyć JavaScript w przeglądarce, a następnie przesłać formularza z błędami, nastąpi trafienie punktu przerwania. Nadal otrzymywać pełne sprawdzanie poprawności bez JavaScript. Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce FireFox.

![](adding-validation-to-the-model/_static/image5.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Poniżej znajduje się *Create.cshtml* Wyświetl szablon, który szkieletu wcześniej w samouczku. Jest on używany przez metody akcji pokazanym powyżej zarówno do wyświetlania formularza początkowego i wyświetl ją ponownie w przypadku wystąpienia błędu.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Zwróć uwagę, jak kod używa `Html.EditorFor` pomocnika do wyjściowego `<input>` elementu dla każdego `Movie` właściwości. Obok tego pomocnika jest wywołanie `Html.ValidationMessageFor` metody pomocnika. Te dwie metody pomocnika pracować z obiektu modelu, który jest przekazywany przez kontrolera do widoku (w tym przypadku `Movie` obiektu). Poszukaj one automatycznie sprawdzania poprawności atrybutów określonych dla modelu i wyświetlanie komunikatów o błędach zależnie od potrzeb.

Naprawdę nieuprzywilejowany o tej metody jest to, że kontroler ani Utwórz szablon widoku zna niczego dotyczące reguł rzeczywista weryfikacja wymuszany lub określone komunikaty o błędach wyświetlane. Reguły sprawdzania poprawności i ciągi błąd są określane tylko w `Movie` klasy. Te tej samej reguły sprawdzania poprawności automatycznie są stosowane do widoku edycji i wszelkie inne widoki szablonów, które można utworzyć które edytować model.

Jeśli chcesz później zmienić logikę weryfikacji, możesz to zrobić w dokładnie jednego miejsca przez dodanie atrybutów sprawdzania poprawności modelu (w tym przykładzie `movie` klasy). Nie musisz martwić się o różnych częściach aplikacji jest niespójna z jak zasady są wymuszane — całą logikę sprawdzania poprawności zostanie zdefiniowana w jednym miejscu i używany wszędzie. Przechowuje kod bardzo czystą i ułatwia utrzymanie i rozwijać. I oznacza, że będziesz w pełni ramach suchej zasady.

## <a name="adding-formatting-to-the-movie-model"></a>Dodanie formatowania do modelu film

Otwórz *Movie.cs* pliku i sprawdź, czy `Movie` klasy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Przestrzeń nazw zawiera atrybuty formatowania oprócz wbudowanych zestaw atrybutów weryfikacji. Zastosowaliśmy już [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) wartość wyliczenia Data wydania i pola cen. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Atrybuty nie są atrybutów sprawdzania poprawności, są one używane do Poinformuj aparat widoku w sposób renderowania kodu HTML. W powyższym przykładzie `DataType.Date` atrybutu Wyświetla daty filmu jako daty, bez czasu. Na przykład następująca [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutów nie sprawdzania poprawności formatu danych:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Atrybuty wymienione powyżej zapewniają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i podaj atrybutów, takich jak &lt;&gt; dla adresu URL i &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; do obsługi poczty e-mail. Można użyć [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybut do zweryfikowania formatu danych.

Informacje o innym podejściu do przy użyciu `DataType` atrybutów, można jawnie ustawić [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) wartości. Poniższy kod przedstawia właściwość Data wydania zawierające ciąg formatu daty (to znaczy, &quot;d&quot;). Spowoduje to użyć do określenia, że nie chcesz czas jako część Data wydania.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Pełną `Movie` klasy są wyświetlane poniżej.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Uruchom aplikację i przejdź do `Movies` kontrolera. Data wydania i ceny są dobrze sformatowane. Na poniższym obrazie pokazano Data wydania i cen za pomocą &quot;fr-FR&quot; kultury.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Na poniższym obrazie przedstawiono te same dane z domyślną kulturę (angielskie US).

![](adding-validation-to-the-model/_static/image8.png)

W następnej części serii, firma Microsoft będzie Przejrzyj aplikacji i poprawiają do automatycznie generowanego `Details` i `Delete` metody.

>[!div class="step-by-step"]
[Poprzednie](adding-a-new-field-to-the-movie-model-and-table.md)
[dalej](examining-the-details-and-delete-methods.md)
