---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Włączanie automatycznych testów jednostkowych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 12 pokazuje, jak tworzyć pakiet testów automatycznych jednostkowych, Sprawdź nasze funkcje NerdDinner, a które zapewniło zaufania, aby wprowadzić zmiany...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819414"
---
<a name="enable-automated-unit-testing"></a>Włączanie automatycznych testów jednostkowych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 12 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Kroku 12 pokazuje, jak opracować pakiet testów automatycznych jednostkowych, Sprawdź nasze funkcje NerdDinner, a które zapewniło zaufania do wprowadzania zmian i usprawnień do aplikacji w przyszłości.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner krok 12: Testy jednostkowe

Możemy tworzyć pakiet testów automatycznych jednostkowych, Sprawdź nasze funkcje NerdDinner, a które zapewniło zaufania do wprowadzania zmian i usprawnień do aplikacji w przyszłości.

### <a name="why-unit-test"></a>Dlaczego Test jednostkowy?

Na dysku do pracy jednym rano masz nagłe flash programu o aplikacji, którą pracujesz. Zorientujesz się, że w przypadku zmiany, które można zaimplementować, aby ułatwić aplikacji znacznie lepiej. Może to być refaktoryzacji, która czyści kod, dodaje nową funkcję lub naprawia błąd.

To pytanie, na które confronts, po przyjeździe do komputera — "jak bezpieczne jest zapewnienie to ulepszenie?" Co w przypadku wprowadzania zmian ma efekty uboczne lub przerywa coś? Zmiany mogą być proste i trwać tylko kilka minut, aby zaimplementować, ale co zrobić, jeśli zajmuje godzin, aby ręcznie przetestować wszystkie scenariusze aplikacji? Co zrobić, Jeśli zapomnisz obejmuje scenariusz i niedziałająca aplikacja przechodzi w środowisku produkcyjnym? Sprawia to ulepszenie naprawdę warto wszystkich wysiłków?

Automatyczne testy jednostkowe można podać projektowi, która pozwala na stale udoskonalaj swoje aplikacje i uniknąć obawiasz się kod, którą pracujesz. Posiadanie testy automatyczne, które szybko sprawdzić, czy funkcje umożliwiają pomogła Ci w celu ulepszenia, użytkownik może w przeciwnym razie nie mają uznało, doświadczenia i kodu bez obaw — wykonywanie. Ułatwiają również tworzenie rozwiązania, które będzie łatwiejszy w utrzymaniu i dłuższy okres istnienia — które potencjalnych klientów, aby znacznie wyższa zwrotu z inwestycji.

Platforma ASP.NET MVC ułatwia naturalna funkcji aplikacji testów jednostkowych. Umożliwia ona także testów opartych na rozwój (TDD) przepływu pracy, który umożliwia oparte na rozwoju pierwszego badania.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

Wtedy stworzyliśmy naszej aplikacji NerdDinner na początku tego samouczka będziemy pojawiał się monit z pytaniem, czy Chcieliśmy, aby utworzyć projekt testu jednostkowego nikogo projekt aplikacji do okna dialogowego:

![](enable-automated-unit-testing/_static/image1.png)

Firma Microsoft przechowywane "Tak, Utwórz projekt testu jednostkowego" zaznaczony przycisk radiowy — co spowodowało w projekcie "NerdDinner.Tests" dodawany do naszego rozwiązania:

![](enable-automated-unit-testing/_static/image2.png)

Projekt NerdDinner.Tests odwołuje się do zestawu projektów aplikacji NerdDinner i pozwala na łatwe dodawanie testów automatycznych, aby sprawdzić działanie aplikacji.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Tworzenie testów jednostkowych dla naszych obiad klasy modelu

Dodajmy niektóre testy do naszego projektu NerdDinner.Tests, który zweryfikować obiad klasy, którą utworzyliśmy, gdy opracowaliśmy nasze warstwy modelu.

Zaczniemy, tworząc nowy folder w projekcie testu o nazwie "Modele", gdzie będzie umieszczać Nasze testy związane z modelu. Następnie utworzymy kliknij prawym przyciskiem myszy w folderze i wybierz **Add -&gt;nowy Test** polecenia menu. Zostanie wyświetlone okno dialogowe "Dodaj nowy Test".

