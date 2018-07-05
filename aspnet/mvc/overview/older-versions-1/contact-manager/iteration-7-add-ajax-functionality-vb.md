---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iteracja #7 — dodawanie funkcji Ajax (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: f951135014c46ef2685955700dc5649581539989
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394258"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iteracja #7 — dodawanie funkcji Ajax (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (VB)
  

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym trzeci iteracji możemy skorzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Tej iteracji

W tym iteracji aplikacji Contact Manager refaktoryzacji naszej aplikacji do korzystania z tych technologii Ajax. Dzięki wykorzystaniu technologii Ajax, udostępnimy naszej aplikacji zwiększyć szybkość reakcji. Firma Microsoft można uniknąć renderowania na całej stronie, gdy będziemy musieli zaktualizować tylko niektórych regionu na stronie.

Tak, aby firma Microsoft don t trzeba ponownie wyświetlić całej strony, zawsze wtedy, gdy ktoś nie wybierze nową grupę skontaktuj się z pomocą naszych widoku indeksu będzie refaktoryzacji. Zamiast tego gdy ktoś kliknie skontaktuj się z grupy, utworzymy tylko zaktualizowanie listy kontaktów i pozostaw samodzielnie pozostałej części strony.

Ponadto zmienimy sposób nasze Usuń łącze działa. Zamiast oddzielnych archiwizacyjnych, firma Microsoft będą wyświetlane okno dialogowe potwierdzenia języka JavaScript. Jeśli upewnij się, że chcesz usunąć kontakt, operacją HTTP DELETE jest wykonywana na serwerze, aby usunąć rekord, skontaktuj się z pomocą z bazy danych.

Ponadto firma Microsoft będzie korzystać z jQuery, aby dodać efektów animacji do naszych widoku indeksu. Firma Microsoft będą wyświetlane animacji, gdy nową listę kontaktów jest pobieranych z serwera.

Ponadto firma Microsoft będzie korzystać z ASP.NET AJAX framework obsługę funkcji zarządzania historię przeglądania. Utworzymy punkty historii, zawsze wtedy, gdy będziemy wykonywać wywołania Ajax do zaktualizowania listy kontaktów. W ten sposób przeglądarki z poprzednimi wersjami i do przodu przyciski będą działać.

## <a name="why-use-ajax"></a>Dlaczego warto używać technologii Ajax?

Za pomocą interfejsu Ajax ma wiele zalet. Po pierwsze dodawanie funkcjonalności Ajax do aplikacji skutkuje lepsze środowisko użytkownika. W aplikacji sieci web normalne całą stronę musi zaksięgowania serwer czasu każdy użytkownik wykona akcję. Zawsze, gdy po wykonaniu akcji blokad przeglądarki i użytkownik musi poczekaj, aż całej strony jest pobierane i wyświetlane ponownie.

Powinien to być niedopuszczalne środowisko, w przypadku aplikacji klasycznej. Jednak tradycyjne możemy znajdowały się za pomocą płynność pracy w przypadku aplikacji sieci web ponieważ firma Microsoft nie wiedział nic, możemy zrobić lepiej. Pomyśleliśmy to ograniczenie aplikacji sieci web, gdy w rzeczywistości była to ograniczenie naszych imaginations.

W aplikacji interfejsu Ajax komputer t trzeba przełączyć środowisko użytkownika do zatrzymania tylko do aktualizacji strony. Zamiast tego można wykonać żądania asynchronicznego w tle do aktualizacji strony. Komputer życie t użytkownikowi Zaczekaj, aż zostanie zaktualizowany części strony.

Dzięki wykorzystaniu technologii Ajax, możesz także może zwiększyć wydajność aplikacji. Należy wziąć pod uwagę, jak aplikacja Contact Manager obecnie działa bez funkcji interfejsu Ajax. Po kliknięciu przycisku grupy kontaktów wyświetlane musi ponownie całego widoku indeksu. Lista kontaktów i listę grup, skontaktuj się z pomocą, musi zostać pobrany z serwera bazy danych. Wszystkie te dane muszą być przekazywane w sieci z serwera sieci web do przeglądarki sieci web.

