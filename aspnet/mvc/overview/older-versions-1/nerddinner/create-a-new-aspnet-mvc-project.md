---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Utwórz nowy projekt ASP.NET MVC | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 1 pokazuje, jak umieścić podstawową strukturę aplikacji NerdDinner w miejscu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374279"
---
<a name="create-a-new-aspnet-mvc-project"></a>Utwórz nowy projekt ASP.NET MVC
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 1 z BEZPŁATNEJ [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 1 pokazuje, jak umieścić podstawową strukturę aplikacji NerdDinner w miejscu.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner krok 1: Plik -&gt;nowy projekt

Firma Microsoft rozpocznie się naszej aplikacji NerdDinner, wybierając **plikach&gt;nowy projekt** element menu w programie Visual Studio 2008 lub bezpłatnego programu Visual Web Developer 2008 Express.

Zostanie wyświetlone okno dialogowe "Nowego projektu". Aby utworzyć nową aplikację ASP.NET MVC, firma Microsoft, wybierz węzeł "Web" po lewej stronie okna dialogowego i następnie wybierz szablon projektu "Aplikacja sieci Web platformy ASP.NET MVC" po prawej stronie:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Ważne: Upewnij się, że zostały pobrane i zainstalowane ASP.NET MVC — w przeciwnym razie nie będzie widoczna w oknie dialogowym Nowy projekt. Używasz wersji 2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) Jeśli nie został on jeszcze zainstalowany (ASP.NET MVC jest dostępna w ramach "platformy sieci Web -&gt;środowisk uruchomieniowych i platform" sekcja).*

Firma Microsoft będzie nazwa nowego projektu, którą zamierzamy utworzyć "NerdDinner", a następnie kliknij przycisk "ok", aby go utworzyć.

Po kliknięciu przycisku "ok" Visual Studio zostanie wyświetlone okno dialogowe dodatkowe, które nam by opcjonalnie utworzyć projekt testu jednostkowego dla nowej aplikacji, a także wyświetla monit o. Ten projekt testu jednostkowego pozwala tworzyć zautomatyzowane testy weryfikujące funkcji i zachowań, naszej aplikacji (coś omówimy sposób zadań do wykonania w dalszej części tego samouczka).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Lista rozwijana "Test framework" w oknie dialogowym powyżej jest wypełniana wszystkie dostępne szablony ASP.NET MVC jednostki testu projektu zainstalowane na komputerze. Wersje mogą być pobierane do XUnit, NUnit i MBUnit. Obsługiwane jest również wbudowane środowiska testów jednostkowych w usłudze Visual Studio.

*Uwaga: Środowiska testów jednostkowych programu Visual Studio jest dostępna tylko za pomocą programu Visual Studio Professional 2008 i nowszych wersji. Jeśli używasz programu VS 2008 Standard Edition lub Visual Web Developer 2008 Express musisz pobrać i zainstalować rozszerzenia NUnit, MBUnit lub XUnit dla platformy ASP.NET MVC w kolejności dla tego okna dialogowego do wyświetlenia. Okno dialogowe nie będzie wyświetlany, jeśli nie ma żadnych platform testów zainstalowane.*

Firma Microsoft będzie Użyj domyślnej nazwy "NerdDinner.Tests" dla projektu testowego, tworzonej przez nas, a następnie użyj opcji "Visual Studio testu jednostkowego" framework. Po kliknięciu przycisku "ok" Visual Studio Utwórz rozwiązanie dla nas przy użyciu dwóch projektów w nim — jeden dla naszej aplikacji internetowej i jeden dla testów jednostkowych:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Badanie struktury katalogów NerdDinner

Po utworzeniu nowej aplikacji platformy ASP.NET MVC z programem Visual Studio automatycznie dodaje wiele plików i katalogów do projektu:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projekty programu ASP.NET MVC domyślnie mają sześć katalogów najwyższego poziomu:

| **Katalog** | **Cel** |
| --- | --- |
| **/ Kontrolerów** | Gdzie umieścić klasy kontrolera, które obsługuje adres URL żądania |
| **/ Modeli** | Gdzie umieścić klas, które reprezentują i manipulowanie danymi |
| **/ Widoków** | Gdzie umieścić pliki szablonów interfejsu użytkownika, które są odpowiedzialne za renderowaniem w danych wyjściowych |
| **/ Skryptów** | Gdzie umieścić pliki biblioteki JavaScript i skrypty (js) |
| **/Content** | Gdzie umieścić CSS i pliki obrazów i innej zawartości innego niż dynamic/inne niż JavaScript |
| **/ Aplikacji\_danych** | W przypadku, gdy są przechowywane pliki danych chcesz odczytu/zapisu. |

