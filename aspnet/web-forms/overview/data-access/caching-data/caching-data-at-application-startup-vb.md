---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Buforowanie danych przy uruchamianiu aplikacji (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W dowolnej aplikacji sieci Web niektórych danych będzie często używane, a niektóre dane będą rzadko używane. Firma Microsoft może poprawić wydajność naszych b aplikacji ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: f8f322dae89480fc7ed5586d7f8eeb4c67d7839f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876348"
---
<a name="caching-data-at-application-startup-vb"></a>Buforowanie danych przy uruchamianiu aplikacji (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> W dowolnej aplikacji sieci Web niektórych danych będzie często używane, a niektóre dane będą rzadko używane. Możemy ulepszyć wydajność aplikacji ASP.NET przez wcześniej podczas ładowania danych często używane techniki nazywanej. W tym samouczku przedstawiono jeden ze sposobów aktywne ładowanie, która umożliwia ładowanie danych do pamięci podręcznej podczas uruchamiania aplikacji.


## <a name="introduction"></a>Wprowadzenie

Dwa poprzednie samouczki przeglądał buforowania danych w prezentacji i buforowanie warstwy. W [buforowanie danych z elementu ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), analizujemy przy użyciu s ObjectDataSource buforowanie funkcje do pamięci podręcznej danych w warstwie prezentacji. [Buforowanie danych w architekturze](caching-data-in-the-architecture-vb.md) zbadane, buforowanie na nowe, oddzielne buforowanie warstwie. Oba te samouczki używane *reaktywne ładowania* w pracy z pamięci podręcznej danych. Z reaktywne, każdy czasu ładowania danych jest wymagana, system najpierw sprawdza, czy jego s w pamięci podręcznej. W przeciwnym razie go grabs danych ze źródła pochodzenia, takie jak bazy danych, a następnie przechowuje je w pamięci podręcznej. Główną zaletą reaktywne obciążenia jest łatwość wdrażania. Jeden z jego wady jest jej wydajność nierówna żądań. Wyobraź sobie strona, która używa warstwa buforowanie z poprzednim samouczek, aby wyświetlić informacje o produkcie. Gdy ta strona jest odwiedzi po raz pierwszy, lub że odwiedzono po raz pierwszy po buforowane dane został wykluczony z powodu ograniczeń pamięci lub po upływie określonego osiągnięte, dane muszą pobierane z bazy danych. W związku z tym te żądania użytkowników będzie trwało dłużej niż żądania użytkowników, które mogą być przekazywane przez pamięć podręczną.

*Aktywne ładowanie* zapewnia strategii zarządzania alternatywnych pamięci podręcznej tego wygładza wydajność żądań przez ładowania pamięci podręcznej danych przed nią s potrzebne. Zazwyczaj aktywne ładowanie używa niektórych procesu, który okresowo sprawdza albo jest powiadomienie, gdy został aktualizacji w danych źródłowych. Następnie ten proces aktualizuje pamięci podręcznej, aby zapewnić jego nowa. Aktywne ładowanie jest szczególnie przydatne, gdy dane pochodzą z połączenia z bazą danych powolne, usługi sieci Web lub z innego źródła danych szczególnie wolna. Jednak takie podejście do aktywnego ładowania jest trudniejsze do zaimplementowania, ponieważ wymaga ona tworzenia, zarządzania i wdrażania procesu, aby sprawdzić zmiany i zaktualizować pamięci podręcznej.

Inną wersję aktywne ładowanie i typ, który firma Microsoft będzie Eksplorowanie w tym samouczku ładuje dane w pamięci podręcznej podczas uruchamiania aplikacji. Ta metoda jest szczególnie przydatna w przypadku buforowania danych statycznych, takich jak rekordy z bazy danych wyszukiwania tabel.

> [!NOTE]
> Więcej informacji na temat przyjrzeć różnice między proaktywne i reaktywne ładowania, a także listę zalet, wad i zalecenia dotyczące wdrażania, można znaleźć w temacie [Zarządzanie zawartość pamięci podręcznej](https://msdn.microsoft.com/library/ms978503.aspx) części [ Buforowanie w aplikacjach .NET Framework w Przewodniku dotyczącym architektury](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Krok 1: Określanie danych do pamięci podręcznej przy uruchamianiu aplikacji

Przykłady buforowania, przy użyciu reaktywne ładowania, możemy badane w wcześniej pracy dwóch samouczki również z danymi, które okresowo mogą ulec zmianie i nie długo exorbitantly do wygenerowania. Ale jeśli nigdy nie zmieniają się dane pamięci podręcznej, wygaśnięcia używane przez ładowanie reaktywne jest zbędny. Podobnie jeśli dane są buforowane przyjmuje nadmiernie długi czas, aby wygenerować, następnie tych użytkowników, których żądania znaleźć pusta pamięci podręcznej będzie musiał generowane podczas długich oczekiwania podczas danych zostanie pobrana. Należy wziąć pod uwagę buforowanie danych statycznych i dane, które przyjmuje wyjątkowo dużo czasu, aby wygenerować podczas uruchamiania aplikacji.

Chociaż baz danych w wielu dynamiczny, często Zmienianie wartości większość ma także odpowiedniej ilości danych statycznych. Na przykład niemal wszystkich modeli danych ma co najmniej jedną kolumnę, zawierające określonej wartości z stałego zestawu opcji. A `Patients` tabeli bazy danych może być `PrimaryLanguage` kolumny, w których zestaw wartości może być angielski, hiszpański, francuski, rosyjski, japońskim i tak dalej. Często, te typy kolumn są implementowane za pomocą *tabele odnośników*. Zamiast przechowywania ciągu angielskim i francuskim w `Patients` tabeli drugiej tabeli zostanie utworzone, często, dwóch kolumn — Unikatowy identyfikator i opis ciągu - z rekordem wszystkich możliwych wartości. `PrimaryLanguage` Kolumny w `Patients` odpowiedniego Unikatowy identyfikator w tabeli są przechowywane w tabeli odnośników. Na rysunku 1 pacjenta Jan Nowak s podstawowy język jest angielski, zablokowaniu rosyjski Ed Johnson s.


![Tabela języków znajduje się tabela odnośników w tabeli pacjentów używana](caching-data-at-application-startup-vb/_static/image1.png)

**Rysunek 1**: `Languages` tabela znajduje się tabela odnośników używana przez `Patients` tabeli


Interfejs użytkownika do edytowania lub tworzenia nowego pacjenta to listy rozwijanej, dopuszczalny języków wypełnienia rekordy w `Languages` tabeli. Bez buforowania, zawsze ten interfejs jest odwiedzi systemu należy zbadać `Languages` tabeli. Jest to niepotrzebne i niepotrzebne ponieważ wartości tabeli odnośników zmienić bardzo rzadko, jeśli kiedykolwiek.

Firma Microsoft może buforować `Languages` danych przy użyciu tych samych metod ładowania reaktywne w poprzednim samouczki. Jednak reaktywne ładowania używa ważności na podstawie czasu, który nie jest wymagany dla danych z tabeli odnośników statycznych. Gdy buforowanie przy użyciu ładowania reaktywne będzie lepszym rozwiązaniem niż w ogóle nie buforowania, najlepszym rozwiązaniem byłoby aktywnego ładowania danych tabeli wyszukiwania w pamięci podręcznej podczas uruchamiania aplikacji.

W tym samouczku przedstawiono sposób pamięci podręcznej danych z tabeli odnośników i innych informacji dotyczących statycznego.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Krok 2: Sprawdzenie różne sposoby dane z pamięci podręcznej

Informacje mogą być buforowane programowo w aplikacji ASP.NET przy użyciu różnych metod. Firma Microsoft kolejnych już pokazano sposób użycia pamięci podręcznej danych w poprzednim samouczki. Alternatywnie obiekty mogą być programowane buforowane przy użyciu *statyczne elementy członkowskie* lub *stan aplikacji*.

Podczas pracy z klasy, zwykle należy najpierw można utworzyć wystąpienia klasy przed jej elementów członkowskich. Na przykład aby można było wywołać metodę z jednej z klas w naszym warstwy logiki biznesowej, należy najpierw utworzymy wystąpienia klasy:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Zanim firma Microsoft może wywołać *SomeMethod* i pracować z *SomeProperty*, firma Microsoft musi najpierw utworzyć wystąpienia klasy przy użyciu `New` — słowo kluczowe. *SomeMethod* i *SomeProperty* są skojarzone z konkretnym wystąpieniem. Okres istnienia tych elementów członkowskich jest powiązany okres istnienia ich skojarzonego obiektu. *Statyczne elementy członkowskie*, z drugiej strony są zmiennych, właściwości i metody, które są współużytkowane przez *wszystkie* wystąpień klasy, i w związku z tym, okres istnienia tak długo, jak klasa. Statyczne elementy członkowskie są wskazywane przez słowo kluczowe `Shared`.

Oprócz elementy członkowskie static mogą być buforowane dane przy użyciu stanu aplikacji. Każda aplikacja ASP.NET przechowuje kolekcji nazwa/wartość tego s współużytkowane przez wszystkich użytkowników i strony aplikacji. Ta kolekcja jest możliwy za pomocą [ `HttpContext` klasy](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` właściwości](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)i użycia w klasie związanej z kodem strony ASP.NET w następujący sposób:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Pamięć podręczna danych oferuje znacznie bardziej zaawansowane funkcje API do buforowania danych, dzięki czemu mechanizmów expiries na podstawie czasu i zależności, priorytetów elementu pamięci podręcznej i tak dalej. Statyczne elementy członkowskie i stan aplikacji takich funkcji, należy dodać ręcznie przez dewelopera strony. Podczas buforowania danych podczas uruchamiania aplikacji przez czas ich istnienia aplikacji, jednak zalety s pamięci podręcznej danych są moot. W ramach tego samouczka przyjrzymy kod, który używa wszystkich trzech metod do buforowania danych statycznych.

## <a name="step-3-caching-thesupplierstable-data"></a>Krok 3: Buforowanie`Suppliers`tabeli danych

Northwind bazy danych tabel, możemy kolejnych zaimplementowana do daty nie zawierają żadnych tabel odnośników tradycyjnych. Cztery DataTables zaimplementowana w naszym DAL wszystkie tabele modelu, w których wartości są niestatycznego. Zamiast poświęcany czas, aby dodać nową tabelę DataTable warstwy DAL nowe klasy i metody logiki warstwy Biznesowej, dla tego samouczka let s właśnie podawać który `Suppliers` danych tabeli s jest statyczna. W związku z tym firma Microsoft może w pamięci podręcznej tych danych podczas uruchamiania aplikacji.

Aby rozpocząć, Utwórz nową klasę o nazwie `StaticCache.cs` w `CL` folderu.


![Utwórz klasę StaticCache.vb w folderze CL](caching-data-at-application-startup-vb/_static/image2.png)

**Rysunek 2**: tworzenie `StaticCache.vb` klasy w `CL` folderu


Musimy dodać metodę, która ładuje dane podczas uruchamiania systemu do magazynu pamięci podręcznej właściwe, a także metody, które zwracają dane z tej pamięci podręcznej.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Powyższy kod używa zmiennej statyczny element członkowski `suppliers`, do przechowywania wyników z `SuppliersBLL` klasy s `GetSuppliers()` metodę, która jest wywoływana z `LoadStaticCache()` — metoda. `LoadStaticCache()` Metoda ma być wywoływana podczas uruchamiania aplikacji s. Po załadowaniu danych podczas uruchamiania aplikacji dowolnej strony, który wymaga do pracy z dostawcy danych można wywołać `StaticCache` klasy s `GetSuppliers()` metody. W związku z tym połączenie z bazą danych można pobrać dostawców tylko odbywa się raz, podczas uruchamiania aplikacji.

Zamiast przy użyciu zmiennej statyczny element członkowski jako magazynu pamięci podręcznej, można też użyliśmy stan aplikacji lub w pamięci podręcznej danych. Poniższy kod przedstawia klasy retooled do korzystania ze stanu aplikacji:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

W `LoadStaticCache()`, informacje są przechowywane w zmiennej aplikacji *klucza*. Go s zwracane jako odpowiedniego typu (`Northwind.SuppliersDataTable`) z `GetSuppliers()`. Gdy stan aplikacji jest dostępny w klasach związane z kodem przy użyciu stron ASP.NET `Application("key")`, możemy użyć architektury `HttpContext.Current.Application("key")` Aby uzyskać bieżącą `HttpContext`.

Ponadto pamięć podręczna danych może służyć jako magazynu pamięci podręcznej, jak przedstawiono na poniższym kodem:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Aby dodać element do pamięci podręcznej danych przez nieograniczony czas na podstawie czasu, należy użyć `System.Web.Caching.Cache.NoAbsoluteExpiration` i `System.Web.Caching.Cache.NoSlidingExpiration` wartości jako parametry wejściowe. Tego konkretnego przeciążenia pamięci podręcznej danych s `Insert` metody wybrano, aby firma Microsoft może określać *priorytet* elementu pamięci podręcznej. Priorytet służy do określenia, jakie elementy, aby oczyścić z pamięci podręcznej, kiedy jest za mało dostępnej pamięci. W tym miejscu używamy priorytet `NotRemovable`, co zapewnia, że ten element pamięci podręcznej won t oczyszczana.

> [!NOTE]
> Implementuje ten plik do pobrania samouczek s `StaticCache` przy użyciu podejścia zmiennej statycznego elementu członkowskiego. Kod techniki pamięci podręcznej stanu i danych aplikacji jest dostępna w komentarzach w pliku klasy.


## <a name="step-4-executing-code-at-application-startup"></a>Krok 4: Wykonywanie kodu przy uruchamianiu aplikacji

Wykonanie kodu po pierwszym uruchomieniu aplikacji sieci web, należy utworzyć specjalny plik o nazwie `Global.asax`. Ten plik może zawierać programy obsługi zdarzeń dla aplikacji-, sesji- i zdarzenia na poziomie żądania która jest tutaj którym można dodać kodu, która zostanie wykonana przy każdym uruchomieniu aplikacji.

Dodaj `Global.asax` plik do katalogu głównego s aplikacji sieci web przez kliknięcie prawym przyciskiem myszy nazwę projektu witryny sieci Web w programie Visual Studio s Eksploratorze rozwiązań i wybierając pozycję Dodaj nowy element. W oknie dialogowym Dodaj nowy element wybierz typ elementu globalnej klasy aplikacji, a następnie kliknij przycisk Dodaj.

> [!NOTE]
> Jeśli masz już `Global.asax` pliku w projekcie, globalnej klasy aplikacji typu elementu nie są wyświetlane w oknie dialogowym Dodawanie nowego elementu.


[![Dodaj plik Global.asax do katalogu głównego s aplikacji sieci Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Rysunek 3**: Dodaj `Global.asax` pliku do s Twoja aplikacja sieci Web katalogu głównego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image5.png))


Wartość domyślna `Global.asax` plik szablonu zawiera pięć metod w ciągu po stronie serwera `<script>` tagu:

- **`Application_Start`** wykonuje po pierwszym uruchomieniu aplikacji sieci web
- **`Application_End`** jest uruchamiany, gdy aplikacja jest zamykana.
- **`Application_Error`** wykonuje zawsze, gdy aplikacja osiągnie nieobsługiwany wyjątek
- **`Session_Start`** wykonuje podczas tworzenia nowej sesji
- **`Session_End`** jest uruchamiany, gdy sesja jest wygasła lub porzucone

`Application_Start` Program obsługi zdarzeń zostanie wywołana tylko raz w cyklu życia aplikacji s. Aplikacja rozpoczyna się po raz pierwszy zasobu ASP.NET zażąda aplikacji i będzie kontynuował działanie aż do ponownego uruchomienia aplikacji, która może się zdarzyć, modyfikując zawartość `/Bin` folderu, modyfikując `Global.asax`, modyfikowanie zawartość w `App_Code` folderu lub modyfikowanie `Web.config` pliku wśród innych przyczyn. Zapoznaj się [Przegląd cyklu życia aplikacji ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) bardziej szczegółowe omówienie w cyklu życia aplikacji.

Te samouczki tylko musimy Dodaj kod, aby `Application_Start` metodę, tak usunąć pozostałe wolne działanie. W `Application_Start`, po prostu Wywołaj `StaticCache` klasy s `LoadStaticCache()` metody, co spowoduje załadowanie i pamięci podręcznej informacji o dostawcy:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Wszystkie dostępne tego s jest! Przy uruchamianiu aplikacji `LoadStaticCache()` — metoda pobrania informacji o dostawcy z logiki warstwy Biznesowej i zapisz go w statycznym elementem członkowskim zmiennej (lub niezależnie od pamięci podręcznej przechowywanie użytkownik zakończył się przy użyciu w `StaticCache` klasy). Aby sprawdzić to zachowanie, ustaw punkt przerwania `Application_Start` — metoda i uruchom aplikację. Należy pamiętać, że punkt przerwania zostaje trafiony po uruchomieniu aplikacji. Kolejne żądania, jednak nie powodują `Application_Start` metody do wykonania.


[![Użyj punktu przerwania w celu sprawdź, czy program obsługi zdarzeń Application_Start jest wykonywana.](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Rysunek 4**: Użyj punkt przerwania w upewnij się, że `Application_Start` procedura obsługi zdarzeń jest wykonywana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Jeśli nie zostanie osiągnięty `Application_Start` przerwania podczas rozpoczynania debugowania, oznacza to, że aplikacja została już uruchomiona. Wymusić ponowne uruchomienie, modyfikując Twojej `Global.asax` lub `Web.config` pliki, a następnie spróbuj ponownie. Można po prostu Dodaj (lub usuń) pusty wiersz na końcu jednego z tych plików, aby szybko uruchomić ponownie aplikację.


## <a name="step-5-displaying-the-cached-data"></a>Krok 5: Wyświetlanie danych pamięci podręcznej

W tym momencie `StaticCache` klasa ma wersję dostawcy danych w pamięci podręcznej podczas uruchamiania aplikacji, które mogą być udostępniane za pośrednictwem jego `GetSuppliers()` metody. Aby pracować przy użyciu tych danych z warstwy prezentacji, możemy użyć ObjectDataSource lub wywołaj programowane `StaticCache` klasy s `GetSuppliers()` metoda z klasy związane z kodem strony ASP.NET. Let s przyjrzeć się za pomocą kontrolki ObjectDataSource i GridView do wyświetlenia informacji o dostawcy pamięci podręcznej.

Uruchamianie przez otwarcie `AtApplicationStartup.aspx` strony `Caching` folderu. Przeciągnij element GridView z przybornika do projektanta, ustawienie jej `ID` właściwości `Suppliers`. Następnie z widoku GridView tagów inteligentnych s wybrać do utworzenia nowego elementu ObjectDataSource o nazwie `SuppliersCachedDataSource`. Element ObjectDataSource umożliwia konfigurowanie `StaticCache` klasy s `GetSuppliers()` metody.


[![Skonfiguruj ObjectDataSource do użycia klasy StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Rysunek 5**: Konfigurowanie ObjectDataSource do użycia `StaticCache` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image11.png))


[![Użyj metody GetSuppliers() do pobierania danych dostawcy pamięci podręcznej](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Rysunek 6**: Użyj `GetSuppliers()` metody do pobierania danych dostawcy w pamięci podręcznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image14.png))


Po zakończeniu działania kreatora programu Visual Studio automatycznie doda BoundFields dla każdego pola danych w `SuppliersDataTable`. Z widoku GridView i ObjectDataSource s deklaratywne znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Rysunek nr 7 przedstawia stronie podczas przeglądania za pośrednictwem przeglądarki. Dane wyjściowe odpowiada firma Microsoft ma pobierane dane z s logiki warstwy Biznesowej `SuppliersBLL` klasy, ale przy użyciu `StaticCache` klasy zwraca dane dostawcy jako pamięci podręcznej podczas uruchamiania aplikacji. Można ustawić punktów przerwania w `StaticCache` klasy s `GetSuppliers()` metodę, aby sprawdzić to zachowanie.


[![Dane w pamięci podręcznej dostawcy są wyświetlane w widoku GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Rysunek 7**: buforowane dane dostawcy są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Podsumowanie

Większość każdy model danych zawiera odpowiedniej ilości danych statycznych, zwykle implementowany w formie tabel odnośników. Ponieważ te informacje są statyczne, że s ma powodu stale dostęp do bazy danych za każdym razem, te informacje mają być wyświetlone. Co więcej, ze względu na charakter statyczny, podczas buforowania danych tam s nie jest konieczne wygaśnięcia. W tym samouczku widzieliśmy sposobu podjęcia takich danych i ją buforują w pamięci podręcznej danych stanu aplikacji i za pośrednictwem zmiennej statycznego elementu członkowskiego. Te informacje są buforowane podczas uruchamiania aplikacji i pozostaje w pamięci podręcznej w okresie istnienia s aplikacji.

W tym samouczku i poprzednich dwóch możemy kolejnych przeglądał buforowanie danych na czas trwania okresu istnienia s aplikacji, a także za pomocą expiries oparte na czasie. Podczas buforowania danych w bazie danych, jednak na podstawie czasu wygaśnięcia może być mniejsza niż idealne. Zamiast okresowo opróżniania pamięci podręcznej, będą optymalne do wykluczenia tylko element pamięci podręcznej, modyfikacji danych bazy danych. Możliwe przy użyciu zależności buforu SQL, które zostaną omówione w naszym samouczku dalej jest to idealne rozwiązanie.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Zack Nowak. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-in-the-architecture-vb.md)
> [dalej](using-sql-cache-dependencies-vb.md)