Po dodamy funkcji interfejsu Ajax do naszej aplikacji, jednak firma Microsoft można uniknąć ponowne wyświetlanie całej strony, gdy użytkownik kliknie skontaktuj się z grupy. Firma Microsoft nie muszą już do pobrania grup kontaktów z bazy danych. Możemy również don konieczność t wypychania całego widoku indeksu faktycznie. Dzięki wykorzystaniu technologii Ajax, zmniejszamy ilość pracy naszych serwera bazy danych należy wykonać, a firma Microsoft zmniejszyć ilość ruchu sieciowego, wymagane przez naszą aplikację.

## <a name="don-t-be-afraid-of-ajax"></a>Don t można obawiasz się elementu Ajax

Niektórzy deweloperzy uniknąć, za pomocą interfejsu Ajax, ponieważ mogą zajmować przeglądarek niskiego poziomu. Chcą upewnić się, że ich aplikacje sieci web będą nadal działać podczas uzyskiwania dostępu do przeglądarki, która nie obsługuje języka JavaScript. Ponieważ Ajax zależy od języka JavaScript, niektórzy deweloperzy uniknąć, za pomocą interfejsu Ajax.

Jednak jeśli zwracać uwagę na sposób implementacji interfejsu Ajax następnie możesz tworzyć aplikacje współpracujące z przeglądarkami wyższej i niskiego poziomu. Nasza aplikacja Contact Manager będzie działać z przeglądarki, które obsługuje języka JavaScript i przeglądarek, które nie obsługują.

Jeśli korzystasz z aplikacji Contact Manager za pomocą przeglądarki, która obsługuje język JavaScript będzie mieć lepsze środowisko użytkownika. Na przykład po kliknięciu przycisku skontaktuj się z grupy zostaną zaktualizowane tylko regionu strony, która zawiera kontakty.

Jeśli z drugiej strony, za pomocą aplikacji Contact Manager za pośrednictwem przeglądarki, która nie obsługuje języka JavaScript (lub ma Obsługa skryptów JavaScript wyłączona) będzie mieć nieco mniej pożądana środowisko użytkownika. Na przykład po kliknięciu przycisku grupy kontaktów całego widoku indeksu musi zaksięgowania przeglądarki w celu wyświetlania zgodnych listy kontaktów.

## <a name="adding-the-required-javascript-files"></a>Trwa dodawanie plików wymagany język JavaScript

Musimy użyć trzy pliki JavaScript, aby dodać funkcji interfejsu Ajax do naszej aplikacji. Wszystkie trzy te pliki znajdują się w folderze skrypty w nowej aplikacji platformy ASP.NET MVC.

Jeśli planujesz korzystanie z technologii Ajax na wielu stronach w aplikacji następnie warto uwzględnić wymagane pliki JavaScript w Twojej aplikacji s widoku strony wzorcowej. Dzięki temu pliki JavaScript będzie załączona wszystkich stron w aplikacji automatycznie.

Dodaj następujący kod JavaScript obejmuje wewnątrz &lt;head&gt; tag strony wzorcowej widoku:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refaktoryzacja widoku indeksu na korzystanie z technologii Ajax

Pozwól s start modyfikując naszych widoku indeksu, tak, że kliknięcie skontaktuj się z grupy aktualizuje tylko regionu widoku, który wyświetla kontaktów. Czerwone pola na rysunku 1 zawiera region, który chcesz zaktualizować.


[![Aktualizowanie tylko kontakty](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Rysunek 01**: aktualizowanie tylko kontakty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-7-add-ajax-functionality-vb/_static/image2.png))


Pierwszym krokiem jest, aby oddzielić część widok, który chcesz zaktualizować asynchronicznie w oddzielnym częściowe (kontrolka użytkownika widoku). Części widoku indeksu, która jest wyświetlana tabela kontaktów została przeniesiona do częściowego w ofercie 1.

**Wyświetlanie listy 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Należy zauważyć, że częściowego w ofercie 1 ma modelu innego niż widok indeksu. *Inherits* atrybutu w &lt;% @ % strony&gt; dyrektywa określa, że częściowego dziedziczy ViewUserControl&lt;grupy&gt; klasy.

