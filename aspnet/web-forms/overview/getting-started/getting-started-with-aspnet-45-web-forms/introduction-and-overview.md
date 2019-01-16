---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie do formularzy sieci Web w wersji 4.7 ASP.NET i programu Visual Studio 2017 | Dokumentacja firmy Microsoft
author: Erikre
description: W tej serii samouczków krok po kroku obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.7 i Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341556"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2017
====================

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

W tej serii samouczków pokazano, jak skompilować aplikację ASP.NET Web Forms, ASP.NET 4.5 i programu Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Wprowadzenie

W tej serii samouczków przeprowadzi Cię przez tworzenie aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio 2017 i program ASP.NET 4.5. Utworzymy aplikację o nazwie **Wingtip Toys** — uproszczone storefront witryny sieci web sprzedaż elementów w trybie online. Podczas serii zostały wyróżnione nowych funkcji ASP.NET 4.5.

### <a name="target-audience"></a>Docelowi odbiorcy

Jesteś nowym użytkownikiem formularzy sieci Web platformy ASP.NET dla deweloperów jest przeznaczony dla tej serii samouczków.

Musisz mieć pewną wiedzę na temat w następujących obszarach:

- Programowanie zorientowane obiektowo (Obiektowo) i języków
- Tworzenie aplikacji sieci Web (HTML, CSS, JavaScript)
- Relacyjne bazy danych
- Architektury N-warstwowej

Aby zapoznać się z tych obszarów, należy wziąć pod uwagę bada następującej zawartości:

- [Wprowadzenie do języka Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database)
- [Architektury wielowarstwowej](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funkcje aplikacji

Funkcje formularz sieci Web ASP.NET, przedstawione w tej serii obejmują:

- Projekt aplikacji sieci Web (nie projekt witryny sieci Web)
- Formularze sieci Web
- Strony wzorcowe, konfiguracji
- Bootstrap
- Entity Framework Code najpierw LocalDB
- Żądanie weryfikacji
- Formanty danych silnie typizowane
- Wiązanie modelu
- Adnotacje danych
- Dostawców wartości
- Protokół SSL i uwierzytelniania OAuth
- ASP.NET Identity, konfiguracji i autoryzacji
- Sprawdzania poprawności dyskretnego kodu
- Routing
- Obsługa błędów platformy ASP.NET

### <a name="application-scenarios-and-tasks"></a>Scenariusze aplikacji i zadania

Seria samouczków zadania obejmują:

- Tworzenie, przeglądanie i uruchamianie nowego projektu
- Tworzenie struktury bazy danych
- Inicjowanie i wstępne wypełnianie bazy danych
- Dostosowywanie interfejsu użytkownika przy użyciu stylów, grafikę i strony wzorcowej
- Dodawanie strony i nawigacja
- Wyświetlanie szczegółów menu i danych produktu
- Tworzenie koszyka
- Obsługa dodawania SSL i uwierzytelniania OAuth
- Dodawanie metody płatności
- W tym rolę administratora i użytkownika do aplikacji
- Ograniczanie dostępu do konkretnych stron i folderów
- Próba przekazania pliku do aplikacji sieci web
- Implementowanie walidacji danych wejściowych
- Rejestrowanie tras dla aplikacji sieci web
- Implementowanie obsługi błędów i rejestrowania błędów

## <a name="overview"></a>Omówienie

W tej serii samouczków jest przeznaczony do kogoś zapoznać się z koncepcjami programistycznymi, ale jesteś nowym użytkownikiem formularzy sieci Web ASP.NET. Jeśli już znasz z formularzy sieci Web ASP.NET, ta seria nadal może pomóc więcej informacji na temat nowych funkcji ASP.NET 4.5. Czytelnicy zaznajomiony z programowania pojęcia i formularzy sieci Web ASP.NET, zobacz dodatkowe samouczki formularzy sieci Web w [wprowadzenie](../../../index.md) sekcji w witrynie sieci Web platformy ASP.NET.

W tej serii samouczków w programie ASP.NET 4.5 zawiera następujące funkcje:

- Prosty interfejs użytkownika do tworzenia projektów oferującą [pomocy technicznej dla wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i interfejs API sieci Web).
- [Usługa ładowania początkowego](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układ, motywy i elastyczne środowisko framework.
- [ASP.NET Identity](../../../../identity/index.md), nowego systemu członkostwa programu ASP.NET, która działa tak samo, we wszystkich platform ASP.NET i działa, za pomocą oprogramowania innych niż IIS hostingu w sieci web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Aktualizacja programu Entity Framework, dzięki któremu można:
  - Pobieranie i manipulowanie danymi jako silnie typizowanych obiektów
  - Dostęp do danych w sposób asynchroniczny
  - Obsługa przejściowe błędy połączenia
  - W instrukcjach SQL

Aby uzyskać pełną listę funkcji ASP.NET 4.5, zobacz [ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Przykładowej aplikacji Wingtip Toys

Poniższe zrzuty ekranu są z aplikacji formularzy sieci Web ASP.NET, który zostanie utworzony w tej serii samouczków. Po uruchomieniu aplikacji w programie Visual Studio pojawia się następującą stronę główną.

![Firmy Wingtip Toys — domyślna strona](introduction-and-overview/_static/image1.png)

Zarejestruj się jako nowy użytkownik lub zaloguj się jako istniejącego użytkownika. Górnym menu nawigacyjnym zawiera linki do kategorie produktów i produktów z bazy danych.

Jeśli wybierzesz **produktów**, są wyświetlane wszystkie dostępne produkty. 

![Firmy Wingtip Toys - produktów](introduction-and-overview/_static/image2.png)

Jeśli wybierzesz określonego produktu, są wyświetlane szczegółowe informacje.


![Firmy Wingtip Toys — szczegóły produktu](introduction-and-overview/_static/image3.png)

Użytkownik może rejestrować i zaloguj się przy użyciu funkcji domyślnego szablonu formularzy sieci Web. W tym samouczku wyjaśniono również, jak zarejestrować się przy użyciu istniejącego konta usługi Gmail. Ponadto można zalogować się jako administrator, aby dodawać i usuwać produktów z bazy danych.

![Firmy Wingtip Toys — logowanie](introduction-and-overview/_static/image4.png)

Po zalogowaniu się jako użytkownik, możesz dodać do koszyka i wyewidencjonowania w systemie PayPal produktów. Przykładowa aplikacja jest przeznaczona do pracy w piaskownicy dla deweloperów PayPal. Odbywa się żadna transakcja rzeczywiste pieniądze.

![Firmy Wingtip Toys - koszyk](introduction-and-overview/_static/image5.png)

PayPal potwierdzenie informacji o koncie, kolejność i płatności.

![Firmy Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Po powrocie z systemu PayPal, można przejrzeć i ukończyć zamówienie.

![Firmy Wingtip Toys — Przegląd zamówienia](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że następujące oprogramowanie jest zainstalowane na komputerze:

- [Microsoft Visual Studio 2017 lub Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

.NET Framework jest instalowana automatycznie.

W tej serii samouczków używa programu Microsoft Visual Studio Community 2017. Można użyć albo lub Microsoft Visual Studio 2017 do ukończenia tej serii samouczków.

Należy pamiętać o następujących dotyczących programu Visual Studio:

* Microsoft Visual Studio 2017 i Microsoft Visual Studio Community 2017 są określane jako *programu Visual Studio* w całej tej serii samouczków.

* Program Visual Studio 2017 jest instalowany obok wszystkie starsze wersje zainstalowane. Utworzone we wcześniejszych wersjach witryn mogą być otwierane w programie Visual Studio 2017 i Kontynuuj, aby w poprzednich wersjach.

* Podczas pierwszego uruchomienia programu Visual Studio, to zakłada, że wybrano *programowania dla sieci Web* ustawienia. Aby uzyskać więcej informacji, zobacz [jak: Wybierz ustawienia środowiska programowania dla sieci Web](https://msdn.microsoft.com/library/ff521558.aspx).

Po zainstalowaniu wymagań wstępnych, możesz rozpocząć tworzenie projektu sieci Web, przedstawione w tej serii samouczków.

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

 Dowolnym applicatiion przykładowe ukończone w można pobrać z witryny MSDN przykłady:

[Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Ten plik do pobrania zawiera następujące elementy:

- Przykładowa aplikacja w *WingtipToys* folderu.
- Zasoby używane do tworzenia przykładowej aplikacji w *zasoby WingtipToys* folderu w *WingtipToys* folderu.

Pliki do pobrania jest *zip* pliku. Aby wyświetlić ukończone projektu, który tworzy tę serię samouczków, Znajdź i wybierz *C#* folder w pliku zip. Zapisz C# folderu do folderu, można użyć do pracy z projektów programu Visual Studio. Domyślnie jest folder projektów programu Visual Studio 2017:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong>

Zmień nazwę ***C#*** folder ***WingtipToys***.

> [!NOTE]
> Jeśli masz już folder o nazwie *WingtipToys* w Twoim folderze projektów tymczasowo zmień nazwę tego istniejącego folderu przed zmianą nazwy *C#* folder *WingtipToys*.

Aby uruchomić projekt ukończone, otwórz *WingtipToys* folder i kliknij dwukrotnie plik *WingtipToys.sln* pliku. Program Visual Studio 2017 zostanie otwarty projekt. Następnie kliknij prawym przyciskiem myszy *Default.aspx* w pliku **Eksploratora rozwiązań** i wybierz **Pokaż w przeglądarce**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Aby zapoznać się z zawartością kwizu wzorca ASP.NET Web Forms

Po ukończeniu tej serii samouczka kwizu sprawdzić swoją wiedzę i wzmocnienia kluczowych pojęć. Każde pytanie zawiera wyjaśnienie i linki do dodatkowych wskazówek.

 * [Quiz formularzy sieci Web ASP.NET](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Samouczek pomocy technicznej i komentarze

Pytania i komentarze, za pomocą sekcji Q i A dołączonym [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) strona przykładu.

Komentarze dotyczące tej serii samouczków to powitalnej. Gdy w tej serii samouczków jest aktualizowany, co wysiłki w celu należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszenia.

Jeśli wystąpi błąd, odpowiednie komunikaty o błędach może być mylące, za pomocą żadne dobre wyjaśnienia dotyczące sposobu rozwiązania go. Aby uzyskać pomoc, możesz sprawdzić [fora ASP.NET](https://forums.asp.net/). Innym dobrym źródłem jest sekcji Q i A [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) strona przykładu. 

> [!div class="step-by-step"]
> [Next](create-the-project.md)
