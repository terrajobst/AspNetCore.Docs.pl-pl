---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: 'Iteracja #5 — Tworzenie testów jednostkowych (C#) | Dokumentacja firmy Microsoft'
author: microsoft
description: W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 68d0ae15db115685b3e1a44f8b3b5b7e33674a8b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755587"
---
<a name="iteration-5--create-unit-tests-c"></a>Iteracja #5 — Tworzenie testów jednostkowych (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (C#)

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym czwarty iteracji możemy korzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.


## <a name="this-iteration"></a>Tej iteracji

W poprzedniej iteracji aplikacji Contact Manager zrefaktoryzowaliśmy aplikacji można bardziej luźno powiązane. Firma Microsoft oddzielone aplikacji na różne kontrolera, usług i warstw repozytorium. Każda warstwa wchodzi w interakcję z warstwą pod nim za pośrednictwem interfejsów.

Zrefaktoryzowaliśmy aplikacji ułatwia konserwację i modyfikowanie aplikacji. Na przykład jeśli musimy użyć nowej technologii dostępu do danych, firma Microsoft wystarczy zmienić warstwę repozytorium bez dotykania kontrolera lub warstwy usług. Podejmując Contact Manager luźno powiązane, firma Microsoft ve wprowadzone aplikacji bardziej odporne na zmiany.

Ale co się stanie, gdy będziemy musieli dodanie nowej funkcji do aplikacji Contact Manager? Lub, co się stanie, gdy firma Microsoft usterki? Sad, ale również sprawdzonych prawdziwość pisania kodu jest zawsze wtedy, gdy touch kod, Utwórz ryzyka wprowadzenia nowych usterek.

Na przykład jednego dnia poprawnie, Menedżer może poprosić o dodanie nowej funkcji do menedżera kontaktu. Chce dodać obsługę skontaktuj się z grup. Użytkownik chce, aby umożliwić użytkownikom umieszczaj kontaktów w grupach, takich jak znajomych, firm i tak dalej.

Aby można było wdrożyć tę nową funkcję, należy zmodyfikować wszystkie trzy warstwy aplikacji Contact Manager. Należy dodać nowe funkcje do kontrolerów, warstwy usług i repozytorium. Zaraz po uruchomieniu modyfikowania kodu, istnieje ryzyko, istotne funkcje, które działało.

Refaktoryzacja naszej aplikacji do oddzielnych warstw, ile My mieliśmy w poprzedniej iteracji jest dobrym znakiem. Ponieważ dzięki temu możemy wprowadzić zmiany do całego warstw bez ingerowania w pozostałe części aplikacji, była dobrym znakiem. Jednak jeśli chcesz wprowadzić kod w obrębie warstwy ułatwia konserwację i modyfikowanie, należy utworzyć testów jednostkowych dla kodu.

Możesz użyć jednostki testami jednostka kodu. Te jednostki kodu są mniejsze niż warstwy całej aplikacji. Zazwyczaj używasz testu jednostkowego, aby sprawdzić, czy konkretną metodę w kodzie działa w oczekiwany sposób. Na przykład możesz utworzyć test jednostkowy jako metodę CreateContact() udostępnianych przez klasy ContactManagerService.

Testy jednostkowe dla aplikacji po prostu działają, takich jak projektowi. Przy każdej modyfikacji kodu w aplikacji, można uruchomić zestaw testów jednostkowych, aby sprawdzić, czy modyfikacja przerywa istniejących funkcji. Testy jednostkowe sprawić, że kod jest bezpieczny do zmodyfikowania. Testy jednostkowe wprowadzić cały kod w Twojej aplikacji bardziej odporne na zmiany.

W tym iteracji możemy dodać testy jednostkowe do naszej aplikacji Contact Manager. W ten sposób, w następnej iteracji, możemy dodać skontaktuj się z grupy do naszej aplikacji bez konieczności martwienia się o przełomowych istniejących funkcji.