Zaktualizowano widoku indeks znajduje się w ofercie 2.

**Wyświetlanie listy 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Istnieją dwie czynności, które użytkownik zauważy, informacje o zaktualizowanych widoku w ofercie 2. Najpierw zwróć uwagę, że całą zawartość przeniesiona do częściowego, jest zastępowany Html.RenderPartial() po wywołaniu. Metoda Html.RenderPartial() jest wywoływana, gdy najpierw żąda widoku indeksu w celu wyświetlenia początkowy zestaw kontaktów.

Po drugie Zwróć uwagę, że Ajax.ActionLink() została zastąpiona Html.ActionLink() używany do wyświetlania grup kontaktów. Ajax.ActionLink() jest wywoływana z następującymi parametrami:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Pierwszy parametr reprezentuje tekst do wyświetlenia dla tego połączenia, drugi parametr reprezentuje wartości trasy i trzeci parametr reprezentuje opcje technologii Ajax. W tym przypadku używamy opcji technologii UpdateTargetId Ajax wskaż HTML &lt;div&gt; tag, który chcemy aktualizacji po ukończeniu żądania Ajax. Chcemy zaktualizować &lt;div&gt; tagu za pomocą nowej listy kontaktów.

Zaktualizowana metoda indeks() kontrolera kontaktu znajduje się w ofercie 3.

**Wyświetlanie listy 3 - Controllers\ContactController.vb (metoda indeks)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Zaktualizowano akcję indeks() warunkowo zwraca jedną z dwóch kwestii. Ten kontroler zwraca częściowym, jeśli akcja indeks() jest wywoływana przez wysłanie żądania Ajax. W przeciwnym razie akcja indeks() zwraca całego widoku.

Należy zauważyć, że akcja indeks() musi zwracać tak dużej ilości danych, gdy wywoływane przez żądanie Ajax. W kontekście normalne żądania akcji indeksu zwraca listę wszystkich grup kontaktów i skontaktuj się z wybranej grupy. W kontekście żądaniem Ajax Akcja indeks() zwraca wybranej grupy. AJAX oznacza mniej pracy na serwerze bazy danych.

Nasze zmodyfikowany widok indeksu działa w przypadku zarówno wyższej, jak i niskiego poziomu. Jeśli kliknij grupę kontaktów i przeglądarka obsługuje język JavaScript, jest aktualizowany tylko regionu widok, który zawiera listy kontaktów. Jeśli z drugiej strony, przeglądarka nie obsługuje języka JavaScript, jest aktualizowana całego widoku.


Nasze zaktualizowany widok indeksu ma jeden problem. Po kliknięciu skontaktuj się z grupy, nie zostanie on wyróżniony wybranej grupy. Ponieważ poza regionem, która jest aktualizowana podczas żądaniem Ajax zostanie wyświetlona lista grup, nie uzyskać wyróżnione odpowiedniej grupy. Naprawimy ten błąd w następnej sekcji.


## <a name="adding-jquery-animation-effects"></a>Dodawanie efektów animacji jQuery

Zwykle po kliknięciu linku na stronie sieci web, pasek postępu przeglądarki można użyć do wykrywania, czy przeglądarka jest aktywnie podczas pobierania zaktualizowanej zawartości. Podczas wykonywania żądaniem Ajax, z drugiej strony, pasek postępu przeglądarki nie wyświetla żadnego postępu. Dzięki temu może być użytkownicy nerwowe. Skąd wiadomo, czy został zamrożony przeglądarki?

Istnieje kilka sposobów, które można wskazać użytkownikowi, że praca odbywa się podczas wykonywania żądaniem Ajax. Jedno z podejść jest wyświetlanie prostej animacji. Na przykład użytkownik może zanikanie region gdy rozpoczyna się żądaniem Ajax, a zanikanie w regionie, po zakończeniu żądania.

Użyjemy biblioteki jQuery, który jest dołączony do programu Microsoft ASP.NET MVC framework, do tworzenia efektów animacji. Zaktualizowano widoku indeks znajduje się w ofercie 4.