Wybraliśmy tworzenie "Testu jednostkowego" i nadaj mu nazwę "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Po kliknięciu przycisku "ok" Visual Studio Dodaj (i otworzyć) DinnerTest.cs plik do projektu:

![](enable-automated-unit-testing/_static/image4.png)

Domyślny szablon testu jednostki programu Visual Studio zawiera bunch kodu kocioł tablicy znajdujący się w nim, który można znaleźć nieco nieuporządkowane. Teraz oczyścić je tylko zawierać poniższy kod:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Atrybut [TestClass] w klasie DinnerTest powyżej identyfikuje je jako klasę, która będzie zawierać testów, oraz opcjonalne badanie inicjowania i kod wirtualnego. Firma Microsoft może zdefiniować testy w nim, przez dodanie metody publiczne, których atrybut [TestMethod].

Poniżej przedstawiono to pierwsza z dwóch testów, które dodamy, które korzystają z naszych klasy firmy Dinner. Pierwszy test sprawdza, nasze obiad jest nieprawidłowy, jeśli nowe obiad została utworzona bez wszystkich właściwości prawidłowo ustawione. Drugi test sprawdza, czy naszych obiad prawidłowe po obiad ma wszystkie właściwości ustaw prawidłowymi wartościami:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Powyżej zauważysz, że nasze nazwy testu są bardzo jawne (i nieco pełne). Robimy to, ponieważ firma Microsoft może się to zakończyć tworzenie setek lub tysięcy testów małych i chcemy ułatwić szybko określić przeznaczenie i zachowanie każdego z nich (szczególnie w przypadku czekamy na liście błędów w programie test runner). Powinien zostać nazwany nazwy testów po funkcje, które poddawanego testom. Powyżej użyto "rzeczownik\_powinien\_czasownik" wzorca nazewnictwa.

Firma Microsoft struktury testów za pomocą testowania wzorca — co znaczyło "Rozmieść, Act, Asercja" "AAA":

- Rozmieść: Konfigurowanie jednostki poddawana testom
- Ustawa: Wykonywanie jednostki w ramach testu i przechwytywania wyników
- Asercja: Zweryfikować zachowanie

Podczas pisania testów, które chcemy, aby uniknąć indywidualnych testów są zbyt dużo. Zamiast tego każdy test powinien sprawdzić tylko jednego koncepcja (co spowoduje, że jej znacznie łatwiejsze w określeniu przyczyny awarii). Dobrą praktyką jest spróbuj i może zawierać tylko jeden assert instrukcji dla każdego testu. Jeśli masz więcej niż jedną asercję instrukcji w metodzie testowej, upewnij się, że są one wszystkie używane do testowania tego samego pojęcia. W razie wątpliwości należy innego testu.

### <a name="running-tests"></a>Uruchamianie testów

Visual Studio 2008 Professional (i nowsze wersje) zawiera wbudowane uruchamiający, który może służyć do uruchamiania jednostki Test programu Visual Studio projektów związanych z poziomu środowiska IDE. Można wybrać **Test -&gt;Uruchom -&gt;wszystkie testy w rozwiązaniu** polecenia menu (lub typu Ctrl R, A) należy uruchomić wszystkie nasze testy jednostkowe. Lub też firma Microsoft Ustaw naszych kursor wewnątrz metody klasy lub test testów określonych i użyj **Test -&gt;Uruchom -&gt;testy z bieżącego kontekstu** polecenia menu (lub typu Ctrl R, T) do uruchamiania podzestaw testów jednostkowych.

Umożliwia umieszczanie naszych kursora w obrębie klasy DinnerTest i wpisz "Ctrl R, T", aby testy wykonywania dwóch, które właśnie zdefiniowane. Gdy odbywa się to okno "Wyniki testu" będą wyświetlane w programie Visual Studio, a zobaczymy wyników naszego testu wymienionych w nim:

![](enable-automated-unit-testing/_static/image5.png)

*Uwaga: Programu VS wyniki testów-okno nie są wyświetlane w kolumnie Nazwa klasy domyślnie. Możesz dodać dziennik, kliknij prawym przyciskiem myszy w oknie wyników badań i za pomocą polecenia menu Dodaj/Usuń kolumny.*