> [!NOTE] 
> 
> Istnieją różne platformy, w tym NUnit i xUnit.net, MbUnit testów jednostkowych. W tym samouczku używamy jednostki testowania framework zawartej z programem Visual Studio. Jednak może równie łatwo użyć jednej z tych środowisk alternatywne.


## <a name="what-gets-tested"></a>Co pobiera przetestowane

W świecie doskonałego kodu byłyby objęte testy jednostek. W świecie doskonałe trzeba doskonałe projektowi. Można zmodyfikować w każdym wierszu kodu w aplikacji i natychmiast będzie znana, wykonując testy jednostkowe, czy zmiana Przerwano istniejących funkcji.

Jednak firma Microsoft don t na żywo w idealnych. W praktyce podczas pisania testów jednostkowych można skoncentrować się na pisaniu testów dla logiki biznesowej (na przykład logikę weryfikacji). W szczególności należy *nie* pisanie testów jednostkowych dla swoich danych dostęp logiki lub logice widoku.

Były przydatne, testy jednostkowe muszą wykonać bardzo szybko. Możesz łatwo może wzrosnąć setek lub nawet tysięcy testów jednostkowych dla aplikacji. Jeśli testy jednostkowe zająć dużo czasu do uruchomienia, a następnie będzie można uniknąć ich wykonania. Innymi słowy długo działające testy jednostkowe są bezużyteczne kodów dnia na dzień.

Z tego powodu zwykle nie pisania testów jednostkowych dla kodu, który współdziała z bazą danych. Uruchamianiu setki testów jednostkowych na żywo bazy danych będzie zbyt wolno. Zamiast tego należy testowanie bazy danych i napisać kod, który wchodzi w interakcję z makiety bazy danych (omówimy pozorowanie database poniżej).

Z podobnego powodu zwykle nie pisania testów jednostkowych dla widoków. W celu przetestowania widoku, możesz zwiększać serwera sieci web. Ponieważ uruchamiając serwera sieci web jest procesem powolnym stosunkowo, tworzenie testów jednostkowych dla widoków nie jest zalecane.

Jeśli widok zawiera skomplikowaną logikę, następnie należy rozważyć przeniesienie logikę do metody pomocnika. Można pisać testy jednostkowe dla metody pomocnika, które są wykonywane bez konieczności uruchamiania serwera sieci web.

> [!NOTE] 
> 
> Podczas pisania testów logiką dostępu do danych lub widoku logiki nie jest dobrze podczas pisania testów jednostkowych, te testy może być bardzo przydatne podczas tworzenia funkcjonalności lub integracji testy.


> [!NOTE] 
> 
> ASP.NET MVC jest aparat widoku w formularzach sieci Web. Aparat widoku w formularzach sieci Web jest zależny od serwera sieci web, mogą być inne aparaty widoku.


## <a name="using-a-mock-object-framework"></a>Za pomocą platformy makiety obiektu

Podczas tworzenia testów jednostkowych, należy prawie zawsze korzystać z zalet framework testowanie obiektu. Framework obiektu pozorowanie umożliwia tworzenie mocks i klas zastępczych dla klas w aplikacji.

Na przykład można użyć framework testowanie obiektu do generowania wersję makiety obiektu klasy repozytorium. W ten sposób służy klasa makiety repozytorium zamiast klasy rzeczywistych repozytorium do testów jednostkowych. Przy użyciu repozytorium makiety pozwala na uniknięcie wykonywania kodu bazy danych, podczas wykonywania testu jednostkowego.

Program Visual Studio nie ma framework testowanie obiektu. Istnieją jednak wiele struktur testowanie obiektu komercyjnych typu open source dostępnych w programie .NET Framework:

1. Moq — ta struktura jest dostępna w ramach licencja BSD "open source". Możesz pobrać Moq z [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks — ta struktura jest dostępna w ramach licencja BSD "open source". Możesz pobrać Rhino Mocks z [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Odłączenia Typemock — to struktura, komercyjnych. Możesz pobrać wersję próbną z [ http://www.typemock.com/ ](http://www.typemock.com/).

W tym samouczku, I chcę korzystać z Moq. Jednak może równie łatwo używać Rhino Mocks lub odłączenia Typemock, aby utworzyć projekt obiektów dla aplikacji Contact Manager.

Zanim użyjesz Moq, należy wykonać następujące czynności:

1. .
2. Zanim użytkownik Rozpakuj pliki do pobrania, upewnij się, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **odblokowanie** (patrz rysunek 1).
3. Rozpakuj pliki do pobrania.
4. Dodaj odwołanie do zestawu Moq, klikając prawym przyciskiem myszy folder odwołania w projekcie ContactManager.Tests i wybierając **Dodaj odwołanie**. Na karcie przeglądania przejdź do folderu, w którym rozpakowano Moq i wybierz zestaw Moq.dll. Kliknij przycisk **OK** przycisku.
5. Po wykonaniu tych kroków, folderze odwołania powinien wyglądać jak rysunek 2.


[![Odblokowywanie Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Rysunek 01**: Moq odblokowywania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-cs/_static/image2.png))


[![Odwołania po dodaniu Moq](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Rysunek 02**: odwołania po dodaniu Moq ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-cs/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Tworzenie testów jednostkowych dla warstwy usług

Pozwól, s, Rozpocznij od utworzenia zestawu testów jednostkowych dla warstwy usług s naszych Contact Manager aplikacji. Użyjemy tych testów, aby zweryfikować nasze logikę weryfikacji.

Utwórz nowy folder o nazwie modeli w projekcie ContactManager.Tests. Następnie kliknij prawym przyciskiem myszy folderu modeli i wybierz **Dodaj, nowy Test**. **Dodaj nowy Test** zostanie wyświetlone okno dialogowe pokazany na rysunku 3. Wybierz **testów jednostkowych** szablonu i nazwy nowego testu ContactManagerServiceTest.cs. Kliknij przycisk **OK** przycisk, aby dodać nowego testu do projektu testu.

> [!NOTE] 
> 
> Ogólnie rzecz biorąc ma strukturę folderu projektu testu w taki sposób, aby dopasować strukturę folderu projektu ASP.NET MVC. Na przykład umieść kontrolera testów w folderze kontrolerów testów modelu w folderze modeli i tak dalej.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Rysunek 03**: Models\ContactManagerServiceTest.cs ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-cs/_static/image6.png))


Początkowo chcemy metoda CreateContact() udostępnianych przez klasy ContactManagerService testu. Utworzymy pięć testów poniżej:

- CreateContact() — testy tego CreateContact() zwraca wartość true, gdy prawidłowy kontakt jest przekazywany do metody.
- CreateContactRequiredFirstName() - testów, że komunikat o błędzie został dodany do stanu modelu, podczas kontaktu z Brak imię jest przekazywany do metody CreateContact().
- CreateContactRequredLastName() - testów, że komunikat o błędzie został dodany do stanu modelu, podczas kontaktu z Brak nazwisko jest przekazywany do metody CreateContact().
- CreateContactInvalidPhone() - testów, że komunikat o błędzie został dodany do stanu modelu, podczas kontaktu z nieprawidłowym numerem telefonu jest przekazywany do metody CreateContact().
- CreateContactInvalidEmail() - testów, że komunikat o błędzie został dodany do stanu modelu, podczas kontaktu z nieprawidłowy adres e-mail jest przekazywany do metody CreateContact()...

Pierwszy test sprawdza, prawidłowy kontakt nie generuje błąd sprawdzania poprawności. Pozostałe testy Sprawdź każdy z reguł sprawdzania poprawności.

Kod dla tych testów znajduje się w ofercie 1.

**Wyświetlanie listy 1 - Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


Ponieważ używamy klasy skontaktuj się z listą 1 musimy dodać odwołanie do Microsoft Entity Framework do naszego projektu testu. Dodaj odwołanie do zestawu System.Data.Entity.


Wyświetlanie listy 1 zawiera metodę o nazwie Initialize(), który zostanie nadany atrybut [TestInitialize]. Ta metoda jest wywoływana automatycznie, przed uruchomieniem każdego z testów jednostkowych (jest to 5 razy bezpośrednio przed każdym z testów jednostkowych). Metoda Initialize() tworzy makiety repozytorium przy użyciu następującego kodu:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Ten wiersz kodu używa Moq framework do wygenerowania makiety repozytorium z interfejsu IContactManagerRepository. Zamiast rzeczywistego EntityContactManagerRepository makiety repozytorium jest używana w celu uniknięcia, uzyskiwanie dostępu do bazy danych, po uruchomieniu każdy test jednostkowy. Repozytorium makiety implementuje metody interfejsu IContactManagerRepository, ale t don metody naprawdę nic nie robi.

> [!NOTE] 
> 
> Korzystając z Moq framework, jest rozróżnienie między \_mockRepository i \_mockRepository.Object. Pierwsza odwołuje się do pozorny&lt;IContactManagerRepository&gt; klasę, która zawiera metody do określania zachowania makiety repozytorium. Te ostatnie odnosi się do rzeczywistego makiety repozytorium, który implementuje interfejs IContactManagerRepository.


Makiety repozytorium jest używana w przypadku metody Initialize() podczas tworzenia wystąpienia klasy ContactManagerService. Wszystkie testy jednostkowe poszczególnych użycie tego wystąpienia klasy ContactManagerService.

Wyświetlanie listy 1 zawiera pięć metod, które odnoszą się do każdego z testów jednostkowych. Każda z tych metod zostanie nadany atrybut [TestMethod]. Kiedy uruchamiasz testy jednostkowe, nosi nazwę dowolnej metody, która ma tego atrybutu. Innymi słowy dowolnej metody, która zostanie nadany atrybut [TestMethod] to test jednostkowy.

Pierwszy test jednostkowy, o nazwie CreateContact(), sprawdza, czy wywołanie CreateContact() zwraca wartość true, gdy prawidłowe wystąpienie klasy kontakt jest przekazywany do metody. Test tworzy wystąpienie klasy, skontaktuj się z pomocą, wywołuje metodę CreateContact() i sprawdza, czy CreateContact() zwraca wartość true.

Pozostałe testy Sprawdź, czy gdy wywoływana jest metoda CreateContact(), z nieprawidłową kontaktu w następnie metoda zwraca wartość FAŁSZ i komunikat o błędzie weryfikacji oczekiwany jest dodawany do stanu modelu. Na przykład CreateContactRequiredFirstName() test tworzy wystąpienie klasy skontaktuj się z pustym ciągiem znaków dla jego właściwości imię. Następnie metoda CreateContact() jest wywoływana z nieprawidłową kontaktu. Ponadto ten test sprawdza, czy CreateContact() zwraca wartość false, i że stanu modelu zawiera komunikat o błędzie weryfikacji oczekiwano "Imię jest wymagane."

Można uruchomić testy jednostkowe w ofercie 1, wybierając opcję menu **, przebieg testu, wszystkie testy w rozwiązaniu (CTRL + R, A)**. Wyniki testów są wyświetlane w oknie wyników testu (zobacz rysunek 4).


[![Wyniki testu](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Rysunek 04**: wyniki testów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-cs/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Tworzenie testów jednostkowych dla kontrolerów

Aplikacja ASP.NETMVC sterowanie przepływem interakcji z użytkownikiem. Podczas testowania z kontrolerem, należy sprawdzić, czy ten kontroler zwraca odpowiednie działania wynik i przeglądanie danych. Można również sprawdzić, czy kontroler wchodzi w interakcję z klasy modeli w oczekiwany sposób.

Na przykład lista 2 zawiera dwa testy jednostkowe dla kontrolera skontaktuj się z metody Create(). Pierwszy test jednostkowy sprawdza, czy podczas prawidłowy kontakt jest przekazywany do metody Create(), a następnie Metoda Create() przekierowuje do akcji indeksu. Innymi słowy Jeśli przekazano nieprawidłowy kontaktu, Metoda Create() powinna zwrócić RedirectToRouteResult, która reprezentuje akcję indeksu.

Firma Microsoft don t chcesz przetestować ContactManager warstwy usług, gdy testujemy warstwy kontrolera. W związku z tym firma Microsoft testowanie warstwy usług, używając następującego kodu w metodzie inicjowania:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

W CreateValidContact() Testy jednostkowe firma Microsoft testowanie zachowania podczas wywoływania warstwy usług CreateContact() metody za pomocą następującego kodu:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Ten wiersz kodu powoduje, że makiety ContactManager usługa zwraca wartość true, jeśli zostanie wywołana jego metoda CreateContact(). Przez pozorowanie warstwy usług, firma Microsoft testowanie zachowania kontrolera, bez konieczności wykonywania żadnych kodu w warstwie usługi.

Drugi test jednostkowy sprawdza, czy akcja Create() błędny kontakt jest przekazywany do metody, zwraca widok Utwórz. Możemy spowodować warstwy usług CreateContact() metoda zwraca wartość false, przy użyciu następującego kodu:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Jeśli Metoda Create() zachowuje się jak oczekujemy, że następnie powinna zwrócić widok Tworzenie warstwy usług zwrócona wartość false. W ten sposób kontroler można wyświetlić komunikaty o błędach weryfikacji w widoku Utwórz, a użytkownik ma szansę, aby poprawić ten nieprawidłowe właściwości skontaktuj się z pomocą.


Jeśli planujesz tworzenie testów jednostkowych dla kontrolerów należy zwrócić nazwy widoków jawne z akcji kontrolera. Na przykład nie zwracają widoku następująco:

Zwróć View();

Zamiast tego zwracają widoku następująco:

return View("Create");

Jeśli nie jesteś jawne gdy zwracany jest widokiem ViewResult.ViewName właściwość zwraca pusty ciąg.


**Wyświetlanie listy 2 - Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Podsumowanie

W tej iteracji utworzyliśmy testów jednostkowych dla naszej aplikacji Contact Manager. W dowolnym momencie, aby sprawdzić, czy naszej aplikacji nadal działa w sposób, że oczekujemy, że można uruchomić te testy jednostkowe. Testy jednostkowe działają jako zabezpieczenie, aby nasza aplikacja pozwala nam bezpiecznie modyfikować naszej aplikacji w przyszłości.

Utworzyliśmy dwa zestawy testów jednostkowych. Po pierwsze przetestowaliśmy naszych logikę weryfikacji przez utworzenie testów jednostkowych dla naszych warstwy usług. Następnie możemy przetestowane naszych przepływu sterowania logiki przez utworzenie testów jednostkowych dla naszej warstwie kontrolera. Podczas testowania naszych warstwy usług, firma Microsoft samodzielnie Nasze testy nasze warstwy usług z warstwy w naszym repozytorium przez pozorowanie nasze warstwy repozytorium. Podczas testowania warstwy kontrolera, możemy samodzielnie Nasze testy dla naszych warstwy kontrolera, przez pozorowanie warstwy usług.

W następnej iteracji zmodyfikujemy aplikacji Contact Manager tak, aby go obsługuje skontaktuj się z grupy. Dodamy tej nowej funkcji do naszej aplikacji przy użyciu procesu projektowania oprogramowania o nazwie programowania sterowanego testami.

> [!div class="step-by-step"]
> [Poprzednie](iteration-4-make-the-application-loosely-coupled-cs.md)
> [dalej](iteration-6-use-test-driven-development-cs.md)
