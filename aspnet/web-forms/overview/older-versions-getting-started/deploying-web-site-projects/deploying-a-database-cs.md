---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: "Wdrożenie bazy danych (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Wdrażanie aplikacji sieci web ASP.NET wymaga pobieranie niezbędnych plików i zasobów z Środowisko deweloperskie do środowiska produkcyjnego. Dla da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: f71e3cd1e81644df7b3dfed363b6f2ca826e610d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-database-c"></a>Wdrożenie bazy danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Wdrażanie aplikacji sieci web ASP.NET wymaga pobieranie niezbędnych plików i zasobów z Środowisko deweloperskie do środowiska produkcyjnego. W aplikacjach opartych na danych sieci web obejmują dane i schemat bazy danych. W tym samouczku jest pierwszy serii, która opisuje kroki niezbędne do pomyślnego wdrożenia bazy danych z Środowisko deweloperskie do środowiska produkcyjnego.


## <a name="introduction"></a>Wprowadzenie

Wdrażanie aplikacji sieci web ASP.NET wymaga pobieranie niezbędnych plików i zasobów z Środowisko deweloperskie do środowiska produkcyjnego. W ciągu ostatnich sześciu samouczki analizujemy wdrażanie prostej aplikacji sieci web przeglądami książki. Tej lokacji pokaz został obejmuje wiele zasobów po stronie serwera - stron ASP.NET, pliki konfiguracji `Web.sitemap` pliku i tak dalej — wraz z zasobów po stronie klienta, takie jak obrazów i plików CSS. Ale co opartych na danych aplikacji sieci web? Jakie kroki należy do wdrożenia aplikacji sieci web, która korzysta z bazy danych?

W następnym samouczki kilka będzie można rozwiązać kroki niezbędne do wdrożenia aplikacji sieci web opartych na danych. W tym samouczku rozpoczyna badanie sposobu uzyskania schematu s bazy danych i zawartość ze środowiska projektowego do środowiska produkcyjnego, podczas kolejnych samouczek sprawdza zmiany konfiguracji potrzebne. Po, że firma Microsoft będzie Eksploruj wyzwania wdrażania bazy danych, która korzysta z usługi aplikacji (członkostwo, role, profilu i tak dalej).

## <a name="examining-the-updated-book-reviews-web-application"></a>Badanie aplikacji sieci Web przeglądami książki zaktualizowane

W celu zaprezentowania wdrażanie aplikacji sieci web opartych na danych, I Zapisz zaktualizowany przeglądami książki aplikacji sieci web z prostego, statycznej witryny sieci Web jednej usługi opartej na danych. Jak wcześniej, istnieją dwie wersje aplikacji, w tym samouczku s pobierania:, który korzysta z modelu projektu aplikacji sieci Web i który korzysta z modelu projekt witryny sieci Web.

Używa aplikacji sieci web zaktualizowane przeglądy książki [programu SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) bazy danych, który jest przechowywany w witrynie s `App_Data` folder (`~/App_Data/Reviews.mdf`). Jeśli masz zainstalowany na komputerze, a następnie pokaz powinny być uruchamiane bez błędów programu SQL Server 2008. Jeśli masz starszą wersję programu SQL Server można zainstalować wolnego programu SQL Server 2008 Express Edition lub skorzystać z bazy danych, Pobierz skrypty dostępne w tym samouczku s samodzielnie utworzyć bazę danych.

`Reviews.mdf` Baza danych zawiera cztery tabele:

- `Genres`-zawiera rekord dla każdego rodzaju, takich jak technologię, Fikcja i biznesowych.
- `Books`-zawiera rekord dla każdego przeglądu z kolumnami, takich jak `Title`, `GenreId`, `ReviewDate`, i `Review`, między innymi.
- `Authors`— zawiera informacje na temat każdego Autor, który przyczynił się je przejrzeć książce.
- `BooksAuthors`-tabeli sprzężenia wiele do wielu, która określa, jakie autorów napisano książek.
  

