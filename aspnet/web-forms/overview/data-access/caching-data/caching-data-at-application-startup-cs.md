---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Buforowanie danych przy uruchamianiu aplikacji (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W dowolnej aplikacji sieci Web niektórych danych często posłuży, a niektóre dane będą rzadko używane. Firma Microsoft może poprawić wydajność naszych b aplikacji ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: c97058e5fd54dfd0393ec5ad020ad957d9719784
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635332"
---
<a name="caching-data-at-application-startup-c"></a>Buforowanie danych przy uruchamianiu aplikacji (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> W dowolnej aplikacji sieci Web niektórych danych często posłuży, a niektóre dane będą rzadko używane. Firma Microsoft może zwiększyć wydajność naszej aplikacji ASP.NET, ładując wcześniej często używane dane, to technika, znana jako. Ten samouczek przedstawia jedno z podejść do aktywnego ładowania, która umożliwia ładowanie danych do pamięci podręcznej podczas uruchamiania aplikacji.


## <a name="introduction"></a>Wprowadzenie

Dwóch poprzednich samouczkach przyjrzano się buforowanie danych w prezentacji i warstwy pamięci podręcznej. W [buforowanie danych za pomocą kontrolki ObjectDataSource](caching-data-with-the-objectdatasource-cs.md), przyjrzeliśmy się za pomocą funkcji buforowania ObjectDataSource do buforowania danych w warstwie prezentacji. [Buforowanie danych w architekturze](caching-data-in-the-architecture-cs.md) zbadane, buforowanie w nowy, oddzielny warstwy pamięci podręcznej. Obu tych samouczków używany *reaktywne ładowania* w pracy z pamięcią podręczną danych. Z ładowaniem reaktywne, każdym razem, gdy wymagane są dane, system najpierw sprawdza, czy jest w pamięci podręcznej. W przeciwnym razie go bierze danych ze źródłowego źródła, takich jak bazy danych, a następnie przechowuje je w pamięci podręcznej. Główną zaletą reaktywne ładowania jest łatwość implementacji. Jednym z jej wady jest realizowaniem nierównomierny żądań. Wyobraź sobie stronę, która używa warstwy pamięci podręcznej z poprzedniego samouczka, aby wyświetlić informacje o produkcie. Gdy ta strona po raz pierwszy odwiedzony lub odwiedzony po raz pierwszy po dane w pamięci podręcznej został wykluczony z powodu ograniczeń pamięci lub po upływie określonego osiągnięte, danych musi zostać pobrany z bazy danych. W związku z tym te żądania użytkowników będzie trwać dłużej niż żądań użytkowników, które mogą być obsługiwane przez pamięć podręczną.

*Aktywne ładowanie* zapewnia strategię zarządzania pamięcią podręczną w alternatywnej tego wygładza wydajność wszystkich żądań, ładując dane w pamięci podręcznej przed jest wymagana. Zazwyczaj aktywne ładowanie używa niektóre procesy, która okresowo sprawdza lub powiadomienie w przypadku, gdy został aktualizacji z danymi źródłowymi. Ten proces aktualizuje następnie pamięci podręcznej, aby utrzymać ją od nowa. Aktywne ładowanie jest szczególnie przydatne, jeśli dane pochodzą z połączenia z bazą danych powolne, usługi sieci Web lub z innego źródła danych szczególnie wolna. Jednak takie podejście do aktywnego ładowania jest trudniejsze do zaimplementowania, ponieważ wymaga ona tworzenia, zarządzania i wdrażania procesów do wyszukania zmian i aktualizowanie pamięci podręcznej.

Inną wersję aktywne ładowanie i typu, w których firma Microsoft będzie eksplorowania w ramach tego samouczka, trwa ładowanie danych w pamięci podręcznej podczas uruchamiania aplikacji. To podejście jest szczególnie przydatne w przypadku buforowania danych statycznych, takich jak rekordy z tabel odnośników bazy danych.

> [!NOTE]
> Więcej informacji na temat przyjrzeć różnice między proaktywne i reaktywne ładowanie, a także listę informatykami, wadach i zalecenia dotyczące wdrożenia, można znaleźć [Zarządzanie zawartość pamięci podręcznej](https://msdn.microsoft.com/library/ms978503.aspx) części [ Buforowanie w aplikacjach .NET Framework w Przewodniku dotyczącym architektury](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Krok 1: Określanie, jakie dane do pamięci podręcznej podczas uruchamiania aplikacji

Przykłady buforowania przy użyciu ładowania reaktywne zbadaliśmy poprzedniego pracy dwóch samouczki dobrze z danymi, które okresowo mogą ulec zmianie, a nie długo exorbitantly do wygenerowania. Ale jeśli nigdy nie zmieni się dane w pamięci podręcznej, po upływie posługują się ładowanie reaktywne jest zbędny. Podobnie jeśli dane są buforowane przyjmuje nadmiernie dużo czasu, aby wygenerować, następnie tych użytkowników, których żądania Znajdź pustą pamięć podręczna będzie musiał zmieścić długiego oczekiwania podczas danych bazowych są pobierane. Należy wziąć pod uwagę buforowanie danych statycznych i danych, która przyjmuje wyjątkowo dużo czasu, można wygenerować przy uruchamianiu aplikacji.

Gdy bazy danych mają wiele dynamiczny, często Zmienianie wartości większość również mieć ilość danych statycznych. Na przykład praktycznie wszystkich modeli danych może mieć jedną lub więcej kolumn zawierających określoną wartość z zakresu od stały zestaw opcji. A `Patients` tabeli bazy danych może być `PrimaryLanguage` kolumny, w których zestaw wartości może być język angielski, hiszpański, francuski, rosyjski, japoński i tak dalej. Często, następujące typy kolumn są implementowane za pomocą *tabel odnośników*. Zamiast przechowywania ciągu angielski lub francuski w `Patients` tabeli drugiej tabeli zostanie utworzone, zwykle dwóch kolumn — Unikatowy identyfikator i opis ciągu — przy użyciu rekordu dla wszystkich możliwych wartości. `PrimaryLanguage` Kolumny w `Patients` odpowiedni identyfikator unikatowy w tabeli są przechowywane w tabeli odnośników. Na rysunku 1 pacjenta Jan Nowak podstawowy język jest angielski, podczas Ed Johnson rosyjski.


![Tabela językach znajduje się tabela odnośników używana w tabeli pacjentów](caching-data-at-application-startup-cs/_static/image1.png)

**Rysunek 1**: `Languages` tabela znajduje się tabela odnośników używane przez `Patients` tabeli


Interfejs użytkownika do edytowania lub tworzenia nowego pacjenta obejmuje listę rozwijaną listę języków dopuszczalny rozmiar, wypełniony przez rekordów w `Languages` tabeli. Bez buforowania, każdym razem, ten interfejs jest odwiedzone system musi wykonać zapytanie `Languages` tabeli. Jest to marnotrawstwa i niepotrzebnych ponieważ wartości tabeli odnośników zmianie w bardzo rzadko, jeśli kiedykolwiek.

Firma Microsoft może w pamięci podręcznej `Languages` danych przy użyciu tych samych technik reaktywne ładowania badany w poprzednich samouczkach. Jednak reaktywne ładowania używa wygaśnięcia na podstawie czasu, który nie jest wymagany dla statycznych tabeli danych wyszukiwania. Podczas buforowania przy użyciu ładowania reaktywne byłoby lepsze niż Brak buforowania na wszystkich, najlepszym rozwiązaniem jest aktywnie ładowania danych tabeli wyszukiwania w pamięci podręcznej podczas uruchamiania aplikacji.

W tym samouczku przedstawiony zostanie sposób dane tabeli wyszukiwania w pamięci podręcznej i innych informacji statycznych.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Krok 2: Badanie różne sposoby dane z pamięci podręcznej

Informacje mogą być buforowane programowo w aplikacji ASP.NET przy użyciu różnych metod. Firma Microsoft ve już już, jak używać pamięci podręcznej danych w poprzednich samouczkach. Alternatywnie obiekty mogą być programowo buforowane przy użyciu *statyczne elementy członkowskie* lub *stan aplikacji*.

Podczas pracy z klasą, zazwyczaj należy najpierw można utworzyć wystąpienia klasy przed jej składowych jest możliwy. Na przykład aby można było wywołać metodę z jednej z klas w naszym warstwy logiki biznesowej, firma Microsoft musi najpierw utworzyć wystąpienia klasy:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Zanim firma Microsoft może wywołać *SomeMethod* i pracować z *SomeProperty*, firma Microsoft musi najpierw utworzyć wystąpienie klasy przy użyciu `new` — słowo kluczowe. *SomeMethod* i *SomeProperty* są skojarzone z określonym wystąpieniem. Okres istnienia tych elementów członkowskich jest powiązany z okresem istnienia ich skojarzonego obiektu. *Statyczne elementy członkowskie*, z drugiej strony są zmiennych, właściwości i metod, które są współużytkowane przez *wszystkich* wystąpienia klasy i w związku z tym, mają tak długo, jak klasa okres istnienia. Statyczne elementy członkowskie są oznaczone przez słowo kluczowe `static`.

Oprócz statycznych składowych danych mogą być buforowane, przy użyciu stanu aplikacji. Każda aplikacja ASP.NET obsługuje kolekcji nazwa/wartość, który jest współużytkowany przez wszystkich użytkowników i stron aplikacji. Ta kolekcja jest możliwy za pomocą [ `HttpContext` klasy](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)firmy [ `Application` właściwość](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)i użyty z komputera na stronie ASP.NET osobna klasa kodu w następujący sposób:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

Pamięć podręczna danych udostępnia znacznie bogatszy interfejs API do buforowania danych, dzięki czemu mechanizmów expiries na podstawie czasu i zależności, priorytetów element pamięci podręcznej i tak dalej. Statyczne elementy członkowskie i stan aplikacji takich funkcji, należy dodać ręcznie przez dewelopera strony. Buforowanie danych przy uruchamianiu aplikacji dla cyklu życia aplikacji, jednak zalety pamięci podręcznej danych są moot. W tym samouczku Zapoznamy się kod, który używa wszystkich trzech metod do buforowania danych statycznych.

## <a name="step-3-caching-thesupplierstable-data"></a>Krok 3: Buforowanie`Suppliers`tabeli danych

Northwind bazy danych tabel, firma Microsoft ve zaimplementowane data nie zawierają żadnych tabel odnośników tradycyjnych. Cztery DataTables zaimplementowany w naszej DAL wszystkie tabele modelu, w których wartości są niestatycznych. Zamiast spędzać czas, aby dodać nową tabelę DataTable warstwy DAL nowej klasy i metody służące do LOGIKI, w tym samouczku możemy po prostu poudawać, `Suppliers` dostosowanie danych z tabeli jest statyczna. W związku z tym firma Microsoft może buforować tych danych, podczas uruchamiania aplikacji.

Aby rozpocząć, Utwórz nową klasę o nazwie `StaticCache.cs` w `CL` folderu.


![Utwórz klasę StaticCache.cs w folderze CL](caching-data-at-application-startup-cs/_static/image2.png)

**Rysunek 2**: tworzenie `StaticCache.cs` klasy w `CL` folderu


Musimy dodać metodę, która służy do ładowania danych przy uruchamianiu w magazynie odpowiedniej pamięci podręcznej, a także metody, które zwracają dane z tej pamięci podręcznej.


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

Powyższy kod używa zmienną statyczną składową `suppliers`, do przechowywania wyników z `SuppliersBLL` klasy `GetSuppliers()` metody, która jest wywoływana z `LoadStaticCache()` metody. `LoadStaticCache()` Metoda jest przeznaczona do wywoływania podczas uruchamiania aplikacji. Po załadowaniu tych danych przy uruchamianiu aplikacji stronę, którą musi współpracować z dostawcą danych może wywołać `StaticCache` klasy `GetSuppliers()` metody. W związku z tym wywołanie do bazy danych w celu uzyskania dostawców odbywa się tylko raz, podczas uruchamiania aplikacji.

Zamiast używania zmienną statycznej składowej jako magazynu pamięci podręcznej, można też użyliśmy stan aplikacji lub pamięci podręcznej danych. Poniższy kod pokazuje klasy retooled do wartości stanu aplikacji:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

W `LoadStaticCache()`, informacji o dostawcy jest przechowywany w zmiennej aplikacji *klucz*. Jest zwracany jako odpowiedniego typu (`Northwind.SuppliersDataTable`) z `GetSuppliers()`. Gdy stan aplikacji jest możliwy w klasach związanym z kodem stron programu ASP.NET przy użyciu `Application["key"]`, w ramach architektury, trzeba użyć `HttpContext.Current.Application["key"]` Aby uzyskać bieżącą `HttpContext`.

Podobnie pamięć podręczną danych może służyć do przechowywania w pamięci podręcznej, co ilustruje poniższy kod:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Aby dodać element do pamięci podręcznej danych przez nieograniczony czas na podstawie czasu, należy użyć `System.Web.Caching.Cache.NoAbsoluteExpiration` i `System.Web.Caching.Cache.NoSlidingExpiration` wartości jako parametry wejściowe. Tego określonego przeciążenia pamięci podręcznej danych `Insert` została wybrana metoda, tak aby można było podać *priorytet* elementu pamięci podręcznej. Priorytet jest używany do określenia, jakie elementy, aby oczyścić z pamięci podręcznej, kiedy jest za mało dostępnej pamięci. W tym miejscu użyjemy priorytet `NotRemovable`, co zapewnia, że ten element pamięci podręcznej wygrał t oczyszczanie.

> [!NOTE]
> W tym samouczku, Pobierz implementuje `StaticCache` klasy przy użyciu podejścia zmiennej członka statycznego. Kod technik pamięci podręcznej stanu i danych aplikacji jest dostępna w komentarzach w pliku klasy.


## <a name="step-4-executing-code-at-application-startup"></a>Krok 4: Wykonywanie kodu przy uruchamianiu aplikacji

Aby wykonać kod, po pierwszym uruchomieniu aplikacji sieci web, należy utworzyć specjalny plik o nazwie `Global.asax`. Ten plik może zawierać programy obsługi zdarzeń dla aplikacji-, sesja- i zdarzeń na poziomie żądania i jest w tym miejscu gdzie można dodać kod, który będzie wykonywany przy każdym uruchomieniu aplikacji.

Dodaj `Global.asax` plik do katalogu głównego aplikacji sieci web, kliknij prawym przyciskiem myszy nazwę projektu witryny sieci Web w Eksploratorze rozwiązań programu Visual Studio i wybierając pozycję Dodaj nowy element. W oknie dialogowym Dodaj nowy element wybierz typ elementu globalna klasa aplikacji, a następnie kliknij przycisk Dodaj.

> [!NOTE]
> Jeśli masz już `Global.asax` plik w projekcie, globalna klasa aplikacji typu elementu nie będzie wyświetlane w oknie dialogowym Dodaj nowy element.


[![Dodaj plik Global.asax do katalogu głównego aplikacji sieci Web](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Rysunek 3**: Dodaj `Global.asax` plik w katalogu głównym Twojej aplikacji sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image5.png))


Wartość domyślna `Global.asax` szablon pliku zawiera pięć metod, w ramach po stronie serwera `<script>` tag:

- **`Application_Start`** wykonuje podczas pierwszego uruchomienia aplikacji sieci web
- **`Application_End`** jest uruchamiany, gdy dana aplikacja jest zamykana.
- **`Application_Error`** wykonywana zawsze, gdy wystąpił nieobsługiwany wyjątek dotrze do aplikacji
- **`Session_Start`** wykonuje, gdy zostanie utworzona nowa sesja
- **`Session_End`** jest uruchamiany, gdy wygasł lub porzucony sesji

`Application_Start` Program obsługi zdarzeń jest wywoływana tylko raz podczas cyklu życia aplikacji. Aplikacja rozpoczyna się po raz pierwszy zasobu ASP.NET żądania od aplikacji i będzie kontynuowane do czasu ponownego uruchomienia aplikacji, która może się zdarzyć, modyfikując zawartość `/Bin` folderu, modyfikując `Global.asax`, modyfikowania zawartość w `App_Code` folderu lub modyfikowanie `Web.config` pliku wśród innych przyczyn. Zapoznaj się [Przegląd cyklu życia aplikacji ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) bardziej szczegółowe omówienie dotyczące cyklu życia aplikacji.

Te samouczki tylko musimy dodać kod, aby `Application_Start` metody, więc możesz usunąć pozostałe. W `Application_Start`, wystarczy wywołać `StaticCache` klasy `LoadStaticCache()` metody, która będzie załadować i buforowania informacji o dostawcy:


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

To wszystko jest do niego! Przy uruchamianiu aplikacji `LoadStaticCache()` metoda pobrania informacji o dostawcy z LOGIKI i go przechowywać w zmiennej członka statycznego (lub niezależnie od pamięci podręcznej przechowywanie możesz zakończył się przy użyciu w `StaticCache` klasy). Aby sprawdzić to zachowanie, ustaw punkt przerwania `Application_Start` metody i uruchomić aplikację. Należy pamiętać, że punkt przerwania zostaje trafiony po uruchomieniu aplikacji. Kolejne żądania, jednak nie powodują `Application_Start` wykonać metodę.


[![Użyj punkt przerwania, aby Sprawdź, czy program obsługi zdarzeń Application_Start jest wykonywana.](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Rysunek 4**: Użyj punkt przerwania, aby upewnij się, `Application_Start` procedura obsługi zdarzeń jest wykonywane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> Jeśli nie zostanie osiągnięty `Application_Start` przerwania podczas uruchamiania debugowania, oznacza to, że aplikacja została już uruchomiona. Wymuś ponowne uruchomienie, modyfikując swoje `Global.asax` lub `Web.config` pliki, a następnie spróbuj ponownie. Można po prostu Dodaj (lub usuń) pusty wiersz na końcu jednego z tych plików, aby szybko uruchomić ponownie aplikację.


## <a name="step-5-displaying-the-cached-data"></a>Krok 5: Wyświetlanie danych pamięci podręcznej

W tym momencie `StaticCache` klasa ma wersję dostawcy danych, pamięci podręcznej podczas uruchamiania aplikacji, który jest możliwy za pośrednictwem jego `GetSuppliers()` metody. Aby pracować z danymi z warstwy prezentacji, możemy użyć kontrolki ObjectDataSource lub programowo wywołania `StaticCache` klasy `GetSuppliers()` metody z klasy CodeBehind strony ASP.NET. Przyjrzyjmy się za pomocą kontrolki ObjectDataSource i GridView do wyświetlania informacji o dostawcy pamięci podręcznej.

Zacznij od otwarcia `AtApplicationStartup.aspx` stronie `Caching` folderu. Przeciągnij GridView z przybornika w projektancie, ustawiając jego `ID` właściwość `Suppliers`. Następnie z tagu inteligentnego GridView zdecydować się na utworzenie nowego elementu ObjectDataSource, o nazwie `SuppliersCachedDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `StaticCache` klasy `GetSuppliers()` metody.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Rysunek 5**: Konfigurowanie kontrolki ObjectDataSource używać `StaticCache` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image11.png))


[![Pobieranie danych dostawcy pamięci podręcznej przy użyciu metody GetSuppliers()](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Rysunek 6**: Użyj `GetSuppliers()` metody do pobierania danych dostawcy pamięci podręcznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image14.png))


Po zakończeniu działania kreatora programu Visual Studio automatycznie doda BoundFields dla każdego pola danych w `SuppliersDataTable`. Z kontrolkami GridView i ObjectDataSource oznaczeniu deklaracyjnym powinien wyglądać podobnie do następującego:


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

Rysunek nr 7 przedstawia stronę po wyświetleniu za pośrednictwem przeglądarki. Dane wyjściowe są, taka sama miał możemy ściągnąć dane od LOGIKI `SuppliersBLL` klasy, ale przy użyciu `StaticCache` klasy zwraca dane dostawcy jako pamięci podręcznej podczas uruchamiania aplikacji. Możesz ustawić punkty przerwania w `StaticCache` klasy `GetSuppliers()` metodę, aby sprawdzić to zachowanie.


[![Dane dostawcy pamięci podręcznej są wyświetlane w widoku GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Rysunek 7**: pamięci podręcznej dostawcy danych jest wyświetlany w kontrolce GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>Podsumowanie

Większość każdy model danych zawiera ilość danych statycznych, zwykle implementowany w postaci tabel odnośników. Ponieważ te informacje są statyczne, nie ma powodu stale dostęp do bazy danych za każdym razem, że te informacje mają być wyświetlone. Ponadto ze względu na charakter statycznego, gdy buforowanie danych nie jest konieczne wygaśnięcia. W tym samouczku będziemy pokazaliśmy, jak przyjmują takie dane i buforowanie w pamięci podręcznej danych stanu aplikacji i za pośrednictwem zmiennej członka statycznego. Te informacje są buforowane przy uruchamianiu aplikacji i pozostaje w pamięci podręcznej w okresie istnienia aplikacji.

W tym samouczku i w ciągu ostatnich dwóch możemy ve przyjrzano się buforowanie danych w okresie istnienia aplikacji, a także przy użyciu expiries oparte na czasie. Podczas buforowania danych bazy danych, jednak na podstawie czasu wygaśnięcia może być mniej niż idealne rozwiązanie. Zamiast okresowo opróżniania pamięci podręcznej, jest optymalna do tylko wykluczać element pamięci podręcznej po zmodyfikowaniu podstawowych danych w bazie danych. Możliwe przy użyciu zależności pamięci podręcznej SQL, które zostaną omówione w naszym następnym samouczku jest to idealne rozwiązanie.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Zack Jones. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-in-the-architecture-cs.md)
> [dalej](using-sql-cache-dependencies-cs.md)
