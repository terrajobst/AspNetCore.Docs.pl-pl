---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Część 1: Omówienie i plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 1 obejmuje przegląd oraz plik -> Nowy projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868974"
---
<a name="part-1-overview-and-file-new-project"></a>Część 1: Omówienie i plik -> Nowy projekt
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 1 zawiera omówienie i pliku -&gt;nowy projekt.


## <a name="overview"></a>Omówienie

Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Web Developer. Firma Microsoft będzie powoli uruchamianie, środowisko rozwoju poziomu sieci web dla początkujących użytkowników jest poprawny.

Aplikacji, które firma Microsoft będzie zbudować jest magazynem muzyka proste. Istnieją trzy główne części do aplikacji: zakupów, wyewidencjonowania i administracji.

![](mvc-music-store-part-1/_static/image1.jpg)

Odwiedzający mogą przeglądać albumy według rodzaju:

![](mvc-music-store-part-1/_static/image2.jpg)

Mogą wyświetlać jednego albumu i dodaj go do ich koszyka:

![](mvc-music-store-part-1/_static/image3.jpg)

Mogą one przejrzeć ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:

![](mvc-music-store-part-1/_static/image4.jpg)

Przejściem do wyewidencjonowania wyświetli monit o ich zalogować się lub zarejestrować się na koncie użytkownika.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Po utworzeniu konta ich zakończeniu kolejności, wypełniając informacji wysyłki i płatności. Aby zachować proste rzeczy, prowadzimy niesamowite podwyższania poziomu: wszystko, co jest bezpłatne, jeśli użytkownik podał kod promocyjny "Wolne"!

![](mvc-music-store-part-1/_static/image5.jpg)

Po określeniu kolejności, zobaczy ekran potwierdzenia prosty:

![](mvc-music-store-part-1/_static/image6.jpg)

Oprócz klienta faceing stron firma Microsoft będzie także kompilacji sekcji administratora, zawierający listę albumy, z których administratorzy mogą tworzyć, edycji i Usuń albumów:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Plik —&gt; nowy projekt

### <a name="installing-the-software"></a>Instalowanie oprogramowania

W tym samouczku rozpocznie się przez utworzenie nowego projektu platformy ASP.NET MVC 3 przy użyciu wolnego Visual Web Developer 2010 Express (który jest wolny), a następnie stopniowo dodamy funkcje tworzenia kompletna aplikacja działa. Przeglądania omówione zostaną następujące czynności dostępu do bazy danych, scenariuszy Księgowanie formularza, sprawdzanie poprawności danych, za pomocą stron wzorcowych w spójne strony układu, za pomocą interfejsu AJAX dla strony aktualizacji i sprawdzania poprawności, dane logowania użytkownika i.

Można wykonać krok po kroku, można również pobrać ukończona aplikacja z [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).

Dodatek SP1 dla programu Visual Studio 2010 lub Visual Web Developer 2010 Express z dodatkiem SP1 (bezpłatna wersja programu Visual Studio 2010) służy do tworzenia aplikacji. Będziemy używać programu SQL Server Compact (również bezpłatnie) do obsługi bazy danych. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej.


- [Wymagania wstępne programu visual Studio Web Developer Express z dodatkiem SP1]
- [Aktualizacji narzędzi programu ASP.NET MVC 3]
- [SQL Server Compact 4.0] - tym obsługi zarówno środowiska uruchomieniowego i narzędzia


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Tworzenie nowego projektu platformy ASP.NET MVC 3

Zaczniemy, wybierając "Nowego projektu" z menu Plik programu Visual Web Developer. Spowoduje to wyświetlenie okna dialogowego Nowy projekt.

![](mvc-music-store-part-1/_static/image5.png)

Wybierzemy Visual C# —&gt; szablonów sieci Web grupę po lewej stronie, a następnie wybierz szablon "Platformy ASP.NET MVC 3 aplikacji sieci Web" w środkowej kolumnie. Nazwa projektu MvcMusicStore i naciśnij przycisk OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Spowoduje to wyświetlenie dodatkowej okno dialogowe, które pozwala nam w tworzeniu niektórych MVC ustawienia specyficzne dla naszych projektu. Wybierz następujące pozycje:

Projekt szablonu — wybierz opcję Pusta

Wyświetl aparat — Wybieranie Razor

Użyj znaczników semantyki HTML5 - zaznaczone

Sprawdź, czy ustawienia są w sposób przedstawiony poniżej, a następnie naciśnij przycisk OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Spowoduje to utworzenie naszych projektu. Spójrzmy na foldery, które zostały dodane do naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.

![](mvc-music-store-part-1/_static/image10.jpg)

Szablon pusty MVC 3 nie jest pusty — dodaje strukturę folderów podstawowe:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC korzysta z niektóre podstawowe konwencje nazewnictwa dla nazwy folderu:

| **Folder** | **Cel** |
| --- | --- |
| **/ Kontrolerów** | Kontrolery odpowiedzieć na dane wejściowe z przeglądarki, czynności, jakie należy wykonać, a odpowiedź zwrócona do użytkownika. |
| **/ Widoków** | Widoki przechowywania naszym szablony interfejsu użytkownika |
| **/ Modeli** | Modele przechowywania i manipulowanie danymi |
| **/Content** | Ten folder przechowuje naszych obrazów, CSS i innej zawartości statycznej |
| **/ Skryptów** | Ten folder przechowuje plików JavaScript |

Te foldery są zawarte nawet w przypadku aplikacji pusty ASP.NET MVC, ponieważ platforma ASP.NET MVC domyślnie korzysta z podejścia "Konwencji za pośrednictwem konfiguracji" i sprawia, że niektóre założenia domyślne oparte na konwencji nazewnictwa folderu. Na przykład kontrolery szukają widoków w folderze Widoki domyślnie bez konieczności jawnego określania to w kodzie. Spełni domyślnych Konwencji zmniejsza ilość kodu, należy napisać, i może również ułatwić inni deweloperzy zrozumienie projektu. Wyjaśniamy tych konwencji więcej, jak mamy utworzyć w naszej aplikacji.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
