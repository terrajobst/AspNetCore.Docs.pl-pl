---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iteracja #7 — funkcje Ajax Dodaj (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d961ee39d7b87a31c7208645148b45c7b0c563
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875269"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iteracja #7 — funkcje Ajax Dodaj (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji ASP.NET MVC kontaktu administracyjnego (VB)
  

W tej serii samouczków budujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacji skontaktuj się z Menedżera umożliwia przechowywanie informacji kontaktowych - nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft kompilowania aplikacji za pośrednictwem przejść przez wiele iteracji. Przy każdej iteracji firma Microsoft stopniowego zwiększenia aplikacji. Celem tego podejścia wiele iteracji jest ułatwia zrozumienie przyczyn, dla każdej zmiany.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracji #2 — należy Szukaj nieuprzywilejowany aplikacji. W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.

- Iteracji #4 — należy luźno powiązane z aplikacji. W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 - tworzenia testów jednostkowych. W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.

- Iteracja #6 - użyj test-driven development. W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 - dodawania funkcjonalności interfejsu Ajax. Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.

## <a name="this-iteration"></a>Tej iteracji

W tej iteracji aplikacji menedżera kontaktu Firma Microsoft Refaktoryzuj naszej aplikacji, aby korzystać z technologii Ajax. Dzięki wykorzystaniu technologii Ajax, wykonujemy naszej aplikacji poprawę reakcji. Firma Microsoft mogą uniknąć renderowanie całej strony, gdy należy zaktualizować tylko określonym regionie na stronie.

Firma Microsoft będzie Refaktoryzuj naszych widoku indeksu, dzięki czemu możemy ADAM t trzeba ponownie wyświetlić dla całej strony, gdy ktoś wybiera nową grupę skontaktuj się z pomocą. Zamiast tego po kliknięciu grupy kontaktów, firma Microsoft będzie tylko zaktualizować listy kontaktów i pozostawić pozostałej części strony samodzielnie.

Zmienimy także sposób naszych Usuń łącze działa. Zamiast stronę potwierdzenia oddzielne będzie wyświetli okno dialogowe potwierdzenia JavaScript. Po potwierdzeniu, że chcesz usunąć kontakt, operacja HTTP DELETE jest wykonywana na serwerze, aby usunąć rekord kontaktu z bazy danych.

Ponadto firma Microsoft będzie korzystać z jQuery, aby dodać efekty animacji do naszej widoku indeksu. Animowany firma Microsoft będzie wyświetlany, gdy nowej listy kontaktów jest jest pobierane z serwera.

Na koniec firma Microsoft będzie korzystać z ASP.NET AJAX framework obsługę zarządzania historii przeglądarki. Utworzymy punkty historii zawsze, gdy wykonać wywołanie Ajax w celu zaktualizowania listy kontaktów. W ten sposób przeglądarki do tyłu i do przodu przyciski będzie działać.

## <a name="why-use-ajax"></a>Dlaczego warto używać technologii Ajax?

Za pomocą interfejsu Ajax ma wiele korzyści. Najpierw dodawania funkcjonalności interfejsu Ajax do aplikacji spowoduje lepsze środowisko pracy użytkownika. W aplikacji sieci web normalne całą stronę zaksięgowania na serwer czasu każdy użytkownik wykona akcję. Przy każdym wykonaniu akcji blokad przeglądarki, a użytkownik musi poczekać na całej stronie jest pobierane i wyświetlane ponownie.

Powinien to być niedopuszczalne doświadczenie w przypadku aplikacji klasycznych. Jednak tradycyjnie, możemy żyła z płynność pracy w przypadku aplikacji sieci web, ponieważ było nie wiadomo, że firma Microsoft może wykonać lepiej. Firma Microsoft myśleli, że było to ograniczenie aplikacji sieci web, gdy w rzeczywistości był to ograniczenie naszych imaginations.

W aplikacji Ajax zrobisz t konieczności wprowadzenia środowisko użytkownika do zatrzymania tylko w celu aktualizacji strony. Zamiast tego można wykonać żądania asynchronicznego w tle, aby zaktualizować stronę. Zrobisz życie t użytkownikowi Zaczekaj, aż zostanie zaktualizowany części strony.