Nasze dwa testy trwały za ułamek chwilę, aby uruchomić — i jak można znaleźć zarówno przekazywały. Firma Microsoft jest teraz przejść na i rozszerzać je, tworząc dodatkowe testy, które sprawdzić poprawności określonej reguły, a także obejmują dwie metody pomocnika - IsUserHost() i IsUserRegisterd() — dodaną do klasy firmy Dinner. Posiadanie tych testów w miejscu dla klasy firmy Dinner, spowoduje to znacznie łatwiejsze i bezpieczniejsze dodać nowe reguły biznesowe i sprawdzania poprawności do niego w przyszłości. Firma Microsoft Dodaj naszej nowej logiki reguły do obiad, a następnie w ciągu kilku sekund Sprawdź, czy go nie zostało przerwane żadnego z naszych poprzedniej funkcji logic.

Zwróć uwagę, jak przy użyciu nazwy, opisu testu można łatwo szybko zrozumieć, co sprawdza każdy test. Zaleca się **narzędzia -&gt;opcje** polecenia menu, otwierając Test Tools —&gt;ekran konfiguracji wykonywania testów i sprawdzanie, czy "dwukrotne kliknięcie wyniku testu jednostkowego nie powiodło się lub niejednoznaczny Wyświetla pole wyboru punktu awarii w teście". Pozwoli to kliknij dwukrotnie błąd w oknie wyników testu i przejść od razu z niepowodzeniem asercji.

### <a name="creating-dinnerscontroller-unit-tests"></a>Tworzenie testów jednostkowych DinnersController

Utwórzmy teraz niektóre testy jednostkowe, które Sprawdź nasze funkcje DinnersController. Firma Microsoft będzie uruchomić, klikając prawym przyciskiem myszy w folderze "Kontrolerów" w projekcie testów, a następnie kliknij **Add -&gt;nowy Test** polecenia menu. Utworzymy "Unit Test" i nadaj mu nazwę "DinnersControllerTest.cs".

Utworzymy dwie metody testowe, weryfikujące metody akcji Details() na DinnersController. Pierwszy sprawdzi, czy widok jest zwracany zleconą obiad z istniejących. Drugi sprawdzi, czy widok "NotFound" jest zwracany, gdy zażądano obiad nieistniejącej:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Powyższy kod kompiluje się czyszczenie. Uruchamiania testów, jednak także zakończyć się niepowodzeniem:

![](enable-automated-unit-testing/_static/image6.png)

Jeśli przyjrzymy się komunikaty o błędach, zobaczymy, że został powodu testy nie powiodło się, ponieważ klasy Nasze DinnersRepository nie nawiązał połączenia z bazą danych. Nasza aplikacja NerdDinner używa parametrów połączenia do lokalnego pliku programu SQL Server Express, który znajduje się w obszarze \App\_danych katalogu projektu aplikacji NerdDinner. Ponieważ projekcie NerdDinner.Tests kompiluje i uruchamia w innym katalogu następnie projekt aplikacji, lokalizację ścieżki względnej nasze parametry połączenia jest niepoprawny.

Firma Microsoft *można* rozwiązać ten problem przez skopiowanie pliku bazy danych programu SQL Express do naszego projektu testu, a następnie dodać do niego parametry połączenia usługi odpowiednich testów w pliku App.config projektu testu w naszym. To otrzymamy powyższych testów zostało odblokowane i uruchomione.

Testy jednostkowe kodu przy użyciu rzeczywistego bazy danych, jednak wnosi wiele wyzwań. W szczególności:

- Znacznie zmniejsza czas wykonania testów jednostkowych. Tym dłużej trwa do uruchomienia testów, mniej prawdopodobne, zostaną wykonane za pomocą często. Najlepiej ma testy jednostkowe, aby można było można uruchomić w ciągu kilku sekund — i być coś, co możesz zrobić, naturalny sposób jako kompilowania projektu.
- Utrudnia to logiki instalacyjne i czyszczące w ramach testów. Chcesz, aby każdy test jednostkowy być izolowane, niezależnie od innych (z nie efektów ubocznych lub zależności). Podczas pracy w bazie danych rzeczywistych należy zachować ostrożność, stanu i zresetowanie jej między testami.