ASP.NET MVC nie wymaga tej struktury. W rzeczywistości deweloperzy pracujący nad dużych aplikacji będzie zazwyczaj partycji aplikacji się w wielu projektach umożliwiają łatwiejsze w zarządzaniu (na przykład: klasy modelu danych często go w projekcie osobnej klasy biblioteki z aplikacji sieci web). Jednak domyślnej struktury projektu, zapewniają nieuprzywilejowany domyślnej konwencji katalogu, który możemy użyć, aby zachować czyste starannością aplikacji.

Podczas rozwijania katalogu /Controllers możemy znaleźć, Visual Studio dodaje dwie klasy kontrolera — HomeController i elementu AccountController — domyślnie do projektu:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Gdy firma Microsoft jest rozwiniesz katalogu /Views, znaleźliśmy trzy podkatalogi — /Home, / Account i /Shared —, a także kilka szablon, który domyślnie pliki znajdujące się w ich również zostały dodane do projektu:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Podczas rozwijania argumencie / i katalogi/skrypty, możemy znaleźć obsługę pliku Site.css, który służy do nadawania stylu wszystkich HTML w witrynie, a także biblioteki JavaScript, umożliwiające ASP.NET AJAX i bibliotece jQuery w aplikacji:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Gdy firma Microsoft rozwiń projekt NerdDinner.Tests znaleźliśmy dwóch klas, które zawierają testy jednostkowe dla naszych zajęć kontrolera:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Te pliki domyślne, dodawane przez program Visual Studio Opisz podstawowa struktura dla działającej aplikacji — ze strony głównej o strony, strony logowania/wylogowania/rejestracja konta i na stronie nieobsługiwany błąd (wszystkie przewodowej w górę i działa poza pole).

### <a name="running-the-nerddinner-application"></a>Uruchomienie aplikacji NerdDinner

Można uruchomić projektu, wybierając opcję **debugowania -&gt;Rozpocznij debugowanie** lub **debugowania -&gt;Rozpocznij bez debugowania** elementy menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

To spowoduje uruchomienie wbudowanego serwera programu ASP.NET w sieci Web-dostarczanego z programem Visual Studio i uruchomić aplikację:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Poniżej znajduje się Strona główna nasz nowy projekt (adres URL: "/") po uruchomieniu:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Klikając kartę "About" Wyświetla informacje o stronie (adres URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Kliknięcie linku "Logowanie" w prawym górnym rogu nam przejście do strony logowania (adres URL: "/ konta/logowania")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Nie mamy konto logowania, możemy kliknąć link Zarejestruj (adres URL: "/ konto/Register") można utworzyć:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Kod implementujący powyżej domu i wylogowania / register funkcja została dodana domyślnie podczas tworzenia nasz nowy projekt. Użyjemy jej jako punktu wyjścia naszej aplikacji.

### <a name="testing-the-nerddinner-application"></a>Testowanie aplikacji NerdDinner

Firma Microsoft korzystania z wersji Professional lub nowszej wersji programu Visual Studio 2008 możemy użyć wbudowaną jednostkę testowania Obsługa środowiska IDE programu Visual Studio, aby przetestować projekt:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Wybranie jednej z powyższych opcji Otwórz okienko "Wyników testów" w środowisku IDE programu i przekazywania nam ze stanem Powodzenie/Niepowodzenie w ramach testów jednostkowych 27, zawarte w naszym nowym projektem, które obejmują wbudowane funkcje:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

W dalszej części tego samouczka będziemy mówić więcej informacji na temat automatycznego testowania i dodać testy jednostkowe dodatkowe, które obejmują funkcje aplikacji, które wdrażamy.

### <a name="next-step"></a>Następny krok

Teraz mamy struktury Podstawowa aplikacja w miejscu. Przejdźmy teraz [utworzyć bazę danych do przechowywania danych aplikacji](create-a-database.md).

> [!div class="step-by-step"]
> [Poprzednie](introducing-the-nerddinner-tutorial.md)
> [dalej](create-a-database.md)
