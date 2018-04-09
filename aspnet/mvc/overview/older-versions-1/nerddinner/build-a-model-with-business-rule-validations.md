---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Tworzenie modelu sprawdzaniem poprawności reguły biznesowej | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 3 przedstawiono sposób tworzenia modelu, że firma Microsoft umożliwia zarówno zapytania i aktualizowania bazy danych dla aplikacji NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="build-a-model-with-business-rule-validations"></a>Tworzenie modelu sprawdzaniem poprawności reguły biznesowej
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 3 bezpłatnych ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 3 przedstawiono sposób tworzenia modelu, że firma Microsoft umożliwia zarówno zapytania i aktualizowania bazy danych dla aplikacji NerdDinner.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner krok 3: Budowanie modelu

W ramach model-view-controller termin "modelu" odnosi się do obiektów, które zawierają dane aplikacji, jak również odpowiedniego logika domeny, która integruje się sprawdzanie poprawności i reguły biznesowe z nim. Model jest na wiele sposobów "pulsu" aplikacji opartych na strukturze MVC i jak zajmiemy się tym później zasadniczo dysków z zachowaniem.

Platforma ASP.NET MVC obsługuje za pomocą innych technologii dostępu do danych i możliwość wyboru z wielu zaawansowanych opcji danych .NET do zaimplementowania ich modeli, w tym: LINQ do jednostek, składnika LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM lub po prostu raw ADO. NET DataReaders ani zestawów danych.

Dla aplikacji NerdDinner zamierzamy LINQ do SQL umożliwia utworzenie prostego modelu, który odpowiada dość ściśle naszych projekt bazy danych i dodaje niektóre niestandardowe reguły sprawdzania poprawności logiki i business. Następnie wprowadzimy klasę repozytorium, która pomaga abstrakcyjny optymalizacji implementacja trwałości danych od pozostałej części aplikacji i umożliwia firmie Microsoft w celu łatwego jednostki przetestować go.

### <a name="linq-to-sql"></a>LINQ do SQL

LINQ do SQL jest ORM (obiektów relacyjnych mapowania) jest dostarczany jako część .NET 3.5.

LINQ do SQL zapewnia prosty sposób mapowania klasy .NET, które firma Microsoft może kodu przed tabel bazy danych. Dla aplikacji NerdDinner użyjemy go do mapowania tabeli kolacji i RSVP w naszej bazie danych obiad oraz RSVP klasy. Kolumny tabeli kolacji i RSVP odpowiada właściwości klasy obiad i RSVP. Każdy obiekt obiad i RSVP będzie reprezentować oddzielnym wierszu w kolacji lub RSVP tabel w bazie danych.

LINQ do SQL pozwala uniknąć konieczności ręcznego utworzenia instrukcje SQL w celu pobierania i aktualizowania obiad i RSVP obiektów z bazy danych. Zamiast tego zdefiniujemy obiad i RSVP klas, w jaki sposób są one wykonywane na/bazy danych i relacje między nimi. LINQ do SQL bierze opieki generowania odpowiednich logiki wykonywania SQL do użycia w czasie wykonywania, gdy firma Microsoft i korzystać z nich.

Możemy użyć LINQ obsługę języka VB i C# do pisania obszerne zapytań, które pobierają obiad i RSVP obiektów z bazy danych. Pozwala to zmniejszyć ilość kodu danych należy zapisać, i umożliwia tworzenie aplikacji naprawdę czyste.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Dodawanie klasy LINQ do SQL do naszej projektu

Firma Microsoft będzie rozpocząć, klikając prawym przyciskiem myszy w folderze "Modele" w ramach naszych projektu i wybierz **Add -&gt;nowy element** polecenie:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj nowy element". Firma Microsoft będzie filtrować według kategorii "Dane" i wybierz szablon "LINQ do SQL klasy" w niej:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Firma Microsoft będzie nazwy elementu "NerdDinner" i kliknij przycisk "Dodaj". Visual Studio będzie dodanie pliku NerdDinner.dbml w naszym katalogu \Models, a następnie otwórz składnika LINQ to SQL Projektant obiektów relacyjnych:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Tworzenie klasy modelu danych za pomocą LINQ do SQL

