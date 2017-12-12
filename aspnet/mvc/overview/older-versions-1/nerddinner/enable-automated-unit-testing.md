---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: "Włącz automatyczne testy jednostkowe | Dokumentacja firmy Microsoft"
author: microsoft
description: "Krok 12 pokazano, jak utworzyć zestaw testów jednostkowych automatycznych, który Sprawdź naszych funkcje NerdDinner i który będzie Prześlij nam zaufania, aby wprowadzić zmiany..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 1a4258054d90b2d5bcc06a63fb6f3b4673a4837d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="enable-automated-unit-testing"></a>Włącz automatyczne testy jednostkowe
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 12 bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 12 pokazano, jak utworzyć zestaw testów jednostkowych automatycznych, który Sprawdź naszych funkcje NerdDinner i który będzie Prześlij nam zaufania do wprowadzania zmian i ulepszeń do aplikacji w przyszłości.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner krok 12: Testy jednostkowe

Umożliwia tworzenie zestawu testów jednostkowych automatycznych, który Sprawdź naszych funkcje NerdDinner i który będzie Prześlij nam zaufania do wprowadzania zmian i ulepszeń do aplikacji w przyszłości.

### <a name="why-unit-test"></a>Dlaczego testu jednostkowego?

Na dysku do pracy rano co masz nagłym flash z inspirującymi o aplikacji, w którym pracujesz na. Okazuje się, że zmiany nie można zaimplementować spowoduje, że aplikacja znacznie lepszą. Może to być refaktoryzacji, czyści kod, dodaje nową funkcję lub rozwiązuje usterki.

Pytania, na które należy confronts po przyjeździe do komputera — "jak bezpieczne jest aby temu usprawnieniu?" Co zrobić, jeśli wprowadzenie zmian ma efekty uboczne lub coś dzieli? Zmiana może być prosty i przyjmuje tylko kilka minut, aby zaimplementować, ale co zrobić, jeśli trwa godziny ręcznie przetestować we wszystkich scenariuszach aplikacji? Co zrobić, Jeśli zapomnisz obejmuje scenariusz i przerwana aplikacja przechodzi w środowisku produkcyjnym? Sprawia to ulepszenie naprawdę warto nakładu pracy?

Testy jednostkowe automatycznych może udostępniać zabezpieczenie, umożliwiający stale ulepszanie aplikacji i przed obawiasz się kod, który użytkownik pracuje. Wykonywanie testów, które szybko sprawdzić, czy funkcje umożliwiają kodu bez obaw — i dają poprawiają użytkownik może w przeciwnym razie nie ma mieli świadomość doświadczenia o automatycznych. Ułatwiają również tworzenie rozwiązań, które są łatwiejsze do utrzymania i dłuższy okres istnienia — która prowadzi do znacznie wyższa zwrotu z inwestycji.

Platforma ASP.NET MVC ułatwia fizyczne do funkcjonalności aplikacji testów jednostkowych. Umożliwia również przepływ pracy Test Driven Development (TDD), który umożliwia programowanie oparte na wcześniejsze testowanie.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests projektu

Gdy utworzyliśmy naszej aplikacji NerdDinner na początku tego samouczka zostały możemy monit okno dialogowe z pytaniem, czy trzeba utworzyć jednostkowy projekt testowy, aby przejść wraz z projektu aplikacji:

![](enable-automated-unit-testing/_static/image1.png)

Firma Microsoft przechowywane "Tak, Utwórz jednostkowy projekt testowy" przycisk radiowy zaznaczono — co spowodowało w projekcie "NerdDinner.Tests" dodawany do naszego rozwiązania:

![](enable-automated-unit-testing/_static/image2.png)

Projekt NerdDinner.Tests odwołuje się do zestawu projektów aplikacji NerdDinner i pozwala na łatwe dodawanie zautomatyzowanych testów, aby sprawdzić funkcjonalność aplikacji.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Tworzenie testów jednostkowych dla klasy modelu nasze obiad

Dodajmy niektórych testów naszych NerdDinner.Tests projekt, który zweryfikować klasy obiad, tworzone budujemy nasze warstwy modelu.

