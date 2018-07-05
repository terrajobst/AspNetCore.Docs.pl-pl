---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Część 1: Omówienie i plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 1 obejmuje omówienie i plik -> Nowy projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377892"
---
<a name="part-1-overview-and-file-new-project"></a>Część 1: Omówienie i plik -> Nowy projekt
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 1 obejmuje przegląd i plikach&gt;nowy projekt.


## <a name="overview"></a>Omówienie

MVC Music Store jest aplikacją z samouczka, który wprowadza i opisano krok po kroku na potrzeby tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Web Developer. Firma Microsoft będzie uruchamiana powoli, a więc środowisko programowania dla początkujących poziomu sieci web jest poprawny.

Aplikacja, którą tworzymy będzie jest proste music store —. Istnieją trzy główne fragmenty aplikacji: zakupów, finalizacja zakupu i administracyjnej.

![](mvc-music-store-part-1/_static/image1.jpg)

Goście mogą przeglądać albumów według gatunku:

![](mvc-music-store-part-1/_static/image2.jpg)

Mogą wyświetlać jednego albumu i dodać go do koszyka ich:

![](mvc-music-store-part-1/_static/image3.jpg)

Mogą przeglądać ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:

![](mvc-music-store-part-1/_static/image4.jpg)

Przejściem do wyewidencjonowania spowoduje wyświetlenie monitu o zalogowanie lub Zarejestruj się, aby konto użytkownika.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Po utworzeniu konta usługi, możliwości ich ukończenia zamówienia, wypełniając informacji wysyłki i płatności. Aby zachować ich prostotę, prowadzimy wspaniałe podwyższania poziomu: wszystko, co jest bezpłatne, jeśli użytkownik podał kod promocyjny "BEZPŁATNY"!

![](mvc-music-store-part-1/_static/image5.jpg)

Po określeniu kolejności, zobaczy ekran potwierdzenia proste:

![](mvc-music-store-part-1/_static/image6.jpg)

Oprócz stron faceing klienta firma Microsoft będzie również tworzenie sekcji administratora, który wyświetla listę albumy, z których administratorzy mogą tworzyć, edytować i Usuń albumów:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Plik —&gt; nowy projekt

### <a name="installing-the-software"></a>Instalowanie oprogramowania

Ten samouczek rozpoczyna się przez utworzenie nowego projektu ASP.NET MVC 3, przy użyciu bezpłatnego programu Visual Web Developer 2010 Express (która jest bezpłatna), a następnie stopniowo dodamy funkcje umożliwiające tworzenie kompletna aplikacja działa. Po drodze omówimy dostępu do bazy danych, scenariusze ogłaszania formularza i sprawdzanie poprawności danych, za pomocą stron wzorcowych w układu strony spójne, za pomocą interfejsu AJAX do aktualizacji strony i sprawdzania poprawności, dane logowania użytkownika i innych.

Można wykonać krok po kroku, można również pobrać gotową aplikację z [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store).

Aby skompilować aplikację, można użyć programu Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer 2010 Express z dodatkiem SP1 (bezpłatna wersja programu Visual Studio 2010). Będziemy używać programu SQL Server Compact (również wersja bezpłatna) do hostowania bazy danych. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej.


- [Wymagania wstępne programu visual Studio Web Developer Express z dodatkiem SP1]
- [Program ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0] — łącznie z obsługą środowiska uruchomieniowego i narzędzi


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Tworzenie nowego projektu ASP.NET MVC 3

Zaczniemy, wybierając pozycję "Nowy projekt" w menu Plik w Visual Web Developer. Wyświetlenie okna dialogowego Nowy projekt.

![](mvc-music-store-part-1/_static/image5.png)

Wybierzemy Visual C# —&gt; szablonów sieci Web grupę po lewej stronie, a następnie wybierz szablon "Platformy ASP.NET MVC 3 aplikacja sieci Web" w środkowej kolumnie. Nazwij swój projekt MvcMusicStore i naciśnij przycisk OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Spowoduje to wyświetlenie dodatkowych okna dialogowego, co pozwala nam w niektórych określonych ustawień MVC naszego projektu. Wybierz jedną z poniższych:

Projekt szablonu — wybierz pusty

Wyświetl aparatu — Wybieranie Razor

Użyj HTML5 znaczników semantycznych - zaznaczone

Sprawdź, czy ustawienia są, jak pokazano poniżej, a następnie naciśnij przycisk OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Spowoduje to utworzenie projekcie. Przyjrzyjmy się w folderach, które zostały dodane do naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.

![](mvc-music-store-part-1/_static/image10.jpg)

Szablonu pusty MVC 3 nie jest całkowicie pusty — dodaje strukturę folderów podstawowe:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC sprawia, że użycie niektóre podstawowe konwencje nazewnictwa dla nazw folderów:

| **Folder** | **Cel** |
| --- | --- |
| **/ Kontrolerów** | Kontrolery odpowiedzieć na dane wejściowe z przeglądarki, podejmowania decyzji o korzystają z nich, a następnie zwraca odpowiedź do użytkownika. |
| **/ Widoków** | Widoki przechowywania nasze szablony interfejsu użytkownika |
| **/ Modeli** | Modele przechowywania i manipulowanie danymi |
| **/Content** | Ten folder przechowuje nasze obrazy, CSS i innej zawartości statycznej |
| **/ Skryptów** | Ten folder zawiera nasz plików JavaScript |

Te foldery znajdują się nawet w przypadku aplikacji pusty ASP.NET MVC, ponieważ platforma ASP.NET MVC, domyślnie korzysta z metody "Konwencji za pośrednictwem konfiguracji" i zakłada pewne domyślne oparte na konwencjach nazewnictwa folderów. Na przykład kontrolery szukają widoków w folderze Widoki domyślnie bez konieczności jawnego określania to w kodzie. Wyobrazić przy użyciu domyślnych Konwencji zmniejsza ilość kodu, należy napisać, i może również ułatwić innym deweloperom zrozumienie projektu. Wyjaśnimy tych konwencji więcej, jak możemy tworzyć naszej aplikacji.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