Rysunek 1 zawiera diagram ER z tych czterech tabel.


[![S aplikacji sieci Web przeglądami książki bazy danych jest składającej się z czterech tabel](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Rysunek 1**: aplikacja sieci Web przeglądami książki s bazy danych jest składającej się z czterech tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image3.jpg))


Poprzedniej wersji witryny sieci Web, książki przeglądami ma osobnej stronie ASP.NET dla każdej księgi. Na przykład znaleziono strony o nazwie `~/Tech/TYASP35.aspx` który zawiera przegląd *nauczyć się ASP.NET 3.5 w ciągu 24 godzin*. Nowa wersja opartych na danych witryny sieci Web ma przeglądy przechowywane w bazie danych i jednej strony ASP.NET Review.aspx?ID=*bookId*, który zawiera przegląd książki określony. Ponadto istnieje Genre.aspx?ID=*genreId* strony zawierającej listę je przejrzeć książek określonego rodzaju.

Dane 2 i 3 Pokaż `Genre.aspx` i `Review.aspx` stron w akcji. Zanotuj adres URL na pasku adresu dla każdej strony. W rysunku 2 it s Genre.aspx? ID = c 85d164ba-1123-4 47-82a0-c8ec75de7e0e. Ponieważ jest 85d164ba-1123-4c47-82a0-c8ec75de7e0e `GenreId` wartość genre technologii i odczytuje nagłówek strony s "Technologii monitoruje" Lista punktowana wylicza przeglądy w lokacji, które są objęte tego rodzaju.


[![Strona Genre technologii](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Rysunek 2**: na stronie Genre technologii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image6.jpg))


[![Przegląd nauczyć się ASP.NET 3.5 w 24 godzin](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Rysunek 3**: Przegląd *nauczyć się ASP.NET 3.5 w ciągu 24 godzin* ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image9.jpg))


Aplikacja sieci web sprawdza książki także sekcji Administracja, w którym administratorzy mogą dodawać, edytować i Usuń genres, recenzje i tworzyć informacji. Obecnie każdy obiekt odwiedzający mogą uzyskać dostęp do sekcji administracji. W samouczku przyszłych możemy dodać obsługę kont użytkowników i tylko zezwala autoryzowanym użytkownikom do stron administracyjnych.