**Wyświetlanie listy 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Należy zauważyć, że zaktualizowany widok indeksu zawiera trzy nowe funkcje języka JavaScript. Pierwsze dwie funkcje Użyj jQuery zanikanie i zanikanie liście kontaktów, po kliknięciu skontaktuj się z nową grupę. Trzecia funkcja wyświetla komunikat o błędzie, gdy wyniki żądania Ajax w błąd (na przykład limit czasu sieci).

Pierwsza funkcja również zajmuje się wyróżnianie wybranej grupy. Klasa = zaznaczony atrybut zostanie dodany do elementu nadrzędnego (LI element) elementu kliknięciu. Ponownie jQuery ułatwia wybieranie odpowiednich elementów i dodać klasę CSS.

Skrypty te są powiązane łącza grup współzależności za pomocą parametru Ajax.ActionLink() AjaxOptions. Zaktualizowano wywołania metody Ajax.ActionLink() wygląda następująco:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Dodanie obsługi historii przeglądarki

Zwykle po kliknięciu łącza do aktualizacji strony, historii przeglądarki jest aktualizowana. W ten sposób możesz kliknąć przycisk Wstecz przeglądarki, aby przenieść z powrotem w czasie do poprzedniego stanu strony. Jeśli kliknij grupę, skontaktuj się z pomocą znajomych, a następnie kliknij grupę, skontaktuj się z pomocą firmy, można na przykład, kliknij przycisk Wstecz przeglądarki, aby przejść z powrotem do stanu strony wybranie grupy kontaktowe znajomych.

Niestety wykonywania żądaniem Ajax nie aktualizuje historię przeglądania automatycznie. Jeśli kliknij grupę kontaktów i listy zgodnych kontakty są pobierane z żądaniem Ajax, historii przeglądania nie jest aktualizowana. Nie można użyć przycisku Wstecz w przeglądarce do przejdź z powrotem do grupy kontaktów po wybraniu nowej grupy skontaktuj się z pomocą.

Aby użytkownicy mogli korzystać z przeglądarki, wróć przycisku po wykonaniu żądania Ajax należy przeprowadzić trochę więcej pracy. Musisz korzystać z zalet funkcji Zarządzanie historii przeglądarki, wbudowane struktury programu ASP.NET AJAX.

Historię przeglądania kodu ASP.NET AJAX, należy wykonać trzy czynności:

1. Włącz historię przeglądania, ustawiając właściwość enableBrowserHistory na wartość true.
2. Zapisz punkty historii, przez wywołanie metody addHistoryPoint() po zmianie stanu widoku.
3. Gdy zostanie wywołane zdarzenie navigate, należy odtworzyć stan widoku.

Zaktualizowano widoku indeks znajduje się w ofercie 5.

**Wyświetlanie listy 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

W ofercie 5 historię przeglądania jest włączone w funkcji pageInit(). Funkcja pageInit() umożliwia również konfigurowanie obsługi zdarzeń dla zdarzenia Nawigacja. Zdarzenie Nawigacja jest wywoływane zawsze wtedy, gdy do przodu w przeglądarce lub przycisk Wstecz powoduje, że stan strony Aby zmienić.

Metoda beginContactList() jest wywoływana, gdy kliknij grupę kontaktów. Ta metoda tworzy nowy punkt historii przez wywołanie metody addHistoryPoint(). Identyfikator grupy kontaktów kliknięto jest dodawana do historii.

Identyfikator grupy jest pobierana z atrybutem expando łącze, skontaktuj się z grupy. Łącze jest renderowane przy użyciu następujących wywołanie Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Ostatni parametr przekazywany do Ajax.ActionLink() dodaje atrybut expando o nazwie groupid łącza (małe litery zgodności XHTML).

Gdy użytkownik trafi w przeglądarce wstecz lub przycisk Prześlij dalej, navigate zdarzenie jest wywoływane i zostanie wywołana metoda navigate(). Ta metoda aktualizuje kontaktów wyświetlane na stronie, aby odpowiadał stanowi stronę która odnosi się do punktów historii przeglądarki przekazywany do metody Nawigacja.

