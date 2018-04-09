---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementowanie stronicowania danych wydajne | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 8 pokazano, jak dodać obsługę stronicowania do naszej /Dinners adres URL, tak aby zamiast 1000s kolacji jednocześnie, firma Microsoft będzie wyświetlane tylko 10 kolejnych kolacji w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="implement-efficient-data-paging"></a>Implementowanie stronicowania wydajne danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 8 bezpłatnych ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 8 pokazano, jak dodać obsługę stronicowania do naszej /Dinners adres URL, tak aby zamiast 1000s kolacji jednocześnie, firma Microsoft będzie tylko wyświetlić 10 kolacji nadchodzących naraz — i Zezwalaj na użytkowników końcowych do strony Wstecz i przekazywania przez całą listę w sposób przyjazną optymalizacji dla aparatów wyszukiwania.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner krok 8: Obsługę stronicowania

Jeśli naszej witrynie zakończy się pomyślnie, będzie mieć tysiące nadchodzących kolacji. Musimy upewnić się, że skaluje, aby zapewnić obsługę wszystkich tych kolacji naszego interfejsu użytkownika i umożliwia użytkownikom ich przeglądanie. Aby je włączyć, dodamy obsługę stronicowania do naszej */Dinners* adres URL tak to zamiast wyświetlanie 1000s kolacji na raz, firma Microsoft będzie tylko wyświetlić 10 kolacji nadchodzących naraz — i Zezwalaj na użytkowników końcowych do strony Wstecz i przekazywania przez całą listę w sposób przyjazną optymalizacji dla aparatów wyszukiwania.

### <a name="index-action-method-recap"></a>Drużyn metody akcji indeks()

Indeks() metody akcji w obrębie klasy Nasze DinnersController obecnie wygląda jak poniżej:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Po wysłaniu żądania do */Dinners* adres URL, powoduje pobranie listy wszystkich kolejnych kolacji, a następnie wyświetla listę wszystkich je:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Opis IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  jest interfejs, który został wprowadzony za pomocą LINQ jako część .NET 3.5. Umożliwia wydajne "wykonanie odroczone" scenariusze, które firma Microsoft może wykorzystać do zaimplementowania obsługę stronicowania.

W naszym DinnerRepository możemy zwracanie IQueryable&lt;obiad&gt; sekwencji z naszych metody FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Element IQueryable&lt;obiad&gt; obiektu zwróconego przez metodę naszych FindUpcomingDinners() hermetyzuje kwerendę, aby pobrać obiekty obiad z naszej bazie danych przy użyciu składnika LINQ to SQL. Ważne ten nie będzie uruchomiony zapytanie w bazie danych do czasu firma Microsoft próba dostępu/iteracja danych w zapytaniu lub nazywamy metody ToList() w nim. Opcjonalnie można dodać dodatkowe operacje/filtrów "łańcuchowa" do elementu IQueryable kodu wywołanie metody naszych FindUpcomingDinners()&lt;obiad&gt; obiektu przed wykonaniem kwerendy. LINQ do SQL jest następnie wykonania Scalonej zapytanie w bazie danych, gdy dane są żądane.

Do implementowania stronicowania logiki zaktualizowania metody akcji indeks() naszych DinnersController tak, aby dotyczył dodatkowe operatory "Pomiń" i "Zabierać" zwrócony IQueryable&lt;obiad&gt; sekwencji przed wywołaniem ToList() w niej:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Powyższy kod nakłada się na 10 pierwszych kolacji nadchodzących w bazie danych, a następnie zwraca wstecz kolacji 20. LINQ do SQL jest inteligentne do utworzenia zoptymalizowanego zapytanie SQL, które wykonuje to pomijanie logikę w bazie danych SQL —, a nie na serwerze sieci web. Oznacza to, że nawet jeśli mamy miliony nadchodzących kolacji w bazie danych tylko 10, którą chcemy udostępnić zostanie pobrany jako część tego żądania (dzięki czemu wydajność i skalowalność).

### <a name="adding-a-page-value-to-the-url"></a>Dodanie wartości "page" do adresu URL

Zamiast kodować określony zakres stron, chcemy zmieniliśmy adresy URL, aby uwzględnić parametr "page", który wskazuje zakres obiad, który żąda użytkownik.

#### <a name="using-a-querystring-value"></a>Przy użyciu wartości Querystring

