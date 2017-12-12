---
uid: single-page-application/overview/templates/breezeknockout-template
title: "Błyskawicznie/odcinania szablonu | Dokumentacja firmy Microsoft"
author: madskristensen
description: "Szablon błyskawicznie/odcinania jednej strony aplikacji"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>Błyskawicznie/odcinania szablonu
====================
przez [Mads Kristensen](https://github.com/madskristensen)

> Błyskawicznie/odcinania szablonu MVC zapisał dzwonka wewnątrz
> 
> [Pobieranie szablonu MVC błyskawicznie/odcinania](https://go.microsoft.com/fwlink/?LinkId=282649)


Wiesz "aplikacji jednej strony" (SPA) zastanawiasz się, co to jest. Podczas odczytu można informacji na ten temat możesz czy raczej możliwości systemu. Ale kto ma czas pobierania próbki? Jeśli masz program Visual Studio, będziesz mieć przykład SPA i uruchomione w mniej niż 60 sekund z platformą ASP.NET MVC 4 szablon "Błyskawicznie/odcinania jednej strony aplikacji".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Co to jest szablon SPA błyskawicznie/odcinania?

Większość szablonów projektu Generowanie szkielet aplikacji. Umieść ciało tych kości przez dodanie kodu i ostatecznie dostarczania działającą aplikację. Błyskawicznie/odcinania SPA szablonu jest inny. Generuje on przykładową aplikację można zbadać. Przedstawia on projekt aplikacji JEDNOSTRONICOWEJ i wiele metod tworzenia SPA.

Szablon błyskawicznie/Knockout jest odmianą na [szablonu SPA elementami KnockoutJS](../introduction/knockoutjs-template.md) dołączony do platformy ASP.NET i zaktualizować 2012.2 narzędzia sieci Web. Szablon SPA błyskawicznie generuje aplikacji przy użyciu tego samego środowiska użytkownika, ale ma inną implementację, przy użyciu błyskawicznie w celu zarządzania danymi.

Szablon SPA elementami KnockoutJS sprawia, że żądania obsługi z jQuery raw AJAX, która jest odpowiednia dla prostej aplikacji. Jednak bardziej złożone aplikacje mają większe wymagania dotyczące zarządzania danych. Na przykład większość aplikacji:

- Zapytania i ponownie zapytania do serwera podczas sesji rozszerzonej użytkownika.
- Dodaj zapytanie filtry, sortowania i stronicowania.
- Udostępnianie tych samych danych między kilka ekranów.
- Accumulate zmiany wiele obiektów, a następnie zapisać je jako jedna transakcja.
- Sprawdzanie poprawności zmian na komputerze klienckim, więc użytkownik może poprawić błędy przed zatwierdzeniem zmian w bazie danych.

Biblioteka BreezeJS obsługuje tych zadań za Ciebie zwalnianie opracowywanie aplikacji logiki i środowisko użytkownika najbardziej interesujące.

[**Błyskawicznie** ](http://www.breezejs.com/?utm_source=ms-spa) jest biblioteki typu open source do tworzenia zaawansowanych danych aplikacji JavaScript i HTML, rodzaju aplikacje, które wcześniej zostały dostarczone jako autonomicznej aplikacji klasycznych.

Szablon błyskawicznie/odcinania pomaga Ci ten pierwszy kluczowy etap na drodze bardziej niezawodna infrastruktura zarządzania danych. Tworzy przykładowej aplikacji Todo wypukłymi identyczną z elementami KnockoutJS SPA szablonu. Wewnątrz zastępuje Warstwa danych AJAX błyskawicznie, aby porównać dwa zbliża się obok siebie. Oczywiście prawie krawędzi możliwości aplikacji błyskawicznie. Ale zobaczysz, jak działa błyskawicznie i jak najmniejszy jest wymagana do udostępnienia tej przejścia.

Zacznijmy od początku.

## <a name="create-a-breezeknockout-template-project"></a>Tworzenie błyskawicznie/odcinania projektu szablonu

Pobierz i zainstaluj szablonu, klikając przycisk Pobierz powyżej. Szablon jest dostarczana jako plik rozszerzenia serwera Visual Studio (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt i kliknij przycisk **OK**.

W **nowy projekt** kreatora wybierz **SPA odcinania błyskawicznie**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Naciśnij klawisze Ctrl-F5, aby skompilować i uruchomić aplikację bez debugowania, lub naciśnij klawisz F5, aby uruchomić z debugowaniem.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Po pierwszym uruchomieniu aplikacji wyświetla ekran logowania. Kliknij łącze "Rejestracji" i glides nową stronę do wyświetlenia, gdzie można wprowadzić nazwę użytkownika i hasło. (Stron logowania i rejestracji są tworzone przy użyciu platformy ASP.NET MVC.) Po przesłaniu formularza rejestracji serwer generuje TodoList z dwoma elementami dla Twojego konta. Następnie stanowi je użytkownikowi na żółty notatki.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Teraz są w ziemi SPA. Wszystko, co możesz zobaczyć i wystąpić podczas manipulacji Todos jest renderowany i zarządzane na komputerze klienckim za pomocą Knockout i błyskawicznie. Eksploruj aplikacji jako użytkownik... ale oka dewelopera. Narzędzia developer w przeglądarce służy do przechwytywania ruchu sieciowego. (W programie Internet Explorer: naciśnij klawisz F12, wybierz **sieci** , a następnie kliknij pozycję **rozpoczęciu przechwytywania**.) Spróbuj wykonać następujące czynności:

- Dodaj nowy element zadania.
- Kliknij etykietę i edytować tytuł elementu Todo
- Sprawdź pole wyboru, aby oznaczyć element gotowe. Czy pole tekstowe jest wyłączony, przez co nie jest edytowalny tytuł powiadomienia.
- Kliknij przycisk "x" z prawej strony etykiety. Element zniknie i zostaną usunięte z bazy danych.
- Wybierz inny element i usuń zaznaczenie tytułu. Zostanie wyświetlony błąd sprawdzania poprawności, że tytuł jest wymagana. Po wstrzymaniu krótki poprzednie tytuł został przywrócony.
- Wpisz tytuł było bardzo długi. Zostanie wyświetlony błąd sprawdzania poprawności różnych czy tytuł jest zbyt długi.
- Kliknij przycisk "Dodaj listy Todo". Nowa lista pojawia się na poprzedniej liście z lewej strony.
- Odtwarzanie z tytułu listy zadań, wyzwalania wymaga i długość operacji sprawdzania poprawności.
- Kliknij w polu tekstowym Tytuł, aby wyczyścić komunikat o błędzie.
- Kliknij przycisk "x" w okręgu w prawym górnym rogu, aby usunąć TodoList i jego todos.

Logika sprawdzania poprawności jest wykonywane po stronie klienta przez błyskawicznie. Atrybuty weryfikacji na serwerze klasy modelu są propagowane do klienta i wykonywane automatycznie, zanim klient kontaktuje się z serwerem.

Przejrzyj ruchu sieciowego. Zwróć uwagę, że nie było żadnych wywołania do serwera podczas błyskawicznie wykrył błąd. Każda zmiana prawidłowy w wyniku żądania POST do "/ api/zadania/SaveChanges". Błyskawicznie zawiera również zmiany i wysyła je razem jako pojedyncze żądanie do kontrolera interfejsu API sieci Web `SaveChanges` metody. Różni się od szablonu KockoutJS SPA, dzięki czemu PUT, POST i usuwania żądań dla każdego elementu indywidualnie.

## <a name="peek-inside"></a>Wgląd w

Ta aplikacja ma po stronie klienta i po stronie serwera. Stos po stronie klienta składa się z niewielką HTML i kombinację modułów JavaScript aplikacji (w folderze "aplikacja") oraz innych firm bibliotek JavaScript (w folderze "Skryptów").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Jeśli szablon SPA elementami KnockoutJS zostały zbadane, to wygląda bardzo znane. Skoncentruj się na polach niebieski. Architektura interfejsu użytkownika jest Model-View-ViewModel (MVVM), w którym elementy widget HTML widoku prawidłowo są oddzielone od obsługi kodu prezentacji w modelu widoku. System powiązania danych (odcinania w tym przypadku) koordynuje widoku i modelu widoku, aby każdy możliwość jego zadania bez wiedzy jednorodnej innych.

Model hermetyzuje dane Todo. Jednostki w modelu są wykonane przez błyskawicznie odcinania właściwości zauważalne, dlatego może być powiązana bezpośrednio do elementów widget w widoku. Model widoku zapyta kontekstu danych uzyskać i zapisać jednostek modelu. Kontekst danych deleguje większość pracy do błyskawicznie.

Stos po stronie serwera składa się z kodu deweloperów oraz trzy bibliotek .NET zasady: interfejs API sieci Web, Entity Framework i Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Podstawowa architektura jest taka sama jak w szablonie KockoutJS SPA. Jednak implementacja jest znacznie prostsza: Usunięto DTOs i Breeze.NET zostały delegowane większość szczegóły Entity Framework.

## <a name="next-steps"></a>Następne kroki

Zalecamy zapoznanie się kodu, kierując [szeroką gamę dyskusji](http://www.breezejs.com/spa-template?utm_source=ms-spa) zarówno klient, jak i stosy serwera w witrynie sieci Web błyskawicznie.

Możesz spróbować odtwarzanie błyskawicznie zapytania po stronie klienta; Dodaj niektóre filtry i sortowanie. Można dodać więcej właściwości modelu i kolejnych jednostek uzyskać lepsze działanie dla rozwoju SPA end-to-end. Po upewnieniu się, projektu, można usunąć funkcje Todo i je zastąpić własnymi.

Wkrótce będzie można przejść do następnego kroku big: Dodawanie ekranów po stronie klienta i przechodzenia między nimi. Zostanie pozostawione ten szablon SPA oraz Włącz na pełniejsze stos SPA, takich jak [ręczników gorących Jan Papa](https://github.com/johnpapa/HotTowel#readme "ręczników gorących"), dodaje do różnych błyskawicznie i Knockout Durandal.
