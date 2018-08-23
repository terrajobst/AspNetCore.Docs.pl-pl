---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementowanie wydajnego stronicowania danych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 8 pokazuje, jak dodać obsługę stronicowania do adresu URL /Dinners tak, aby zamiast 1000s kolacji jednocześnie, firma Microsoft będzie wyświetlane tylko 10 kolejnych kolacji na...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2bef690355cd1f89a15a67f0c49775296d551136
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755758"
---
<a name="implement-efficient-data-paging"></a>Implementowanie wydajnego stronicowania danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 8 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 8 pokazuje, jak dodać obsługę stronicowania do adresu URL /Dinners, tak aby zamiast 1000s kolacji jednocześnie, firma Microsoft będzie wyświetlić tylko 10 kolejnych kolacji naraz — oraz umożliwia użytkownikom końcowym na stronie Wstecz i przekazywać je za pośrednictwem całą listę, w sposób przyjazna optymalizacji dla aparatów wyszukiwania.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner krok 8: Obsługa stronicowania

Naszą witrynę zakończy się pomyślnie, będzie miał tysiące nadchodzących kolacji. Musimy upewnić się, że jest skalowana tak, aby zapewnić obsługę wszystkich tych kolacji naszego interfejsu użytkownika i pozwala użytkownikom na przeglądanie ich. Aby to umożliwić, dodamy obsługę stronicowania do naszych */Dinners* adres URL tak, to zamiast wyświetlanie 1000s kolacji na raz, firma Microsoft będzie wyświetlić tylko 10 kolejnych kolacji naraz — oraz umożliwia użytkownikom końcowym na stronie Wstecz i przekazywać je za pośrednictwem całą listę w sposób przyjazna optymalizacji dla aparatów wyszukiwania.

### <a name="index-action-method-recap"></a>Podsumowanie metody akcji indeks()

Metody akcji indeks() w ramach naszych klasy DinnersController obecnie wygląda jak poniżej:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Po wysłaniu żądania do */Dinners* adresu URL, umożliwia pobranie listy wszystkich przyszłych kolacji, a następnie powoduje wyświetlenie listy wszystkich z nich się:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Opis IQuerable&lt;T&gt;

*Element IQueryable&lt;T&gt;*  jest interfejsem, która została wprowadzona za pomocą LINQ jako część .NET 3.5. Umożliwia zaawansowane "wykonanie odroczone" scenariusze, które będziemy korzystać z zalet zaimplementować obsługę stronicowania.

W naszym DinnerRepository firma Microsoft jest zwracany element IQueryable&lt;obiad&gt; sekwencji z naszych FindUpcomingDinners() metody:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Element IQueryable&lt;obiad&gt; zwracany przez metodę naszych FindUpcomingDinners() hermetyzuje zapytanie, aby pobrać obiekty obiad z naszej bazie danych za pomocą LINQ to SQL. Co ważniejsze go nie wykonaj zapytanie względem bazy danych do momentu kolejna próba dostępu/Iterowanie po danych w zapytaniu, lub możemy wywołać metodę ToList() na nim. Kod, wywołanie metody naszych FindUpcomingDinners() można opcjonalnie dodać operacje/filtry "łańcuchowych" do elementu IQueryable&lt;obiad&gt; obiektu przed wykonaniem kwerendy. LINQ do SQL jest następnie inteligentne, można wykonać połączone zapytanie w bazie danych, jeśli wymagane są dane.

Aby zaimplementować logikę stronicowania zaktualizowania metody akcji indeks() naszych DinnersController tak, że dotyczy on dodatkowych operatorów "Pomiń" i "Take", aby zwrócony element IQueryable&lt;obiad&gt; sekwencji przed wywołaniem ToList() na nim:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Powyższy kod nakłada się na 10 pierwszych kolacji nadchodzących w bazie danych, a następnie zwraca ponownie kolacji 20. LINQ do SQL jest inteligentnych do konstruowania zoptymalizowane zapytania SQL, który wykonuje to pomijanie logiki w usłudze SQL database — a nie na serwerze sieci web. Oznacza to, że nawet, jeśli mamy milionów nadchodzących kolacji w bazie danych tylko 10, którą chcemy będą pobierane jako część tego żądania (tworząc z niej wydajność i skalowalność).

### <a name="adding-a-page-value-to-the-url"></a>Dodanie wartości "page" do adresu URL

Zamiast kodować określony zakres stron, firma Microsoft zachodnich stanach USA adresy URL obejmujący parametr "page", który wskazuje zakres obiad, który żąda użytkownika.