Poniższy kod pokazuje, jak aktualizujemy nasze metody akcji indeks() obsługują parametr querystring i włączyć adresy URL, takie jak */Dinners? strony = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Metoda akcji indeks() powyżej ma parametr o nazwie "page". Parametr jest zadeklarowany jako liczba całkowita dopuszczające wartości zerowe (to jakie int? wskazuje). Oznacza to, że */Dinners? strony = 2* spowoduje, że wartość "2" do przekazania jako wartość parametru adresu URL. */Dinners* adresu URL (bez wartości querystring) spowoduje, że wartości null do przekazania.

Firma Microsoft są mnożenia wartości strony według rozmiaru strony (w tym przypadku 10 wierszy), aby określić, ile kolacji można pominąć. Używamy [C# null "" operatora łączącego (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) co jest przydatne podczas pracy nad typy dopuszczające wartości zerowe. Powyższy kod przypisuje strony wartość 0, jeśli parametr strony ma wartość null.

#### <a name="using-embedded-url-values"></a>Przy użyciu wartości osadzony adres URL

Zamiast wartości querystring byłoby osadzić parametr strony w obrębie samego rzeczywistego adresu. Na przykład: */Dinners/Page/2* lub */kolacji/2*. Platforma ASP.NET MVC zawiera zaawansowany aparat routingu adres URL, który ułatwia obsługi scenariuszy, takich jak to.

Firma Microsoft można zarejestrować tego mapy żadnych przychodzącego adresu URL lub adres URL w formacie do dowolnego kontrolera klasy lub metody akcji, którą chcemy niestandardowe reguły routingu. Wszystkie potrzebne do wykonania, należy otworzyć plik Global.asax w ramach naszych projektu:

![](implement-efficient-data-paging/_static/image2.png)

A następnie zarejestrować nową regułę mapowania za pomocą metody pomocnika MapRoute(), podobnie jak w pierwszym wywołaniu trasy. MapRoute() poniżej:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Firma Microsoft powyżej rejestracji nową regułę routingu o nazwie "UpcomingDinners". Firma Microsoft wskazujący, ma format adresu URL "kolacji/strona / {strony}" — gdy {page} jest wartość parametru osadzone w adresie URL. Trzeci parametr do metody MapRoute() wskazuje, że firma Microsoft powinien mapować adresy URL zgodne z tego formatu do metody akcji indeks() w klasie DinnersController.

Możemy użyć dokładnie tego samego kodu indeks() było przed w naszym scenariuszu Querystring — z wyjątkiem teraz naszych parametru "page" będą pochodzić z adresu URL i nie querystring:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

I teraz możemy uruchomić aplikację po wpisz */Dinners* zajmiemy się 10 pierwszych kolacji nadchodzących:

![](implement-efficient-data-paging/_static/image3.png)

Kiedy możemy typu w */Dinners/Page/1* zajmiemy się na następnej stronie kolacji:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Dodawanie Nawigacja strony interfejsu użytkownika

Ostatni krok, aby ukończyć scenariusz naszych stronicowania należy zaimplementować "dalej" i "poprzednie" nawigację interfejsu użytkownika w ramach naszych szablonu widoku, aby użytkownicy mogli łatwo pominąć danych obiad.

Poprawnej implementacji, potrzebujemy wiedzieć, łączna liczba kolacji w bazie danych, a także jak wiele stron danych przekłada się. Potrzebujemy następnie obliczanie, czy wartość aktualnie żądanego "page" jest na początku lub na końcu danych oraz wyświetlanie lub ukrywanie interfejsu użytkownika "poprzednich" i "dalej" odpowiednio. Firma Microsoft można zaimplementować tę logikę w naszym indeks() metody akcji. Można również dodać klasę pomocy, naszych projekt, który hermetyzuje tę logikę w sposób bardziej wielokrotnego użytku.

Poniżej znajduje się prostą klasę pomocnika "PaginatedList", która pochodzi z listy&lt;T&gt; klasy kolekcji wbudowane programu .NET Framework. Implementuje klasy wielokrotnego użytku kolekcji, która może służyć do dowolnej sekwencji IQueryable danych z podziałem na strony. W naszej aplikacji NerdDinner firma Microsoft będzie będzie on działać przez IQueryable&lt;obiad&gt; wyniki, ale może równie łatwo można użyć w odniesieniu do IQueryable&lt;produktu&gt; lub IQueryable&lt;klienta&gt;wyniki w innych scenariuszach aplikacji:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Zwróć uwagę, powyżej sposób obliczania, a następnie udostępnia właściwości, takie jak "PageIndex", "PageSize", "TotalCount" i "TotalPages". Przedstawia on także następnie dwie właściwości pomocnika "HasPreviousPage" i "HasNextPage", które wskazują, czy strony danych w kolekcji jest na początku lub na końcu oryginalnej sekwencji. Powyższy kod spowoduje, że dwa zapytania SQL do uruchomienia — najpierw pobrać liczba całkowita liczba obiektów obiad (to nie zwraca obiekty — zamiast wykonuje instrukcję "Wybierz licznik", która zwraca liczbę całkowitą), a drugi do pobierania tylko wiersze dane potrzebujemy z naszej bazy danych dla bieżącej strony danych.

Następnie zaktualizowania naszych metodę pomocnika DinnersController.Index() w celu utworzenia PaginatedList&lt;obiad&gt; z naszych DinnerRepository.FindUpcomingDinners() powoduje i przekaż go do naszego szablon widoku:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Firma Microsoft można zaktualizować szablon widoku \Views\Dinners\Index.aspx odziedziczone ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;obiad&gt; &gt; zamiast ViewPage&lt;IEnumerable&lt;Obiad&gt;&gt;, a następnie dodaj następujący kod do dołu widoku naszych szablonu, aby pokazać lub ukryć następny i poprzedni nawigacji interfejsu użytkownika:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Zwróć uwagę, nad jak używamy Html.RouteLink() metody pomocnika do generowania naszych hiperłącza. Ta metoda jest podobna do metody pomocnika Html.ActionLink(), które firma Microsoft używano wcześniej. Różnica polega na generowania adresu URL za pomocą "UpcomingDinners" routingu reguł, będziemy konfigurować w naszym pliku Global.asax. Dzięki temu, że firma Microsoft będzie generowania adresów URL do naszego indeks() metody akcji, który ma format: */Dinners/strona / {page}* — w przypadku, gdy wartość {strony} jest zmienną zapewniamy powyżej oparte na bieżącej PageIndex.

I teraz podczas możemy uruchomić aplikację ponownie zajmiemy się 10 kolacji naraz w naszej przeglądarki:

![](implement-efficient-data-paging/_static/image5.png)

Mamy także &lt; &lt; &lt; i &gt; &gt; &gt; nawigacji interfejsu użytkownika w dolnej części strony, który pozwala przejść do przodu i wstecz przez naszych danych przy użyciu wyszukiwania aparatu dostępne adresy URL:

![](implement-efficient-data-paging/_static/image6.png)

| **Temat po stronie: Opis skutków IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; jest bardzo przydatna funkcja, która umożliwia różnych scenariuszy, interesujące odroczonego wykonania (takich jak stronicowania i kompozycji na podstawie kwerendy). Jako wszystkie funkcje zaawansowane, możesz chcesz ostrożność przy sposobu ich używania i upewnij się, że nie jest użyte. Ważne jest, aby rozpoznać zwracanie IQueryable&lt;T&gt; wyników z repozytorium umożliwia kod wywołujący Dołącz metod łańcuchowa operatora i tak należeć do wykonywania zapytania ultimate. Jeśli nie chcesz podać kod wywołujący tę możliwość, a następnie powinien zwrócić kopii IList&lt;T&gt; lub IEnumerable&lt;T&gt; wyników — zawierających wyniki zapytania, które zostało już wykonane. W scenariuszach podział na strony wymagałoby to przekazywania danych rzeczywistych logiki podział na strony do wywołania metody repozytorium. W tym scenariuszu firma Microsoft może aktualizować nasze metody wyszukiwania FindUpcomingDinners() ma podpis, że zwrócony albo PaginatedList: PaginatedList&lt; obiad&gt; FindUpcomingDinners (int pageIndex, int pageSize) {} lub return kopii IList &lt;Obiad&gt;i użyj "totalCount" limit param, aby zwrócić łącznej liczby kolacji: IList&lt;obiad&gt; FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Następny krok

Teraz Przyjrzyjmy się jak możemy dodać obsługę uwierzytelniania i autoryzacji do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](re-use-ui-using-master-pages-and-partials.md)
> [dalej](secure-applications-using-authentication-and-authorization.md)
