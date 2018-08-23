---
uid: single-page-application/overview/templates/breezeknockout-template
title: Szablon BREEZE/Knockout | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon BREEZE/Knockout aplikacja jednostronicowa
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 006d360748674a645ceddb82017f68b0f80f041b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756251"
---
<a name="breezeknockout-template"></a>Szablon BREEZE/Knockout
====================
przez [: Mads Kristensen](https://github.com/madskristensen)

> Szablon Breeze/Knockout MVC został napisany przez lej dzwonka
> 
> [Pobierz szablon MVC Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Została uzyskana informacja "aplikacji jednostronicowej" (SPA) zastanawiasz się, co to jest. Podczas czytania można na jego temat, użytkownik będzie zamiast wystąpić dla siebie. Ale kto ma czas, aby pobrać próbkę? Jeśli masz program Visual Studio, będziesz mieć przykład SPA i uruchomiona w mniej niż 60 sekund przy użyciu wzorca ASP.NET MVC 4 szablon "Breeze/Knockout aplikacji jednostronicowej".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Co to jest szablon SPA Breeze/Knockout?

Większość szablonów projektu Generuj szkielet aplikacji. Umieść ciało w tych kości przez dodanie kodu i po pewnym czasie dostarczać działającą aplikację. Szablon Breeze/Knockout SPA jest inny. Generuje on przykładowej aplikacji umożliwia badanie. Pokazuje projekt aplikacji SPA i wiele technik tworzenia SPA.

Szablon Breeze/Knockout jest odmianą na [szablon KnockoutJS SPA](../introduction/knockoutjs-template.md) zawarte w ASP.NET i Web Tools 2012.2 Update. Szablon Breeze SPA generuje aplikacji przy użyciu tego samego środowiska użytkownika, ale ma inną implementację, za pomocą szybka i bezproblemowa dla zarządzania danymi.

Szablon KnockoutJS SPA sprawia, że żądania obsługi z pierwotnych jQuery AJAX, która jest odpowiednia dla prostej aplikacji. Jednak bardziej zaawansowane aplikacje mają większe wymagania związane z zarządzaniem danych. Na przykład większość aplikacji:

- Zapytania i ponownie wysyłać zapytań do serwera podczas sesji rozszerzonej użytkownika.
- Dodawanie zapytań filtrów, sortowania i stronicowania.
- Udostępnianie tych samych danych na wielu ekranach.
- Accumulate zmiany wielu obiektów, a następnie zapisać je jako jedna transakcja.
- Sprawdzanie poprawności zmiany na kliencie, dzięki czemu użytkownik może Poprawiaj błędy przed zatwierdzeniem zmian w bazie danych.

Biblioteka BreezeJS obsługuje tych zadań można dzięki temu możesz rozwijać aplikację logiki i środowisko użytkownika, najważniejsze.

[**Szybka i bezproblemowa** ](http://www.breezejs.com/?utm_source=ms-spa) to biblioteka typu open source do tworzenia zaawansowanych danych aplikacji w języku JavaScript i HTML, rodzajów aplikacji, które zostały dostarczone w przeszłości jako autonomicznej aplikacji klasycznych.

Szablon Breeze/Knockout pomaga podjąć tym pierwszym krokiem kluczowe znaczenie podczas procesu bardziej niezawodna infrastruktura zarządzania danych. Tworzy przykładowej aplikacji zadań do wykonania, która jest identyczna wypukłymi szablon KnockoutJS SPA. Wewnątrz zamienia Warstwa danych AJAX szybka i bezproblemowa, dzięki czemu można porównać dwa podejścia side-by-side. Oczywiście go dotyka ledwie potencjał aplikacji szybka i bezproblemowa. Ale dowiesz się, jak działa błyskawicznie i w sposób nieco jest wymagana do udostępnienia tego przejścia.

Zaczynajmy.

## <a name="create-a-breezeknockout-template-project"></a>Utwórz projekt szablon Breeze/Knockout

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Szablon jest spakowany jako plik programu Visual Studio rozszerzenia (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę, a następnie kliknij przycisk **OK**.

W **nowy projekt** kreatora wybierz **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Naciśnij kombinację klawiszy Ctrl-F5, aby skompilować i uruchomić aplikację bez debugowania lub naciśnij klawisz F5, aby uruchomić debugowanie.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

Logika sprawdzania poprawności jest wykonywane po stronie klienta, szybka i bezproblemowa. Atrybutów sprawdzania poprawności na serwerze klasy modelu są propagowane do klienta, a następnie wykonywane automatycznie, zanim klient kontaktuje się z serwerem.

Przejrzyj ruchu sieciowego. Należy zauważyć, że żadne wywołania do serwera podczas było Breeze wykryła błąd. Każda zmiana prawidłowy w wyniku żądania POST do "/ api/zadania/SaveChanges". Szybka i bezproblemowa razem zmiany i wysyła je ze sobą jako pojedyncze żądanie do kontrolera internetowego interfejsu API `SaveChanges` metody. To różni się od szablonu KockoutJS SPA, który sprawia, że PUT, POST i usuwania żądań dla każdego elementu indywidualnie.

## <a name="peek-inside"></a>Wgląd do wewnątrz

Ta aplikacja ma po stronie klienta i po stronie serwera. Stos po stronie klienta składa się z niewielką ilością kodu HTML i kombinację modułach JavaScript aplikacji (w folderze "aplikacja") oraz innych bibliotek JavaScript (w folderze "Skryptów").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Jeśli zostały zbadane szablon KnockoutJS SPA, szablon powinien wyglądać znajomo. Skup się na niebieskich polach. Architektura interfejsu użytkownika jest Model-View-ViewModel (MVVM), w którym elementy widget HTML widoku nie pozostawia żadnych śladów są oddzielone od obsługi kodu prezentacji w modelu widoku. System powiązań danych (Knockout w tym przypadku) służy do koordynowania widoku i modelu widoku, aby każdy można wykonać swoje zadania bez obsługi wiedzę na temat innych.

Model hermetyzuje danych zadań do wykonania. Jednostki w modelu są konstruowane przy szybka i bezproblemowa Knockout właściwości zauważalne, więc może być powiązana bezpośrednio do elementów widget w widoku. Model widoku pyta, czy kontekst danych, aby pobrać i zapisać modelowania jednostek. Kontekst danych deleguje większość pracy, aby łatwo.

Stos po stronie serwera składa się z kodu dla deweloperów i trzy bibliotek .NET zasadą: interfejs API sieci Web, platformy Entity Framework i Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Podstawowa architektura jest taka sama jak szablon KockoutJS SPA. Jednak implementacja jest znacznie prostsza: dto zostały usunięte, a większość szczegóły Entity Framework zostały delegowane Breeze.NET.

## <a name="next-steps"></a>Następne kroki

Sugerujemy, zapoznaj się kod, kierując [rozbudowane dyskusji](http://www.breezejs.com/spa-template?utm_source=ms-spa) zarówno klient, jak i stosów serwera w witrynie sieci Web Breeze.

Możesz spróbować poznawać Breeze zapytania po stronie klienta. Dodaj niektóre filtry i sortowanie. Można dodać więcej właściwości modelu i więcej jednostek, aby uzyskać lepsze działanie opracowywania SPA end-to-end. Po upewnieniu się, projektu, możesz usunąć funkcje zadań do wykonania i je zastąpić własnymi.

Wkrótce będzie gotowa do kolejny ważny etap: dodanie ekranów po stronie klienta i nawigowanie między nimi. Zostaną pozostawione ten szablon SPA i włączyć bardziej szczegółowy stos SPA, takich jak [John Papa Hot Towel](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), która dodaje do mieszanki szybka i bezproblemowa i Knockout Durandal.