Dzięki wykorzystaniu technologii Ajax, możesz także może zwiększyć wydajność aplikacji. Należy wziąć pod uwagę, jak aplikacji Menedżera skontaktuj się z od razu działa bez funkcjonalności interfejsu Ajax. Po kliknięciu grupy kontaktów wyświetlane musi ponownie całego widoku indeksu. Listy kontaktów i listę grup kontaktów musi zostać pobrana z serwera bazy danych. Wszystkie te dane muszą być przekazywane przez sieć z serwera sieci web w przeglądarce sieci web.

Po możemy dodać funkcje Ajax do aplikacji, jednak firma Microsoft może pominąć ponowne wyświetlanie całej strony, gdy użytkownik kliknie grupy kontaktów. Nie należy do pobrania grup kontaktów z bazy danych. Możemy również ADAM potrzebę t push całego widoku indeksu przez sieć. Dzięki wykorzystaniu technologii Ajax, możemy zmniejszyć ilość pracy, które należy wykonać serwera bazy danych i możemy zmniejszyć ilość ruchu sieciowego wymagane przez naszą aplikację.

## <a name="don-t-be-afraid-of-ajax"></a>ADAM t można obawiasz się z Ajax

Niektórzy deweloperzy uniknąć za pomocą technologii Ajax, ponieważ ich martwić przeglądarki niskiego poziomu. Chcą upewnić się, że ich aplikacji sieci web będzie nadal działać podczas uzyskiwania dostępu do przeglądarki, która nie obsługuje języka JavaScript. Ponieważ Ajax zależy od języka JavaScript, niektórzy deweloperzy uniknąć za pomocą interfejsu Ajax.

Jednak w przypadku rozważnie sposobu implementacji interfejsu Ajax następnie można tworzyć aplikacje, które współpracują z wyższej, jak i niskiego poziomu. Naszą aplikację kontaktów Menedżerze będzie działać z przeglądarek, które obsługują JavaScript i przeglądarek, które nie.

Jeśli używasz aplikacji menedżera kontaktu z przeglądarki, która obsługuje język JavaScript będzie mieć lepsze środowisko pracy użytkownika. Na przykład po kliknięciu grupy kontaktów zostaną zaktualizowane dla regionu strona, wyświetlająca kontaktów.

Użycie, z drugiej strony, aplikacji menedżera kontaktu przy użyciu przeglądarki, który nie obsługuje języka JavaScript (lub ma JavaScript wyłączone) będzie mieć nieco mniej pożądana środowisko użytkownika. Na przykład po kliknięciu grupy kontaktów całego widoku indeksu musi ponownego zaksięgowania przeglądarki w celu wyświetlania zgodnych listy kontaktów.

## <a name="adding-the-required-javascript-files"></a>Dodawanie plików JavaScript wymagane

Potrzebujemy dodać funkcje Ajax do aplikacji przy użyciu trzech plików JavaScript. Wszystkie trzy te pliki znajdują się w folderze skryptów nowej aplikacji ASP.NET MVC.

Jeśli planujesz używać technologii Ajax na wielu stronach w aplikacji następnie warto uwzględnić wymagane pliki JavaScript w Twojej aplikacji s widoku strony wzorcowej. Dzięki temu pliki JavaScript będą uwzględniane we wszystkich stron w aplikacji automatycznie.

Dodaj następujący kod JavaScript obejmuje wewnątrz &lt;head&gt; tag strony wzorcowej widoku:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refaktoryzacja widoku indeksu na Ajax

Let s start, modyfikując naszych widoku indeksu, aby tylko kliknięcie grupy kontaktów aktualizuje region widoku, który wyświetla kontaktów. Czerwonym prostokątem na rysunku 1 zawiera regionu, którą chcemy aktualizowanie.


[![Aktualizowanie tylko kontakty](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Rysunek 01**: aktualizowanie tylko kontaktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-7-add-ajax-functionality-vb/_static/image2.png))