Jeśli możesz pobrać aplikację przeglądami książki należy należy pamiętać, że jej celem jest, aby zademonstrować, wdrażanie aplikacji opartych na danych. Nie wykazuje najlepszych rozwiązań, o ile jest to projekt aplikacji. Na przykład istnieje nie oddzielnego danych Access warstwy (DAL); strony ASP.NET komunikują się bezpośrednio z bazą danych za pomocą formantu SqlDataSource lub kod ADO.NET w ich klasy związane z kodem. Więcej informacji na temat tworzenia aplikacji opartych na danych przy użyciu architektury warstwowych przeglądać, można znaleźć w mojej [ *Praca z danymi* samouczki](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bazy danych na programowanie i produkcji

Po uruchomieniu Programowanie w aplikacji sieci web opartych na danych należy określić ciąg połączenia bazy danych, który udostępnia aplikacji szczegółowe informacje na temat połączenia z bazą danych. Określa parametry połączenia, między innymi, serwera bazy danych, nazwy bazy danych i informacji o zabezpieczeniach. Najczęściej używane przez aplikację podczas tworzenia bazy danych jest inny niż bazy danych używane podczas jego s w środowisku produkcyjnym. Istnieje wiele zalet używanie różnych baz danych do tworzenia i produkcji. ADAM t o innej bazy danych w sposób programowanie trzeba się martwić o przypadkowo modyfikacja lub usunięcie danych na żywo. Umożliwia również umieścić w danych testowych zastępczego lub wprowadzić przełomowe zmiany do modelu danych, nie martwiąc się o skutki dla aplikacji w środowisku produkcyjnym. Wadą interfejsu o innej bazy danych w środowisku projektowania i produkcji, gdy aplikacja jest wdrożona bazy danych i wszelkie istotne zmiany do schematu bazy danych s lub danych musi być także wdrożony.

Przed ich pierwszym wdrożeniu istnieje tylko jedno wystąpienie bazy danych, a to wystąpienie jest w środowisku programistycznym. W przypadku wdrażania aplikacji w środowisku produkcyjnym po raz pierwszy został muszą nie tylko skopiować się niezbędne pliki po stronie serwera i klienta, ale również skopiować bazę danych ze środowiska projektowego do środowiska produkcyjnego. Jest to, gdzie możemy autonomiczna prawo teraz z aplikacji sieci web przeglądami Book - bazy danych znajdują się w `App_Data` folder w naszym środowisku programistycznym nie ma jednak jeszcze został naciśnięty do środowiska produkcyjnego.

Po wdrożeniu aplikacji istnieją dwie kopie bazy danych. Miarę rozwoju aplikacji, można dodać nowe funkcje, wymagających zmiany do modelu danych (takich jak dodawanie nowej kolumny do istniejących tabel, wprowadzania zmian do istniejących kolumn, dodając nowe tabele i tak dalej). Gdy obok wdrożonej aplikacji sieci web, zmiany zastosowane do bazy danych w środowisku programistycznym od czasu ostatniego wdrożenia należy zastosować w produkcyjnej bazie danych. W przyszłych samouczek omówiono kilka strategii zarządzania tego procesu. Ten samouczek koncentruje się na temat wdrażania całą bazę danych ze środowiska projektowego do środowiska produkcyjnego.

## <a name="deploying-the-database-to-the-production-environment"></a>Wdrożenie bazy danych do środowiska produkcyjnego

W pozostałej części tego samouczka analizuje sposobu wdrażania bazy danych z Środowisko deweloperskie do środowiska produkcyjnego. Jeśli wykonujesz wzdłuż możesz należy się upewnić, że Twoje konto u dostawcy usługi hosta sieci web zawiera programu Microsoft SQL Server baza danych obsługuje. Należy również mieć pewne informacje, a mianowicie nazwa serwera bazy danych, nazwy bazy danych oraz nazwy użytkownika i hasło używane do łączenia z bazą danych.

Jak wspomniano wcześniej w tym samouczku, bazy danych witryny sieci Web s przeglądami książki jest przechowywane w bazie danych programu SQL Server 2008 Express Edition `App_Data` folderu. Będzie znajdować się na przyczyny, że wdrożenie bazy danych jest tak proste, jak kopiowanie `App_Data` folderu środowiska programowania do środowiska produkcyjnego. Jednak większość dostawców usług hosta sieci web nie obsługuje hostingu baz danych w `App_Data` folderu z powodu ze względów bezpieczeństwa. Zamiast tego hosty sieci web podać konto, na serwerze bazy danych programu SQL Server w ich środowisku. Wdrożenie bazy danych z środowiska deweloperskiego do środowiska produkcyjnego wymaga bazy danych zarejestrowany na serwerze sieci web hosta s bazy danych.

W jaki sposób otrzymujesz bazy danych ze środowiska projektowego do środowiska produkcyjnego? Istnieje kilka sposobów, aby to zrobić, w zależności od tego, jakie usług oferty hosta sieci web. Z niektóre hosty, na przykład DiscountASP.NET, można FTP kopii zapasowej bazy danych lub rzeczywiste `.mdf` plików do witryny sieci Web, a następnie z poziomu Panelu sterowania, Przywróć plik kopii zapasowej lub Dołącz `.mdf` pliku na serwerze bazy danych programu SQL Server. Z tych narzędzi wdrażania bazy danych jest tak proste, jak kopiowanie `App_Data` folderu w środowisku produkcyjnym i dołączenie go za pomocą Panelu sterowania. Jest to prawdopodobnie najłatwiejszy i najszybszy sposób publikowania bazy danych po raz pierwszy.

Innym rozwiązaniem jest użycie Kreatora publikowania bazy danych. Kreator publikowania bazy danych jest generowany poleceń SQL, aby utworzyć schemat s bazy danych — tabele, procedury składowane, widoki, funkcje zdefiniowane przez użytkownika i tak dalej - i, opcjonalnie, dane w tabelach jego aplikacji pulpitu systemu Windows. Można następnie połączenie z serwerem sieci web hosta dostawcy s bazy danych za pomocą programu SQL Server Management Studio, a następnie wykonaj tego skryptu, które mają zostać zduplikowane bazy danych w środowisku produkcyjnym. Jeszcze bardziej poprawić jakość, jeśli dostawca hosta sieci web obsługuje Microsoft s [usługi publikowania bazy danych](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) może mieć skryptu generowane przez Kreatora publikowania bazy danych, automatycznie są wykonywane na serwerze bazy danych w Twoim imieniu. Ponieważ Kreator publikowania bazy danych generuje skrypt, który tworzy danych i schemat bazy danych s, będzie ona działać niezależnie od tego, czy dostawcy usługi hosta sieci web udostępnia funkcje, takie jak dołączanie przekazane `.mdf` pliku.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generowanie poleceń SQL, aby utworzyć schemat bazy danych i danych za pomocą Kreatora publikacji bazy danych

Let s przeprowadzenie wdrażania przeglądami książki bazy danych w środowisku produkcyjnym za pomocą Kreatora publikowania bazy danych. Jeśli używasz programu Visual Studio 2008 lub poza Kreator publikowania bazy danych jest już zainstalowana. Jeśli używasz programu Visual Studio 2005, musisz najpierw [Pobierz i zainstaluj](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) kreatora.

Otwórz program Visual Studio i przejdź do `Reviews.mdf` bazy danych. Jeśli używasz programu Visual Web Developer, przejdź do Eksploratora bazy danych; Jeśli używasz programu Visual Studio, skorzystaj z Eksploratora serwera. Na rysunku 4 przedstawiono `Reviews.mdf` bazy danych w Eksploratorze bazy danych w Visual Web Developer. Jak pokazano na rysunku 4, `Reviews.mdf` bazy danych składa się z czterech tabel, trzech procedur składowanych i funkcji zdefiniowanych przez użytkownika.


[![Zlokalizuj bazę danych w Eksploratorze bazy danych lub Eksploratora serwera](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Rysunek 4**: Zlokalizuj bazę danych w Eksploratorze bazy danych lub Eksploratora serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image12.jpg))


Kliknij prawym przyciskiem myszy nazwę bazy danych i wybierz opcję "Publikuj do dostawcy" z menu kontekstowego. Spowoduje to uruchomienie Kreatora publikowania bazy danych (zobacz rysunek 5). Kliknij przycisk Dalej wcześniejszym poza ekranu powitalnego.


[![Ekran powitalny Kreatora publikowania bazy danych](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Rysunek 5**: ekran powitalny Kreatora publikowania bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image15.jpg))


Drugi ekran Kreatora listy baz danych dostępnych do Kreatora publikowania bazy danych i umożliwia wybranie, czy skrypt wszystkich obiektów w wybranej bazy danych i wybierz obiekty, które do skryptu. Wybierz odpowiednią bazę danych i pozostaw zaznaczoną opcją "Skrypt wszystkie obiekty w wybranej bazy danych".

> [!NOTE]
> Jeśli zostanie wyświetlony błąd "nie ma żadnych obiektów w bazie danych *databaseName* typów skryptowe przez tego kreatora" po kliknięciu przycisku Dalej na ekranie przedstawiono na rysunku 6, upewnij się, że ścieżka do pliku bazy danych nie jest zbyt długa. Wykryto, że tego błędu mogą wystąpić, jeśli ścieżka do pliku bazy danych jest za długa.


[![Ekran powitalny Kreatora publikowania bazy danych](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Rysunek 6**: ekran powitalny Kreatora publikowania bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image18.jpg))


Z następnym ekranie można wygenerować plik skryptu lub, jeśli host sieci web obsługuje, publikowanie bazy danych bezpośrednio do serwera sieci web hosta dostawcy s bazy danych. Jak pokazano na rysunku 7, występują zapisywane do pliku skryptu `C:\REVIEWS.MDF.sql`.


[![Bazy danych do pliku skryptu lub opublikować go bezpośrednio Your dostawcy hosta sieci Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Rysunek 7**: skryptu bazy danych do pliku lub opublikować go bezpośrednio Your dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image21.jpg))


Kolejne ekranu wyświetli monit o podanie różne opcje obsługi skryptów. Można określić, czy skrypt ma uwzględniać upuszczania instrukcji, aby usuwać te istniejących obiektów. Domyślnie na wartość True, która jest poprawnie podczas wdrażania bazy danych po raz pierwszy. Można również określić, czy docelowa baza danych programu SQL Server 2000, SQL Server 2005 lub SQL Server 2008. Na koniec można wskazać, czy skrypt: schemat i dane, tylko te dane, lub tylko schemat. Schemat jest kolekcją obiektów bazy danych, tabel, procedur składowanych, widoków i tak dalej. Dane są informacji znajdujących się w tabelach.

Jak pokazano w rysunku 8, I wystąpił kreatora skonfigurowany tak, aby porzucić istniejące obiekty bazy danych, do wygenerowania skryptu bazy danych programu SQL Server 2008 oraz publikowania schemat i dane.


[![Określ publikowania opcje](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Rysunek 8**: Określ opcje publikowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image24.jpg))


Końcowe obu ekranach podsumowanie działań, które mają zostać podjęte, a następnie Wyświetl stan skryptów. Wynikiem kreatora jest firma Microsoft plik skryptu, który zawiera potrzebne do utworzenia bazy danych w środowisku produkcyjnym i wypełnić je z tymi samymi danymi na programowanie polecenia SQL.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Wykonywanie polecenia SQL na bazie środowiska produkcyjnego

Teraz, gdy mamy skrypt, który zawiera poleceń SQL, aby utworzyć bazę danych i jego dane wszystkich i który pozostaje jest można wykonać skryptu w produkcyjnej bazie danych. Niektóre dostawców usług hosta sieci web oferuje pole tekstowe w Panelu sterowania, ich wprowadzane polecenia SQL do wykonania na bazie danych. Jeśli masz bardzo dużych skryptu, ta opcja może nie działać ( `REVIEWS.MDF.sql` pliku skryptu jest ponad 425 KB rozmiaru, na przykład).

Lepszym rozwiązaniem jest bezpośrednie łączenie się z produkcyjnym serwera bazy danych przy użyciu programu SQL Server Management Studio (SSMS). Jeśli masz nie - Express wersji programu SQL Server zainstalowana na danym komputerze następnie prawdopodobnie masz już zainstalowany w programie SSMS. W przeciwnym razie można [Pobierz i zainstaluj](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) uzyskać bezpłatną kopię programu SQL Server Management Studio Express Edition.

Uruchom narzędzia SSMS i Połącz z serwerem sieci web hosta s bazy danych przy użyciu informacji dostarczonych przez dostawcę hosta sieci web.


[![Połączenie z serwerem sieci Web hosta dostawcy s bazy danych](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Rysunek 9**: nawiązania s Your dostawcy hosta sieci Web serwera bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image27.jpg))


Rozwiń kartę baz danych i Znajdź bazy danych. Kliknij przycisk Nowa kwerenda w lewym górnym rogu paska narzędzi, Wklej polecenia SQL z pliku skryptu utworzonego przez Kreatora publikowania bazy danych i kliknij przycisk Execute do uruchamiania tych poleceń na serwerze bazy danych w środowisku produkcyjnym. Jeśli plik skryptu jest szczególnie duże go może potrwać kilka minut, można wykonać polecenia.


[![Połączenie z serwerem sieci Web hosta dostawcy s bazy danych](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Na rysunku nr 10**: nawiązania s Your dostawcy hosta sieci Web serwera bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image30.jpg))


Wszystkie dostępne tego s jest! W tym momencie projektowej bazie danych został zduplikowany w środowisku produkcyjnym. Jeśli odświeżania bazy danych w programie SSMS powinno być widoczne nowe obiekty bazy danych. Rysunek 11 zawiera tabele bazy danych s produkcji, procedury składowane i funkcje zdefiniowane przez użytkownika, które dublują tymi w projektowej bazie danych. I ponieważ firma Microsoft instrukcją Kreatora publikowania bazy danych, aby opublikować dane, s tabel bazy danych produkcyjnych tych samych danych co tabel s programowania bazy danych w czasie, które zostało wykonane przez kreatora. Rysunek 12 zawiera dane w `Books` tabeli w produkcyjnej bazie danych.


[![Obiekty bazy danych ma został zduplikowany w produkcyjnej bazie danych](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Rysunek 11**: baza danych obiekty mają został zduplikowany w produkcyjnej bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image33.jpg))


[![Baza danych produkcyjnych zawiera tych samych danych, co w projektowej bazie danych](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Rysunek 12**: produkcyjnej bazy danych zawiera tych samych danych w projektowej bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image36.jpg))


Na tym etapie firma Microsoft została wdrożona tylko projektowej bazie danych do środowiska produkcyjnego. Firma Microsoft nie zostały jeszcze przeglądał wdrażanie aplikacji sieci web lub zbadać, jakie zmiany konfiguracji są wymagane do aplikacji w środowisku produkcyjnym Użyj produkcyjną bazę danych. Te problemy omówione zostaną następujące czynności w następnym samouczku!

## <a name="summary"></a>Podsumowanie

Wdrażanie aplikacji sieci web opartych na danych wymaga kopiowanie bazy danych używane podczas tworzenia do środowiska produkcyjnego. Wielu dostawców usług hosta sieci web oferuje narzędzia w celu uproszczenia procesu wdrażania bazy danych. Na przykład z DiscountASP.NET można FTP bazy danych `.mdf` pliku (lub kopii zapasowej), a następnie dołączyć bazy danych na serwerze bazy danych z poziomu Panelu sterowania. Inną opcją, która działa niezależnie od tego, jakie funkcje oferty dostawcy hosta sieci web jest s narzędzia Kreator publikowania bazy danych firmy Microsoft, które generuje skrypt poleceń SQL, aby utworzyć programowanie bazy danych s schemat i dane. Po wygenerowaniu tego skryptu można ją wykonać na produkcyjną bazę danych.

Teraz, recenzje książki sieci web aplikacji s baza danych jest w środowisku produkcyjnym możemy wdrożyć aplikację. Informacje o konfiguracji sieci web aplikacji s określa parametry połączenia z bazą danych i projektowej bazie danych odwołuje się do tego ciągu połączenia. Należy zaktualizować tego ciągu połączenia w przypadku wdrażania lokacji do środowiska produkcyjnego. Następny samouczek przegląda tych różnic konfiguracji i przeprowadzi Cię przez kroki niezbędne do publikowania witryny przeglądami książki opartych na danych w środowisku produkcyjnym.

Programowanie przyjemność!

#### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Pobierz bazy danych programu Microsoft SQL Server 1.1 Kreator publikacji](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Pobierz program Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

>[!div class="step-by-step"]
[Poprzednie](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
[dalej](configuring-the-production-web-application-to-use-the-production-database-cs.md)