Zaczniemy tworząc nowy folder w naszym projekt testowy o nazwie "Modele" gdzie będzie umieszczać testów związanych z modelu. Firma Microsoft będzie następnie kliknij prawym przyciskiem folder i wybierz **Add -&gt;nowego testu** polecenia menu. Zostanie wyświetlone okno dialogowe "Dodaj nowy Test".

Wybrano do tworzenia "Unit Test" i nadaj jej nazwę na "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Po kliknięciu przycisku "ok" przycisk Visual Studio spowoduje dodanie (i otworzyć) DinnerTest.cs plik do projektu:

![](enable-automated-unit-testing/_static/image4.png)

Domyślny szablon testu jednostki programu Visual Studio ma licznych płytkę kocioł kodu wewnątrz znaleźć nieco lepiej. Załóżmy Oczyść je tylko zawierać poniższy kod:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Atrybut [TestClass] w klasie DinnerTest powyżej identyfikuje ją jako klasa, która będzie zawierać testów, oraz opcjonalne badanie inicjowania i usuwania kodu. Firma Microsoft można zdefiniować testy znajdujące się w nim przez dodanie metody publiczne, których atrybut [TestMethod].

Poniżej przedstawiono pierwszy z dwóch testów, który zostanie dodany, które wykonuje klasy Nasze obiad. Pierwszego testu weryfikuje, czy naszych obiad nieprawidłowy nowe obiad zostanie utworzony bez wszystkich właściwości poprawnie ustawiona. Drugi test sprawdza naszych obiad jest prawidłowa, gdy obiad ma wszystkie właściwości prawidłowymi wartościami:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Można zauważyć powyżej czy nazwy naszych testów są bardzo jawnego (i nieco pełne). Są to robimy ponieważ możemy tworzenia setek lub tysięcy testów małych i chcemy ułatwić szybko określić opcje i zachowanie każdego z nich (szczególnie w przypadku czekamy na liście błędów w programie test runner). Nazwy testu powinno być nazwanym po funkcji, które są one testowania. Powyżej użyto "rzeczownik\_powinien\_zlecenie" Wzorzec nazewnictwa.

Firma Microsoft struktury testy za pomocą "AAA" testowania wzorca — oznaczającą "Rozmieść, Act, Assert":

- Rozmieść: Konfiguracja jednostki poddawana testom
- Działanie: Wykonywanie jednostki w ramach testu i przechwytywania wyników
- Assert: Sprawdzić działanie

Pisząc testy chcemy, aby uniknąć poszczególne testy są zbyt szybko. Zamiast tego każdy test należy sprawdzić tylko jednej koncepcji (co stanie się on łatwiej wskazać przyczynę niepowodzenia). Dobrą praktyką jest spróbuj i może zawierać tylko jeden assert instrukcji dla każdego z testów. Jeśli masz więcej niż jeden assert instrukcji w metodzie testowej, upewnij się, że są one wszystkie używane do testowania tego samego pojęcia. W razie wątpliwości, należy innego testu.

### <a name="running-tests"></a>Uruchamianie testów

Visual Studio 2008 Professional (i nowsze wersje) zawiera wbudowane uruchamiający, który może służyć do uruchamiania programu Visual Studio jednostki Test projektów w środowisku IDE. Firma Microsoft może wybrać **Test -&gt;Uruchom -&gt;wszystkie testy z rozwiązania** polecenia menu (lub typu Ctrl R, A) do wszystkich naszych testów jednostkowych. Lub też możemy Ustaw naszych kursor w metodzie klasy lub testowym specyficznego testu i użyj **Test -&gt;Uruchom -&gt;testy z bieżącego kontekstu** polecenia menu (lub typ Ctrl R T) do uruchomienia podzbioru testów jednostkowych.

Teraz umieść naszych kursora w klasie DinnerTest a wpisz "Ctrl R, T" uruchamianie dwóch testów, wystarczy zdefiniowanego. Gdy firma Microsoft tym okno "Wyników testu" będą wyświetlane w programie Visual Studio i zajmiemy się tym wyniki uruchomienia wymienionych w niej naszych testu:

![](enable-automated-unit-testing/_static/image5.png)

