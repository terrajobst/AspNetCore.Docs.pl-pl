---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Użyć AJAX w celu dostarczenia aktualizacji dynamicznych | Dokumentacja firmy Microsoft"
author: microsoft
description: "Implementuje kroku 10 obsługuje zalogowany użytkownikom RSVP zainteresowanie uczestniczący obiad, opartych na technologii Ajax podejście zintegrowane w ramach szczegółów obiad..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Użyj AJAX, aby dostarczać aktualizacje dynamiczne
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 10 bezpłatnych ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Implementuje kroku 10 obsługuje zalogowany użytkownikom RSVP zainteresowanie uczestniczący obiad, zintegrowane na stronie szczegółów obiad podejście opartych na technologii Ajax.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner krok 10: Akceptuje AJAX włączenie zbędne

Umożliwia teraz implementuje obsługę zalogowanych użytkowników do RSVP zainteresowanie uczestniczący obiad. Firma Microsoft będzie to umożliwić przy użyciu zintegrowanych na stronie szczegółów obiad podejście opartych na technologii AJAX.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Wskazującą, czy użytkownik jest RSVP'd

Użytkownicy mogą odwiedzać */Dinners/szczegóły / [identyfikator*] adres URL, aby zobaczyć szczegółowe informacje dotyczące konkretnego obiad:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Details() metody akcji jest zaimplementowana w następujący sposób:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Naszym pierwszym krokiem Obsługa RSVP będzie Dodaj metodę pomocnika "IsUserRegistered(username)" do naszej obiad obiektu (w ramach klasy częściowe Dinner.cs, którą wcześniej budujemy). Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy użytkownik jest obecnie RSVP'd na obiad:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Firma Microsoft następnie dodaj następujący kod do naszych Details.aspx widoku szablonu, aby wyświetlić odpowiedni komunikat wskazujący, czy użytkownik jest zarejestrowany lub nie dla zdarzenia:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

I teraz, gdy użytkownik odwiedza obiad, są zarejestrowane dla one wyświetlony ten komunikat:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

I po odwiedzeniu obiad, nie są zarejestrowane dla one wyświetlone poniżej komunikat:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementacja metody akcji rejestru

Teraz Dodajmy funkcje niezbędne do obsługi użytkowników do RSVP na obiad na stronie szczegółów.

Aby zaimplementować to, utworzymy nową klasę "RSVPController" katalogu \Controllers prawym przyciskiem myszy i wybierając Add -&gt;polecenia menu kontrolera.

Firma Microsoft będzie zaimplementować metodę akcji "Register" w ramach nowej klasy RSVPController, który ma identyfikator na obiad jako argument, pobiera odpowiedni obiekt obiad sprawdza, czy zalogowany użytkownik jest obecnie na liście użytkowników, którzy mają zarejestrowany, i czy nie dodaje obiekt RSVP dla nich:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Zwróć uwagę, nad jak możemy zwróconego prostego ciągu jako dane wyjściowe metody akcji. Firma Microsoft może osadzania tego komunikatu w wyświetlanie szablonu, ale ponieważ jest to mała właśnie użyjemy Content() metody pomocniczej kontrolera klasy podstawowej i zwracany ciąg komunikatu, takich jak powyżej.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Wywoływanie metody akcji RSVPForEvent za pomocą interfejsu AJAX

Użyjemy AJAX do wywołania metody akcji rejestrowania z naszych widok szczegółów. To wdrożenie jest dość proste. Najpierw dodamy dwa odwołania do biblioteki skryptu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

Biblioteka pierwszym odwołuje się do podstawowej biblioteki skryptu po stronie klienta ASP.NET AJAX. Ten plik jest około 24k rozmiaru (skompresowane) i zawiera podstawowe funkcje AJAX po stronie klienta. Drugi biblioteka zawiera funkcje narzędzia, które zintegrować z platformy ASP.NET MVC AJAX pomocnika metod wbudowanych (które użyjemy wkrótce).

Możemy zaktualizować kodu szablonu widoku, które wcześniej dodano tak, aby zamiast outputing komunikat "Użytkownik nie są zarejestrowane dla tego zdarzenia", firma Microsoft zamiast renderować łącze po naciśnięciu wykona wywołanie AJAX, która wywołuje metodę akcji naszych RSVPForEvent na kontrolera RSVP i RSVPs użytkownika:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Metodę pomocnika Ajax.ActionLink() powyżej jest zbudowany na platformie ASP.NET MVC i jest podobna do metody pomocniczej Html.ActionLink() z tą różnicą, że zamiast wykonywania standardowych nawigacji powoduje wywołanie AJAX do metody akcji po kliknięciu łącza. Jesteśmy powyżej wywołanie metody akcji "Register" controller "RSVP" i przekazywanie DinnerID jako parametru "id" do niego. Ostatni parametr AjaxOptions możemy przekazywane wskazuje chęć pobrać zawartości, zwracany przez metodę akcji i zaktualizować HTML &lt;div&gt; elementu na stronie, których identyfikator jest "rsvpmsg".

A teraz, gdy użytkownik przechodzi do obiad nie są one zarejestrowane dla jeszcze one wyświetlone łącze do RSVP dla niej:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Jeśli odbiorca kliknie link "RSVP dla tego zdarzenia" finalizowania będzie wywołanie AJAX do metody akcji rejestru na kontrolerze RSVP i po ukończeniu będzie zobaczy komunikat zaktualizowane, takich jak poniżej:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Przepustowość sieci i związane z wykonaniem tego wywołania AJAX jest naprawdę lekkie ruchu. Gdy użytkownik kliknie łącze "RSVP dla tego zdarzenia", mała POST protokołu HTTP sieci żądań do */Dinners/Register/1* adres URL, który wygląda jak poniżej w sieci:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

