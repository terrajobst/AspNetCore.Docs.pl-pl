---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych | Dokumentacja firmy Microsoft
author: microsoft
description: Implementuje kroku 10 obsługuje zalogowanym użytkownikom RSVP zainteresowanie udziałowi obiad, przy użyciu podejścia opartego na technologii Ajax, zintegrowane w ramach szczegółów obiad...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825204"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 10 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Implementuje kroku 10 obsługuje zalogowanym użytkownikom RSVP zainteresowanie obiad, przy użyciu podejścia opartego na technologii Ajax, zintegrowane w ramach strony szczegółów obiad bierzesz udział w wydarzeniu.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner krok 10: Akceptuje AJAX, umożliwiając zbędne

Przejdźmy teraz zaimplementować obsługę zalogowanym użytkownikom RSVP zainteresowanie udziałowi obiad. Firma Microsoft będzie to umożliwić przy użyciu podejścia opartego na technologii AJAX, zintegrowane w ramach strony szczegółów obiad.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Wskazującą, czy użytkownik jest RSVP'd

Użytkownicy mogą odwiedzać */Dinners/szczegóły / [identyfikator*] adres URL, aby zobaczyć szczegółowe informacje dotyczące konkretnego obiad:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Details() metody akcji jest zaimplementowana w następujący sposób:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Naszym pierwszym krokiem do zaimplementowania obsługi RSVP zostaną dodane z metody pomocnika "IsUserRegistered(username)" nasze obiektowi obiad (w ramach Dinner.cs klasy częściowej których wcześniej skompilowany). Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy użytkownik jest aktualnie RSVP'd na obiad:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Firma Microsoft następnie dodaj następujący kod, aby szablon widok Details.aspx na wyświetli odpowiedni komunikat wskazujący, czy użytkownik jest zarejestrowany lub nie dla zdarzenia:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

A teraz, gdy użytkownik odwiedzi obiad, są rejestrowane dla zobaczą ten komunikat:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

I podczas odwiedzania obiad, nie są zarejestrowane dla zobaczą poniżej komunikat:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementacja metody akcji rejestru

Teraz Dodajmy funkcje niezbędne do obsługi użytkowników do RSVP na obiad na stronie szczegółów.

Aby wdrożyć to rozwiązanie, utworzymy nową klasę "RSVPController", kliknij prawym przyciskiem myszy w katalogu \Controllers i wybierając Add -&gt;polecenia menu kontrolera.

Firma Microsoft będzie zaimplementować metodę akcji "Register" w nowej klasie RSVPController, która ma identyfikator na obiad jako argument, pobiera odpowiedniego obiektu obiad sprawdza, jeśli zalogowany użytkownik jest obecnie na liście użytkowników, którzy zarejestrowali się go, a nie dodaje obiekt RSVP dla nich:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Zwróć uwagę na powyżej, jak firma Microsoft jest zwracany prostego ciągu jako dane wyjściowe metody akcji. Firma Microsoft może zostały osadzone tego komunikatu w ramach Wyświetl szablon, ale ponieważ tak mały, po prostu użyjemy Content() metody pomocniczej kontrolera klasy bazowej i zwracany komunikat w formacie ciągu, takich jak powyżej.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Wywoływanie metody akcji RSVPForEvent za pomocą interfejsu AJAX

Użyjemy AJAX do wywołania metody akcji rejestru z naszych widok szczegółów. Zaimplementowanie tego jest całkiem proste. Najpierw dodamy dwa odwołania do biblioteki skryptu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

Biblioteka pierwszego odwołuje się do podstawowej biblioteki skryptu po stronie klienta ASP.NET AJAX. Ten plik jest około 24k rozmiar (skompresować) i zawiera podstawowe funkcje AJAX po stronie klienta. Drugi biblioteka zawiera funkcje narzędzia, które integrują się z platformy ASP.NET MVC wbudowanych AJAX metody pomocnicze (które będziemy używać wkrótce).

Firma Microsoft może, a następnie Wyświetl kod szablonu, które dodaliśmy wcześniej, aby zamiast outputing komunikat "Nie zarejestrowano Cię do tego zdarzenia", firma Microsoft zamiast renderować łącze wypchnięcie aktualizacji wykonuje wywołanie AJAX, która wywołuje naszych RSVPForEvent metody akcji kontrolera RSVP i RSVPs użytkownika:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Metoda pomocnika Ajax.ActionLink() powyżej jest wbudowana w program ASP.NET MVC i jest podobna do metody pomocnika Html.ActionLink(), z tą różnicą, że zamiast wykonywania standardowych nawigacji to sprawia, że wywołanie AJAX do metody akcji po kliknięciu łącza. Jesteśmy powyżej wywołaniem metody akcji "Zarejestruj" na kontrolerze "RSVP" i przekazując DinnerID jako parametr "id" do niego. Ostatni parametr AjaxOptions możemy kończy się sukcesem wskazuje, czy chcesz pobrać zawartości, zwracany przez metodę akcji i zaktualizuj kod HTML &lt;div&gt; elementu na stronie, w których identyfikator jest "rsvpmsg".

A teraz gdy użytkownik przegląda obiad nie są one zarejestrowane dla, ale zobaczą łącze do RSVP dla niego:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Po kliknięciu linku "RSVP dla tego zdarzenia" dokonają będzie wywołanie AJAX do metody akcji, zarejestruj się na kontrolerze RSVP i po jej zakończeniu użytkownik zobaczy komunikat zaktualizowane, takich jak poniżej:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Przepustowość sieci i związanych z wywołania AJAX ruch jest bardzo uproszczone. Gdy użytkownik kliknie link "RSVP dla tego zdarzenia", małe POST protokołu HTTP sieci żądań do */Dinners/Register/1* adresu URL, który wygląda jak poniżej w sieci:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