Przyjrzyjmy się wzorca projektowego o nazwie "wstrzykiwanie zależności", które mogą pomóc nam obejścia tych problemów i uniknąć konieczności rzeczywistego bazy danych za pomocą naszych testach.

### <a name="dependency-injection"></a>Wstrzykiwanie zależności

Teraz DinnersController jest "ściśle" do klasy DinnerRepository. "Sprzężenia" odwołuje się do sytuacji, w których klasa jawnie opiera się na innej klasy do działania:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Ponieważ klasa DinnerRepository wymaga dostępu do bazy danych, ściśle sprzężonych zależności DinnersController klasa ma na końcach DinnerRepository się konieczności byliśmy bazy danych w kolejności dla metody akcji DinnersController, który ma zostać przetestowana.

Firma Microsoft można uzyskać wokół tego zatrudniająca wzorca projektowego o nazwie "wstrzykiwanie zależności" — czyli podejścia, gdzie (na przykład klasy repozytorium, które zapewniają dostęp do danych) nie jest już niejawnie tworzenia zależności w obrębie klasy, które z nich korzystają. Zamiast tego zależności mogą być jawnie przekazywane do klasy, która korzysta z nich przy użyciu argumentów konstruktora. Jeśli zależności są zdefiniowane przy użyciu interfejsów, następnie mamy elastyczność, aby przekazać zależności "fałszywych" implementacje dla scenariuszy testowania jednostek. Pozwala to nam do utworzenia implementacji specyficznych dla testu zależności, które faktycznie nie wymagają dostępu do bazy danych.

Aby to zobaczyć w działaniu, umożliwia Implementowanie wstrzykiwanie zależności za pomocą naszych DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Wyodrębnianie interfejsu IDinnerRepository

Naszym pierwszym krokiem będzie utworzenie nowego interfejsu IDinnerRepository, który hermetyzuje kontrakt repozytorium, który naszym kontrolery wymagają pobierania i aktualizowania kolacji.

Firma Microsoft można zdefiniować ten kontrakt interfejsu ręcznie, klikając prawym przyciskiem myszy w folderze \Models, a następnie wybierając **Add -&gt;nowy element** polecenia menu i tworzenie nowego interfejsu o nazwie IDinnerRepository.cs.

Możemy również extract refaktoryzację narzędzia wbudowana w program Visual Studio Professional (i nowsze wersje) automatycznie używać i tworzenia interfejsu dla nas z naszej istniejącej klasy DinnerRepository. Aby wyodrębnić ten interfejs, za pomocą programu VS, po prostu umieść kursor w edytorze tekstów w klasie DinnerRepository, a następnie kliknij prawym przyciskiem myszy i wybierz **Zrefaktoryzuj -&gt;Wyodrębnij interfejs** polecenia menu:

![](enable-automated-unit-testing/_static/image7.png)

To spowoduje uruchomienie okna dialogowego "Wyodrębnij interfejs" i Monituj nas o nazwa interfejsu, aby utworzyć. Zostanie domyślnie IDinnerRepository i automatycznie wybiera wszystkich metod publicznych w istniejącej klasie DinnerRepository do dodania do interfejsu:

![](enable-automated-unit-testing/_static/image8.png)

Kliknięcie przycisku "ok", programu Visual Studio doda nowy interfejs IDinnerRepository do naszej aplikacji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

I naszej istniejącej klasy DinnerRepository zostanie zaktualizowana tak, aby go implementuje interfejs:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualizowanie DinnersController w celu obsługi iniekcji konstruktora

Teraz zaktualizujemy klasy DinnersController, aby korzystać z nowego interfejsu.

Obecnie DinnersController zostało zakodowane taki sposób, że jego pole "dinnerRepository" jest zawsze klasy DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Zmienimy je, tak aby pole "dinnerRepository" jest typu IDinnerRepository zamiast DinnerRepository. Następnie dodamy dwa publiczne konstruktory DinnersController. Jednym z konstruktorów umożliwia IDinnerRepository został przekazany jako argument. Domyślny konstruktor używający naszej istniejącej implementacji DinnerRepository drugi to:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Ponieważ platformy ASP.NET MVC, domyślnie tworzy klasy kontrolera przy użyciu domyślnych konstruktorów, nasze DinnersController w czasie wykonywania będzie korzystanie z klasy DinnerRepository przeprowadzić dostępu do danych.