I odpowiedzi z naszych rejestru metody akcji jest po prostu:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

To wywołanie lekkie jest szybkie i będzie działać nawet w przypadku wolnej sieci.

### <a name="adding-a-jquery-animation"></a>Dodawanie jQuery animacji

Funkcje AJAX, które wprowadziliśmy działa dobrze i szybko. Czasami może się zdarzyć tak szybko, że użytkownik nie zauważyć, że łącze RSVP został zastąpiony nowym tekstem. Aby można było nieco bardziej oczywistymi wyniku dodamy prostych animacji, aby zwrócić uwagę na komunikat aktualizacji.

Domyślny szablon projektu programu ASP.NET MVC zawiera jQuery — biblioteka języka JavaScript znakomity (i bardzo popularny) typu open source, który także jest obsługiwany przez firmę Microsoft. jQuery oferuje następujące funkcje, takie jak nieuprzywilejowany HTML DOM wybór i efekty biblioteki.

Aby użyć jQuery najpierw dodamy skryptu odwołanie do niej. Ponieważ zamierzamy się przy użyciu jQuery w różnych miejscach w naszej witrynie, dodamy odwołanie do skryptu w ramach naszych plik strony głównej Site.master tak, aby wszystkie strony może być używany.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Porada: Upewnij się, że zainstalowano poprawkę intellisense języka JavaScript dla wersji programu VS 2008 z dodatkiem SP1, która umożliwia bardziej rozbudowane obsługę funkcji intellisense dla JavaScript plików (w tym jQuery). Możesz pobrać go z: http://tinyurl.com/vs2008javascripthotfix*

Kod napisany za pomocą JQuery często używa globalnej "$ ()" metody JavaScript, która pobiera jeden lub więcej elementów HTML za pomocą selektora CSS. Na przykład *$("#rsvpmsg")* wybiera dowolnego elementu HTML z identyfikatorem rsvpmsg, podczas gdy *$(".something")* wybrać wszystkie elementy z coś, co"CSS nazwę klasy. Można również napisać bardziej zaawansowanych zapytań, takie jak "return wszystkie przyciski zaznaczenia opcji" przy użyciu selektora zapytania takie jak: *$("dane wejściowe [@type= radiowych] [@checked]")*.

Po wybraniu elementów można wywoływać metod na podjęcie działań, takich jak ich: *$("#rsvpmsg").hide();*

W naszym scenariuszu RSVP zdefiniujemy prostych funkcji JavaScript o nazwie "AnimateRSVPMessage", który wybiera "rsvpmsg" &lt;div&gt; i animuje rozmiar jego zawartości tekstowej. Poniższego kodu, rozpoczyna się tekst małe, a następnie przyczyny go zwiększa się w milisekundach 400 przedział czasu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Firma Microsoft może następnie przewodowy w górę tej funkcji JavaScript do wywołania po pomyślnym przez przekazanie jej nazwę metody pomocnika naszych Ajax.ActionLink() naszych wywołanie AJAX (za pośrednictwem AjaxOptions "OnSuccess" właściwości zdarzenia):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

I po kliknięciu łącza "RSVP dla tego zdarzenia" i naszych wywołanie AJAX zostało ukończone pomyślnie, zawartości komunikat wysłany wstecz będzie animować wzrostu dużych:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oprócz zapewnienia zdarzenie "OnSuccess", obiekt AjaxOptions udostępnia OnBegin, OnFailure i OnComplete zdarzenia, które może obsłużyć (wraz z różnych innych właściwości i opcje przydatne).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Oczyszczanie - Zrefaktoryzuj limit RSVP widok częściowy

Nasze szablon widoku szczegółów rozpoczyna uzyskać nieco długi, które nadgodzinach będzie nieco trudniejsze do zrozumienia. Aby poprawić czytelność kodu, możemy zakończyć, tworzenie widoku częściowego — RSVPStatus.ascx — które hermetyzują cały kod RSVP widoku dla strony szczegółów.

Firma Microsoft można to zrobić, klikając prawym przyciskiem myszy w folderze \Views\Dinners, a następnie wybierając polecenie Add -&gt;wyświetlić polecenia menu. Firma Microsoft będziesz mieć ona potrwać obiektu obiad jako jego jednoznacznie ViewModel. Firma Microsoft może następnie skopiuj/Wklej zawartość RSVP z naszych widok Details.aspx do niego.

Gdy firma Microsoft to również Utwórzmy innego widoku częściowego — EditAndDeleteLinks.ascx - hermetyzujący naszego edytowanie i usuwanie link widoku kodu. Firma Microsoft będzie także dostępna go zająć obiektu obiad jako jego ViewModel jednoznacznie i kopiowania/wklejania logiki edytowanie i usuwanie z naszych widok Details.aspx do niego.

Szczegóły naszych wyświetlić szablonu, a następnie po prostu obejmują dwa wywołania metody Html.RenderPartial() u dołu:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Dzięki temu kod czyszczący do odczytu i obsługi.

### <a name="next-step"></a>Następny krok

Teraz Przyjrzyjmy się jak możemy jeszcze bardziej używać technologii AJAX i dodawanie interaktywnych mapowania obsługi do naszej aplikacji.

>[!div class="step-by-step"]
[Poprzednie](secure-applications-using-authentication-and-authorization.md)
[dalej](use-ajax-to-implement-mapping-scenarios.md)