*Uwaga: Okno wyników testu VS nie są wyświetlane kolumny Nazwa klasy domyślnie. Możesz dodać to przez kliknięcie prawym przyciskiem myszy w oknie wyników testu i za pomocą polecenia menu Dodaj/Usuń kolumny.*

Nasze testy dwóch trwało tylko ułamek sekund do uruchomienia —, a użytkownik może zobacz obydwie przekazany. Firma Microsoft teraz Przejdź i rozszerzyć je, tworząc dodatkowe testy, które sprawdzić poprawności określonej reguły, a także opisano dwie metody pomocnika - IsUserHost() i IsUserRegisterd() — dodaną do klasy obiad. Posiadanie tych testów w klasie obiad, spowoduje to znacznie łatwiejsze i bezpieczniejsze dodać nowe reguły biznesowe i sprawdzanie poprawności do niego w przyszłości. Możemy dodać nowe logiki reguły do obiad, a następnie w ciągu kilku sekund Sprawdź, czy go nie uszkodzony żadnego z naszych poprzednie funkcjonalności logiki.

Zwróć uwagę, jak przy użyciu nazwy opisowej testu ułatwia szybkie zrozumieć, co sprawdza każdego z testów. Zaleca się **narzędzia —&gt;opcje** polecenie otwarcia Test Tools —&gt;ekranie konfiguracji wykonywania testów i sprawdzanie, czy "dwukrotne kliknięcie wyniku testu jednostkowego nie powiodło się lub niejednoznaczny Wyświetla pole wyboru punktu awarii w teście". Pozwoli to na kliknij dwukrotnie z powodu błędu w oknie wyników testu i od razu przejść do awarii potwierdzenia.

### <a name="creating-dinnerscontroller-unit-tests"></a>Tworzenie testów jednostkowych DinnersController

Teraz Utwórzmy niektórych testów jednostkowych, weryfikujących naszej funkcji DinnersController. Firma Microsoft będzie uruchomić, klikając prawym przyciskiem myszy folder "Kontrolery" w ramach naszych projektu testowego, a następnie wybierz pozycję **Add -&gt;nowego testu** polecenia menu. Firma Microsoft będzie utworzyć "Unit Test" i nadaj jej nazwę na "DinnersControllerTest.cs".

Utwórz dwie metody testowe, które Sprawdź metody akcji Details() na DinnersController. Pierwszy sprawdzi, czy widok jest zwracany, jeśli istniejące obiad jest wymagane. Drugi sprawdzi, czy widok "NotFound" jest zwracany, jeśli wymagany jest obiad nieistniejącą:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Powyższy kod kompiluje oczyszczania. Uruchamianych testów, jednak mogą zakończyć się niepowodzeniem:

![](enable-automated-unit-testing/_static/image6.png)

Przyjrzymy się komunikaty o błędach, możemy Zobacz, czy został powodu testy nie powiodło się, ponieważ klasy Nasze DinnersRepository nie mógł połączyć się z bazą danych. Naszej aplikacji NerdDinner używa parametrów połączenia do lokalnego pliku programu SQL Server Express, który znajduje się w obszarze \App\_katalog danych NerdDinner projekt aplikacji. Ponieważ naszych projektu NerdDinner.Tests kompiluje i uruchamia w innym katalogu następnie projekt aplikacji, lokalizację ścieżki względnej naszych ciągu połączenia jest niepoprawna.

Firma Microsoft *można* rozwiązać ten problem przez skopiowanie pliku bazy danych programu SQL Express do naszej projektu testowego, a następnie dodaj parametry połączenia testowego odpowiednie do niego w pliku App.config projektu testu w naszym. Jak to powyższe testy odblokowany i uruchomiona.

Kod przy użyciu rzeczywistego bazy danych, testy jednostkowe łączy z nim jednak wiele wyzwań. W szczególności:

- Znacznie zmniejsza czas wykonania testów jednostkowych. Już trwa uruchamianie testów, tym mniej prawdopodobne są często je wykonać. W idealnym przypadku ma testy jednostkowe mógł działać w sekundach — i została ona być coś, co należy zrobić, naturalny sposób jako Kompilowanie projektu.
- Utrudnia to ustawień i czyszczenia logiki w ramach testów. Chcesz, aby każdy test jednostkowy być izolowane i niezależnie od innych użytkowników (z nie efektów ubocznych lub zależności). Podczas pracy w rzeczywistych bazy danych należy zachować ostrożność, stanu i resetuje je między testy.

