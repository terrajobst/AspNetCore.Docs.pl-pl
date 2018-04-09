---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Część 4: Modele i dostęp do danych | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 4 obejmuje modele i dostęp do danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-4-models-and-data-access"></a>Część 4: Modele i dostęp do danych
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.
> 
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 4 obejmuje modele i dostęp do danych.


Do tej pory firma Microsoft został właśnie zostały przekazaniem "fikcyjny" z naszych kontrolerów do naszym szablony widoku. Teraz już wszystko gotowe do Podłączanie rzeczywistych bazy danych. W tym samouczku firma Microsoft będzie obejmował jak używać programu SQL Server Compact Edition (często nazywane SQL CE) jako naszych aparatu bazy danych. SQL CE jest bezpłatne, plik osadzony na podstawie bazy danych, która nie wymaga żadnych instalacji lub konfiguracji, która ułatwia naprawdę dla rozwoju lokalnych.

## <a name="database-access-with-entity-framework-code-first"></a>Dostęp do bazy danych z Entity Framework kod pierwszego

Użyjemy Obsługa Entity Framework (EF) znajduje się w projektach ASP.NET MVC 3 w celu wykonywania zapytań i zaktualizować bazę danych. EF jest obiektem elastyczne relacyjne mapowanie danych (ORM) interfejsu API, który umożliwia deweloperom zapytań i aktualizacji danych przechowywanych w bazie danych w sposób zorientowane obiektowo.

Entity Framework w wersji 4 obsługuje modelu programowania, nazywany pierwszy kod. Pierwszy kod służy do tworzenia obiektu modelu pisząc klasy proste (znanej także jako POCO z obiektów CLR "zwykły stary"), a może nawet utworzyć bazę danych na bieżąco z klas.

### <a name="changes-to-our-model-classes"></a>Zmiany w naszej klasy modelu

Firma Microsoft będzie można korzystania z funkcji tworzenia bazy danych w programie Entity Framework w tym samouczku. Zanim przejdziemy, jednak upewnijmy kilka drobne zmiany do naszej klasy modelu mają zostać dodane w kilka rzeczy, który będzie używany na dalszym etapie.

#### <a name="adding-the-artist-model-classes"></a>Dodawanie klas modelu wykonawcy

Nasze albumów zostanie skojarzona z Artystami, więc dodamy klasę modelu proste do opisania wykonawcy. Dodaj nową klasę do folderu modeli o nazwie Artist.cs przy użyciu kodu pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualizowanie naszej klasy modelu

Klasa albumu aktualizacji, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Następnie wprowadzić następujące aktualizacje do klasy Genre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Dodawanie aplikacji\_folderu danych

Dodamy aplikacji\_katalog danych do naszej projektu do przechowywania plików bazy danych programu SQL Server Express. Aplikacja\_dane są specjalne katalogu w programie ASP.NET, który ma już uprawnienia dostępu poprawnych zabezpieczeń dla dostępu do bazy danych. Wybierz z menu Projekt Dodaj Folder programu ASP.NET, a następnie aplikacji\_danych.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Tworzenie parametrów połączenia w pliku web.config

Dodamy kilka wierszy do pliku konfiguracji witryny sieci Web tak, aby Entity Framework potrafi nawiązać połączenia z naszej bazie danych. Kliknij dwukrotnie plik Web.config znajduje się w katalogu głównym projektu.

![](mvc-music-store-part-4/_static/image2.png)

Przewiń w dół ten plik i dodać &lt;connectionStrings&gt; sekcji bezpośrednio nad ostatni wiersz, jak pokazano poniżej.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Dodawanie klasy kontekstu

Kliknij prawym przyciskiem myszy folder modeli i Dodaj nową klasę o nazwie MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Ta klasa zostanie reprezentują kontekst bazy danych programu Entity Framework i zostanie obsługi naszych Utwórz, odczytu aktualizacji i usuwanie operacji firmie Microsoft. Poniżej przedstawiono kod dla tej klasy.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

To wszystko — nie żadnych innych konfiguracji specjalnych interfejsów, itp. Przez rozszerzenie klasy podstawowej typu DbContext, klasy Nasze MusicStoreEntities jest w stanie obsłużyć naszego operacje bazy danych w firmie Microsoft. Teraz, gdy mamy który podłączonymi Dodajmy kilka więcej właściwości do naszej klasy modeli, aby móc korzystać z niektórych dodatkowych informacji w naszej bazie danych.

### <a name="adding-our-store-catalog-data"></a>Dodawanie magazynu danych katalogu

Firma Microsoft będzie korzystać z funkcji w Entity Framework, która dodaje "seed" danych do nowo utworzonej bazy danych. Spowoduje to wstępnego wypełniania naszego katalogu magazynu z listą Genres, artystów i albumów. Pobieranie MvcMusicStore Assets.zip - zawierającego plików projektu lokacji używany we wcześniejszej części tego samouczka - ma plik klasy przy użyciu tych danych inicjatora, znajduje się w folderze o nazwie kodu.

W kodzie / folderu modeli, zlokalizuj plik SampleData.cs i upuść go w folderze modeli w naszym projektu, jak pokazano poniżej.

![](mvc-music-store-part-4/_static/image4.png)

