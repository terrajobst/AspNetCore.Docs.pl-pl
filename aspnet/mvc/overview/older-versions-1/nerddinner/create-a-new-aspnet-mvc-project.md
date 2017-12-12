---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: "Utwórz nowy projekt ASP.NET MVC | Dokumentacja firmy Microsoft"
author: microsoft
description: "Krok 1 przedstawiono sposób wprowadzone podstawowej struktury NerdDinner w aplikacji."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-a-new-aspnet-mvc-project"></a>Utwórz nowy projekt ASP.NET MVC
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 1 z bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 1 przedstawiono sposób wprowadzone podstawowej struktury NerdDinner w aplikacji.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner krok 1: Plik -&gt;nowy projekt

Naszej aplikacji NerdDinner rozpocznie się po wybraniu **pliku -&gt;nowy projekt** elementu menu w programie Visual Studio 2008 lub wolnego Visual Web Developer 2008 Express.

Zostanie wyświetlone okno dialogowe "Nowego projektu". Aby utworzyć nową aplikację ASP.NET MVC, firma Microsoft, wybierz węzeł "Web" po lewej stronie okna dialogowego i wybierz szablon projektu "Aplikacji sieci Web platformy ASP.NET MVC" po prawej stronie:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Ważne: Upewnij się, zostały pobrane i zainstalowane ASP.NET MVC — w przeciwnym razie nie będą wyświetlane w oknie dialogowym Nowy projekt. Można użyć V2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) , jeśli nie został on jeszcze zainstalowany (ASP.NET MVC jest dostępne w ramach "platformy sieci Web -&gt;struktury i środowisk uruchomieniowych" sekcji).*

Firma Microsoft będzie nazwę nowego projektu, któremu zamierzamy utworzyć "NerdDinner", a następnie kliknij przycisk "ok", aby go utworzyć.

Po kliknięciu przycisku "ok" programu Visual Studio zostaną wyświetlone dodatkowe okno z monitem o nas można opcjonalnie utworzyć jednostkowy projekt testowy, jak również nowej aplikacji. Ten jednostkowy projekt testowy pozwala na tworzenie zautomatyzowanych testów, które pozwalają sprawdzić, funkcje i zachowanie aplikacji (coś omówione zostaną następujące czynności jak zadań do wykonania w dalszej części tego samouczka).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

"Test" rozwijaną platform w oknie dialogowym powyżej jest wypełniana wszystkie dostępne szablony ASP.NET MVC jednostki testu projektu zainstalowane na komputerze. NUnit, MBUnit i XUnit można pobrać wersji. Obsługiwane jest również wbudowana struktura Visual Studio Test jednostki.

*Uwaga: Platformy testów jednostkowych programu Visual Studio jest dostępne tylko z programu Visual Studio Professional 2008 i nowszych wersjach. Jeśli używasz programu VS 2008 Standard Edition lub Visual Web Developer 2008 Express musisz pobrać i zainstalować rozszerzenia NUnit, MBUnit lub XUnit dla platformy ASP.NET MVC w kolejności dla tego okna dialogowego, który będzie wyświetlany. Okno dialogowe nie będzie wyświetlany, jeśli nie ma żadnych platform testów zainstalowane.*

Firma Microsoft będzie używać domyślnej nazwy "NerdDinner.Tests" dla projektu testowego, tworzonej przez nas i użyj opcji "Programu Visual Studio testu jednostkowego" framework. Po kliknięciu przycisku przycisk "ok" programu Visual Studio utworzy rozwiązania dla nas z dwa projekty w nim — jeden dla aplikacji sieci web i jeden na potrzeby testów jednostkowych:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Badanie NerdDinner struktury katalogów

Podczas tworzenia nowej aplikacji ASP.NET MVC z programem Visual Studio, automatycznie dodaje liczbę plików i katalogów do projektu:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projekty składnika ASP.NET MVC domyślnie ma sześć katalogów najwyższego poziomu:

| **Katalog** | **Cel** |
| --- | --- |
| **/ Kontrolerów** | Gdzie umieścić klasy kontrolera, które obsługuje adresu URL żądania |
| **/ Modeli** | Gdzie umieścić klasy, które reprezentują i manipulowanie danymi |
| **/ Widoków** | Gdzie umieścić plików szablonów interfejsu użytkownika, które są zobowiązani do renderowania danych wyjściowych |
| **/ Skryptów** | Gdzie umieścić pliki bibliotek JavaScript i skrypty (js) |
| **/ Zawartości** | Gdzie umieścić CSS i pliki obrazów i innej zawartości z systemem innym niż dynamic/z systemem innym niż — JavaScript |
| **/ Aplikacji\_danych** | Gdzie można przechowywać pliki danych chcesz odczytu/zapisu. |