Przyjrzyjmy się wzorca projektowego o nazwie "iniekcji zależności", które mogą pomóc nam obejścia tych problemów i uniknąć konieczności korzystanie z testów rzeczywistych bazy danych.

### <a name="dependency-injection"></a>Iniekcji zależności

W tej chwili DinnersController jest ściśle "powiązane" do klasy DinnerRepository. "Sprzężenia" odwołuje się do sytuacji, gdy klasa jawnie zależy od innej klasy funkcjonowania:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Ponieważ klasa DinnerRepository wymaga dostępu do bazy danych, silnie sprzężonego zależności DinnersController klasa ma na końcach DinnerRepository się wymagające nam w celu zapewnienia bazy danych dla metody akcji DinnersController do sprawdzenia.

Można pobrać obejścia tego problemu przez zastosowanie wzorca projektowego o nazwie "iniekcji zależności" — czyli podejście, gdzie zależności (takich jak klasy repozytorium, które zapewniają dostęp do danych) nie jest już niejawnie są tworzone w ramach klasy, które z nich korzystają. Zamiast tego zależności można jawnie przekazany do klasy, która używa ich przy użyciu argumentów konstruktora. Jeśli zależności są zdefiniowane przy użyciu interfejsów, następnie mamy elastyczność do przekazania w implementacjach zależności "fałszywych" scenariusze testowania jednostki. Pozwala na tworzenie implementacje zależności specyficzne dla testów, które faktycznie nie wymagają dostępu do bazy danych.

Aby wyświetlić to działanie, umożliwia wdrożenie iniekcji zależności z naszych DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Wyodrębnianie IDinnerRepository — interfejs

Naszym pierwszym krokiem będzie można utworzyć nowy interfejs IDinnerRepository, który hermetyzuje kontrakt repozytorium, który naszych kontrolery wymagają pobierania i aktualizowania kolacji.

Firma Microsoft można ręcznie zdefiniować tego interfejsu kontraktu prawym przyciskiem myszy \Models folder, a następnie wybierając **Add -&gt;nowy element** polecenia menu i tworzenie nowy interfejs o nazwie IDinnerRepository.cs.

Firma Microsoft możesz też Użyj automatycznie refaktoryzacji narzędzia wbudowane w program Visual Studio Professional (i nowsze wersje) do wyodrębniania i utworzyć interfejs dla nas z naszych istniejącej klasy DinnerRepository. Aby wyodrębnić ten interfejs, za pomocą programu VS, po prostu umieść kursor w edytorze tekstu w klasie DinnerRepository, a następnie kliknij prawym przyciskiem myszy i wybierz **Zrefaktoryzuj -&gt;wyodrębniania interfejsu** polecenie:

![](enable-automated-unit-testing/_static/image7.png)

Spowoduje to Uruchom okno dialogowe "Wyodrębnij Interface" i monitować nam Nazwa interfejsu, aby utworzyć. Go spowoduje domyślne ustawienie IDinnerRepository i automatycznie wybrać wszystkie metody publiczne na istniejącej klasy DinnerRepository, aby dodać do interfejsu:

![](enable-automated-unit-testing/_static/image8.png)

Kliknięcie przycisku "ok", Visual Studio spowoduje dodanie nowego interfejsu IDinnerRepository do naszej aplikacji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

I naszych istniejącej klasy DinnerRepository zostaną zaktualizowane, aby ją implementuje interfejs:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualizowanie DinnersController w celu obsługi iniekcji konstruktora

Firma Microsoft będzie teraz aktualizować klasę DinnersController, aby korzystać z nowego interfejsu.