LINQ do SQL pozwala szybko utworzyć klasy modelu danych z istniejącego schematu bazy danych. Zadania do wykonania to firma Microsoft będzie otworzyć NerdDinner bazy danych w Eksploratorze serwera i wybierz tabele chcemy modelu w tym:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Firma Microsoft następnie przeciągnij tabel na LINQ do powierzchni projektanta SQL. Gdy przejdziemy LINQ to SQL zostanie automatycznie utworzony obiad i klasy RSVP przy użyciu schematu tabeli (z właściwości klasy, które mapują kolumny tabeli bazy danych):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Domyślnie LINQ do SQL projektanta automatycznie "generuje końcówki liczbie mnogiej" nazwy tabel i kolumn podczas tworzenia klasy na podstawie schematu bazy danych. Na przykład: w tabeli "Kolacji" w naszym przykładzie powyżej spowodowała klasy "Obiad". Ta klasa nazewnictwa ułatwia naszych modeli zgodnie z konwencją nazewnictwa .NET i zwykle znaleźć ten, którego poprawkę projektanta to zapasowej wygodny (szczególnie podczas dodawania wiele tabel). Jeśli nie chcesz, aby nazwa klasy lub właściwości, które generuje projektanta, ale zawsze możesz zmienić i zmień ją na dowolną nazwę, która ma. Można to zrobić, edytując właściwości/jednostki nazwy w wierszu projektanta lub modyfikując przy użyciu siatki właściwości.

Domyślnie LINQ do SQL projektanta również przeprowadzający podstawowy klucz/relacje klucza obcego tabel, a oparte na nich automatycznie tworzy domyślną "skojarzenia relacji" między klasami inny model, który tworzy. Na przykład gdy firma Microsoft przeciągnięto kolacji i RSVP tabel na LINQ do SQL projektanta skojarzenie relacji jeden do wielu między tymi dwoma wywnioskowano opiera się na fakt, że tabela RSVP miał kluczy obcych do tabeli kolacji (jest to określane przez strzałkę w Projektant):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Powyżej skojarzenia spowoduje, że LINQ do SQL, aby dodać właściwość "Obiad" silnie typizowaną do klasy RSVP, której deweloperzy mogą używać obiad skojarzone z danym RSVP dostępu do. Spowoduje także klasy obiad mieć "RSVPs" właściwość kolekcji, który umożliwia deweloperom pobierania i aktualizowania RSVP obiekty skojarzone z określonym obiad.

Poniżej widać przykład intellisense w programie Visual Studio po możemy utworzyć nowego obiektu RSVP i dodaj go do kolekcji zbędne obiad. Zwróć uwagę, jak LINQ do SQL automatycznie dodawane kolekcji "Zbędne" w obiekcie obiad:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Dodawanie obiektu RSVP do kolekcji zbędne obiad możemy informuje LINQ do SQL do skojarzenia klucza obcego relacji między obiad i wiersz RSVP w naszej bazie danych:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Jeśli nie lubisz jak projektanta został uformowany lub o nazwie skojarzenia tabeli, które można zmienić. Po prostu kliknij strzałkę skojarzenia w Projektancie i uzyskać dostęp do jego właściwości za pośrednictwem siatki właściwości, aby zmienić nazwę, usuń lub zmodyfikuj go. Dla naszej aplikacji NerdDinner, domyślne reguły kojarzenia pracy dla klasy modelu danych, które możemy budowania i możemy użyć domyślnego zachowania.

### <a name="nerddinnerdatacontext-class"></a>Klasa NerdDinnerDataContext

Visual Studio automatycznie utworzy klasy .NET, które reprezentują modeli i bazy danych relacje zdefiniowane przy użyciu składnika LINQ to SQL projektanta. LINQ do klasy SQL DataContext zostanie również wygenerowany dla każdego składnika LINQ to SQL pliku projektanta dodany do rozwiązania. Ponieważ firma Microsoft o nazwie naszych LINQ do SQL klasy elementu "NerdDinner" klasy DataContext utworzone zostanie wywołana "NerdDinnerDataContext". Ta klasa NerdDinnerDataContext jest podstawowy sposób firma Microsoft będzie współpracować z bazy danych.

Klasy Nasze NerdDinnerDataContext udostępnia dwie właściwości — "Kolacji" i "RSVPs" - reprezentujących dwóch tabel, które możemy modelowane w bazie danych. Możemy użyć C# do zapisu zapytań LINQ te właściwości zapytania i pobrać obiad i RSVP obiektów z bazy danych.