## <a name="performing-ajax-deletes"></a>Usuwa wykonywania Ajax

Obecnie, aby możliwe było usunięcie kontakt, należy kliknąć łącze Usuń, a następnie kliknij przycisk Usuń, wyświetlany na stronie Potwierdzenie usuwania (patrz rysunek 2). Prawdopodobnie wiele żądań strony, aby zrobić coś prostego, takich jak usunięcie rekordu bazy danych.


[![Strona potwierdzenia usunięcia](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Rysunek 02**: strona potwierdzenia usunięcia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Jest kuszące, aby pominąć strona potwierdzenia usunięcia i usuwanie kontaktu bezpośrednio z widoku indeksu. Ta możliwość przesłania należy unikać, ponieważ w ten sposób spowoduje otwarcie aplikacji luk w zabezpieczeniach. Ogólnie rzecz biorąc don t chcesz wykonać operację GET protokołu HTTP, podczas wywoływania akcji, która modyfikuje stan aplikacji sieci web. Podczas usuwania, należy wykonać metodę POST protokołu HTTP lub jeszcze lepiej operacją HTTP DELETE.

Łącze Usuń znajduje się w ContactList częściowe. Zaktualizowaną wersję ContactList częściowe są zawarte w ofercie 6.

**Wyświetlanie listy 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Łącze Usuń jest renderowany przy użyciu następujących wywołania do metody Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() nie jest wchodzi w skład struktury ASP.NET MVC. Ajax.ImageActionLink() jest metod niestandardowego elementu pomocniczego, zawarty w projekcie Contact Manager.


Parametr AjaxOptions ma dwie właściwości. Po pierwsze właściwość Potwierdź służy do wyświetlenia menu podręczne okno dialogowe potwierdzenia języka JavaScript. Po drugie HttpMethod jest używana do wykonywania operacji HTTP DELETE.

Wyświetlanie listy 7 zawiera nową akcję AjaxDelete(), która została dodana do kontrolera kontaktu.

**Wyświetlanie listy 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Akcja AjaxDelete() zostanie nadany atrybut AcceptVerbs. Ten atrybut zapobiega akcji wywoływanie z wyjątkiem przy każdej operacji HTTP inne niż operacją HTTP DELETE. W szczególności nie można wywołać tę czynność za pomocą żądania HTTP GET.

Po usunięciu rekordu bazy danych, należy wyświetlić zaktualizowaną listę kontaktów, który nie zawiera usunięty rekord. Metoda AjaxDelete() zwraca ContactList częściowe i zaktualizowaną listę kontaktów.

## <a name="summary"></a>Podsumowanie

W tej iteracji dodaliśmy funkcji interfejsu Ajax do naszej aplikacji Contact Manager. Użyliśmy Ajax, aby poprawić czas odpowiedzi i wydajność naszej aplikacji.

Po pierwsze zrefaktoryzowaliśmy widoku indeksu tak, że kliknięcie skontaktuj się z grupy nie powoduje aktualizacji całego widoku. Zamiast tego kliknij grupę, skontaktuj się z tylko zaktualizowanie listy kontaktów.

Następnie użyliśmy efektów animacji jQuery zanikanie do zastosowania blaknięcia liście kontaktów. Dodawanie animacji do aplikacji Ajax może służyć do udzielania aplikacji ekwiwalent pasek postępu przeglądarki.

Dodaliśmy także obsługę historii przeglądarki do naszej aplikacji Ajax. Umożliwiliśmy użytkownikom kliknij przeglądarki, wróć i przekazywać je przycisków, aby zmienić stan widoku indeksu.

Na koniec utworzyliśmy łącze Usuń, który obsługuje operacje HTTP DELETE. Wykonując usuwa Ajax, firma Microsoft pozwalają użytkownikom na usuwanie rekordów bazy danych bez konieczności użytkownika żądania na stronie Potwierdzenie usunięcia dodatkowych.

> [!div class="step-by-step"]
> [Poprzednie](iteration-6-use-test-driven-development-vb.md)