Możemy teraz zaktualizować Nasze testy jednostkowe, jednak do przekazania w implementacji repozytorium obiad z "fałszywą" za pomocą konstruktora parametru. To repozytorium obiad z "fałszywą" nie będzie wymagać dostępu do rzeczywistego bazy danych, a zamiast tego użyje danych przykładowych w pamięci.

#### <a name="creating-the-fakedinnerrepository-class"></a>Tworzenie klasy FakeDinnerRepository

Utwórz klasę FakeDinnerRepository.

Firma Microsoft będzie Rozpocznij od utworzenia katalogu "Substytuty" w projekcie NerdDinner.Tests, a następnie dodaj nową klasę FakeDinnerRepository do niego (kliknij prawym przyciskiem myszy w folderze, a następnie wybierz **Add -&gt;nową klasę**):

![](enable-automated-unit-testing/_static/image9.png)

Tak, aby klasa FakeDinnerRepository implementuje interfejs IDinnerRepository kodu zostaną zaktualizowane. Firma Microsoft następnie kliknij prawym przyciskiem myszy na nim i wybierz polecenie "Implementuj interfejs IDinnerRepository" z menu kontekstowego:

![](enable-automated-unit-testing/_static/image10.png)

To spowoduje, że Visual Studio, aby automatycznie dodać wszyscy członkowie interfejsu IDinnerRepository klasy Nasze FakeDinnerRepository przy użyciu domyślnej implementacji "namiastki":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Firma Microsoft może uaktualnić wdrożenia FakeDinnerRepository pracę zniżki w stosunku do listy w pamięci&lt;obiad&gt; kolekcji przekazany jako argument konstruktora:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

W efekcie powstał fałszywych implementację IDinnerRepository, która nie wymaga bazy danych, a zamiast tego mogą pracować poza w pamięci listę obiektów obiad.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Za pomocą FakeDinnerRepository przy użyciu testów jednostkowych

Powrócimy do testów jednostkowych DinnersController, które nie powiodły się wcześniej, ponieważ baza danych nie była dostępna. Aktualizujemy metody testowe, aby użyć FakeDinnerRepository wypełniona przykładowymi danymi o obiad w pamięci do DinnersController przy użyciu poniższego kodu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

A teraz możemy uruchamiania tych testów zarówno przekazują:

![](enable-automated-unit-testing/_static/image11.png)

Przede wszystkim mogą zająć jedynie ułamek sekund do uruchomienia i nie wymagają wszelka logika skomplikowane instalacji/czyszczenia. Możemy teraz testów jednostkowych wszystkich naszych DinnersController akcji kodu metody (takie jak listy, stronicowanie i uzyskać szczegółowe informacje, tworzenie, aktualizowanie i usuwanie) bez połączenia z prawdziwą bazą danych.

| **Po stronie temat: Struktury iniekcji zależności** |
| --- |
| Wykonywanie wstrzykiwanie zależności ręczne (np. Firma Microsoft znajdują się powyżej) działa poprawnie, ale stać się trudniejsze do utrzymania jako liczba zależności i zwiększa składników w aplikacji. Istnieje kilka środowisk iniekcji zależności dla platformy .NET, która pomaga zapewnić jeszcze większą elastyczność zarządzania zależności. Te struktury, czasami nazywane "Inwersja kontroli" (IoC) kontenerów, zapewniają mechanizmy, umożliwiających dodatkowy poziom obsługi konfiguracji dotyczące określania i przekazanie zależności do obiektów w czasie wykonywania (w większości przypadków przy użyciu iniekcji konstruktora ). Niektóre z najpopularniejszych wstrzykiwanie zależności OSS / include IOC platform na platformie .NET: AutoFac, Ninject, Spring.NET, StructureMap i Windsor. ASP.NET MVC udostępnia rozszerzalności interfejsów API, które deweloperzy mogą uczestniczyć w rozdzielczości i tworzenia wystąpień kontrolerów i umożliwiająca wstrzykiwanie zależności / platform IoC powinny zostać prawidłowo włączone w ramach tego procesu. Za pomocą platformy DI/IOC będzie również pozwalają nam można usunąć domyślnego konstruktora z naszych DinnersController — która całkowicie usunąć sprzężenia między nim a DinnerRepositorys. Firma Microsoft nie będzie można za pomocą iniekcji zależności / framework IOC z naszej aplikacji NerdDinner. Ale to coś, które firma Microsoft może wziąć pod uwagę na przyszłość Jeśli zwiększył bazy kodu NerdDinner i możliwości. |