Poniższy kod przedstawia sposób tworzenia wystąpienia obiektu NerdDinnerDataContext i wykonywać zapytania LINQ, aby uzyskać sekwencję kolacji, które występują w przyszłości. Program Visual Studio udostępnia pełną intellisense podczas pisania zapytań LINQ i obiektów zwróconych z niego są silnie typizowane i obsługiwać intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oprócz umożliwienia zapytań dotyczących obiektów obiad i RSVP, NerdDinnerDataContext automatycznie śledzi zmiany możemy następnie obiad i RSVP obiekty, które możemy pobierać za jego pośrednictwem. Tej funkcji można używać łatwo zapisanie zmian w bazie danych — bez konieczności pisania kodu jawne aktualizacji SQL.

Na przykład poniższy kod pokazuje, jak używać zapytania LINQ pobierania pojedynczy obiekt obiad z bazy danych, zaktualizuj dwóch właściwości obiad i następnie zapisać zmiany w bazie danych:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Obiekt NerdDinnerDataContext w kodzie powyżej automatycznie śledzone zmiany właściwości do obiektu obiad, do którego możemy pobrać z niego. Wywołanego metody "SubmitChanges()", wykona odpowiednie "AKTUALIZUJ" instrukcji SQL do bazy danych, aby utrwalić wstecz zaktualizowane wartości.

### <a name="creating-a-dinnerrepository-class"></a>Tworzenie klasy DinnerRepository

Małych aplikacjach czasami jest wyrażenie zgody na kontrolery pracować bezpośrednio przed składnika LINQ to SQL DataContext klasy, a następnie osadź zapytań LINQ w ramach kontrolerów. Jako dłuższego aplikacji, to rozwiązanie staje się skomplikowane do utrzymywania i testowania. Również może prowadzić do nas duplikowania tego samego zapytania LINQ w kilku miejscach.

Użyj wzorca "repository" jest jeden podejście, które mogą ułatwić aplikacje do obsługi i testowania. Klasa repozytorium pomaga Hermetyzowanie danych zapytań i logikę trwałości i optymalizacji streszczenia szczegóły implementacji trwałości danych z aplikacji. Oprócz tworzenia czyszczący kodu aplikacji, za pomocą wzorca repozytorium może ułatwić w przyszłości zmienić implementacji magazynu danych, a może pomóc ułatwienia testowania aplikacji bez konieczności rzeczywistych bazy danych jednostki.

Dla aplikacji NerdDinner zdefiniujemy klasy DinnerRepository z poniżej podpisu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Uwaga: Dalej w tym rozdziale firma Microsoft będzie wyodrębniania interfejsu IDinnerRepository z tej klasy i Włącz iniekcji zależności z nim naszych kontrolerów. Najpierw jednak zamierzamy uruchomić proste i po prostu pracować bezpośrednio z klasy DinnerRepository.*

Do implementowania tej klasy, firma Microsoft będzie kliknij prawym przyciskiem myszy na naszych folderu "Modeli" i wybierz polecenie **Add -&gt;nowy element** polecenia menu. W oknie dialogowym "Dodaj nowy element" możemy wybierz szablon "Class" i "DinnerRepository.cs" do pliku o nazwie:

![](build-a-model-with-business-rule-validations/_static/image10.png)

Następnie możemy wdrożyć klasy Nasze DinnerRespository przy użyciu kodu poniżej:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Pobieranie, aktualizowania, wstawiania i usuwanie za pomocą klasy DinnerRepository

Teraz, utworzyliśmy klasy Nasze DinnerRepository Oto kilka przykładów kodu, które wykazują typowych zadań, które firma Microsoft może z nim zrobić:

#### <a name="querying-examples"></a>Przykłady zapytań

Poniższy kod pobiera jeden obiad, przy użyciu wartości DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Poniższy kod pobiera wszystkie nadchodzących kolacji i pętle nad nimi:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT i Update przykłady

Poniższy kod pokazuje, dodawanie dwóch nowych kolacji. Dodatki/zmiany do repozytorium nie są zapisane w bazie danych, dopóki wywołania metody "Zapisz()" na nim. LINQ do SQL jest automatycznie zawijany, wszystkie zmiany w transakcji bazy danych — tak się zdarzyć, wszystkie zmiany albo żadna z nich podczas zapisywania w naszym repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Poniższy kod pobiera istniejący obiekt obiad i modyfikuje dwie właściwości na nim. Zmiany zostały zastosowane w bazie danych, gdy wywoływana jest metoda "Zapisz()" naszym repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Poniższy kod pobiera obiad, a następnie dodaje zaproszenie do niego. Jest to przy użyciu kolekcji zbędne na obiad utworzony obiekt LINQ do SQL firmie Microsoft (ponieważ istnieje relacja podstawowy klucz/klucz obcy między dwoma w bazie danych). Ta zmiana jest zachowywane w bazie danych jako nowy wiersz tabeli RSVP po wywołaniu metody "Zapisz()" repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Usuń przykładowe

