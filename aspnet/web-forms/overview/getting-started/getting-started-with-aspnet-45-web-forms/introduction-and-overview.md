---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: W tej serii samouczków krok po kroku obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio wyrażanie...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: ad5e97cd596e146f742c4c5e882d3938005070d1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752481"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków krok po kroku obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. [Quiz formularzy sieci Web ASP.NET](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Sprawdź swoją wiedzę i wzmocnić kluczowych pojęć, wykonując Quiz formularzy sieci Web platformy ASP.NET. Ten test został opracowany z myślą z zawartości znajdujących się w tej serii samouczków. Pytania quizu zawiera wyjaśnienie wraz z linkami do dodatkowych wskazówek.


## <a name="introduction"></a>Wprowadzenie

W tej serii samouczków przeprowadzi Cię przez kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio Express 2013 for Web i program ASP.NET 4.5.

Nosi nazwę aplikacji, należy utworzyć **Wingtip Toys**. Jest to uproszczony przykład witryny sieci web frontonu magazynu, która sprzedaje elementów w trybie online. W tej serii samouczków wyróżnia nowych funkcji dostępnych w programie ASP.NET 4.5.

Komentarze są powitalnej, a my sprawiamy, żeby wszelkich starań, aby zaktualizować tę serię samouczków, w oparciu o Twoje sugestie.

### <a name="download-completed-project"></a>Zakończono pobieranie projektu

Możesz pobrać projekt C#, który zawiera samouczek ukończone.

