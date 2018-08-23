---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Budowanie modelu z weryfikacją reguł biznesowych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 3 pokazuje, jak utworzyć model, że firma Microsoft umożliwia zarówno zapytania i aktualizują bazę danych dla naszej aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: b735c414c629b9ff6617dbf80782d57543306c00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756208"
---
<a name="build-a-model-with-business-rule-validations"></a>Budowanie modelu z weryfikacją reguł biznesowych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 3 z BEZPŁATNEJ [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 3 pokazuje, jak utworzyć model, że firma Microsoft umożliwia zarówno zapytania i aktualizują bazę danych dla naszej aplikacji NerdDinner.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner krok 3: Budowanie modelu

W ramach model-view-controller termin "model" odnosi się do obiektów, które reprezentują dane aplikacji, a także odpowiednie logiki domeny, która integruje się z nim sprawdzania poprawności i reguł biznesowych. Model jest pod wieloma względami "serce" aplikacji opartych na strukturze MVC i jak zajmiemy się później całkowicie dyski z zachowaniem.

Platforma ASP.NET MVC obsługuje przy użyciu dowolnej technologii dostępu do danych i deweloperzy mogą wybrać jeden z wielu zaawansowanych opcji danych .NET wdrożyć swoje modele, w tym: LINQ do jednostek oraz narzędzi LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM lub po prostu pierwotne ADO. NET DataReaders lub zestawów danych.

Dla naszej aplikacji NerdDinner będziemy używać programu LINQ to SQL do tworzenia prostego modelu stosunkowo blisko odnosi się do naszych projekt bazy danych, która dodaje niektórych reguł biznesowych i logiki niestandardowego sprawdzania poprawności. Następnie wprowadzimy klasę repozytorium, która pomaga abstrakcyjny natychmiast implementacji trwałości danych od pozostałej części aplikacji i umożliwia nam łatwo jednostki do testowania.

### <a name="linq-to-sql"></a>LINQ do SQL

LINQ do SQL jest ORM (obiektów relacyjnych mapowania), który jest dostarczany jako część .NET 3.5.

LINQ do SQL zapewnia łatwy sposób mapowania klasy .NET, których będziemy programować względem tabel bazy danych. Dla naszej aplikacji NerdDinner będziemy z niego korzystać do mapowania tabel kolacji i RSVP w naszej bazie danych obiad oraz RSVP klasy. Kolumny tabeli kolacji i RSVP odpowiada właściwości klasy obiad i RSVP. Każdy obiekt obiad i RSVP będzie reprezentować w oddzielnym wierszu w ramach kolacji lub RSVP tabel w bazie danych.

LINQ do SQL pozwala uniknąć konieczności ręcznego konstruowania instrukcji SQL do pobierania i aktualizowania obiad i RSVP obiektów z bazy danych. Zamiast tego zdefiniujemy obiad i RSVP klas, w jaki sposób mapowania ich na i z niej bazy danych i relacje między nimi. LINQ do SQL bierze opieki nad Generowanie odpowiednich logiki wykonywania SQL do użycia w czasie wykonywania, gdy firma Microsoft i korzystać z nich.

Możemy użyć obsługę języka programu LINQ w VB i C#, aby pisać zapytania ekspresyjna, które pobierają obiad i RSVP obiektów z bazy danych. Zmniejsza to ilość kodu danych, należy napisać, i pozwala tworzyć aplikacje naprawdę eleganckie.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Dodawanie klasy programu LINQ to SQL do naszego projektu

Firma Microsoft będzie zacząć, klikając prawym przyciskiem myszy w folderze "Modele" w projekcie i wybierz **Add -&gt;nowy element** polecenia menu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj nowy element". Firma Microsoft będzie filtrować według kategorii "Dane" i wybierz szablon "LINQ do klas SQL" znajdujący się w nim:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Firma Microsoft będzie nazwa elementu "NerdDinner" i kliknij przycisk "Dodaj". Program Visual Studio będzie dodać plik NerdDinner.dbml naszego katalogu \Models, a następnie otwórz LINQ to SQL obiektów relacyjnych projektanta:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Tworzenie klas modelu danych za pomocą LINQ to SQL

LINQ to SQL oferuje nam na szybkie tworzenie klas modelu danych z istniejącego schematu bazy danych. Do wykonania to firma Microsoft będzie otworzyć NerdDinner bazy danych w Eksploratorze serwera i wybrać tabele, które chcemy modelu w nim:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Firma Microsoft można przeciągnąć tabele na LINQ, do powierzchni projektanta SQL. Podczas prowadzenia LINQ to SQL będzie automatycznie tworzył obiad i klasy RSVP przy użyciu schematu tabel (z właściwości klasy mapowane na kolumny tabeli bazy danych):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Domyślnie LINQ to SQL Projektant automatycznie "liczbę mnogą" nazwy tabel i kolumn podczas tworzenia klas na podstawie schematu bazy danych. Na przykład: w tabeli "Kolacji" w powyższym przykładzie spowodowało klasy "Obiad". Ta klasa nazewnictwa ułatwia naszych modeli zgodne z konwencjami nazewnictwa platformy .NET i zwykle znaleźć ten, którego poprawkę projektanta to się wygodne (szczególnie podczas dodawania wiele tabel). Jeśli nie podoba Ci się nazwa klasy lub Projektant generuje, chociaż, zawsze możesz go zastąpić i zmienić ją na dowolną nazwę, która ma właściwość. Możesz można to zrobić, edytując właściwości jednostki/nazwa wbudowane w Projektancie lub modyfikując za pośrednictwem siatki właściwości.

Domyślnie LINQ to SQL projektanta również sprawdza podstawowego klucza/relacje klucza obcego tabel i na podstawie ich automatycznie tworzy domyślne "relacja skojarzenia" między klasami innego modelu, który on generuje. Na przykład, kiedy możemy przeciągnąć kolacji i RSVP tabel na LINQ do SQL projektanta skojarzenia typu relacji jeden do wielu między tymi dwoma została wywnioskowana opiera się na fakcie, że tabela RSVP miał kluczy obcych do tabeli kolacji (jest to wskazywane przez strzałkę na liście Projektant):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Powyżej skojarzenia spowoduje, że LINQ do SQL, aby dodać silnie typizowaną właściwość "Obiad" do klasy RSVP, która umożliwia programistom dostępu obiad skojarzone z danym RSVP. Spowoduje także klasy obiad mieć "RSVPs" właściwość kolekcji, która umożliwia deweloperom pobierania i aktualizowania obiektów RSVP skojarzonych z określonym obiad.

Poniżej przedstawiono przykład technologii intellisense w programie Visual Studio podczas utworzymy nowy obiekt RSVP i dodać go do kolekcji zbędne obiad. Zwróć uwagę, jak LINQ to SQL automatycznie dodawane kolekcji "Zbędne" w obiekcie obiad:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Dodając obiekt RSVP kolekcji zbędne obiad kolejne parametry LINQ to SQL do skojarzenia klucza obcego relacji między Dinner i wiersz RSVP w naszej bazie danych:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Jeśli nie potrzebujesz, jak projektant ma modelowane lub o nazwie skojarzenia tabeli, można ją zastąpisz. Wystarczy kliknąć strzałkę skojarzenia, w Projektancie i uzyskiwanie dostępu do jego właściwości, za pośrednictwem siatki właściwości, aby zmienić nazwę, usuń lub zmodyfikuj go. Dla naszej aplikacji NerdDinner, domyślne reguły kojarzenia zadziałać dla klasy modelu danych, które tworzymy i możemy użyć zachowanie domyślne.

### <a name="nerddinnerdatacontext-class"></a>Klasa NerdDinnerDataContext

Program Visual Studio automatycznie utworzy klas platformy .NET, które reprezentują modeli i relacje bazy danych zdefiniowanych za pomocą LINQ to SQL projektanta. LINQ do klas SQL DataContext również jest generowany dla każdego plik LINQ to SQL projektanta dodawany do rozwiązania. Ponieważ firma Microsoft o nazwie naszych LINQ to SQL klasy: element "NerdDinner", klasy DataContext, utworzony zostanie wywołana "NerdDinnerDataContext". Ta klasa NerdDinnerDataContext jest podstawowym sposobem, w których firma Microsoft będzie wchodzić w interakcje z bazą danych.

Klasa naszych NerdDinnerDataContext udostępnia dwie właściwości — "Kolacji" i "RSVPs -", które reprezentują dwie tabele, które firma Microsoft modelowane w bazie danych. Możemy użyć C# można zapisać zapytania LINQ w odniesieniu do tych właściwości do zapytania i pobieranie obiektów obiad i RSVP z bazy danych.

Poniższy kod przedstawia sposób tworzenia wystąpienia obiektu NerdDinnerDataContext i wykonują kwerendę LINQ go uzyskać sekwencję kolacji występujących w przyszłości. Program Visual Studio zapewnia pełną obsługą technologii intellisense podczas pisania zapytań LINQ i obiektów zwróconych z niego są silnie typizowane i obsługiwać intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oprócz umożliwienia nami, aby wyszukać obiekty obiad i RSVP, NerdDinnerDataContext również automatycznie śledzi wszelkie zmiany, które możemy następnie dokonać obiad i RSVP obiekty, które możemy pobierać za jego pośrednictwem. Możemy użyć tej funkcji, można łatwo zapisać zmiany w bazie danych — bez konieczności pisania kodu jawne aktualizacji programu SQL.

Na przykład poniższy kod pokazuje, jak korzystać z zapytania LINQ do pobrania pojedynczego obiektu obiad z bazy danych, dwie właściwości obiad aktualizacji i następnie zapisz zmiany w bazie danych:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Obiekt NerdDinnerDataContext w kodzie powyżej są automatycznie śledzone zmiany właściwości do obiektu obiad pobrane z niego. Po wywołaniu metody "SubmitChanges()" Firma Microsoft wykona odpowiednie "AKTUALIZUJ" instrukcja SQL do bazy danych, aby utrwalić ponownie zaktualizowanymi wartościami.

### <a name="creating-a-dinnerrepository-class"></a>Tworzenie klasy DinnerRepository

Dla małych aplikacji, czasami jest dobrym rozwiązaniem kontrolery pracować bezpośrednio w odniesieniu do składnika LINQ to SQL DataContext klasy, a następnie osadź zapytań LINQ w ramach kontrolerów. Jak uzyskiwania większej aplikacji, jednak takie podejście staje się problematyczne jego utrzymywanie i testowanie. Ponadto może to prowadzić do nas duplikowania tego samego zapytania LINQ w kilku miejscach.

Jeden podejście, które mogą ułatwić aplikacje utrzymywanie i testowanie jest stosowany jest wzorzec "repozytorium". Klasa repozytorium pomaga hermetyzacji, wykonywanie zapytań i logikę trwałości i natychmiast Abstract, szczegóły implementacji funkcji trwałości danych z aplikacji. Oprócz tworzenia kodu aplikacji jest bardziej przejrzysty, przy użyciu wzorca repozytorium może ułatwić w przyszłości zmienić implementacje magazynu danych, a jego może ułatwić testowanie aplikacji bez konieczności rzeczywista baza danych jednostek.

Dla naszej aplikacji NerdDinner zdefiniujemy klasą DinnerRepository z poniżej podpisu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Uwaga: Dalej w tym rozdziale utworzymy Wyodrębnij interfejs IDinnerRepository z tej klasy i włączyć wstrzykiwanie zależności z nim naszych kontrolerach. Najpierw jednak zamierzamy prosty sposób Rozpocznij pracę i pracują bezpośrednio z klasą DinnerRepository.*

Do implementowania tej klasy, utworzymy kliknij prawym przyciskiem myszy w folderze naszych "Modele" i wybierz **Add -&gt;nowy element** polecenia menu. W oknie dialogowym "Dodaj nowy element" Firma Microsoft wybierz szablon "Class" i nazwij plik "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Następnie możemy wdrożyć nasze klasy DinnerRespository, za pomocą poniższego kodu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Pobierania, aktualizowania, wstawiania i usuwania, za pomocą klasy DinnerRepository

Teraz, utworzyliśmy klasy Nasze DinnerRepository Spójrzmy na kilka przykładów kodu, które przedstawiają typowych zadań, które firma Microsoft może z nim zrobić:

#### <a name="querying-examples"></a>Przykłady zapytań

Poniższy kod pobiera pojedynczy obiad, przy użyciu wartości DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Poniższy kod pobiera wszystkie nadchodzące kolacji i pętle nad nimi:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT i Update przykłady

Poniższy kod demonstruje, dodanie dwóch nowych kolacji. Dodatki/zmiany do repozytorium nie są zatwierdzona w bazie danych, dopóki nie jest wywoływać w nim metody "Zapisz()". LINQ do SQL jest automatycznie zawijany, wszystkie zmiany w transakcji bazy danych — dzięki czemu wszystkie zmiany warstw albo żadna z nich podczas zapisywania w naszym repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Poniższy kod pobiera istniejący obiekt obiad i modyfikuje dwie właściwości na nim. Zmiany zostały zastosowane w bazie danych, gdy wywoływana jest metoda "Zapisz()" repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Poniższy kod pobiera obiad, a następnie dodaje odpowiedź na zaproszenie do niego. Jest to realizowane przy użyciu kolekcji zbędne na obiad utworzony obiekt LINQ to SQL dla nas (ponieważ nie istnieje podstawowy klucz/klucz obcy relacji między tymi dwoma w bazie danych). Ta zmiana jest utrwalony w bazie danych jako nowy wiersz tabeli RSVP, gdy wywoływana jest metoda "Zapisz()" w repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Usuwanie przykładu

Poniższy kod pobiera istniejący obiekt obiad i oznaczy go do usunięcia. Gdy wywoływana jest metoda "Zapisz()" w repozytorium go spowoduje to zatwierdzenie usuwania w bazie danych:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrowanie walidację i logikę regułę biznesową, przy użyciu klasy modelu

Integrowanie sprawdzania poprawności i regułę biznesową, że logika jest kluczowym elementem każdej aplikacji, która działa z danymi.

#### <a name="schema-validation"></a>Sprawdzanie poprawności schematu

Po zdefiniowaniu klas modelu za pomocą LINQ to SQL projektanta typy danych właściwości w klasach modeli danych odpowiadają typy danych w tabeli bazy danych. Na przykład: Jeśli kolumny "EventDate" w tabeli kolacji jest typu "datetime", klasy modelu danych, które są tworzone w składniku LINQ to SQL jest typu "DateTime" (która jest wbudowana datatype .NET). Oznacza to, że otrzymasz błędów kompilacji, Jeśli spróbujesz przypisać liczbą całkowitą lub wartość logiczną do niej z poziomu kodu, a jego zgłosi błąd automatycznie przy próbie niejawnie przekonwertować typu ciąg nie jest ważna do niej w czasie wykonywania.

LINQ do SQL również automatycznie zostanie obsługuje ucieczki wartości SQL dla Ciebie, podczas korzystania z ciągów — co ułatwia ochronę przed atakami polegającymi na iniekcji SQL podczas korzystania z niego.

#### <a name="validation-and-business-rule-logic"></a>Sprawdzanie poprawności i logiki reguły biznesowej

Sprawdzanie poprawności schematu przydaje się jako pierwszy krok, ale jest rzadko wystarczające. Większość scenariuszy w rzeczywistych warunkach muszą mieć możliwość określenia bardziej rozbudowane logikę weryfikacji, może obejmować wiele właściwości, wykonanie kodu, która często mają świadomość stan modelu (na przykład: jest ona tworzona/zaktualizować/usunięte, lub w ramach stanu specyficznego dla domeny takie jak "zarchiwizowane"). Istnieje szereg różnych wzorców i struktur, które mogą służyć do definiowania i zastosować reguły sprawdzania poprawności do modelu klas, wiąże się z kilku .NET na podstawie struktury tam, które mogą służyć do w tym pomóc. Można użyć praktycznie dowolnego z nich w aplikacjach ASP.NET MVC.

Dla celów naszej aplikacji NerdDinner użyjemy wzorca stosunkowo proste i proste gdzie uwidaczniamy IsValid właściwości i metody GetRuleViolations() naszych obiad modelu obiektu. Właściwość IsValid zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy są prawidłowymi reguł biznesowych i sprawdzania poprawności. Metoda GetRuleViolations() zwróci listę wszystkich błędów, reguły.

Firma Microsoft będzie implementować IsValid i GetRuleViolations() dla modelu Dinner przez dodanie "klasy częściowej" do naszego projektu. Klasy częściowe może służyć do dodawania metody/właściwości/zdarzenia do klasy obsługiwane przez projektanta programu VS (na przykład klasy obiad generowanej przez LINQ to SQL projektanta) i zapobiec narzędzie z poświęcana dzięki naszemu kodowi. Możemy dodać nową klasę częściowe do naszego projektu, klikając prawym przyciskiem myszy w folderze \Models, a następnie wybierz polecenie "Dodaj nowy element". Firma Microsoft następnie wybierz szablon "Klasy" w oknie dialogowym "Dodaj nowy element" i nadaj mu nazwę Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Kliknięcie przycisku "Dodaj". Dodaj plik Dinner.cs do naszego projektu i otwórz go w środowisku IDE. Firma Microsoft może następnie zaimplementować podstawowe/sprawdzania poprawności reguły wymuszania framework za pomocą poniższego kodu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Kilka notatek dotyczących kodu powyżej:

- Klasa obiad jest poprzedzona słowem kluczowym "częściowej" — oznacza to, kod w nim zawarte będą połączone z klasą wygenerowanych/zachowane w składniku LINQ to SQL projektanta i skompilowane w ramach jednej klasy.
- Klasa RuleViolation jest klasa pomocnika, który zostanie dodany do projektu, który pozwala zapewnić więcej informacji na temat naruszenie reguły.
- Metoda Dinner.GetRuleViolations() powoduje, że nasze reguł biznesowych i sprawdzania poprawności, ma zostać obliczone (będzie wdrażamy je wkrótce). Następnie zwraca ponownie sekwencję obiektów RuleViolation, które będą dostarczać szczegółowe informacje o błędach reguły.
- Właściwość Dinner.IsValid udostępnia właściwość wygodne pomocnika, która wskazuje, czy obiekt obiad ma żadnych aktywnych RuleViolations. On aktywne można sprawdzić przez dewelopera w dowolnym momencie za pomocą obiektu obiad (i nie zgłaszała wyjątku).
- Metoda częściowa Dinner.OnValidate() jest hook zapewniająca LINQ to SQL umożliwiający otrzymywania powiadomień w dowolnym momencie obiekt obiad ma zostać utrwalone w bazie danych. Naszej implementacji OnValidate() powyżej gwarantuje, że Dinner ma nie RuleViolations, zanim zostaną zapisane. Jeśli jest w nieprawidłowym stanie zgłasza wyjątek, który spowoduje, że LINQ to SQL, aby przerwać transakcji.

To podejście zapewnia prostą strukturę, która integrujemy sprawdzania poprawności i reguł biznesowych do. Teraz Dodajmy poniższych reguł do naszych GetRuleViolations() metody:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Używamy "yield return" funkcji języka C# w celu zwrócenia sekwencji żadnych RuleViolations. Po pierwsze sprawdza reguły sześć powyżej wymusić po prostu, czy właściwości ciągu na naszych obiad nie może być o wartości null ani być pusta. Ostatnia reguła jest nieco bardziej interesująca i wywołania metody pomocnika PhoneValidator.IsValidNumber() czy możemy dodać do naszej projektu, aby sprawdzić, czy ContactPhone numer format dopasowania Dinner w kraju.

Możemy użyć. Obsługa wyrażeń regularnych przez sieć do zaimplementowania tej weryfikacji telefonicznej pomocy technicznej. Poniżej przedstawiono proste wdrażanie PhoneValidator, które możemy dodać do naszej projektu, który pozwala na dodawanie sprawdzania wzorzec wyrażenia regularnego specyficzne dla kraju:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Obsługa sprawdzania poprawności i naruszeń logiki biznesowej

Teraz, dodaliśmy powyżej sprawdzania poprawności i firm kodu reguły, każdym razem, gdy podejmowane są próby utworzenia lub zaktualizowania obiad, nasze reguł logiki weryfikacji będzie obliczane i wymuszane.

Programiści mogą pisać kod podobnie jak poniżej aktywnie ustalenia, czy obiekt obiad jest prawidłowy i pobranie listy wszystkich naruszeń w nim bez zgłaszania wyjątków:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Jeśli firma Microsoft podejmie próbę zapisania obiad w nieprawidłowym stanie, jeśli możemy wywołać metodę Zapisz() w DinnerRepository będzie zgłaszany wyjątek. Dzieje się tak, ponieważ LINQ to SQL automatycznie wywołuje metody częściowej naszych Dinner.OnValidate() przed zapisuje zmiany obiad i dodaliśmy kod do Dinner.OnValidate(), aby zgłosić wyjątek, jeśli ewentualne naruszenia reguły istnieje w Dinner. Możemy przechwycić ten wyjątek i reagować i odbierać listy naruszeń, aby rozwiązać problem:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Ponieważ naszych reguł biznesowych i sprawdzania poprawności są implementowane w ramach naszych warstwy modelu, a nie w warstwie interfejsu użytkownika, będzie można zastosować i stosować w przypadku wszystkich scenariuszy, w ramach naszej aplikacji. Firma Microsoft później zmienić lub dodać reguły biznesowe i mają cały kod działa z naszych obiektów obiad uznaje ich.

Może zmienić reguły biznesowe w jednym miejscu, bez konieczności zmiany emulatora ripple w całej aplikacji i logika interfejsu użytkownika jest znakiem dobrze napisane aplikacji i korzyści z platformy MVC pomaga zachęcać.

### <a name="next-step"></a>Następny krok

Teraz mamy modelu, który możemy użyć do zapytania i zaktualizuj naszej bazie danych.

Teraz Dodajmy kilka widoków i kontrolerów do projektu, który możemy użyć do tworzenia interfejsu użytkownika HTML środowisko wokół niej.

> [!div class="step-by-step"]
> [Poprzednie](create-a-database.md)
> [dalej](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