Poniższy kod pobiera istniejący obiekt obiad i dołącza go do usunięcia. Po wywołaniu metody "Zapisz()" repozytorium go zobowiązuje delete w bazie danych:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrowanie sprawdzania poprawności i logiki reguły biznesowej z klasami modelu

Integrowanie sprawdzania poprawności i reguły biznesowej jest kluczowym elementem dowolnej aplikacji, która współdziała z danych.

#### <a name="schema-validation"></a>Sprawdzanie poprawności schematu

Po zdefiniowaniu klasy modelu przy użyciu składnika LINQ to SQL projektanta typy danych właściwości w modelu danych klasy odpowiadają typów danych w tabeli bazy danych. Na przykład: Jeśli kolumna "EventDate" w tabeli kolacji jest typu "datetime", klasy modelu danych, które są tworzone w składniku LINQ to SQL jest typu "DateTime" (która jest wbudowane datatype .NET). Oznacza to, że otrzymasz błędy kompilacji, jeśli próby przypisania liczbą całkowitą lub wartość logiczną do niego z kodu i jego zgłosi błąd automatycznie przy próbie niejawnie przekonwertować typu ciąg nie jest ważna do niego w czasie wykonywania.

LINQ do SQL zostanie automatycznie obsługuje ucieczki wartości SQL dla Ciebie, podczas używania ciągów — co pomaga chronić przed ataki podczas korzystania z niego.

#### <a name="validation-and-business-rule-logic"></a>Sprawdzanie poprawności i logiki reguły biznesowej

Sprawdzanie poprawności schematu jest przydatne w pierwszej kolejności, ale jest rzadko wystarczające. Większość przypadków ze świata rzeczywistego muszą mieć możliwość określenia bardziej zaawansowane funkcje logiki sprawdzania poprawności, które obejmują wiele właściwości, wykonanie kodu i często mają pogłębianie wiedzy na temat stanu modelu (na przykład: jej tworzenia/aktualizacji/usunięte, lub jest w stanie specyficznego dla domeny tak samo, jak "archiwizowane"). Istnieje szereg różnych wzorców i platform, które mogą służyć do definiowania i stosowania reguł sprawdzania poprawności do klasy modeli, a istnieje kilka .NET na podstawie platform tam używane ułatwiające to. Można użyć niemal żadnej z nich w aplikacji ASP.NET MVC.

Na potrzeby aplikacji NerdDinner użyjemy wzorzec stosunkowo proste i bezpośrednie gdzie uwidaczniamy IsValid właściwości i metody GetRuleViolations() naszych obiad obiektu modelu. Właściwość IsValid zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy wszystkie ważne są reguły sprawdzania poprawności i biznesowych. Metoda GetRuleViolations() zwróci listę wszystkich błędów reguły.

Firma Microsoft będzie implementują IsValid i GetRuleViolations() dla modelu obiad przez dodanie "klasy częściowej" do naszej projektu. Klasy częściowe może służyć do dodawania metod/właściwości/zdarzenia do klasy obsługiwany przez projektanta VS (np. klasa obiad generowane przez składnik LINQ to SQL projektanta) i uniknąć narzędzie z messing z naszego kodu. Możemy dodać nową klasę częściowe do naszej projektu przez kliknięcie prawym przyciskiem myszy \Models folder, a następnie wybierz polecenie "Dodaj nowy element". Firma Microsoft następnie wybierz szablon "Class" w oknie dialogowym "Dodaj nowy element" i nadaj mu nazwę Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Kliknięcie przycisku "Dodaj" spowoduje dodanie pliku Dinner.cs do naszej projektu i otwórz go w środowisku IDE. Firma Microsoft można następnie wdrożyć to rozwiązanie podstawowe reguły/sprawdzanie poprawności wymuszania framework przy użyciu poniższego kodu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Kilka uwagi dotyczące kodu powyżej:

- Klasa obiad jest poprzedzona słowem kluczowym "częściowej" — co oznacza kod źródłowy znajdujący się w niej będą połączone z klasy generowane utrzymywane w składniku LINQ to SQL projektanta i skompilowany w jednej klasy.
- Klasa RuleViolation jest klasa pomocy, który zostanie dodany do projektu, który pozwala zawierają więcej szczegółowych informacji o naruszenie reguły.
- Metoda Dinner.GetRuleViolations() powoduje, że nasze zasady sprawdzania poprawności i business ma zostać obliczone (Firma Microsoft będzie ich wdrażania wkrótce). Następnie zwraca ponownie sekwencję RuleViolation obiektów, które zawierają więcej szczegółowych informacji o błędach reguły.
- Właściwość Dinner.IsValid umożliwia wygodne pomocnika właściwość, która wskazuje, czy obiekt obiad ma żadnych aktywnych RuleViolations. On aktywne można sprawdzić przez dewelopera przy użyciu obiektu obiad w dowolnym momencie (i nie zgłaszał wyjątku).
- Metoda częściowa Dinner.OnValidate() jest haku udostępniający LINQ do SQL umożliwiający powiadamianych w dowolnym momencie obiad obiekt ma zostać utrwalony w bazie danych. Nasze implementacji OnValidate() powyżej zapewnia obiad ma nie RuleViolations, dopóki zostanie zapisane. Jeśli jest w nieprawidłowym stanie zgłasza wyjątek, który spowoduje, że LINQ do SQL, aby przerwać transakcji.

Metoda ta umożliwia proste platforma, która zostanie zintegrowana, weryfikacji i reguł biznesowych do. Teraz Dodajmy poniżej reguł do naszej metody GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Użyto funkcji "yield return" języka C# do zwrócenia sekwencji żadnych RuleViolations. Najpierw sprawdza reguły sześciu powyżej wymusić po prostu, że właściwości ciągu na naszych obiad nie może być zerowa lub pusta. Ostatnia reguła jest nieco bardziej interesujące i wywołania metody pomocniczej PhoneValidator.IsValidNumber(), że można dodać do naszej projektu, aby sprawdzić, czy ContactPhone number format dopasowań obiad w kraju.

Możemy użyć. Obsługa wyrażenia regularnego przez sieć do implementacji sprawdzania poprawności telefoniczna pomoc techniczna. Poniżej przedstawiono prosty implementacji PhoneValidator, które można dodać do naszych projektu, który pozwala na dodawanie kontroli wzorzec wyrażenia regularnego określonego kraju:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Obsługa weryfikacji i naruszeń logika biznesowa

Teraz, po dodaniu powyżej sprawdzania poprawności i business kodu reguły, spróbujemy można utworzyć lub zaktualizować obiad za każdym razem, nasze zasady logikę weryfikacji będą oceniane, a wymuszane.

Programiści mogą pisać kod jak poniżej, aby aktywnie ustalić, czy obiekt obiad jest prawidłowy i pobrać listy wszystkich naruszeń w nim bez podnoszenia wszelkie wyjątki:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Firma Microsoft podejmie próbę zapisania obiad w nieprawidłowym stanie, gdy firma Microsoft wywołać metody Zapisz() DinnerRepository zostanie zgłoszony wyjątek. Dzieje się tak, ponieważ przed zapisaniem zmian obiad i dodano kod do Dinner.OnValidate(), aby zgłosić wyjątek, jeśli istnieje wszelkie naruszenia reguły w obiad LINQ do SQL automatycznie wywołuje metody częściowej naszych Dinner.OnValidate(). Firma Microsoft przechwycić tego wyjątku i reagować i odbierać listę naruszeń ustalenie:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Ponieważ naszych reguł sprawdzania poprawności i business zostaną zaimplementowane w naszym warstwy modelu, a nie w ramach warstwy interfejsu użytkownika, będzie można zastosować i używane we wszystkich scenariuszach w naszej aplikacji. Firma Microsoft później zmienić lub dodać reguły biznesowe i wszystkie kod działa z naszych obiektów obiad uwzględnić je.

Mając możliwość zmiany reguły biznesowe w jednym miejscu, bez konieczności zmiany ripple w całej aplikacji i logika interfejsu użytkownika jest znak dobrze napisanych aplikacji i korzyści, które platforma MVC pomaga zachęca.

### <a name="next-step"></a>Następny krok

Mamy teraz modelu, który możemy użyć zarówno zapytania i zaktualizować naszej bazie danych.

Teraz Dodajmy niektóre kontrolery i widoki do projektu, który firma Microsoft może wykorzystać do tworzenia obsługi interfejsu użytkownika HTML wokół niego.

> [!div class="step-by-step"]
> [Poprzednie](create-a-database.md)
> [dalej](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
