---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Użyj ViewData i wdrożenie ViewModel klasy | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 6 przedstawia jak włączyć obsługę bardziej zaawansowane funkcje edycji scenariuszach formularza i omówiono również dwie metody, które mogą służyć do przekazywania danych z kontrolerów z widokami:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Użyj ViewData i wdrożenie ViewModel klasy
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 6 bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 6 przedstawia jak włączyć obsługę bardziej zaawansowane funkcje edycji scenariuszach formularza i omówiono również dwie metody, które mogą służyć do przekazywania danych z kontrolerów z widokami: ViewData i ViewModel.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner krok 6: ViewData i ViewModel

Wprowadzeniu pasuje do wielu operacji post formularza i opisano sposób wykonania tworzenia, aktualizacji i usuwania (CRUD) w pomocy technicznej. Firma Microsoft będzie teraz wykonywanie dodatkowych implementacja naszej DinnersController i włączyć obsługę bardziej zaawansowane funkcje edycji scenariuszy formularza. Podczas w ten sposób przedstawimy dwa podejścia, które mogą służyć do przekazywania danych z kontrolerów z widokami: ViewData i ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Przekazywanie danych z kontrolerów do widoku szablonów

Definiowanie właściwości wzorca MVC jest strict "separacji" pomaga wymuszania między poszczególnymi składnikami aplikacji. Modele, kontrolery i widoki każdej dobrze zdefiniowane role i obowiązki i komunikują się między sobą w sposób jasno określone. Pomaga to podwyższyć poziom kontroli i ponowne użycie kodu.

Klasy kontrolera decyduje o tym, do renderowania odpowiedzi HTML z powrotem do klienta, jest odpowiedzialny za jawnie przekazywanie do szablonu widoku wszystkie dane wymagane do renderowania odpowiedzi. Wyświetl szablony nigdy nie należy wykonywać żadnych danych lub pobieranie aplikacji logiki — i zamiast tego należy ograniczyć się znajdować renderowania kodu, który jest wymuszany wylogowuje modelu danych przekazanego przez kontroler.

W tej chwili modelu danych przekazywanych przez naszych DinnersController klasy naszym szablony widoku jest prosty i proste — listę obiektów obiad w przypadku indeks() oraz obiad pojedynczy obiekt w przypadku Details(), Edit(), Create() i Delete(). Jak możemy dodać więcej możliwości interfejsu użytkownika do naszej aplikacji, zamierzamy często muszą podawać więcej niż tylko te dane do renderowania odpowiedzi HTML w naszym szablony widoku. Na przykład może chcemy zmiany w polu "Kraju" w ramach naszych edycji i tworzyć widoki przed polu tekstowym HTML dropdownlist. Zamiast kodowane listy rozwijanej kraj nazw wyświetlanie szablonu chcemy może generować go z listy obsługiwanych krajach, które wypełnimy dynamicznie. Firma Microsoft będzie muszą mieć możliwość przekazania obiektu obiad *i* listę obsługiwanych krajach z kontrolera naszym szablony widoku.

Przyjrzyjmy się firma Microsoft będzie można to zrobić na dwa sposoby.

### <a name="using-the-viewdata-dictionary"></a>Za pomocą słownika ViewData

Klasa podstawowa kontrolera udostępnia właściwości słownika "ViewData", który może służyć do przekazywania danych dodatkowe elementy z kontrolerów z widokami.

Na przykład do obsługi scenariusza, w którym chcemy się zmienić pole tekstowe "Kraju" w ramach naszych widoku edycji przed polu tekstowym HTML dropdownlist, zaktualizowania naszych Edit() metody akcji do przekazania (oprócz obiekt obiad) obiektu SelectList, który może służyć jako m odelu dropdownlist krajach.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Konstruktor SelectList powyżej przyjmuje listę powiatów do wypełnienia listy downlist do z, a także obecnie wybranej wartości.

Następnie mogą zaktualizować naszych Edit.aspx wyświetlanie szablonu przy użyciu metody pomocnika Html.DropDownList() zamiast metody pomocnika Html.TextBox() użyliśmy wcześniej:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Metoda pomocnika Html.DropDownList() powyżej przyjmuje dwa parametry. Pierwsza to nazwa elementu formularza HTML do danych wyjściowych. Drugim jest przekazane za pomocą słownika ViewData modelu "SelectList". Użyto C# — "słowo kluczowe jako" można rzutować typu w słowniku jako SelectList.

I teraz możemy uruchomienia naszych aplikacji i dostępu */Dinners/Edit/1* adres URL w naszej przeglądarki zajmiemy się tym, że nasze edycji interfejsu użytkownika został zaktualizowany do wyświetlenia dropdownlist krajów zamiast pole tekstowe:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Ponieważ firma Microsoft również renderowania Edytuj szablon widoku z metody POST protokołu HTTP Edytuj (w scenariuszach, w przypadku wystąpienia błędów), chcemy upewnij się, że firma Microsoft również zaktualizować tę metodę, aby dodać SelectList do ViewData podczas renderowania szablonu widoku w scenariuszach błąd:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

I w naszym scenariuszu edycji DinnersController obsługuje teraz DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Przy użyciu wzorca ViewModel

