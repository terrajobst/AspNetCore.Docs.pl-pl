---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Część 4: Modele i dostęp do danych | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 4 obejmuje modele i dostęp do danych.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6a07bf6c8a3fb926ae25fe1f6c9359e64cd7a290
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756976"
---
<a name="part-4-models-and-data-access"></a>Część 4: Modele i dostęp do danych
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.
> 
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 4 obejmuje modele i dostęp do danych.


Do tej pory firma Microsoft została tylko zostały przekazywanie "fikcyjne dane" z naszych kontrolerów do nasze szablony widoku. Teraz jesteśmy gotowi zaczepić rzeczywista baza danych. W tym samouczku będzie dotyczyć sposób używania programu SQL Server Compact Edition (często nazywanej SQL CE) jako naszego aparatu bazy danych. SQL CE jest bezpłatna, plik osadzony na podstawie bazy danych, która nie wymaga instalacji ani konfiguracji, która sprawia, że naprawdę wygodne do tworzenia aplikacji lokalnej.

## <a name="database-access-with-entity-framework-code-first"></a>Dostęp do bazy danych za pomocą Entity Framework najpierw kod

Użyjemy obsługi Entity Framework (EF), który znajduje się w projektach ASP.NET MVC 3 do wykonywania zapytań i aktualizacji bazy danych. EF jest obiektem elastyczne, relacyjne mapowanie danych (ORM) interfejsu API, który umożliwia deweloperom zapytania i aktualizację danych przechowywanych w bazie danych w sposób zorientowane obiektowo.

Entity Framework w wersji 4 obsługuje paradygmat programowania, o nazwie najpierw kod. Najpierw kod umożliwia utworzenie obiektu modelu, pisząc klasy proste (znany także jako POCO z obiektów CLR "zwykły stary"), a nawet utworzyć bazy danych na bieżąco z klas.

### <a name="changes-to-our-model-classes"></a>Zmiany w naszych zajęć modelu

Firma Microsoft będzie się korzystanie z funkcji tworzenia bazy danych platformy Entity Framework w ramach tego samouczka. Zanim do tego, jednak upewnijmy się kilka drobne zmiany do naszych zajęć modelu, aby dodać niektóre czynności, które będzie używana później.

#### <a name="adding-the-artist-model-classes"></a>Dodawanie klasy modelu wykonawcy

Nasze albumów, zostanie skojarzona z artyści, dlatego dodamy klasę modelu proste do opisania wykonawcy. Dodaj nową klasę do folderu modeli o nazwie Artist.cs przy użyciu kodu pokazany poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualizowanie naszych zajęć modelu

Aktualizacja klasy fotograficzne, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Następnie wprowadź następujące aktualizacje do klasy gatunku.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Dodawanie aplikacji\_folderu danych

Dodamy aplikacji\_katalog danych do naszego projektu do przechowywania plików bazy danych programu SQL Server Express. Aplikacja\_dane są specjalne katalogu w programie ASP.NET, który już ma poprawne uprawnienia dostępu dla dostępu do bazy danych. Menu projektu i wybierz polecenie Dodaj Folder programu ASP.NET, a następnie aplikacja\_danych.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Tworzenie parametrów połączenia w pliku web.config

Dodamy kilka wierszy do pliku konfiguracji witryny internetowej tak aby wie, jak połączyć się z naszym bazy danych platformy Entity Framework. Kliknij dwukrotnie plik Web.config znajduje się w folderze głównym projektu.

![](mvc-music-store-part-4/_static/image2.png)

Przewiń w dół tego pliku i Dodaj &lt;connectionStrings&gt; sekcji bezpośrednio nad ostatni wiersz, jak pokazano poniżej.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Dodawanie klasy kontekstu

Kliknij prawym przyciskiem myszy folderu modeli i Dodaj nową klasę o nazwie MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Tej klasy będzie reprezentują kontekstu bazy danych programu Entity Framework, będzie obsłużyć nasz tworzenia, odczytu, aktualizacji i operacje usuwania dotyczące nam. Poniżej przedstawiono kod dla tej klasy.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

To wszystko — Brak nie innych konfiguracji specjalnych interfejsów, itp. Rozszerzając klasy bazowej typu DbContext, klasy Nasze MusicStoreEntities będzie mogło obsłużyć nasz operacji bazy danych dla nas. Skoro mamy podłączyłeś, Dodajmy kilka innych właściwości do naszych zajęć na model, aby móc korzystać z niektórych dodatkowych informacji w naszej bazie danych.

### <a name="adding-our-store-catalog-data"></a>Dodawanie magazynu danych katalogu

Firma Microsoft będzie korzystać z funkcji platformy Entity Framework, który dodaje dane "seed" nowo utworzoną bazę danych. Spowoduje to wstępnie wypełnić nasz katalog sklepu za pomocą listy gatunki, artystów i albumów. Pobrania MvcMusicStore Assets.zip - uwzględnione naszych plików projektu lokacji używanych wcześniej w tym samouczku — zawiera plik klasy z tymi danymi inicjatora, znajduje się w folderze o nazwie kodu.

W kodzie / folderu modeli, zlokalizuj plik SampleData.cs i upuść go w folderze modeli w projekcie, jak pokazano poniżej.

![](mvc-music-store-part-4/_static/image4.png)