ASP.NET MVC nie wymaga tej struktury. W rzeczywistości deweloperów pracujących w dużych aplikacji będzie zazwyczaj partycji aplikacji się w wielu projektach, aby była łatwiejsza w zarządzaniu (na przykład: klasy modelu danych często Przejdź w osobnej klasy biblioteki projektu z aplikacji sieci web). Jednak domyślnej struktury projektu, podaj nieuprzywilejowany domyślnej konwencji katalogu, które pomagają zachować wyczyść zastrzeżenia co do naszej aplikacji.

Gdy firma Microsoft rozwiń katalog /Controllers możemy znaleźć, Visual Studio dodane dwie klasy kontrolera — HomeController i elementu AccountController — domyślnie do projektu:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Gdy firma Microsoft rozwiń katalog /Views, znaleźliśmy trzy podkatalogów — /Home, /Account i /Shared —, a także kilka szablon, który domyślnie zawartych w nich plików również zostały dodane do projektu:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Gdy firma Microsoft rozwinąć argumencie / i/skrypty katalogów, możemy znajdziesz obsługę pliku Site.css, który służy do określania stylu HTML wszystkie w witrynie, a także biblioteki JavaScript, w których można włączyć, jQuery i ASP.NET AJAX w aplikacji:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Gdy firma Microsoft rozwiń projekt NerdDinner.Tests znaleźliśmy dwie klasy zawierające testów jednostkowych dla naszej klasy kontrolera:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Te pliki domyślne, dodawane przez program Visual Studio uzyskaliśmy podstawową strukturę dla działającą aplikację - wraz z strony głównej, strony, strony logowania/wylogowania/rejestracji konta i stronę błędu nieobsługiwany (wszystkie przewodowej w górę i jest uruchomiony bez) informacje.

### <a name="running-the-nerddinner-application"></a>Uruchomienie aplikacji NerdDinner

Można uruchomić projektu, wybierając opcję **debugowania -&gt;Rozpocznij debugowanie** lub **debugowania -&gt;uruchomić bez debugowania** elementów menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Spowoduje to Uruchom wbudowane ASP.NET serwera sieci Web-dostarczonego z programem Visual Studio i uruchomić aplikację:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Poniżej znajduje się Strona główna dla naszego nowego projektu (adres URL: "/") po uruchomieniu:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Klikając kartę "Informacje o" Wyświetla o stronie (adres URL: "/ domowej/o"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Kliknięcie łącza "Logowanie" w prawym górnym rogu powoduje nam do strony logowania (adres URL: "/ Account/logowania")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Nie mamy konto logowania, firma Microsoft może kliknąć łącze rejestru (adres URL: "/ Account/Register") aby go utworzyć:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Kod w celu zaimplementowania domu powyżej i wylogowania / register funkcja została dodana domyślnie, gdy utworzono naszego nowego projektu. Użyjemy go jako punkt początkowy aplikacji.

### <a name="testing-the-nerddinner-application"></a>Testowanie aplikacji NerdDinner

Jeśli użyto Professional Edition lub nowszej wersji programu Visual Studio 2008, możemy użyć wbudowanych jednostki testowania Obsługa środowiska IDE w programie Visual Studio do projektu testowego:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Wybranie jednej z powyższych opcji Otwórz okienko "Wyników testu" w środowisku IDE i uzyskaliśmy stanu przeszedł/nie przeszedł testów jednostkowych 27 uwzględnione w naszej nowego projektu, które obejmują wbudowanej funkcji:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

W dalszej części tego samouczka nasz komunikować się więcej o automatycznych testów i dodać dodatkowych testów, które dotyczą funkcji aplikacji, które wprowadzania.

### <a name="next-step"></a>Następny krok

Mamy teraz struktury aplikacji w warstwie podstawowa w miejscu. Umożliwia teraz [Utwórz bazę danych do przechowywania danych aplikacji](create-a-database.md).

>[!div class="step-by-step"]
[Poprzednie](introducing-the-nerddinner-tutorial.md)
[dalej](create-a-database.md)
