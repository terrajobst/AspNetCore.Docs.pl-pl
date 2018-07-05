---
uid: single-page-application/overview/templates/breezeangular-template
title: Szablon BREEZE/Angular | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon BREEZE/Angular aplikacja jednostronicowa
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 541d1a71b58a0d55651d823dc7425b7d1ce73cdb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378063"
---
<a name="breezeangular-template"></a>Szablon BREEZE/Angular
====================
przez [: Mads Kristensen](https://github.com/madskristensen)

> Szablon Breeze/Angular MVC został napisany przez lej dzwonka
> 
> [Pobierz szablon Breeze/Angular MVC](https://go.microsoft.com/fwlink/?LinkId=286437)


[Moduł AngularJS](http://angularjs.org) jest biblioteki typu open source, od firmy Google umożliwiające tworzenie aplikacji (źródła). Oferuje ona powiązania danych, wstrzykiwanie zależności i zarządzanie ekranu. Połącz ją z [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), innej biblioteki typu open source, modelowanie danych i zarządzania danymi i możesz mieć składniki podstawowe atrakcyjną aplikację klienta HTML/JavaScript.

Szablon Breeze/Angular SPA jest odmianą na [szablon KnockoutJS SPA](../introduction/knockoutjs-template.md) zawarte w ASP.NET i Web Tools 2012.2 Update. Jeśli masz program Visual Studio, będziesz mieć przykład SPA pracę w mniej niż 60 sekund.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Wypukłymi aplikacja wygląda bardzo podobnie do szablon KnockoutJS SPA. Ale zupełnie różne pod maską. Szablon KnockoutJS używa Knockout dla wiązania danych i raw AJAX uzyskać dostęp do danych. Szablon Breeze/Angular używa Angular dla powiązania danych i łatwo uzyskać dostęp do danych. Te bibliotekami włączyć dodatkowe funkcje, w tym nawigowania po stronach i historię.

Poniżej przedstawiono informacje o stronie aplikacji:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Ta strona wyświetla uruchomionej dziennik zdarzeń w bieżącej sesji użytkownika, w tym:

- Stronicowanie. Należy pamiętać, tworzenia kontrolera zadań do wykonania w #2 i #7.
- Zdalne zapytania (#3) i lokalnej pamięci podręcznej zapytań (#7).
- Zapisywanie nowych (5, #6) i zmodyfikować jednostki (nr 4).
- Zmiany zweryfikowane na komputerze klienckim (#9), dzięki czemu użytkownik może Poprawiaj błędy przed zatwierdzeniem zmian w bazie danych.

To jednak, aby zapoznać się w tym szablonie, w tym:

- Dynamiczne ładowanie szablonów widoków kodu HTML.
- Powiązanie danych niestandardowych przy użyciu usługi Angular "dyrektywy".
- Iniekcja modułowości i zależności.
- Zapytania filtrów, sortowania, stronicowania, projekcje i włączenia powiązanych jednostek.
- Udostępnianie danych między wiele ekranów.
- Zapisywanie wielu zmian jako jedna transakcja.
- Reguły sprawdzania poprawności automatycznie propagowane z serwera do klienta JavaScript.

Zaczynajmy.

## <a name="create-a-breezeangular-template-project"></a>Utwórz projekt szablon Breeze/Angular

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Szablon jest spakowany jako plik programu Visual Studio rozszerzenia (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę, a następnie kliknij przycisk **OK**.

W **nowy projekt** kreatora wybierz **Breeze Angular SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Naciśnij kombinację klawiszy Ctrl-F5, aby skompilować i uruchomić aplikację bez debugowania lub naciśnij klawisz F5, aby uruchomić debugowanie.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Po pierwszym uruchomieniu aplikacji wyświetli ekran logowania. Kliknij link "Zaloguj się", a nowa strona glides w widoku, gdzie można wprowadzić nazwę użytkownika i hasło. (Stron logowania i rejestracji są tworzone przy użyciu platformy ASP.NET MVC.) Gdy prześlesz formularz rejestracji, serwer generuje TodoList z dwoma elementami dla swojego konta. Następnie stanowi je dla Ciebie na żółty Uwaga.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Teraz masz ziemi SPA. Wszystko, co można zobaczyć, a środowisko podczas manipulowania zadań do wykonania jest renderowana i zarządzane na komputerze klienckim, za pomocą Knockout i szybka i bezproblemowa. Eksplorowanie aplikacji jako użytkownika... ale oka dla deweloperów. Narzędzia dla deweloperów w przeglądarce służy do przechwytywania ruchu sieciowego. (W programie Internet Explorer: naciśnij klawisz F12, wybierz **sieci** kartę, a następnie kliknij przycisk **Rozpocznij przechwytywanie**.) Spróbuj wykonać następujące czynności:

- Dodaj nowy element Todo.
- Kliknij etykietę, a następnie Edytuj tytuł elementu Todo
- Zaznacz pole wyboru, aby oznaczyć element gotowe. Zwróć uwagę, że pole tekstowe jest wyłączona, więc tytuł nie jest edytowalny.
- Kliknij przycisk "x" z prawej strony etykiety. Element znika i zostanie usunięty z bazy danych.
- Wybierz inny element, a następnie wyczyść jego tytuł. Otrzymasz błąd sprawdzania poprawności, że tytuł jest wymagany. Po wstrzymaniu krótki poprzednie tytuł został przywrócony.
- Wpisz tytuł bardzo długi. Otrzymasz błąd sprawdzania poprawności innego że tytuł jest zbyt długa.
- Kliknij przycisk "Dodaj listę zadań do wykonania". Nowa lista pojawia się po lewej stronie poprzedniej listy.
- Poeksperymentuj z tytułu listy zadań, wyzwalając wymaga i poprawności długości.
- Kliknij w polu tekstowym tytułu, aby wyczyścić komunikat o błędzie.
- Kliknij przycisk "x" w okręgu w prawym górnym rogu, aby usunąć TodoList i jego zadań do wykonania.
- Kliknij link "About" w prawym górnym rogu, aby wyświetlić dziennik te działania.

Logika sprawdzania poprawności jest wykonywane po stronie klienta, szybka i bezproblemowa. Atrybutów sprawdzania poprawności na serwerze klasy modelu są propagowane do klienta, a następnie wykonywane automatycznie, zanim klient kontaktuje się z serwerem.

Przejrzyj ruchu sieciowego. Należy zauważyć, że żadne wywołania do serwera podczas było Breeze wykryła błąd. Każda zmiana prawidłowy w wyniku żądania POST do "/ api/zadania/SaveChanges". Szybka i bezproblemowa razem zmiany i wysyła je ze sobą jako pojedyncze żądanie do kontrolera internetowego interfejsu API `SaveChanges` metody. To różni się od szablonu KockoutJS SPA, który sprawia, że PUT, POST i usuwania żądań dla każdego elementu indywidualnie.

Zauważ również, że istnieje żaden ruch sieciowy podczas przełączania między TodoList i dotyczących stron. Wynika to z zapytania została ograniczona do lokalnej pamięci podręcznej szybka i bezproblemowa.

## <a name="peek-inside"></a>Wgląd do wewnątrz

Ta aplikacja ma po stronie klienta i po stronie serwera. Stos po stronie klienta składa się z niewielką ilością kodu HTML i kombinację modułach JavaScript aplikacji (w folderze "aplikacja") oraz innych bibliotek JavaScript (w folderze "Skryptów").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Architektura interfejsu użytkownika oddziela widżetów HTML z widoków z pomocniczych kodu prezentacji w kontrolerów. Platformy Angular system powiązanie danych służy do koordynowania widoków i kontrolerów, aby każdy można wykonać swoje zadania bez obsługi wiedzę na temat innych.

Kontroler pyta, czy kontekst danych, aby pobrać i zapisać modelowania jednostek. Kontekst danych deleguje większość pracy, aby łatwo, która tworzy własnym śledzenie obiekty modelu z wyników zapytania w formacie JSON.

Stos po stronie serwera składa się z kodu dla deweloperów i trzy bibliotek .NET zasadą: interfejs API sieci Web, platformy Entity Framework i Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Podstawowa architektura jest taka sama jak szablon KockoutJS SPA. Jednak implementacja jest znacznie prostsza: dto zostały usunięte, a większość szczegóły Entity Framework zostały delegowane Breeze.NET.

## <a name="next-steps"></a>Następne kroki

Sugerujemy, zapoznaj się kod, kierując [rozbudowane dyskusji](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) zarówno klient, jak i stosów serwera w witrynie sieci Web Breeze.

Możesz spróbować poznawać Breeze zapytania po stronie klienta. Dodaj niektóre filtry i sortowanie. Można dodać więcej właściwości modelu i więcej jednostek, aby uzyskać lepsze działanie opracowywania SPA end-to-end. Po upewnieniu się, projektu, możesz usunąć funkcje zadań do wykonania i je zastąpić własnymi.

Udanego kodowania!
