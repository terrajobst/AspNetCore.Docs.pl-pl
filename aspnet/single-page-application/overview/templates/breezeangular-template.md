---
uid: single-page-application/overview/templates/breezeangular-template
title: Błyskawicznie/kątową szablonu | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon błyskawicznie/kątową jednej strony aplikacji
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566312"
---
<a name="breezeangular-template"></a>Błyskawicznie/kątową szablonu
====================
przez [Mads Kristensen](https://github.com/madskristensen)

> Błyskawicznie/kątową szablonu MVC zapisał dzwonka wewnątrz
> 
> [Pobieranie błyskawicznie/kątowego szablonu MVC](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) jest biblioteki typu open source z Google tworzenia jednej strony aplikacji (źródła). Oferuje wiązania z danymi, iniekcji zależności i zarządzania ekranu. Połączyć ją z [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), innej biblioteki typu open source do modelowania danych i zarządzanie danymi, a ma składniki podstawowe wspaniałych aplikacji klienta HTML/JavaScript.

Szablon SPA błyskawicznie/kątową jest odmianą na [szablonu SPA elementami KnockoutJS](../introduction/knockoutjs-template.md) dołączony do platformy ASP.NET i zaktualizować 2012.2 narzędzia sieci Web. Jeśli masz program Visual Studio, będziesz mieć przykład SPA działa w mniej niż 60 sekund.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Wypukłymi aplikacja wygląda bardzo podobnie do szablonu SPA elementami KnockoutJS. Jednak jest zupełnie różne pod maską. Szablon elementami KnockoutJS używa odcinania dla powiązania danych i raw AJAX dla dostępu do danych. Szablon błyskawicznie/kątową używa kątową powiązania danych i błyskawicznie do dostępu do danych. Te bibliotekami włączyć dodatkowe możliwości, takie jak strona nawigacji i Historia.

Poniżej przedstawiono informacje o stronie aplikacji:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Ta strona wyświetla uruchomione dziennika zdarzeń w bieżącej sesji użytkownika, w tym:

- Stronicowania. Należy pamiętać, tworzenia kontrolera Todo #2 i #7.
- Zapytań zdalnych (#3) i lokalnej pamięci podręcznej zapytania (#7).
- Zapisywanie nowych (nr 5, #6) i modyfikować jednostek (#4).
- Zmiany zweryfikowane na kliencie (#9), więc użytkownik może poprawić błędy przed zatwierdzeniem zmian w bazie danych.

Istnieje więcej, aby eksplorować w tym szablonie, w tym:

- Dynamiczne ładowanie szablonów widoków kodu HTML.
- Powiązanie danych niestandardowych za pomocą kątową "dyrektywy".
- Iniekcji modułowości i zależności.
- Zapytanie filtry, sortowania, stronicowania, rzutowania i włączenia powiązanych jednostek.
- Udostępnianie danych między kilka ekranów.
- Zapisywanie zmian wielu jako jedna transakcja.
- Reguły sprawdzania poprawności automatycznie propagowane z serwera do klienta kodu JavaScript.

Zacznijmy od początku.

## <a name="create-a-breezeangular-template-project"></a>Tworzenie błyskawicznie/kątowego projektu szablonu

Pobierz i zainstaluj szablonu, klikając przycisk Pobierz powyżej. Szablon jest dostarczana jako plik rozszerzenia serwera Visual Studio (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt i kliknij przycisk **OK**.

W **nowy projekt** kreatora wybierz **SPA kątowego błyskawicznie**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Naciśnij klawisze Ctrl-F5, aby skompilować i uruchomić aplikację bez debugowania, lub naciśnij klawisz F5, aby uruchomić z debugowaniem.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Kliknij łącze "O" w prawym górnym rogu, aby wyświetlić dziennik tych działań.

Logika sprawdzania poprawności jest wykonywane po stronie klienta przez błyskawicznie. Atrybuty weryfikacji na serwerze klasy modelu są propagowane do klienta i wykonywane automatycznie, zanim klient kontaktuje się z serwerem.

Przejrzyj ruchu sieciowego. Zwróć uwagę, że nie było żadnych wywołania do serwera podczas błyskawicznie wykrył błąd. Każda zmiana prawidłowy w wyniku żądania POST do "/ api/zadania/SaveChanges". Błyskawicznie zawiera również zmiany i wysyła je razem jako pojedyncze żądanie do kontrolera interfejsu API sieci Web `SaveChanges` metody. Różni się od szablonu KockoutJS SPA, dzięki czemu PUT, POST i usuwania żądań dla każdego elementu indywidualnie.

Zauważ również, że istnieje ruchu sieciowego podczas przełączania między TodoList i stron. Wynika to z zapytania została ograniczona do lokalnej pamięci podręcznej błyskawicznie.

## <a name="peek-inside"></a>Wgląd w

Ta aplikacja ma po stronie klienta i po stronie serwera. Stos po stronie klienta składa się z niewielką HTML i kombinację modułów JavaScript aplikacji (w folderze "aplikacja") oraz innych firm bibliotek JavaScript (w folderze "Skryptów").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Architektura interfejsu użytkownika oddziela elementy widget HTML z widoków z obsługi kodu prezentacji w kontrolerów. Dyrektywy Angular systemu wiązania danych koordynuje widoki i kontrolery tak, aby każdy możliwość jego zadania bez wiedzy jednorodnej innych.

Kontroler zapyta kontekstu danych uzyskać i zapisać jednostek modelu. Kontekst danych deleguje większość pracy do błyskawicznie, która tworzy śledzeniem własnym modelu obiektów z wyników zapytania w formacie JSON.

Stos po stronie serwera składa się z kodu deweloperów oraz trzy bibliotek .NET zasady: interfejs API sieci Web, Entity Framework i Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Podstawowa architektura jest taka sama jak w szablonie KockoutJS SPA. Jednak implementacja jest znacznie prostsza: Usunięto DTOs i Breeze.NET zostały delegowane większość szczegóły Entity Framework.

## <a name="next-steps"></a>Następne kroki

Zalecamy zapoznanie się kodu, kierując [szeroką gamę dyskusji](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) zarówno klient, jak i stosy serwera w witrynie sieci Web błyskawicznie.

Możesz spróbować odtwarzanie błyskawicznie zapytania po stronie klienta; Dodaj niektóre filtry i sortowanie. Można dodać więcej właściwości modelu i kolejnych jednostek uzyskać lepsze działanie dla rozwoju SPA end-to-end. Po upewnieniu się, projektu, można usunąć funkcje Todo i je zastąpić własnymi.

Kodowanie przyjemność!