### <a name="creating-edit-action-unit-tests"></a>Tworzenie testów jednostkowych akcji edycji

Utwórzmy teraz niektóre testy jednostkowe, które sprawdzają funkcje edycji DinnersController. Zaczniemy od testowania wersji HTTP GET naszej akcji edycji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Utworzymy test, który sprawdza renderowania widoku, wspierane przez obiekt DinnerFormViewModel ponownie w przypadku, gdy wymagane jest prawidłowe obiad:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Uruchomienie testu, jednak firma Microsoft znajdują się to niemożliwe, ponieważ wyjątek odwołania o wartości null jest zgłaszany, gdy właściwość User.Identity.Name sprawdzania Dinner.IsHostedBy() uzyskuje dostęp do funkcji edycji.

Obiekt użytkownika w klasie bazowej kontrolera hermetyzuje szczegóły dotyczące zalogowanego użytkownika i jest wypełniana przez platformę ASP.NET MVC, podczas tworzenia kontrolera w czasie wykonywania. Ponieważ testujemy DinnersController poza środowiskiem serwera sieci web, obiekt użytkownika nie jest ustawiony (dlatego wyjątek pustej referencji).

### <a name="mocking-the-useridentityname-property"></a>Pozorowanie właściwość User.Identity.Name

Struktury pozorowania ułatwić, testowanie, dzięki czemu możemy umożliwia dynamiczne tworzenie fałszywych wersje obiektów zależnych, które obsługują Nasze testy. Na przykład możemy użyć pozorowania framework w naszym teście akcji edycji umożliwia dynamiczne tworzenie obiektu użytkownika, używanego przez naszych DinnersController do wyszukiwania symulowane nazwy użytkownika. Zapobiegnie to odwołanie o wartości null z zgłaszane, gdy Uruchamiamy nasze badania.