Teraz należy dodać jeden wiersz kodu, aby poinformować Entity Framework o tej klasy SampleData. Kliknij dwukrotnie plik Global.asax w katalogu głównym projektu, aby otworzyć go i Dodaj następujący wiersz u góry aplikacji\_Uruchom metodę.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

W tym momencie możemy ukończono pracę niezbędne do skonfigurowania programu Entity Framework naszego projektu wygląda.

## <a name="querying-the-database"></a>Zapytanie do bazy danych

Teraz zaktualizujmy naszych StoreController tak, aby zamiast "fikcyjny dane" zamiast niego w naszej bazie danych do wykonywania zapytań, wszystkie jej informacje. Rozpoczniemy od zadeklarowania pola na **StoreController** zawierającą wystąpienia klasy MusicStoreEntities o nazwie storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualizowanie indeksu Store do wykonywania zapytań w bazie danych

Klasa MusicStoreEntities jest obsługiwany przez program Entity Framework i ujawnia właściwość kolekcji, dla każdej tabeli w naszej bazie danych. Zaktualizujmy naszych StoreController akcji indeksu można pobrać wszystkich gatunki w naszej bazie danych. Wcześniej zrobiliśmy to zakodowane na stałe danych ciągu. Teraz możemy zamiast tego użyć kontekstu platformy Entity Framework Generes kolekcji:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Nie zmiany muszą zostać przeprowadzona do naszych szablon widoku, ponieważ firma Microsoft nadal zwracany tego samego StoreIndexViewModel, firma Microsoft zwrócone, zanim — firma Microsoft jest po prostu zwracanie danych na żywo z naszej bazie danych teraz.

Jeśli firma Microsoft Uruchom projekt ponownie, odwiedź adres URL "/ Store" teraz zobaczymy listę wszystkich gatunki w naszej bazie danych:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualizowanie Przeglądaj Store i szczegółowe informacje na korzystanie z danych na żywo

Za pomocą/Store/przeglądania? gatunku =*[niektóre gatunek]* metody akcji, Trwa przeszukiwanie dla określonego rodzaju według nazwy. Tylko oczekujemy, że jeden wynik, ponieważ firma Microsoft nigdy nie powinny mieć dwóch wpisów dla tej samej nazwie gatunku, więc możemy użyć. Funkcja Single() rozszerzenia w składniku LINQ do kwerendy do odpowiedniego obiektu gatunku następująco (nie należy umieszczać to jeszcze):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Pojedyncza metoda przyjmuje wyrażenia Lambda jako parametr, który określa, że chcemy, aby pojedynczy obiekt gatunku taki sposób, że jego nazwa pasuje do wartości, który zdefiniowaliśmy. W przypadku powyższych ładujemy pojedynczy obiekt gatunku dopasowania Najdywania wartości nazw.

Firma Microsoft będzie korzystać z to funkcja programu Entity Framework, która pozwala nam w celu wskazania innych powiązanych jednostek, które chcemy, aby załadować także po pobraniu obiektu gatunku. Ta funkcja jest wywoływana, kształtowanie wynik zapytania i pozwala na zmniejszenie liczby potrzebujemy dostępu do bazy danych, aby pobrać wszystkie informacje potrzebne. Chcemy wstępnego pobierania albumów dla gatunku Pobieramy, dlatego zaktualizujemy naszego zapytania do dołączenia z Genres.Include("Albums"), aby wskazać, że chcemy także powiązane ze zdjęciami. Może to być bardziej efektywne, ponieważ pobierze dane naszych gatunku i albumów w żądaniu pojedynczej bazy danych.

Wraz z wyjaśnieniem na bok poniżej przedstawiono, jak wygląda naszej zaktualizowane akcji kontrolera przeglądania:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Firma Microsoft jest teraz zaktualizować widoku Przeglądaj Store, aby wyświetlić ze zdjęciami, które są dostępne w każdego gatunku. Otwórz szablon widoku (znaleziony w /Views/Store/Browse.cshtml), a następnie Dodaj listę punktowaną ze zdjęciami, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Uruchamianie aplikacji i przechodząc do/Store/przeglądania? gatunku = pokazują Jazz naszych wyników są teraz pochodzi z bazy danych, wyświetlanie albumów wszystkich naszych wybranego rodzaju.

![](mvc-music-store-part-4/_static/image2.jpg)

Wybierzemy taką samą Zmień/Store/szczegóły / [id] adresu URL i Zamień zastępczy danych zapytanie bazy danych, która ładuje albumu o identyfikatorze pasuje do wartości parametru.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Uruchamianie aplikacji i przechodząc do /Store/Details/1 pokazuje, że naszych wyników są teraz pobierane z bazy danych.

![](mvc-music-store-part-4/_static/image5.png)

Teraz, że nasza strona szczegółów Store jest skonfigurowany do wyświetlania albumu według Identyfikatora fotograficzne, zaktualizujmy **Przeglądaj** widok, aby utworzyć łącze do widoku szczegółów. Firma Microsoft użyje Html.ActionLink, dokładnie tak, jak zrobiliśmy się połączyć z indeksu Store Przeglądaj Store na końcu w poprzedniej sekcji. Pełne źródło widoku przeglądania pojawia się poniżej.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Teraz możemy przejść z poziomu strony Store do strony gatunku, którym jest wyświetlana lista dostępnych ze zdjęciami, i klikając albumu możemy wyświetlić szczegóły dotyczące danego albumu.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-3.md)
> [dalej](mvc-music-store-part-5.md)