Pierwszym krokiem jest do oddzielania część widoku, która ma być aktualizację asynchronicznie w oddzielnych częściowe (kontrola użytkownika widoku). Sekcja widoku indeksu, który wyświetla tabeli kontaktów został przeniesiony do częściowego wyświetlania 1.

**Wyświetlanie listy 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Zwróć uwagę, że częściowego wyświetlania 1 ma modelu innego niż widok indeksu. *Inherits* atrybutu w &lt;% @ strony %&gt; dyrektywa określa, że częściowego dziedziczy ViewUserControl&lt;grupy&gt; klasy.

Zaktualizowany widok indeks znajduje się w wyświetlania 2.

**Wyświetlanie listy 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Istnieją dwie rzeczy, które powinien uwagi dotyczące zaktualizowany widok wyświetlania 2. Po pierwsze powiadomienia, że cała zawartość przenoszony do częściowego jest zastępowany wywołania Html.RenderPartial(). Metoda Html.RenderPartial() jest wywoływane, gdy widok indeksu najpierw zostanie zażądana w celu wyświetlenia początkowego zestawu kontaktów.

Po drugie Zwróć uwagę, że Html.ActionLink() używany do wyświetlania grup kontaktów zostało zastąpione Ajax.ActionLink(). Ajax.ActionLink() jest wywoływana z następującymi parametrami:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Pierwszy parametr reprezentuje tekst do wyświetlenia łącza, drugi parametr reprezentuje wartości trasy i trzeci parametr reprezentuje opcje technologii Ajax. W takim przypadku używamy opcji technologii UpdateTargetId Ajax wskaż HTML &lt;div&gt; tag, którego chcemy, aby zaktualizować po zakończeniu żądania Ajax. Jeśli chcesz zaktualizować &lt;div&gt; tag z nowej listy kontaktów.

Zaktualizowana metoda indeks() kontrolera kontaktu znajduje się w 3 wyświetlania.

**Wyświetlanie listy 3 - Controllers\ContactController.vb (metoda indeksu)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Zaktualizowano akcję indeks() warunkowo zwraca jedną z następujących operacji. Jeśli akcja indeks() jest wywoływany przez żądanie Ajax kontrolera zwraca częściowym. W przeciwnym razie akcja indeks() zwraca całego widoku.

Zwróć uwagę, akcja indeks() nie musi zwracać jak najwięcej danych, gdy została wywołana przez żądaniem Ajax. W kontekście żądania normalne akcji indeksu zwraca listę wszystkich grup, kontaktów i wybranej grupy. W kontekście żądania Ajax Akcja indeks() zwraca wybranej grupy. AJAX oznacza mniej pracy na serwerze bazy danych.

Nasze zmodyfikowany widok indeksu działa w przypadku zarówno wyższej, jak i niskiego poziomu. Jeśli kliknij grupę kontaktów i przeglądarka obsługuje język JavaScript, jest aktualizowany tylko region widoku, który zawiera listy kontaktów. Jeśli z drugiej strony, przeglądarka nie obsługuje języka JavaScript, jest aktualizowana całego widoku.


Nasze zaktualizowany widok indeksu ma jeden problem. Po kliknięciu grupy kontaktów, nie zostanie wyróżniona wybranej grupy. Ponieważ poza region aktualizowanego w trakcie żądania Ajax zostanie wyświetlona lista grup, nie pobrać wyróżniony grupie prawa. Firma Microsoft będzie Rozwiąż ten problem w następnej sekcji.


## <a name="adding-jquery-animation-effects"></a>Dodawanie efektów animacji jQuery

Zwykle po kliknięciu łącza na stronie sieci web, można użyć przeglądarki pasek postępu do wykrywania, czy przeglądarka jest aktywnie pobierania zaktualizowanej zawartości. Podczas wykonywania żądania Ajax, z drugiej strony, pasek postępu przeglądarki nie wyświetla żadnego postępu. Możliwość nerwowe użytkowników. Jak sprawdzić, czy przeglądarka ma zablokowane?

Istnieje kilka metod, które można wskazać użytkownikowi, że praca odbywa się podczas wykonywania żądania Ajax. Jednym z podejść jest do wyświetlenia prostego animacji. Na przykład użytkownik może zanikania poza obszarem podczas żądania Ajax rozpoczyna się i zanikania w regionie, po zakończeniu żądania.