I odpowiedź z naszej rejestru metody akcji jest po prostu:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

To wywołanie uproszczone jest szybkie i będzie działać nawet w przypadku wolnej sieci.

### <a name="adding-a-jquery-animation"></a>Dodawanie jQuery animacji

Funkcji interfejsu AJAX, którą wdrożyliśmy działa dobrze i szybkie. Czasami może się zdarzyć tak szybko, że użytkownik może nie zostać zauważony czy RSVP link został zastąpiony nowym tekstem. Aby w wyniku nieco bardziej oczywisty możemy dodać prostej animacji, aby zwrócić uwagę na komunikat aktualizacji.

Domyślny szablon projektu platformy ASP.NET MVC zawiera jQuery — Biblioteka JavaScript doskonałą (i bardzo popularna) typu open source, który także jest obsługiwany przez firmę Microsoft. jQuery oferuje pewną liczbę funkcji, takich jak nieuprzywilejowany HTML DOM wybór i efekty bibliotekę.

Aby użyć jQuery najpierw dodamy skryptu odwołanie do niej. Ponieważ firma Microsoft zamierza się przy użyciu jQuery w różnych miejscach w naszej witrynie, dodamy odwołanie do skryptu w ramach naszych plik strony głównej Site.master tak, aby go używać wszystkich stron.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Porada: Upewnij się, że została zainstalowana poprawka funkcji intellisense języka JavaScript dla programu VS 2008 z dodatkiem SP1, która umożliwia bogatszych obsługę funkcji intellisense dla plików JavaScript (w tym jQuery). Możesz pobrać go z: http://tinyurl.com/vs2008javascripthotfix*

Kod napisany za pomocą technologii JQuery często używa globalnego "$ ()" metody JavaScript, która pobiera jeden lub więcej elementów HTML za pomocą selektora CSS. Na przykład <em>$("#rsvpmsg")</em> wybiera dowolnego elementu HTML o identyfikatorze rsvpmsg, podczas gdy <em>$(".something")</em> wybrać wszystkie elementy o coś, co"CSS nazwy klasy. Można także napisać bardziej zaawansowanych zapytań, takich jak "return wszystkie przyciski radiowe zaznaczone" przy użyciu selektora zapytania takiego jak: <em>$("dane wejściowe [@type= radio] [@checked]")</em>.

Po wybraniu elementów może wywoływać metody na nich podjęcia działania, takie jak ich: *$("#rsvpmsg").hide();*

W naszym scenariuszu RSVP zdefiniujemy prostą funkcję języka JavaScript o nazwie "AnimateRSVPMessage", który wybiera "rsvpmsg" &lt;div&gt; i animuje rozmiar jego zawartości tekstowej. Poniższego kodu, rozpoczyna się mały tekst, a następnie powoduje, że jego zwiększenie w czasu 400 milisekund:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Firma Microsoft może następnie o komunikacji sieciowej w górę ta funkcja JavaScript do wywołania po naszym wywołanie AJAX pomyślnym ukończeniu przez przekazanie nazwy do naszych metody pomocnika Ajax.ActionLink() (za pośrednictwem AjaxOptions "OnSuccess" właściwości zdarzenia):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

A teraz po kliknięciu łącza "RSVP dla tego zdarzenia" i naszych wywołanie AJAX zakończy się pomyślnie, zawartości komunikat wysłany wstecz będzie animować i powiększać dużych:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oprócz zapewniania zdarzenie "OnSuccess", obiekt AjaxOptions udostępnia zdarzenia OnBegin, OnFailure i OnComplete, które może obsłużyć (wraz z różnych innych właściwości i użyteczne opcje).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Oczyszczanie — Zrefaktoryzuj się widok częściowy Odpowiedz na zaproszenie

Szablon widoku szczegółów rozpoczyna pobieranie nieco długie, które nadgodzinach, spowoduje to nieco trudniejsze do zrozumienia. Aby poprawić czytelność kodu, umożliwia zakończenie się przez utworzenie widoku częściowego — RSVPStatus.ascx — która hermetyzacji wszystkich RSVP Wyświetl kod dla naszej stronie szczegółów.

Możemy to zrobić przez kliknięcie prawym przyciskiem myszy \Views\Dinners folder, a następnie wybierając polecenie Add -&gt;wyświetlać polecenia menu. Odpowiemy od niego obiekt obiad jako jego silnie typizowane ViewModel. Firma Microsoft może następnie kopiowania/wklejania zawartości RSVP z naszych widok Details.aspx tę sytuację.

Gdy dotychczasowej, również Utwórz innego widoku częściowego — EditAndDeleteLinks.ascx —, który hermetyzuje naszych edytowania i usuwania łącze Wyświetl kod. Zostanie też przeprowadzona pobrać obiekt obiad, jako jego ViewModel silnie typizowane i kopiowania/wklejania logiki edytowanie i usuwanie z naszych widok Details.aspx tę sytuację.

Nasze Szczegóły szablonu można wyświetlić, a następnie po prostu Dołącz dwa wywołania metody Html.RenderPartial() u dołu:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

To sprawia, że kod czyszczenia do odczytywania i obsługa.

### <a name="next-step"></a>Następny krok

Teraz Przyjrzyjmy jak możemy dalsze korzystanie z technologii AJAX i dodanie obsługi interaktywnych mapowania do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](secure-applications-using-authentication-and-authorization.md)
> [dalej](use-ajax-to-implement-mapping-scenarios.md)