Metody słownika ViewData jest zaletą jest dość szybki i łatwy do zaimplementowania. Niektórzy deweloperzy nie odpowiada za pomocą oparte na ciągach słowników, ponieważ literówki może prowadzić do błędów, które nie zostanie przechwycony w czasie kompilacji. Wyrażeniami bez typu słownika ViewData wymaga również za pomocą operatora "as" lub Rzutowanie, korzystając z języka jednoznacznie takich jak C# w szablonie widoku.

Alternatywne metody, która może używamy jest jednym często określany jako wzorzec "ViewModel". Korzystając z tego wzorca utworzymy jednoznacznie klas, które są zoptymalizowane pod kątem scenariuszami określony widok, a które udostępniają właściwości dla zawartości dynamicznej, wartości/wymagane przez naszym szablony widoku. Nasze klasy kontrolera można wypełnić i przekazać te klasy zoptymalizowanych pod kątem widoku do naszej Wyświetl szablon do użycia. Umożliwia to zabezpieczenie typów, sprawdzanie kompilacji i Edytor intellisense w widoku szablonów.

Na przykład, aby włączyć formularz obiad edycji scenariusze można utworzyć "DinnerFormViewModel" klas takich jak poniżej, który udostępnia dwa jednoznacznie właściwości: obiekt obiad oraz model SelectList potrzebnych do wypełnienia dropdownlist krajach:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Firma Microsoft następnie zaktualizuj naszych Edit() metodę akcji w celu utworzenia DinnerFormViewModel przy użyciu obiektu obiad, do którego możemy pobrać z naszym repozytorium, a następnie przekaż go do naszego szablon widoku:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Wyślemy wiadomość, a następnie aktualizacji naszych Wyświetl szablon, którego nie oczekuje "DinnerFormViewModel" zamiast "Obiad" obiekt zmieniając atrybut "inherits" w górnej części strony edit.aspx w następujący sposób:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Po możemy to zrobić, intellisense właściwości "Modelu" w szablonie naszych widoku zostaną zaktualizowane do uwzględnienia model obiektów typu DinnerFormViewModel, które firma Microsoft jest przekazanie jej przez:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Firma Microsoft następnie aktualizować naszego kodu widoku do pracy wylogowuje go. Uwaga poniżej sposób firma Microsoft nie zmienia nazwy elementów wejściowych tworzymy (elementów formularza będzie nadal nosić nazwy "Title", "Kraju") — ale aktualizujemy metody pomocnika kodu HTML do pobierania wartości przy użyciu klasy DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Ponadto będziemy informować naszych edycji metody post do użycia klasy DinnerFormViewModel podczas renderowania błędów:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Aktualizacje mogą również naszych Create() metod akcji ponownego używania dokładnie takie same *DinnerFormViewModel* klasę, aby włączyć krajów DropDownList w ramach tych również. Poniżej znajdują się implementacja GET protokołu HTTP:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Poniżej znajdują się implementacja metody POST protokołu HTTP, Utwórz:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Teraz naszych edycji i tworzenie ekranów obsługę oraz rozwijanych pobrania kraju.

### <a name="custom-shaped-viewmodel-classes"></a>W kształcie niestandardowej klasy ViewModel

W powyższym scenariuszu klasy Nasze DinnerFormViewModel bezpośrednio udostępnia obiekt modelu obiad jako właściwość, wraz z właściwością modelu SelectList obsługi. Ta metoda działa prawidłowo w scenariuszach, w którym interfejsu użytkownika HTML chcemy, aby utworzyć w naszym Wyświetl szablon odpowiada stosunkowo ściśle naszych obiektami modelu domeny.

W scenariuszach, w którym nie jest przypadek, jedną opcję, która służy jest utworzenie klasy ViewModel w kształcie niestandardowy model obiektów, których więcej zoptymalizowana pod kątem użycia przez widok — i może wyglądać całkowicie różni się od źródłowego obiektu modelu domeny. Na przykład może potencjalnie spowodować narażenie inną właściwość nazwy i/lub agregacji właściwości zebranych z wielu obiektów modelu.

W kształcie niestandardowej klasy ViewModel mogą być używane zarówno do przekazywania danych z kontrolerów z widokami do renderowania, które dodatkowo ułatwią obsługę danych formularza z powrotem do metody akcji kontrolera. W tym scenariuszu nowsze może być aktualizowanie obiektu ViewModel zaksięgowany formularza danymi, a następnie użyć wystąpienia ViewModel mapujące lub pobrać obiekt modelu domeny rzeczywiste metody akcji.

W kształcie niestandardowej klasy ViewModel zapewniają dużą elastyczność i są coś do sprawdzania, czy możesz znaleźć kodu renderowania w szablonach widoku lub kod księgowej formularza wewnątrz metody akcji uruchamiania uzyskanie zbyt skomplikowane, za każdym razem. Jest to często logowania, że modeli domeny prawidłowo nie odnoszą się do interfejsu użytkownika jest generowany i ułatwiają klasa pośrednicząca ViewModel w kształcie niestandardowe.

### <a name="next-step"></a>Następny krok

Teraz Przyjrzyjmy się jak możemy użyć częściowe i stron wzorcowych do ponownego użycia i udostępniać interfejsu użytkownika w naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [dalej](re-use-ui-using-master-pages-and-partials.md)