Obecnie DinnersController jest ustalony tak, aby jego pola "dinnerRepository" jest zawsze klasy DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Zmienimy go tak, aby w polu "dinnerRepository" jest typu IDinnerRepository zamiast DinnerRepository. Następnie dodamy dwa konstruktory DinnersController publiczne. Jeden z konstruktorów umożliwia IDinnerRepository przekazywany jako argument. Druga to domyślny konstruktor używający naszych istniejącej implementacji DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Ponieważ ASP.NET MVC domyślnie tworzy klasy kontrolera przy użyciu domyślnych konstruktorów, naszych DinnersController w czasie wykonywania będzie klasa DinnerRepository służy do uzyskania dostępu do danych.

Mamy teraz zaktualizować Nasze testy jednostek, do przekazania w implementacji repozytorium obiad "fałszywych" za pomocą parametru konstruktora. To repozytorium obiad "fałszywych" nie wymaga dostępu do rzeczywistego bazy danych, a zamiast tego użyje danych przykładowych w pamięci.

#### <a name="creating-the-fakedinnerrepository-class"></a>Tworzenie klasy FakeDinnerRepository

Ta funkcja pozwala utworzyć klasę FakeDinnerRepository.

Firma Microsoft będzie Rozpocznij od utworzenia katalogu "Elementów sztucznych" w ramach naszych NerdDinner.Tests projektu, a następnie dodaj nową klasę FakeDinnerRepository do niego (kliknij prawym przyciskiem myszy folder, a następnie wybierz pozycję **Add -&gt;nowa klasa**):

![](enable-automated-unit-testing/_static/image9.png)

Będziemy informować kod, aby FakeDinnerRepository klasa implementuje interfejs IDinnerRepository. Następnie możemy kliknij prawym przyciskiem myszy na nim i wybierz polecenie "Implementuje interfejs IDinnerRepository" z menu kontekstowego:

![](enable-automated-unit-testing/_static/image10.png)

Spowoduje to Visual Studio, aby automatycznie dodać wszystkie elementy członkowskie interfejsu IDinnerRepository do naszej klasy FakeDinnerRepository z domyślnej implementacji "stub out":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Następnie zaktualizowania implementacji FakeDinnerRepository pracy wylogowuje na liście w pamięci&lt;obiad&gt; kolekcji do niej przekazany jako argument konstruktora:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Mamy teraz fałszywych implementację IDinnerRepository, która nie wymaga bazy danych, a zamiast tego można pracować poza listę obiad obiektów w pamięci.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Przy użyciu FakeDinnerRepository przy użyciu testów jednostkowych

Teraz wróć do testów jednostkowych DinnersController, których nie powiodła się wcześniej, ponieważ w bazie danych nie była dostępna. Aktualizujemy metody testu FakeDinnerRepository wypełniane przy użyciu przykładowych danych obiad w pamięci, aby DinnersController przy użyciu kodu poniżej:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

I teraz możemy uruchomienie tych testów zarówno przekazują:

![](enable-automated-unit-testing/_static/image11.png)

Najlepsze ich podjąć tylko ułamek sekund do uruchomienia i nie wymagają wszelka logika skomplikowane instalacji/czyszczenia. Możemy teraz testu jednostkowego wszystkich naszych DinnersController metody kodu działania (takie jak lista, stronicowanie, uzyskać szczegółowe informacje, tworzenie, aktualizowanie i usuwanie) bez konieczności kiedykolwiek nawiązać połączenia z bazą danych rzeczywistych.

| **Temat po stronie: Struktury iniekcji zależności** |
| --- |
| Iniekcji zależności ręczne (np. pracujemy nad) działa prawidłowo, ale coraz trudniej Obsługa jako liczba zależności i zwiększa składników w aplikacji. Istnieje kilka struktur iniekcji zależności dla platformy .NET, które mogą ułatwić zapewniają większą elastyczność zarządzania zależności. Tych struktur, nazywanych także kontenery "Inwersja kontroli" (IoC) zapewniają mechanizmy, które umożliwiają dodatkowy poziom obsługi konfiguracji dotyczące określania i przekazywanie zależności do obiektów w czasie wykonywania (w większości przypadków przy użyciu iniekcji konstruktora ). Niektóre z bardziej popularnych iniekcji zależności OSS / include platform Inwersja kontroli w .NET: AutoFac, Ninject Spring.NET, StructureMap i Windsor. ASP.NET MVC udostępnia rozszerzalności interfejsów API, które umożliwiają deweloperom uczestniczyć w rozdzielczość i konkretyzacji kontrolerów, a co pozwala iniekcji zależności / platform Inwersja kontroli powinny zostać prawidłowo włączone w ramach tego procesu. Przy użyciu platformy Podpisane/Inwersja kontroli będzie również umożliwiają usunięcie domyślnego konstruktora z naszych DinnersController — która całkowicie usunąć sprzężenia między nim a DinnerRepositorys. Firma Microsoft nie będzie używany iniekcji zależności / framework Inwersja kontroli z naszą aplikacją NerdDinner. Ale coś, co możemy można rozważyć w przyszłości, jeśli zwiększył Baza kodów NerdDinner i możliwości. |