Użyjemy biblioteki jQuery, która znajduje się w ramach Microsoft ASP.NET MVC, aby utworzyć efekty animacji. Zaktualizowany widok indeks znajduje się w listę 4.

**Wyświetlanie listy 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Zwróć uwagę, że zaktualizowano widok indeksu zawiera trzy nowe funkcje kodu JavaScript. Pierwsze dwie funkcje Użyj jQuery zanikania i zanikania listy kontaktów, po kliknięciu przycisku nowe grupy kontaktów. Funkcja trzeci wyświetla komunikat o błędzie, gdy wyniki żądania Ajax błąd (na przykład limit czasu sieci).

Pierwszej funkcji również odpowiada on za wyróżnianie wybranej grupy. Klasa = zaznaczony atrybut zostanie dodany do elementu nadrzędnego (LI element) kliknięty element. Ponownie jQuery można łatwo wybrać prawy element i Dodaj klasę CSS.

Skrypty te są powiązane z łącza grupy za pomocą parametru Ajax.ActionLink() AjaxOptions. Zaktualizowano wywołania metody Ajax.ActionLink() wygląda następująco:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Dodawanie obsługi historii przeglądarki

Zwykle po kliknięciu łącze do strony aktualizacji historii przeglądarki jest aktualizowana. W ten sposób można kliknij przycisk Wstecz przeglądarki, aby przenieść z powrotem w czasie do poprzedniego stanu strony. Na przykład jeśli kliknij grupę, skontaktuj się z pomocą znajomych, a następnie kliknij przycisk grupy kontaktów biznesowych, kliknięcie przeglądarce przycisk Wstecz, aby nawigować do stanu strony, gdy została wybrana grupa kontaktów znajomych.

Niestety wykonywanie żądania Ajax nie aktualizuje historii przeglądarki automatycznie. Jeśli kliknij grupę kontaktów i listy zgodnych kontaktów są pobierane z żądaniem Ajax, historii przeglądania nie jest aktualizowana. Nie umożliwia przycisk Wstecz w przeglądarce przejdź z powrotem do grupy kontaktów po wybraniu nowej grupy kontaktów.

Jeśli chcesz, aby użytkownicy mogli używać Wstecz w przeglądarce przycisk po wykonaniu żądania Ajax następnie należy wykonać nieco więcej pracy. Należy korzystać z funkcji zarządzania historii przeglądarki wbudowanych w ASP.NET AJAX Framework.

Historia przeglądarki ASP.NET AJAX, należy wykonać trzy czynności:

1. Włącz historii przeglądarki, ustawiając właściwość enableBrowserHistory na wartość true.
2. Zapisz punkty historii przez wywołanie metody addHistoryPoint() po zmianie stanu widoku.
3. Rekonstrukcji stan widoku, gdy zdarzenia nawigacji.

Zaktualizowany widok indeks znajduje się w 5 wyświetlania.

**Wyświetlanie listy 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Wyświetlanie 5 historii przeglądarki jest włączona w funkcji pageInit(). Funkcja pageInit() służy również do konfigurowania programu obsługi zdarzeń dla zdarzenia nawigacji. Zdarzenia nawigacji jest wywoływane zawsze, gdy przeglądarki do przodu lub przycisk Wstecz powoduje, że stan strony, aby zmienić.

Metoda beginContactList() jest wywoływana po kliknięciu grupy kontaktów. Ta metoda tworzy nowy punkt historii przez wywołanie metody addHistoryPoint(). Identyfikator grupy kliknięty zostanie dodany do historii.

Identyfikator grupy jest pobierana z atrybutem expando łącze grupy kontaktów. Łącze jest renderowane z następujące wywołanie Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Ostatni parametr przekazany do Ajax.ActionLink() dodaje atrybut expando o nazwie identyfikatora groupid, który łączy (małe litery zgodności XHTML).

Gdy użytkownik trafi Wstecz w przeglądarce lub przycisk Prześlij dalej, zdarzenia Nawigacja i navigate() metoda jest wywoływana. Ta metoda aktualizacji kontaktów wyświetlane na stronie do stanu strony, która odpowiada punkt historii przeglądarki przekazywany do metody Nawigacja.