- [Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Przejrzyj zawartość, wykonując powiązane quiz wzorca ASP.NET Web Forms

Po ukończeniu tego samouczka sprawdzić swoją wiedzę i wzmocnić kluczowych pojęć, wykonując [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Ten test został opracowany z myślą z zawartości znajdujących się w tej serii samouczków. Pytania quizu zawiera wyjaśnienie wraz z linkami do dodatkowych wskazówek.

- [Quiz formularzy sieci Web ASP.NET](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Odbiorcy

Odbiorców tej serii samouczków jest doświadczonych deweloperów, którzy dopiero formularzy sieci Web ASP.NET. Zainteresowane w tej serii samouczków Deweloper powinien mieć następujące umiejętności:

- Powszechnie znane z obiektem obiektowo języka programowania (Obiektowo)
- Znasz koncepcji programowania w sieci Web (HTML, CSS, JavaScript)
- Znasz koncepcji relacyjnych baz danych
- Znasz koncepcji architektury n warstwowej

Jeśli jesteś zainteresowani zapoznaniem obszarów wymienionych powyżej, należy wziąć pod uwagę przejrzeniu następującej zawartości:

- [Wprowadzenie do języka Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Opracowywanie sieci Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP i JQuery](http://w3schools.com/)
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database)
- [Architektury wielowarstwowej](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funkcje aplikacji

Funkcje formularz sieci Web ASP.NET, przedstawione w tej serii obejmują:

- Projekt aplikacji sieci Web (nie projekt witryny sieci Web)
- Formularze sieci Web
- Strony wzorcowe, konfiguracji
- Usługa ładowania początkowego
- Entity Framework Code najpierw LocalDB
- Żądanie weryfikacji
- Silnie Typizowane kontrolki danych powiązanie adnotacje danych modelu i dostawców wartości
- Protokół SSL i uwierzytelniania OAuth
- ASP.NET Identity, konfiguracji i autoryzacji
- Sprawdzania poprawności dyskretnego kodu
- Routing
- Obsługa błędów platformy ASP.NET

### <a name="application-scenarios-and-tasks"></a>Scenariusze aplikacji i zadania

Zadania przedstawione w tej serii obejmują:

- Tworzenie, przeglądanie i uruchamianie nowego projektu
- Tworzenie struktury bazy danych
- Inicjowanie i wstępne wypełnianie bazy danych
- Dostosowywanie interfejsu użytkownika przy użyciu stylów, grafiki i strony wzorcowej
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

Jeśli dopiero zaczynasz korzystać z ASP.NET Web Forms ale znajomość pojęcia związane z programowaniem, masz prawo samouczka. Jeśli masz już zapoznać się z wzorca ASP.NET Web Forms, mogą korzystać z tej serii samouczków, nowe funkcje dostępne w programie ASP.NET 4.5. Jeśli jesteś zaznajomiony z programowania pojęcia i formularzy sieci Web ASP.NET, zobacz dodatkowe samouczki w formularzach sieci Web [wprowadzenie](../../../index.md) sekcji w witrynie sieci Web platformy ASP.NET.

Konkretne **najnowsze** ASP.NET 4.5 funkcje wprowadzone w formularzach sieci Web, ta seria samouczków obejmują następujące elementy:

- Prosty interfejs użytkownika do tworzenia projektów tę ofertę [Obsługa wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i interfejs API sieci Web).
- [Usługa ładowania początkowego](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układ i motywów strukturę, która oferuje elastyczny możliwości projektowania i motywów.
- [ASP.NET Identity](../../../../identity/index.md), nowego systemu członkostwa programu ASP.NET, która działa tak samo, we wszystkich platform ASP.NET i działa, za pomocą oprogramowania innych niż IIS hostingu w sieci web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizację programu Entity Framework, która umożliwia pobieranie i manipulowanie danymi jako silnie typizowanych obiektów, dostęp do danych asynchronicznie, obsługi przejściowe błędy połączenia i zaloguj się instrukcji języka SQL.

Aby uzyskać pełną listę funkcji ASP.NET 4.5, zobacz [ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Firmy Wingtip Toys przykładowej aplikacji

Poniższe zrzuty ekranu zapewniają szybki przegląd aplikacji formularzy sieci Web ASP.NET, która zostanie utworzona w tej serii samouczków. Po uruchomieniu aplikacji z programu Visual Studio Express 2013 for Web zobaczysz poniższą stronę główną sieci web.

![Firmy Wingtip Toys — domyślna strona](introduction-and-overview/_static/image1.png)

Zarejestruj się jako nowy użytkownik lub zaloguj się jako istniejącego użytkownika. Nawigacji podano u góry dla każdej kategorii produktów, pobierając dostępne produkty z bazy danych.

Wybierając łącze produktów, można wyświetlić listę wszystkich dostępnych produktów.

![Firmy Wingtip Toys - produktów](introduction-and-overview/_static/image2.png)

Można również wyświetlić szczegółowe informacje indywidualnych produktów, wybierając jedną z wymienionych produktów.

![Firmy Wingtip Toys — szczegóły produktu](introduction-and-overview/_static/image3.png)

Użytkownik może rejestrować i zaloguj się przy użyciu funkcji domyślny szablon formularzy sieci Web. W tym samouczku wyjaśniono również, jak zalogować się przy użyciu istniejącego konta Gmail. Ponadto możesz zalogować się jako administrator, aby dodawać i usuwać produktów z bazy danych.

![Firmy Wingtip Toys — logowanie](introduction-and-overview/_static/image4.png)

Po zalogowaniu się jako użytkownik, możesz dodać produkty do koszyka i finalizacja zakupu z systemu PayPal. Należy pamiętać, że ta Przykładowa aplikacja jest przeznaczona do funkcją piaskownicy dla deweloperów PayPal. Żadna transakcja rzeczywiste pieniądze zostaną zrealizowane.

![Firmy Wingtip Toys - koszyk](introduction-and-overview/_static/image5.png)

PayPal potwierdzi, Twoje konto, kolejność i informacje o płatności.

![Firmy Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Po powrocie z systemu PayPal, można przejrzeć i ukończyć zamówienie.

![Firmy Wingtip Toys — Przegląd zamówienia](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz zainstalowane na komputerze następujące oprogramowanie:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie.

W tej serii samouczków używa programu Microsoft Visual Studio Express 2013 for Web. Aby ukończyć tę serię samouczków, można użyć programu Microsoft Visual Studio Express 2013 for Web albo programu Microsoft Visual Studio 2013.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 i Microsoft Visual Studio Express 2013 for Web będzie często określane jako programu Visual Studio w całej tej serii samouczków.


Jeśli masz już zainstalowaną wersję programu Visual Studio, proces instalacji instaluje Visual Studio 2013 lub programu Microsoft Visual Studio Express 2013 for Web obok istniejącą wersję. Lokacje, które zostały utworzone w starszych wersjach mogą być otwierane w programie Visual Studio 2013 i Kontynuuj, aby w poprzednich wersjach.

> [!NOTE] 
> 
> W tym przewodniku przyjęto założenie, że wybrana *programowania dla sieci Web* zbiór ustawień podczas pierwszego uruchomienia programu Visual Studio. Aby uzyskać więcej informacji, zobacz [porady: Wybieranie ustawienia środowiska programowania sieci Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

Po zainstalowaniu wymagań wstępnych, można przystąpić do rozpoczęcia tworzenia nowego projektu sieci Web, które są prezentowane w tej serii samouczków. Jeśli chcesz **opcjonalnie** uruchamianie przykładowej aplikacji, który tworzy tę serię samouczków, możesz ją pobrać z witryny MSDN przykładów. Ten plik do pobrania zawiera następujące informacje:

- Przykładowa aplikacja w *WingtipToys* folderu.
- Zasoby używane do tworzenia przykładowej aplikacji w *zasoby WingtipToys* folderu w *WingtipToys* folderu.

#### <a name="download-the-file-from-msdn-samples-site"></a>Pobierz plik z witryny MSDN próbek:

[Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

Pliki do pobrania jest <em>zip</em> pliku. Aby wyświetlić ukończone projektu, który tworzy tę serię samouczków, Znajdź i wybierz <em>C#</em>folderu w <em>zip</em> pliku. Zapisz <em>C#</em> folderto folderu do pracy z projektami Visual Studio 2013 można użyć. Domyślnie folder projektów programu Visual Studio 2013 jest następująca:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Zmień nazwę ***C#*** folder ***WingtipToys***.

> [!NOTE]
> Jeśli masz już folder o nazwie *WingtipToys* w Twoim folderze projektów tymczasowo zmień nazwę tego istniejącego folderu przed zmianą nazwy *C#* folder *WingtipToys*.


Aby uruchomić projekt ukończone, otwórz *WingtipToys* folder i kliknij dwukrotnie plik *WingtipToys.sln* pliku. Visual Studio 2013, zostanie otwarty projekt. Następnie kliknij prawym przyciskiem myszy *Default.aspx* pliku w oknie Eksploratora rozwiązań, a następnie kliknij pozycję Pokaż w przeglądarce z menu podręcznego.

### <a name="tutorial-support-and-comments"></a>Samouczek pomocy technicznej i komentarze

Sekcja Q i A dołączone [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) przykładowy (C#) w przypadku jakichkolwiek pytań lub komentarzy.

Komentarze dotyczące tej serii samouczków są powitalnej, a po zaktualizowaniu tej serii samouczków wszelkich starań, będzie nawiązywane w przypadku uwzględnienia poprawki konta lub sugestie dotyczące ulepszeń, które znajdują się w samouczku komentarzy.

Po wystąpieniu błędu podczas tworzenia lub witryny sieci Web nie działa poprawnie, komunikaty o błędach mogą spowodować wskazówek złożone źródło problemu, lub nie może wyjaśnić, jak go naprawić. Do udzielenia odpowiedzi na kilka typowych scenariuszy, problem, można również użyć [fora ASP.NET](https://forums.asp.net/) lub sekcji Q i A dołączone [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) próbki. Jeśli otrzymasz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczków, pamiętaj, że Sprawdzanie lokalizacji powyżej.

> [!div class="step-by-step"]
> [Next](create-the-project.md)