### <a name="creating-edit-action-unit-tests"></a>Tworzenie testów jednostkowych akcji edycji

Teraz Utwórzmy niektóre testy jednostek, które sprawdzają funkcje edycji DinnersController. Zaczniemy testując wersji HTTP GET nasze działania edycji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Utworzymy test, który sprawdza renderowania widoku obsługiwanej przez obiekt DinnerFormViewModel Wstecz, gdy wymagane jest prawidłowe obiad:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Uruchomienie testu, jednak firma Microsoft znajdziesz będzie ponieważ wyjątek odwołania zerowego jest zgłaszany, gdy właściwość User.Identity.Name sprawdzania Dinner.IsHostedBy() uzyskuje dostęp do metody edycji.

Obiekt użytkownika w klasie podstawowej kontrolera hermetyzuje szczegółów dotyczących zalogowanego użytkownika i wypełnione przez platformę ASP.NET MVC, podczas tworzenia kontrolera w czasie wykonywania. Ponieważ testujemy DinnersController poza środowiskiem serwera sieci web, obiekt użytkownika nie jest ustawiony (stąd wyjątek odwołania zerowego).

### <a name="mocking-the-useridentityname-property"></a>Właściwość User.Identity.Name mocking

Struktury mocking ułatwić testowanie włączając firmie Microsoft w celu dynamicznego tworzenia fałszywych wersji obiektów zależnych, które obsługują Nasze testy. Na przykład zastosowanie mocking framework w naszym teście akcji edycji można dynamicznie utworzyć obiekt użytkownika, który naszych DinnersController można użyć do wyszukania symulowane nazwy użytkownika. Pozwoli to uniknąć odwołanie o wartości null z jest generowany, gdy przeprowadzana naszym teście.

