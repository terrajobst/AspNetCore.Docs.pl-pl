---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: Ten krok samouczka serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 572b263a5f968b473457771a1dd4075910218c01
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten krok samouczka serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. [Kwizu formularzy sieci Web ASP.NET](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Sprawdź swoją wiedzę i wzmocnienie kluczowe założenia wykonując quizu formularzy sieci Web ASP.NET. Kwizu zostało zaprojektowane specjalnie z zawartości zawarte w tym samouczku. Każde pytanie w kwizu zawiera wyjaśnienie oraz linki do dodatkowych wskazówek.


## <a name="introduction"></a>Wprowadzenie

Tej serii samouczków prowadzi użytkownika przez kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Visual Studio Express 2013 dla sieci Web i ASP.NET 4.5.

Nosi nazwę aplikacji, należy utworzyć **Wingtip Toys**. Jest uproszczony przykład magazynu frontonu witryny sieci web, która sprzedaje elementów w trybie online. Ten samouczek serii prezentuje nowych funkcji dostępnych w programie ASP.NET 4.5.

Komentarze są powitalnej i wybierzemy wszelkich starań, aby zaktualizować ten samouczek serii oparta na sugestii.

### <a name="download-completed-project"></a>Pobieranie ukończone projektu

Możesz pobrać projekt C#, który zawiera samouczek ukończone.

- [Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Przejrzyj zawartość, wykonując pokrewne kwizu formularzy sieci Web ASP.NET

Po ukończeniu tego samouczka, sprawdź swoją wiedzę i wzmocnienia podstawowych pojęć, wykonując [ASP.NET Web Forms quizu](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Kwizu zostało zaprojektowane specjalnie z zawartości zawarte w tym samouczku. Każde pytanie w kwizu zawiera wyjaśnienie oraz linki do dodatkowych wskazówek.

- [Kwizu formularzy sieci Web ASP.NET](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Odbiorcy

Docelowa grupa odbiorców tego samouczka serii jest doświadczonych projektantów, którzy dopiero zaczynasz korzystać z formularzy sieci Web ASP.NET. Projektant zainteresowane w tym samouczku powinny mieć następujące umiejętności:

- Dobrze znany z obiektem zorientowane na język programowania (Obiektowo)
- Znasz koncepcji Projektowanie sieci Web (HTML, CSS, JavaScript)
- Znasz koncepcji relacyjnej bazy danych
- Znasz koncepcji architektura n warstwowa

Jeśli jesteś zrecenzować obszarów wymienionych powyżej, należy wziąć pod uwagę zapoznaniu się z następującą zawartością:

- [Wprowadzenie do języka Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Wdrażanie sieci Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database)
- [Architektura wielowarstwowa](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funkcje aplikacji

Funkcje formularza sieci Web ASP.NET przedstawionych w tej serii obejmują:

- Projekt aplikacji sieci Web (nie projekt witryny sieci Web)
- Formularze sieci Web
- Strony wzorcowe, konfiguracji
- Bootstrap
- Entity Framework Code najpierw LocalDB
- Sprawdzanie poprawności żądań
- Silnie Typizowane formantów danych wiązań adnotacji danych modelu i dostawców wartości
- Protokół SSL i uwierzytelniania OAuth
- Tożsamość platformy ASP.NET, konfiguracji i autoryzacji
- Sprawdzania poprawności dyskretnego kodu
- Routing
- Obsługa błędów ASP.NET

### <a name="application-scenarios-and-tasks"></a>Aplikacja scenariuszy i zadań

Zadania zostało to pokazane w tej serii obejmują:

- Tworzenie, przeglądanie i uruchamianie nowego projektu
- Tworzenie struktury bazy danych
- Inicjowanie i wstępne wypełnianie bazy danych
- Dostosowywanie interfejsu użytkownika przy użyciu stylów, grafiki i strony wzorcowej
- Dodawanie stron i nawigacji
- Wyświetlanie szczegółów menu i dane produktu
- Tworzenie koszyk
- Obsługa dodawania SSL i uwierzytelniania OAuth
- Dodawanie metody płatności
- W tym rolę administratora i użytkownika do aplikacji
- Ograniczanie dostępu do określonych stron i folderów
- Przekazywanie pliku do aplikacji sieci web
- Implementowanie sprawdzania poprawności danych wejściowych
- Rejestrowanie tras dla aplikacji sieci web
- Implementowanie obsługi błędów i rejestrowania błędów

## <a name="overview"></a>Omówienie

Jeśli jesteś nowym użytkownikiem programu ASP.NET Web Forms ale ma znajomość koncepcje programowania, masz prawo samouczka. Jeśli już znasz formularzy sieci Web ASP.NET, mogą korzystać z tego samouczka serii przez nowych funkcji dostępnych w programie ASP.NET 4.5. Jeśli znasz programowania pojęcia i formularzy sieci Web ASP.NET, zobacz dodatkowe samouczki, podany w formularzach sieci Web [wprowadzenie](../../../index.md) sekcji w witrynie sieci Web programu ASP.NET.

Konkretnym **najnowsze** ASP.NET 4.5 funkcje wprowadzone w formularzach sieci Web, ten samouczek serii są następujące:

- Prosty interfejs użytkownika służący do tworzenia projektów tej oferty [obsługę wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i interfejsu API sieci Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układ i motywów platforma, która zapewnia dynamiczne możliwości projektowania i tworzenia motywów.
- [ASP.NET Identity](../../../../identity/index.md), nowego systemu członkostwa programu ASP.NET, który działa tak samo we wszystkich platform ASP.NET i współpracuje z oprogramowania innego niż IIS hostingu w sieci web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizacja do narzędzia Entity Framework, dzięki czemu możesz pobrać i manipulowanie danymi jako silnie typizowanych obiektów, uzyskiwanie dostępu do danych asynchronicznie, obsługi błędów przejściowych połączenia i dziennika instrukcji SQL.

Aby uzyskać pełną listę funkcji ASP.NET 4.5, zobacz [ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 informacje o wersji](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys przykładowej aplikacji

Poniższe zrzuty ekranu zapewniają szybki przegląd aplikacji formularzy sieci Web ASP.NET, która zostanie utworzona w tym samouczku. Po uruchomieniu aplikacji z programu Visual Studio Express 2013 for Web zobaczą następującą stronę główną.

![Wingtip Toys — domyślna strona](introduction-and-overview/_static/image1.png)

Zarejestruj się jako nowy użytkownik lub zaloguj się jako istniejącego użytkownika. Nawigacji podano u góry dla każdej kategorii produktów, pobierając dostępnych produktów z bazy danych.

Po wybraniu łącza produktów, można wyświetlić listę wszystkich dostępnych produktów.

![Wingtip Toys - produktów](introduction-and-overview/_static/image2.png)

Można również wyświetlić szczegółowe informacje indywidualnych produktów, wybierając jedną z wymienionych produktów.

![Wingtip Toys — szczegółowe informacje o produkcie](introduction-and-overview/_static/image3.png)

Użytkownik może rejestrować i zaloguj się za pomocą funkcji domyślnego szablonu formularzy sieci Web. W tym samouczku wyjaśniono również sposób zalogować się przy użyciu istniejącego konta usługi Gmail. Ponadto możesz zalogować się jako administrator, aby dodawać i usuwać produkty z bazy danych.

![Zaloguj się do niego Wingtip Toys-](introduction-and-overview/_static/image4.png)

Gdy użytkownik zalogował się jako użytkownik, można dodać produktów koszyk i wyewidencjonowania z PayPal. Zauważ, że ta Przykładowa aplikacja jest działa w programie PayPal developer piaskownicy. Żadna transakcja rzeczywiste pieniędzy będą miały miejsce.

![Wingtip Toys - koszyka](introduction-and-overview/_static/image5.png)

PayPal potwierdzi Twojego konta, kolejność i informacje o płatności.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Po powrocie z PayPal, możesz sprawdzić i ukończyć zamówienia.

![Wingtip Toys — Przegląd kolejności](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz następujące oprogramowanie zainstalowane na komputerze:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie.

Ten samouczek serii używa programu Microsoft Visual Studio Express 2013 for Web. Do ukończenia tego samouczka serii można użyć programu Microsoft Visual Studio Express 2013 for Web albo Microsoft Visual Studio 2013.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 i programu Microsoft Visual Studio Express 2013 for Web będzie często określane jako Visual Studio w tej serii samouczka.


Jeśli masz już zainstalowany wersji programu Visual Studio, proces instalacji zainstaluje Visual Studio 2013 lub Microsoft Visual Studio Express 2013 for Web obok istniejącą wersję. Lokacje, które zostały utworzone we wcześniejszych wersjach mogą być otwierane w programie Visual Studio 2013 i nadal otworzyć we wcześniejszych wersjach.

> [!NOTE] 
> 
> W tym przewodniku przyjęto założenie, że wybrano *projektowanie witryn sieci Web* kolekcję ustawień przy pierwszym uruchomieniu programu Visual Studio. Aby uzyskać więcej informacji, zobacz [porady: Wybierz ustawienia środowiska sieci Web Development](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Pobierz aplikację przykładową

Po zainstalowaniu wymagań wstępnych, można przystąpić do rozpoczęcia tworzenia nowego projektu sieci Web, które są prezentowane w tym samouczku. Jeśli chcesz **opcjonalnie** Uruchom przykładową aplikację tworzącą tego samouczka serii, można go pobrać z witryny MSDN próbek. Ten plik do pobrania zawiera następujące elementy:

- Przykładową aplikację w *WingtipToys* folderu.
- Zasoby używane do tworzenia przykładowej aplikacji w *zasoby WingtipToys* folderu w *WingtipToys* folderu.

#### <a name="download-the-file-from-msdn-samples-site"></a>Pobierz plik z witryny MSDN próbek:

[Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

Pobieranie jest <em>.zip</em> pliku. Aby wyświetlić ukończone projektu, który tworzy tę serię samouczek, Znajdź i wybierz <em>C#</em>folderu w <em>.zip</em> pliku. Zapisz <em>C#</em> folderto folderu używać do pracy z projektów Visual Studio 2013. Domyślnie folderu projektów programu Visual Studio 2013 jest następujący:

<strong>C:\Users\</ strong ><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual 2013\Projects w Studio</strong>

Zmień nazwę ***C#*** folder ***WingtipToys***.

> [!NOTE]
> Jeśli masz już folder o nazwie *WingtipToys* w folderze projektów tymczasowo zmień nazwę tego folderu istniejących przed zmianą nazwy *C#* folder *WingtipToys*.


Aby uruchomić projekt ukończone, otwórz *WingtipToys* folder i kliknij dwukrotnie *WingtipToys.sln* pliku. Visual Studio 2013 spowoduje otwarcie projektu. Następnie kliknij prawym przyciskiem myszy *Default.aspx* pliku w oknie Eksploratora rozwiązań i kliknij polecenie Wyświetl w przeglądarce w menu kontekstowym.

### <a name="tutorial-support-and-comments"></a>Samouczek pomocy technicznej i komentarze

Użyj sekcji pytania dołączonego [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5 i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) przykładowy (C#) w przypadku jakichkolwiek pytań lub komentarzy.

Komentarze dotyczące tego samouczka serii są powitalną, a po zaktualizowaniu tego samouczka serii wszelkich starań, zostaną wprowadzone należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszenia, które znajdują się w samouczku komentarze.

Po wystąpieniu błędu podczas tworzenia lub witryny sieci Web nie działa poprawnie, komunikaty o błędach mogą spowodować wskazówek złożone źródło problemu lub nie może wyjaśnić, jak to naprawić. Pomóc w niektórych typowych scenariuszy problem, można również użyć [fora ASP.NET](https://forums.asp.net/) lub sekcji pytania dołączonego [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5 i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) próbki. Jeśli coś nie działa podczas wykonywania kroków samouczków wyświetlony komunikat o błędzie, należy sprawdzić w powyższych lokalizacjach.

> [!div class="step-by-step"]
> [Next](create-the-project.md)