Istnieje wiele .NET pozorowanie struktur, które mogą być używane z platformy ASP.NET MVC (zobaczysz listę je tutaj: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Na potrzeby testowania naszych aplikacji NerdDinner, użyjemy pozorowanie o nazwie "Moq" open source, który można pobrać bezpłatnie z [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Po pobraniu, dodamy odwołanie w projekcie NerdDinner.Tests do zestawu Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Następnie dodamy metody pomocnika "CreateDinnersControllerAs(username)" nasze klasy testowej, który przyjmuje nazwę użytkownika jako parametr i które, a następnie "mocks" Właściwość User.Identity.Name wystąpieniu DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Powyżej użyto Moq można utworzyć obiektu projekt, który elementów sztucznych obiekt kontekstem ControllerContext (czyli platformy ASP.NET MVC przekazuje do klas kontrolera do udostępnienia środowiska uruchomieniowego obiektów, takich jak użytkownik, żądania, odpowiedzi i sesji). Wywołania metody "SetupGet" na projekt, aby wskazać, że właściwość HttpContext.User.Identity.Name kontekstem ControllerContext powinna zwrócić ciąg nazwy użytkownika, który możemy przekazać do metody pomocnika.

Firma Microsoft jest testowanie dowolną liczbę kontekstem ControllerContext właściwości i metody. Aby zilustrować to ja dodałem również wywołanie SetupGet() dla właściwości Request.IsAuthenticated (który nie jest faktycznie potrzebny do testów poniżej — ale pozwalającemu pokazują, jak można testowanie właściwości żądania). Gdy gotowe możemy przypisać wystąpienie pozorny kontekstem ControllerContext DinnersController zwraca naszych metody pomocnika.

Firma Microsoft jest teraz pisanie testów jednostkowych, korzystających z tej metody pomocnika do testowania scenariuszy edycji obejmujące różnych użytkowników:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

A teraz możemy uruchamiania testów przekazują:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() scenariuszy testowania

Utworzyliśmy testy, które obejmują HTTP GET wersję akcji edycji. Utwórzmy teraz niektóre testy weryfikujące wersji żądania HTTP POST akcji edycji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Interesujące nowy scenariusz testowania w firmie Microsoft w celu obsługi przy użyciu tej metody akcji jest jej użycie metody pomocnika UpdateModel() w klasie bazowej kontrolera. Używamy tej metody pomocnika do powiązania wartości post formularza do naszych obiad wystąpienia obiektu.

Poniżej przedstawiono dwa testy, które pokazuje, jak firma Microsoft mogła dostarczyć opublikowane wartości metodę pomocnika używaną UpdateModel() do formularza. Firma Microsoft będzie to zrobić, tworzenie i wypełnianie obiektu FormCollection, a następnie przypisać ją do właściwości "ValueProvider" na kontrolerze.

Pierwszy test sprawdza, czy na pomyślne Zapisz przeglądarka jest przekierowywana do akcji details. Drugi test sprawdza, czy po opublikowaniu nieprawidłowe dane wejściowe akcji zostanie ponownie widoku edycji, ponownie komunikatu o błędzie.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testowanie Wrap-Up

Omówiliśmy podstawowe pojęcia związane z klas kontrolera testów jednostek. Z łatwością tworzyć setki proste testy weryfikujące działanie naszej aplikacji możemy użyć tych metod.

Ponieważ naszych kontrolera i testy modelu nie wymagają rzeczywista baza danych, są one bardzo szybkie i łatwe do uruchomienia. Firma Microsoft będzie można wykonać setki zautomatyzowanych testów w ciągu kilku sekund i natychmiast uzyskuj opinie dotyczące tego, czy zmiany, jakie wprowadziliśmy Przerwano coś. Ułatwi to przekazać nam zaufanie, stale poprawić, Refaktoryzacja, i dostosowywać naszej aplikacji.

Omówiono testowanie jako ostatni temat w tym rozdziale —, ale nie, ponieważ testowanie jest coś, co należy zrobić, na końcu procesu opracowywania! Przeciwnie należy wpisać zautomatyzowane testy możliwie najwcześniejszym etapie procesu rozwoju. Takie działanie umożliwia więc natychmiast otrzymać opinię podczas opracowywania, pomaga odnoszącej zastanów się, scenariuszy przypadków użycia aplikacji i przeprowadzi Cię do projektowania aplikacji przy użyciu czystego, Układanie warstwowo i sprzęgania na uwadze.

Nowsze rozdziału w książce przedstawimy testów opartych na rozwój (TDD) i jak z niej korzystać ze wzorca ASP.NET MVC. Projektowanie oparte na testach jest iteracyjne kodowania najpierw pisze się testy, które będzie spełniać wynikowy kod. Za pomocą TDD rozpoczęciem każdej funkcji, tworząc test, który weryfikuje funkcjonalność, którą chcesz wdrożyć. Zapisywanie jednostki testu najpierw pomaga upewnić się, że wyraźnie rozumiesz funkcja i jak powinien pracować. Tylko wtedy, gdy test jest zapisywany (i upewnieniu się, że nie jest on) czy, a następnie zaimplementować rzeczywiste funkcje, które ten test sprawdza. Ponieważ został już spędzony czas zastanawiać się, jak funkcja powinien działać przypadek użycia, trzeba będzie lepiej zrozumieć wymagania i jest to najlepszy sposób ich implementacji. Po zakończeniu wdrożenia można ponownie uruchomić test — i natychmiast otrzymać opinię w do tego, czy funkcja działa prawidłowo. Omówimy TDD więcej w rozdziale 10.

### <a name="next-step"></a>Następny krok

Niektóre końcowego zawijania komentarze.

> [!div class="step-by-step"]
> [Poprzednie](use-ajax-to-implement-mapping-scenarios.md)
> [dalej](nerddinner-wrap-up.md)