Istnieje wiele .NET mocking struktury, których można użyć z platformą ASP.NET MVC (wyświetlana lista z nich w tym miejscu: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Do testowania aplikacji NerdDinner użyjemy typu open source mocking framework o nazwie "Moq", który można pobrać bezpłatnie z [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Po pobraniu, dodamy odwołanie w naszym NerdDinner.Tests projektu do zestawu Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Następnie dodamy "CreateDinnersControllerAs(username)" metody pomocnika do naszej klasy testowej, która przyjmuje jako parametr, a które nazwę użytkownika, a następnie "mocks" User.Identity.Name właściwość w wystąpieniu DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Powyżej użyto Moq Aby utworzyć obiekt makiety elementów sztucznych obiektu kontekstem ControllerContext (czyli ASP.NET MVC przekazuje do klas kontrolera na uwidocznienie obiektów czasu wykonywania, takie jak użytkownika, żądania, odpowiedzi i sesji). Wywołania metody "SetupGet" na makiety, aby wskazać, że właściwość HttpContext.User.Identity.Name kontekstem ControllerContext powinien zwrócić przekazane do metody pomocnika ciąg nazwy użytkownika.

Firma Microsoft może mock dowolną liczbę kontekstem ControllerContext właściwości i metody. Aby zilustrować to również zostały dodane wywołanie SetupGet() dla właściwości Request.IsAuthenticated (której faktycznie nie jest potrzebny do testów poniżej — ale umożliwiająca zilustrować, jak można mock właściwości żądania). Gdy firma Microsoft gotowe możemy przypisać wystąpienia makiety kontekstem ControllerContext DinnersController zwraca naszych metody pomocnika.

Możemy teraz zapisać testy jednostek, które ta metoda pomocnika do testowania scenariuszy edycji obejmujące różnych użytkowników:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

I teraz możemy uruchamianych testów przekazują:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() scenariuszy testowania

Utworzyliśmy testy, które obejmują wersji HTTP GET akcji edycji. Teraz Utwórzmy niektórych testów, które pozwalają sprawdzić wersji protokołu HTTP POST akcji edycji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Nowe interesujące scenariuszu testowania firmie Microsoft w celu obsługi przy użyciu tej metody akcji jest jej użycie metody pomocnika UpdateModel() w klasie podstawowej kontrolera. Ta metoda pomocnika jest używany do powiązania wartości post formularza do naszej obiad wystąpienia obiektu.

Poniżej przedstawiono dwa testy, które pokazuje, jak firma Microsoft mogła dostarczyć Formularz ogłoszony wartości UpdateModel() metody pomocnika do użycia. Firma Microsoft będzie to zrobić, tworzenie i wypełnianie obiektu FormCollection, a następnie przypisać je do właściwości "ValueProvider" na kontrolerze.

Pierwszego testu sprawdza, czy na pomyślne Zapisz przeglądarki jest przekierowywany do wykonania akcji details. Drugi test sprawdza, czy podczas nieprawidłowe dane wejściowe akcji zostanie ponownie widoku edycji ponownie z komunikatem o błędzie.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testowanie Wrap-Up

Firma Microsoft zostały objęte podstawowe koncepcje związane z klas kontrolera testowania jednostek. Możemy użyć tych metod, można łatwo utworzyć setki proste testy weryfikujące działanie aplikacji.

Ponieważ naszych kontrolera i testy modelu nie wymagają rzeczywistych bazy danych, są one bardzo szybki i łatwy do uruchomienia. Firma Microsoft będzie można wykonać setki testów automatycznych w sekundach i natychmiast Uzyskaj opinie określające, czy zmiany, które wprowadziliśmy spowodowało przerwanie coś. Dzięki temu, podaj nam zaufania do stale poprawy refaktoryzacji i Dostosuj naszej aplikacji.

Firma Microsoft pasuje do testowania jako ostatni tematu w tym rozdziale —, ale ponieważ testy są coś zrobić pod koniec procesu projektowania! Przeciwnie należy zapisać testów automatycznych możliwie jak najszybciej w procesie tworzenia aplikacji. Wykonanie tej tak pozwala uzyskać natychmiast uzyskuje opinie podczas opracowywania, pomaga odnoszącej zastanowić scenariuszy przypadków zastosowań aplikacji i ułatwia projektowanie aplikacji z czystą Układanie warstwowo i sprzężenia pamiętać.

Nowsze rozdziału w książce przedstawimy testu Driven Development (TDD) i jak z niego korzystać z platformy ASP.NET MVC. TDD jest iteracyjne praktyk kodowania, gdzie najpierw zapisać testów, które będą spełniały warunki wynikowy kod. Z TDD rozpoczęciem każdej funkcji, tworząc test, który sprawdza, czy funkcje, które zamierzasz wdrożyć. Pisanie jednostki test ułatwia najpierw upewnij się, że wyraźnie rozumiesz funkcji i jak powinien pracować. Tylko po testu są zapisywane (i upewnieniu się, że nie jest on) czy, a następnie wdrożyć sprawdza rzeczywistej funkcji testu. Ponieważ został już poświęconego czasu planowania jak funkcja powinien działać w przypadku użycia, trzeba będzie lepiej zrozumieć wymagania i jak najlepiej ich wdrażania. Po zakończeniu wdrożenia można ponownie uruchomić test — i natychmiast uzyskuje opinie, aby uzyskać Określa, czy ta funkcja działa prawidłowo. Firma Microsoft będzie obejmować TDD więcej w rozdziale 10.

### <a name="next-step"></a>Następny krok

Niektóre końcowego zawijania komentarze.

>[!div class="step-by-step"]
[Poprzednie](use-ajax-to-implement-mapping-scenarios.md)
[dalej](nerddinner-wrap-up.md)