Teraz należy dodać jeden wiersz kodu powiedzieć o tej klasy SampleData Entity Framework. Kliknij dwukrotnie plik Global.asax w folderze głównym projektu, aby go otworzyć i Dodaj poniższy wiersz do góry aplikacji\_Start — metoda.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

W tym momencie została ukończona pracy niezbędne do skonfigurowania programu Entity Framework dla naszych projektu.

## <a name="querying-the-database"></a>Zapytanie bazy danych

Teraz załóżmy zaktualizować naszych StoreController tak, aby zamiast "fikcyjny danych" zamiast tego w naszej bazie danych do zapytania, wszystkie jej informacje. Zaczniemy od zadeklarowania pola na **StoreController** do przechowywania wystąpienia klasy MusicStoreEntities o nazwie storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualizowanie indeks magazynu w bazie danych

Klasa MusicStoreEntities jest obsługiwany przez program Entity Framework i udostępnia właściwości kolekcji dla każdej tabeli w naszej bazie danych. Teraz zaktualizuj naszych StoreController akcji indeksu można pobrać wszystkich Genres w naszej bazie danych. Wcześniej Robiliśmy to przez kodować danych ciągu. Teraz możemy zamiast tego użyć kontekstu programu Entity Framework Generes kolekcji:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Brak zmian muszą się stać z naszych wyświetlanie szablonu, ponieważ firma Microsoft jest nadal zwracanie tego samego StoreIndexViewModel, możemy zwrócić przed - firma Microsoft jest po prostu zwracać dane na żywo z naszej bazie danych teraz.

Gdy firma Microsoft Uruchom ponownie projekt i odwiedź adres URL "/ przechowywania", firma Microsoft teraz wyświetlona lista wszystkich gatunkami muzyki w naszej bazie danych:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualizowanie Przeglądaj magazynu i szczegóły korzystać z bieżących danych

Z/magazynu/Przeglądaj? genre =*[niektóre genre]* metody akcji, możemy poszukiwaną określonego rodzaju według nazwy. Tylko oczekujemy jeden wynik, ponieważ firma Microsoft nigdy nie powinien zawierać dwa wpisy dla tej samej nazwie Genre, w związku z czym możemy użyć. Rozszerzenie Single() w składniku LINQ do zapytania dla odpowiedniego obiektu Genre następująco (nie należy wpisywać to jeszcze):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Pojedynczy metoda korzysta z wyrażenia Lambda jako parametr, który określa, że chcemy pojedynczy obiekt Genre taki sposób, że jego nazwa jest zgodna z wartością, już zdefiniowanego. W przypadku powyższych możemy ładowania obiektu jednego rodzaju z wartością nazwy dopasowania Disco.

Firma Microsoft będzie korzystać z funkcji programu Entity Framework, umożliwiający wskazywać inne pokrewne jednostki, którą chcemy udostępnić również załadowany po pobraniu obiektu rodzaju. Ta funkcja jest wywoływana kształtowania wynik zapytania i pozwala zmniejszyć liczbę razy, musimy dostęp do bazy danych, aby pobrać wszystkie potrzebne informacje. Chcemy wstępnie pobrać albumów dla rodzaju Pobieramy, więc będziemy informować naszych zapytania do dołączenia z Genres.Include("Albums"), aby wskazać, że firma Microsoft ma również albumów powiązane. Jest to bardziej wydajne, ponieważ będą pobierane dane zarówno naszych Genre, jak i albumu w żądaniu pojedynczej bazy danych.

Wraz z wyjaśnieniem przeszkadza poniżej przedstawiono wygląd naszego zaktualizowane akcji kontrolera przeglądania:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Mamy teraz zaktualizować widoku Przeglądaj magazynu, aby wyświetlić albumów, które są dostępne każdego rodzaju. Otwórz szablon widoku (znaleziony w /Views/Store/Browse.cshtml), a następnie dodać listy punktowane albumy, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Z naszej aplikacji i przechodząc do sklepu/Przeglądaj? genre = Jazz wskazuje, że nasze wyniki są teraz pochodzi z bazy danych, wyświetlanie albumów wszystkich naszych wybranego rodzaju.

![](mvc-music-store-part-4/_static/image2.jpg)

Wybierzemy takie same, Zmień na naszych/Store/szczegółów lub [id] adres URL i Zastąp fikcyjny danych zapytania bazy danych, który jest ładowany albumu o identyfikatorze jest zgodna z wartością parametru.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Z naszej aplikacji i wyszukując /Store/Details/1 pokazuje, że nasze wyniki są teraz pobierane z bazy danych.

![](mvc-music-store-part-4/_static/image5.png)

Teraz, kiedy naszą stronę Szczegóły magazynu jest skonfigurowane do wyświetlenia albumu według Identyfikatora albumy, możemy zaktualizować **Przeglądaj** widok, aby utworzyć łącze do widoku szczegółów. Używamy Html.ActionLink, dokładnie tak, jak robiliśmy link z indeksem magazynu do magazynu, przejdź na końcu poprzedniej sekcji. Pełne źródło dla widoku przeglądania pojawia się poniżej.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Teraz możemy przejść z naszą stronę Sklepu do strony Genre, który znajduje się lista dostępnych albumy, i klikając albumu możemy wyświetlić szczegóły dotyczące danego albumu.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-3.md)
> [dalej](mvc-music-store-part-5.md)
