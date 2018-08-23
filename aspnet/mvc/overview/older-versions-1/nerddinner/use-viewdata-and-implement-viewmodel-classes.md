---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Korzystanie z podejścia ViewData i Implementowanie klas ViewModel | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 6 przedstawia jak włączyć obsługę formularza bardziej zaawansowane edytowanie scenariuszy, a także w tym artykule omówiono dwie metody, które mogą służyć do przekazywania danych z kontrolerów do widoków:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 8df1ca30f8c0415b68d29eeaa0f7d83a606989ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753737"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Korzystanie z podejścia ViewData i Implementowanie klas ViewModel
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 6 z BEZPŁATNEJ [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Pokazuje krok 6, jak włączyć obsługę formularza bardziej zaawansowane edytowanie scenariuszy, a także w tym artykule omówiono dwie metody, które mogą służyć do przekazywania danych z kontrolerów do widoków: z podejścia ViewData i ViewModel.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner krok 6: Z podejścia ViewData i ViewModel

Firma Microsoft została wchodzi w zakres różnych scenariuszy post formularza i omówiono sposób implementacji tworzenia, aktualizacji i usuwania (CRUD) w pomocy technicznej. Teraz utworzymy podjęcie dalszych naszej implementacji DinnersController i włączyć obsługę formularza bardziej zaawansowane edytowanie scenariuszy. Podczas wykonywania tego omówimy dwie metody, które mogą służyć do przekazywania danych z kontrolerów do widoków: z podejścia ViewData i ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Przekazywanie danych z kontrolerów do widoku szablonów

Jedną z właściwości definiujące wzorzec MVC jest ścisłym "separacji" ułatwia wymuszanie między różnymi składnikami aplikacji. Modeli, widoków i kontrolerów każdego mają dobrze zdefiniowane role i obowiązki i komunikują się między sobą w sposób dobrze zdefiniowane. Dzięki temu promowania testowania i ponowne użycie kodu.

Klasa kontrolera zdecyduje się do renderowania odpowiedzi HTML z powrotem do klienta, jest odpowiedzialny za jawnie przekazywanie z szablonem widoku wszystkie dane wymagane do renderowania odpowiedzi. Wyświetl szablony nigdy nie należy wykonywać żadnych danych lub pobieranie aplikacji logiki — i zamiast tego należy ograniczyć samodzielnie, aby tylko kod renderowania, który jest wymuszany w modelu danych przekazanego przez kontroler.

Teraz modelu danych przekazywanych przez naszych DinnersController klasy do naszych szablonów widok jest prosty i proste — listę obiektów obiad w przypadku indeks() i obiad pojedynczy obiekt w przypadku Details(), Edit(), Create() i Delete(). Ponieważ nieustannie dodajemy więcej możliwości interfejsu użytkownika do naszej aplikacji są często użyjemy muszą podawać więcej niż tylko te dane do renderowania odpowiedzi HTML w ramach naszych szablonów widoku. Na przykład może być chcemy Zmień wartość pola "Kraj" w ramach naszych edycji i tworzyć widoki miałyby polu tekstowym HTML kontrolki dropdownlist. Zamiast trwale kodować listy rozwijanej kraj nazw Wyświetl szablon firma Microsoft może być wygenerowany na podstawie listy obsługiwane kraje, które wypełnimy dynamicznie. Firma Microsoft będzie należy sposób przekazania obiektu obiad *i* listę obsługiwanych krajów z kontrolera do naszych szablonów widoku.

Przyjrzyjmy się firma Microsoft będzie można to zrobić na dwa sposoby.

### <a name="using-the-viewdata-dictionary"></a>Za pomocą słownika ViewData

Klasa bazowa kontrolera udostępnia "ViewData" słownika właściwości, który może służyć do przekazania dodatkowych danych elementów z kontrolerów do widoków.

Na przykład aby móc obsługiwać scenariusz, w której chcemy to zmienić w polu tekstowym "Kraj", w ramach naszych widoku edycji miałyby polu tekstowym HTML kontrolki dropdownlist, można aktualizujemy nasze metody akcji Edit() do przekazania (oprócz obiektu obiad) obiekt SelectList, który może służyć jako m odelu dropdownlist krajach.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Konstruktor SelectList powyżej akceptuje listę hrabstwa do wypełniania listy downlist do z, a także obecnie wybranej wartości.

Firma Microsoft można zaktualizować szablon widoku Edit.aspx przy użyciu metody pomocnika Html.DropDownList() zamiast metody pomocnika Html.TextBox(), która była używana wcześniej:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Powyższej metody pomocnika Html.DropDownList() przyjmuje dwa parametry. Pierwszy jest nazwy elementu formularza HTML w danych wyjściowych. Drugim jest model "SelectList" przekazanych za pośrednictwem słownika ViewData. Użyto języka C# — "słowo kluczowe jako" rzutowanie typu w słowniku jako SelectList.

A teraz możemy uruchamiania naszych aplikacji i dostępu */Dinners/Edit/1* adresu URL w ramach przeglądarki zobaczymy, że naszych edycji interfejsu użytkownika został zaktualizowany do wyświetlania kontrolki dropdownlist krajach zamiast pole tekstowe:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Ponieważ renderować możemy również Edytuj szablon widoku z metody POST protokołu HTTP, Edytuj (w scenariuszach, w przypadku wystąpienia błędów), chcemy upewnić się, czy też aktualizujemy tę metodę, aby dodać SelectList ViewData, gdy szablon widoku jest wyświetlana w scenariuszach błąd:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

A teraz obsługuje kontrolki DropDownList w naszym scenariuszu edycji DinnersController.

### <a name="using-a-viewmodel-pattern"></a>Przy użyciu wzorca ViewModel

Słownik podejścia ViewData ma tę zaletę dość szybkie i łatwe do wdrożenia. Niektórzy deweloperzy nie podoba przy użyciu opartego na ciągach słowników, jednak ponieważ literówki może prowadzić do błędów, które nie zostanie przechwycony w czasie kompilacji. Niezaznaczone typizowanego słownika ViewData wymaga również za pomocą operatora "as" lub rzutowania w przypadku korzystania z językiem jednoznacznym takich jak C# w szablonie widoku.

Alternatywne podejście, które firma Microsoft może używać to jedna często nazywany wzorcem "ViewModel". Korzystając z tego wzorca, możemy utworzyć silnie typizowanych klas, które są zoptymalizowane pod kątem scenariuszy określonego widoku, a które udostępnianie właściwości dla zawartości dynamicznej, wartości/wymagane przez naszych szablonów widoku. Naszych zajęć kontrolera można wypełnić i przekazać te zoptymalizowane pod kątem widoku klasy do naszych Wyświetl szablon do użycia. Dzięki temu, bezpieczeństwo typów, sprawdzanie w czasie kompilacji i Edytor intellisense w szablonach widoku.

Na przykład, aby włączyć formularza obiad edycji scenariusze, możemy utworzyć "DinnerFormViewModel" klasy takie jak poniżej, który udostępnia dwa silnie typizowane właściwości: obiekt obiad i model SelectList potrzebnych do wypełnienia kontrolki dropdownlist krajach:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Firma Microsoft następnie zaktualizuj naszych Edit() metodę akcji w celu utworzenia DinnerFormViewModel za pomocą obiektu obiad, które możemy pobierać z repozytorium, a następnie przekazać go do szablonu widoku:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Wyślemy wiadomość, a następnie aktualizację naszej Wyświetl szablon, tak że oczekuje "DinnerFormViewModel" zamiast "Obiad" obiektu przez zmianę atrybutu "inherits" w górnej części strony edit.aspx w następujący sposób:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Po możemy to zrobić, aby odzwierciedlały model obiektu typu DinnerFormViewModel, które firma Microsoft jest przekazanie jej zostaną zaktualizowane funkcję intellisense właściwości "Model" w ramach naszych Wyświetl szablon:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Firma Microsoft jest następnie zaktualizuj naszego kodu widok do pracy z nich. Ogłoszenie poniżej jak firma Microsoft nie zmieniają się nazwami elementów wejściowych tworzymy (elementów formularza będzie nadal miała "Title", "Kraj") — ale będziemy aktualizować metody pomocnika kodu HTML do pobierania wartości przy użyciu klasy DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Zaktualizujemy również naszych edycji metody post, aby użyć klasy DinnerFormViewModel podczas renderowania błędów:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Aktualizacje mogą również naszych Create() metody akcji, ponownego używania dokładnie takie same *DinnerFormViewModel* klasy, aby włączyć krajów DropDownList w nich także. Poniżej przedstawiono implementację GET protokołu HTTP:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Poniżej przedstawiono implementację metody POST protokołu HTTP, Utwórz:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

I naszych Edit and Create ekrany obsługują teraz rozwijanych dla pobrania kraju.

### <a name="custom-shaped-viewmodel-classes"></a>W kształcie niestandardowych klas ViewModel

W powyższym scenariuszu klasy Nasze DinnerFormViewModel zapewnia bezpośredni dostęp obiekt modelu obiad jako właściwość, wraz z właściwością modelu SelectList pomocniczych. Ta metoda działa prawidłowo w scenariuszach, gdzie interfejsu użytkownika HTML, chcemy utworzyć w ramach naszych Wyświetl szablon odpowiada względnie ściśle naszych obiektami modelu domeny.

W scenariuszach, w której nie jest to tak, jedną z opcji, możesz użyć, jest do tworzenia klas ViewModel ukształtowane niestandardowy model obiektu, którego jest bardziej zoptymalizowane pod kątem do użycia przez widok — i może wyglądać całkowicie różni się od obiektu źródłowego modelu domeny. Na przykład może potencjalnie uwidaczniać inną właściwość nazwy i/lub agregacji właściwości zebranych z wielu obiektów modelu.

W kształcie niestandardowych klas ViewModel może być używany zarówno do przekazywania danych z kontrolerów z widokami renderowania, a także pomóc, obsługi danych formularza z powrotem do metody akcji kontrolera. W tym scenariuszu nowsze Niewykluczone, że metody akcji, aktualizowanie obiektu ViewModel danymi opublikowane w formularzu, a następnie użyć wystąpienia ViewModel mapowania lub pobrać obiekt modelu domeny rzeczywistych.

W kształcie niestandardowych klas ViewModel zapewniają dużą elastyczność i są do badania w dowolnym momencie możesz znaleźć kod renderowania w widoku szablonów lub kodu formularza ogłaszania wewnątrz metody akcji uruchamiania uzyskać zbyt skomplikowany. Jest to często logowania, że modeli domeny nie pozostawia żadnych śladów nie odnoszą się do interfejsu użytkownika jest generowany i ułatwiają klasa pośrednicząca ViewModel ukształtowane niestandardowe.

### <a name="next-step"></a>Następny krok

Teraz Spójrzmy na zastosowanie częściowych i stronami wzorcowymi ponowne używanie i udostępnianie interfejsu użytkownika w naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [dalej](re-use-ui-using-master-pages-and-partials.md)