#### <a name="using-a-querystring-value"></a>Przy użyciu wartości Querystring

Poniższy kod demonstruje, jak firma Microsoft aktualizuje naszych metody akcji indeks() obsługuje parametr querystring i włączyć adresy URL, takich jak */Dinners? strony = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Metody akcji indeks() powyżej ma parametr o nazwie "page". Parametr jest zadeklarowany jako liczbę całkowitą typu dopuszczającego wartość null (to, jakie int? oznacza). Oznacza to, że */Dinners? strony = 2* adres URL spowoduje, że wartość "2", które zostaną przekazane jako wartość parametru. */Dinners* adresu URL (bez wartości querystring) spowoduje, że wartość null, które zostaną przekazane.

Firma Microsoft są pomnożenie wartości strony przez rozmiar strony (w tym przypadku 10 wierszy), aby określić, ile kolacji można pominąć. Używamy [C# o wartości null "" operatora łączącego (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) co jest przydatne podczas rozwiązywania problemów związanych z typami zerowalnymi. Powyższy kod przypisuje strony wartość 0, jeśli parametr strony ma wartość null.

#### <a name="using-embedded-url-values"></a>Przy użyciu wartości adresu URL osadzonego

Alternatywa dla użycia wartości querystring byłoby osadzić parametr strony w ramach rzeczywistego adresu sam. Na przykład: */Dinners/Page/2* lub */kolacji/2*. Platforma ASP.NET MVC zawiera zaawansowany aparat routingu adresów URL, ułatwiająca do obsługi scenariuszy, takich jak to.

Firma Microsoft można zarejestrować niestandardowe reguły routingu mapowane wszystkie przychodzące adres URL lub adres URL formatu dowolnego kontrolera klasy lub metody akcji, którą chcemy. Wszystkie potrzebne do wykonania jest, aby otworzyć plik Global.asax w projekcie:

![](implement-efficient-data-paging/_static/image2.png)

A następnie zarejestruj nową regułę mapowania, przy użyciu metody pomocnika MapRoute(), takich jak pierwsze wywołanie do trasy. MapRoute() poniżej:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Firma Microsoft powyżej rejestrowania nową regułę routingu o nazwie "UpcomingDinners". Firma Microsoft wskazujący, ma format adresu URL "kolacji/strony / {strona}" — gdzie {strony} jest wartością parametru osadzone w adresie URL. Trzeci parametr do metody MapRoute() wskazuje, że firma Microsoft adresów URL, które pasują do tego formatu do metody akcji indeks() w klasie DinnersController powinny być mapowane.

Możemy użyć dokładnie tego samego kodu indeks(), który mieliśmy przed z naszym scenariuszu Querystring — z wyjątkiem teraz naszych parametru "page" będą pochodzić z adresu URL i nie querystring:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Teraz po abyśmy uruchomić aplikację i wpisać */Dinners* zobaczymy 10 pierwszych kolacji nadchodzących:

![](implement-efficient-data-paging/_static/image3.png)

A gdy wpiszesz w */Dinners/Page/1* zobaczymy następnej strony kolacji:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Dodawanie nawigacji na stronie interfejsu użytkownika

Ostatni krok, aby ukończyć scenariusz naszych stronicowania będzie do zaimplementowania "dalej" i "starszych" interfejs użytkownika nawigacji w ramach naszych Wyświetl szablon, aby użytkownicy mogli łatwo pominąć danych obiad.

Poprawnej implementacji, musimy wiedzieć, całkowita liczba kolacji w bazie danych oraz jak wiele stron danych przekłada się to. Następnie należy obliczyć, czy wartość aktualnie żądanego "page" jest na początku lub na końcu danych i pokazać lub ukryć interfejsu użytkownika "poprzedni" i "dalej", odpowiednio. Firma Microsoft zaimplementować tę logikę w ramach naszych indeks() metody akcji. Można również możemy dodać klasę pomocy do naszego projektu, który hermetyzuje tę logikę w sposób bardziej wielokrotnego użytku.

Poniżej przedstawiono prosty "PaginatedList" pomocnikiem klasy, który pochodzi z listy&lt;T&gt; klasy kolekcji wbudowana w .NET Framework. Implementuje klasy wielokrotnego użytku kolekcji, która może służyć do dowolnej sekwencji danych IQueryable podzielony na strony. W naszej aplikacji NerdDinner odpowiemy od jego pracy nad IQueryable&lt;obiad&gt; wyniki, ale może równie łatwo zostać użyty dla elementu IQueryable&lt;produktu&gt; lub IQueryable&lt;klienta&gt;wyniki w innych scenariuszach aplikacji:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Zwróć uwagę, powyżej sposób obliczania, a następnie udostępnia właściwości, takie jak "PageIndex", "PageSize", "TotalCount" i "TotalPages". Udostępnia ona także dwie właściwości pomocnika "HasPreviousPage" i "HasNextPage", które wskazują, czy strony danych w kolekcji jest na początku lub końcu oryginalnej sekwencji. Powyższy kod spowoduje, że dwa zapytania SQL w celu uruchomienia — pierwszy można pobrać liczba całkowita liczba obiektów obiad (to nie zwraca obiekty — zamiast wykonuje instrukcję "Wybierz licznik", która zwraca liczbę całkowitą), a druga, aby pobrać tylko wiersze dane, których potrzebujemy w naszej bazie danych dla bieżącej strony danych.

Firma Microsoft może uaktualnić naszych metodą pomocnika DinnersController.Index() tworzenia PaginatedList&lt;obiad&gt; z naszych DinnerRepository.FindUpcomingDinners() wyniku i przekazać go do szablonu widoku:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Firma Microsoft można zaktualizować szablon widoku \Views\Dinners\Index.aspx odziedziczone ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;obiad&gt; &gt; zamiast ViewPage&lt;IEnumerable&lt;Obiad&gt;&gt;, a następnie dodaj następujący kod do dolnej części naszych szablon widoku, aby pokazać lub ukryć następnej i poprzedniej nawigacji interfejsu użytkownika:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Zwróć uwagę na powyżej sposób używania Html.RouteLink() metody pomocnika do generowania naszych hiperłącza. Ta metoda jest podobna do metody pomocniczej Html.ActionLink(), którą firma Microsoft była używana wcześniej. Różnica polega na tym, że firma Microsoft generuje adres URL, za pomocą "UpcomingDinners" routing regułę, że będziemy konfigurować w ramach naszego pliku Global.asax. Daje to gwarancję, że polega na wygenerowaniu adresy URL do naszego indeks() metody akcji, która ma format: */Dinners/strona / {strony}* — w przypadku, gdy wartość {strony} jest zmienną, zapewniamy powyżej oparte na bieżącym PageIndex.

Teraz gdy Uruchamiamy naszą aplikację ponownie teraz poczekamy kolacji 10 w danym momencie w przeglądarki:

![](implement-efficient-data-paging/_static/image5.png)

Mamy także &lt; &lt; &lt; i &gt; &gt; &gt; nawigacji interfejsu użytkownika w dolnej części strony, która umożliwia nam przejść do przodu i wstecz za pośrednictwem naszych danych za pomocą wyszukiwania aparatu dostępne adresy URL:

![](implement-efficient-data-paging/_static/image6.png)

| **Temat po stronie: Zrozumienia konsekwencji IQueryable&lt;T&gt;** |
| --- |
| Element IQueryable&lt;T&gt; to ogromne funkcja, która pozwala korzystać z wielu ciekawych scenariuszy odroczonego wykonania (takich jak stronicowania i kompozycji oparte na zapytaniach). Jako ze wszystkich zaawansowanych funkcji, chcesz należy zachować ostrożność, sposobu ich używania i upewnij się, że nie jest użyte. Ważne jest, aby rozpoznać, zwracając element IQueryable&lt;T&gt; wyników z repozytorium umożliwia kod wywołujący Dołącz na metodach łańcuchowych operator, a więc należą do wykonywania zapytania ultimate. Jeśli nie chcesz podać kod wywołujący tę możliwość, a następnie powinna zwrócić kopię IList&lt;T&gt; lub typ IEnumerable&lt;T&gt; wyniki - zawierające wyniki zapytania, która została już wykonana. W scenariuszach z podziałem na strony wymagałoby to umożliwia wypychanie logiki dzielenia na strony rzeczywiste dane do wywołania metody repozytorium. W tym scenariuszu firma Microsoft może aktualizować naszych metodę wyszukiwania FindUpcomingDinners() mają podpis, czy zwrócone albo PaginatedList: PaginatedList&lt; obiad&gt; FindUpcomingDinners (int pageIndex, int pageSize) {} i zwrócenia wstecz IList &lt;Obiad&gt;i użyj "totalCount" out param, aby zwrócić łączna liczba kolacji: IList&lt;obiad&gt; FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Następny krok

Teraz Przyjrzyjmy się jak możemy dodać obsługę uwierzytelniania i autoryzacji do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](re-use-ui-using-master-pages-and-partials.md)
> [dalej](secure-applications-using-authentication-and-authorization.md)