## <a name="performing-ajax-deletes"></a>Usuwa wykonywania Ajax

Aktualnie, aby usunąć kontakt, należy kliknij łącze Usuń, a następnie kliknij przycisk Usuń, wyświetlane na stronie Potwierdzenie usuwania (patrz rysunek 2). Prawdopodobnie wiele żądań strony czymś prosty przykład usunięcie rekordu bazy danych.


[![Usuń stronę potwierdzenia](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Rysunek 02**: strona potwierdzenia usunięcia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Jest kuszące Aby pominąć stronę potwierdzenia usunięcia i usunie kontakt bezpośrednio z widoku indeksu. Należy unikać to możliwość przesłania, ponieważ pobieranie takie podejście otwiera aplikację luk w zabezpieczeniach. Ogólnie rzecz biorąc ADAM t chcesz przeprowadzić operacji HTTP GET podczas wywoływania akcji, który modyfikuje stan aplikacji sieci web. Podczas usuwania, chcesz wykonać akcję POST protokołu HTTP lub jeszcze lepiej operacji HTTP DELETE.

Usuń łącze znajduje się w ContactList częściowej. Zaktualizowaną wersję z częściowa ContactList znajduje się w 6 wyświetlania.

**Wyświetlanie listy 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Usunięcie połączenia jest odwzorowywany z następujące wywołanie do metody Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() nie jest standard częścią struktury ASP.NET MVC. Ajax.ImageActionLink() jest metod niestandardowego elementu pomocniczego, dołączony do projektu menedżera kontaktu.


Parametr AjaxOptions ma dwie właściwości. Po pierwsze właściwość Potwierdź jest używana do wyświetlenia menu podręczne okno dialogowe potwierdzenia JavaScript. Po drugie właściwość HttpMethod jest używana do wykonywania określonej operacji HTTP DELETE.

Wyświetlanie listy 7 zawiera nową akcję AjaxDelete() dodaną skontaktuj się z kontrolerem.

**Wyświetlanie listy 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Akcja AjaxDelete() zostanie nadany atrybut AcceptVerbs. Ten atrybut zapobiega akcji wywoływanie z wyjątkiem przez żadnej operacji HTTP innych niż operacji HTTP DELETE. W szczególności nie można wywołać tej akcji z HTTP GET.

Po usunięciu rekordu bazy danych, należy wyświetlić zaktualizowaną listę kontaktów, który nie zawiera usunięty rekord. Metoda AjaxDelete() zwraca ContactList częściowe i zaktualizowanej listy kontaktów.

## <a name="summary"></a>Podsumowanie

W tym iteracji dodaliśmy funkcjonalności interfejsu Ajax do naszej aplikacji menedżera kontaktu. Aby zwiększyć czas reakcji i wydajności aplikacji My używamy Ajax.

Po pierwsze zrefaktoryzowaliśmy widoku indeksu, aby kliknięcie grupy kontaktów nie aktualizację całego widoku. Zamiast tego klikając grupę skontaktuj się z pomocą tylko zaktualizowanie listy kontaktów.

Następnie użyliśmy efekty animacji jQuery zanikania i zanikania listy kontaktów. Dodawanie animacji do aplikacji Ajax może służyć do zapewnienia użytkownikom odpowiednikiem pasek postępu przeglądarki aplikacji.

Dodaliśmy również obsługi w przeglądarce historii do naszej aplikacji Ajax. Firma Microsoft włączona użytkowników, kliknij przycisk Wstecz przeglądarki i przekazuj przycisk, aby zmienić stan widoku indeksu.

Na koniec utworzyliśmy łącze Usuń, który obsługuje operacje HTTP DELETE. Wykonując usuwa Ajax, możemy Zezwól użytkownikom na usuwanie rekordów bazy danych bez konieczności użytkownikowi żądanie stronę potwierdzenia usunięcia dodatkowe.

> [!div class="step-by-step"]
> [Poprzednie](iteration-6-use-test-driven-development-vb.md)
