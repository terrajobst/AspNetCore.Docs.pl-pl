---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: Wdrażanie bazy danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Wdrażanie aplikacji sieci web ASP.NET pociąga za sobą pobieranie niezbędnych plików i zasobów środowiska programowania do środowiska produkcyjnego. Dla aplikacji rozproszonej...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dc5e9b4e189929b2b898b997b7577a623bdc8a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381325"
---
<a name="deploying-a-database-vb"></a>Wdrażanie bazy danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> Wdrażanie aplikacji sieci web ASP.NET pociąga za sobą pobieranie niezbędnych plików i zasobów środowiska programowania do środowiska produkcyjnego. W przypadku aplikacji sieci web opartej na danych w tym schemat bazy danych i danych. W tym samouczku jest to pierwszy z serii, który pokazuje kroków niezbędnych do pomyślnego wdrożenia bazy danych ze środowiska projektowego w środowisku produkcyjnym.


### <a name="introduction"></a>Wprowadzenie

Wdrażanie aplikacji sieci web ASP.NET pociąga za sobą pobieranie niezbędnych plików i zasobów środowiska programowania do środowiska produkcyjnego. W ciągu ostatnich sześciu samouczki zobaczyliśmy, wdrażając prostą aplikację sieci web przeglądy książki. Tej wersji demonstracyjnej witryny zostało obejmuje wiele zasobów po stronie serwera — strony ASP.NET, pliki konfiguracji `Web.sitemap` pliku i tak dalej — wraz z zasobów po stronie klienta, takich jak obrazy i pliki CSS. Ale co opartych na danych aplikacje sieci web? Jakie dodatkowe kroki należy podjąć w taki sposób, aby wdrożyć aplikację sieci web, która korzysta z bazy danych?

Za pośrednictwem dalej kilka samouczków zajmujemy kroki niezbędne do wdrożenia aplikacji sieci web opartej na danych. Ten samouczek rozpoczyna się przez badanie sposobu uzyskania s schemat bazy danych i zawartość ze środowiska projektowego do środowiska produkcyjnego, podczas kolejnych samouczek analizuje zmiany konfiguracji potrzebne. Po pokażemy wyzwaniom wdrażania bazy danych, która korzysta z usług aplikacji (członkostwo, role, profilu i tak dalej).

## <a name="examining-the-updated-book-reviews-web-application"></a>Badanie aplikacji sieci Web przeglądy zaktualizowane książki

W celu przedstawienia wdrażanie aplikacji sieci web opartej na danych, I Zapisz zaktualizowany przeglądy książki aplikacji sieci web z prostego, statycznej witryny internetowej opartej na danych niż ten, który. Jak poprzednio, istnieją dwie wersje aplikacji, w tym samouczku s: jedną, która używa modelu projektu aplikacji sieci Web i jedną, która używa modelu projektu witryny sieci Web.

Zaktualizowano przeglądy książki sieci web używa aplikacji [programu SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) bazy danych, który jest przechowywany w witrynie s `App_Data` folder (`~/App_Data/Reviews.mdf`). Jeśli masz zainstalowany na komputerze, a następnie wersję demonstracyjną powinny być uruchamiane bez błędów programu SQL Server 2008. Jeśli masz starszą wersję programu SQL Server, można kliknąć przycisk Zainstaluj bezpłatny program SQL Server 2008 Express Edition lub można użyć bazy danych dostępnych w ramach tego samouczka s skryptów pobieranie samodzielnie utworzyć bazę danych.

`Reviews.mdf` Baza danych zawiera cztery tabele:

- `Genres` -zawiera rekord dla każdego gatunku, takich jak technologia Fikcja i biznesowych.
- `Books` -zawiera rekord dla każdej recenzji, za pomocą kolumny, takie jak `Title`, `GenreId`, `ReviewDate`, i `Review`, między innymi.
- `Authors` — zawiera informacje na temat każdego autora, który przyczynił się do przeglądu książki.
- `BooksAuthors` -tabelę sprzężenia wiele do wielu, która określa, jakie autorzy zostały napisane książek.
  

Rysunek 1 zawiera diagram ER te cztery tabele.


[![S aplikacji sieci Web przeglądy książki bazy danych jest składa się z czterech tabel](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**Rysunek 1**: aplikacja sieci Web przeglądy książki bazy danych jest składa się z czterech tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image3.jpg))


Poprzedniej wersji witryny sieci Web, książki, przeglądy ma osobnej strony ASP.NET dla każdej książki. Na przykład podczas stronę o nazwie `~/Tech/TYASP35.aspx` znajdujących się przegląd dla *uczyć się ASP.NET 3.5 w ciągu 24 godzin*. Nowa wersja opartych na danych w witrynie sieci Web ma przeglądy przechowywanych w bazie danych i jednej strony ASP.NET Review.aspx?ID=*bookId*, który zawiera przegląd dla określonej książki. Podobnie, brak Genre.aspx?ID=*genreId* strony zawierającej listę przeglądu książki określonego rodzaju.

Dane 2 i 3 show `Genre.aspx` i `Review.aspx` stron w działaniu. Zanotuj adres URL na pasku adresu dla każdej strony. W elemencie rysunek 2 it s Genre.aspx? ID = c 85d164ba 1123 4 47-82a0-c8ec75de7e0e. Ponieważ 85d164ba-1123-4c47-82a0-c8ec75de7e0e `GenreId` wartość gatunku technologii i odczytuje nagłówek strony s "Technologii przeglądy" listy punktowanej wylicza przeglądy w witrynie, mieszczące się w ramach tego gatunku.


[![Na stronie gatunku technologii](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**Rysunek 2**: na stronie gatunku technologii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image6.jpg))


[![Przegląd dla prowadzenia ASP.NET 3.5 w 24 godziny](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**Rysunek 3**: Przegląd dla *uczyć się ASP.NET 3.5 w ciągu 24 godzin* ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image9.jpg))


Aplikacja sieci web książki przeglądów zawiera także sekcji Administracja, w którym administratorzy mogą dodawać, edytować i Usuń gatunki, przeglądy i tworzyć informacji. Obecnie wszystkie osoby odwiedzającej można uzyskać dostęp do sekcji Administracja. W przyszłości samouczku utworzymy dodanie obsługi dla kont użytkowników i tylko zezwala autoryzowanym użytkownikom do stron administracji.

Jeśli pobierzesz aplikację przeglądy książki, należy pamiętać, że jego celem jest, aby zademonstrować, wdrażanie aplikacji opartych na danych. Nie jest zauważalna podczas najlepsze rozwiązania z zakresu projektowania aplikacji. Na przykład nie ma żadnych oddzielnych danych dostęp do warstwy (DAL); strony ASP.NET komunikują się bezpośrednio z bazą danych przy użyciu kontrolki SqlDataSource lub kodu ADO.NET w ich klasy CodeBehind. Aby uzyskać bardziej szczegółowe omówienie procesu tworzenia aplikacji opartych na danych, przy użyciu architektury warstwowej, zobacz mój [ *Praca z danymi* samouczki](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bazy danych na rozwoju i produkcji

Po uruchomieniu rozwoju w aplikacji sieci web opartej na danych, należy określić ciąg połączenia bazy danych, który zawiera szczegóły aplikacji dotyczące sposobu łączenia z bazą danych. Ten ciąg połączeń określa między innymi, serwer bazy danych, nazwę bazy danych oraz informacje o zabezpieczeniach. Najczęściej używanych przez aplikację podczas tworzenia bazy danych jest inny niż bazy danych używane podczas jego s w środowisku produkcyjnym. Ma wiele zalet dla rozwoju i produkcji przy użyciu różnych baz danych. O różnych baz danych w rozwoju oznacza don t musisz martwić się o przypadkowe modyfikowania i usuwania danych na żywo. Umożliwia także umieścić w danych testowych fikcyjnego lub wprowadzić przełomowe zmiany w modelu danych bez konieczności martwienia się o wpływ na tę aplikację w środowisku produkcyjnym. Wadą o innej bazy danych w środowiskach deweloperskich i produkcyjnych jest to, że gdy aplikacja jest wdrożona bazy danych i wszystkie istotne zmiany schematu bazy danych s lub danych musi być także wdrożony.

Przed pierwszym wdrożeniem jest tylko jedno wystąpienie bazy danych, a to wystąpienie znajduje się w środowisku programistycznym. Podczas wdrażania aplikacji w środowisku produkcyjnym po raz pierwszy firma Microsoft musi nie tylko kopiować się niezbędne pliki po stronie serwera i klienta, ale również skopiować bazę danych ze środowiska projektowego w środowisku produkcyjnym. Jest to, gdzie możemy pozostawić odpowiednie teraz z aplikacją sieci web przeglądy książki — baza danych znajduje się w `App_Data` folderu w naszym środowisku programistycznym nie został jednak jeszcze przesunięte w do środowiska produkcyjnego.

Po wdrożeniu aplikacji istnieją dwie kopie bazy danych. Jak dojrzewa aplikacji, można dodać nowe funkcje, przez co konieczna zmiana modelu danych (takich jak dodawanie nowych kolumn do istniejących tabel, wprowadzania zmian w istniejących kolumn, dodając nowe tabele i tak dalej). Gdy następnie wdrożono aplikację sieci web, zmiany zastosowane do bazy danych w środowisku programistycznym od czasu ostatniego wdrożenia muszą być stosowane do produkcyjnej bazy danych. Kilka strategii zarządzania tego procesu są omówione w przyszłości zapoznać się z samouczkiem. Ten samouczek koncentruje się na temat wdrażania całej bazy danych ze środowiska projektowego do środowiska produkcyjnego.

## <a name="deploying-the-database-to-the-production-environment"></a>Wdrażanie bazy danych w środowisku produkcyjnym

W pozostałej części tego samouczka patrzy na sposób wdrażania bazy danych ze środowiska projektowego w środowisku produkcyjnym. Jeśli wykonujesz wzdłuż możesz należy się upewnić, że Twoje konto za pomocą dostawcy hosta sieci web zawiera program Microsoft SQL Server database obsługuje. Należy również mieć pewne informacje, a mianowicie nazwa serwera bazy danych, nazwę bazy danych i nazwę użytkownika i hasło używane do łączenia z bazą danych.

Jak wspomniano wcześniej w tym samouczku, bazy danych witryny sieci Web s przeglądy książki jest przechowywane w bazie danych programu SQL Server 2008 Express Edition `App_Data` folderu. Będzie ona autonomiczna do przyczyny, że wdrażanie bazy danych będzie proste i polega na kopiowaniu `App_Data` folderu ze środowiska projektowego do środowiska produkcyjnego. Jednak większość dostawców usług hosta sieci web nie obsługują hostowania baz danych w `App_Data` folderu z powodów bezpieczeństwa. Zamiast tego hosty sieci web podać konto na serwerze bazy danych programu SQL Server w ich środowisku. Wdrażanie bazy danych w środowisku deweloperskim, do środowiska produkcyjnego wymaga bazy danych zarejestrowany na serwerze sieci web hosta s bazy danych.

W jaki sposób otrzymujesz bazy danych ze środowiska projektowego w środowisku produkcyjnym? Istnieje kilka sposobów, w tym celu w zależności od tego, jakich usług ofert hosta sieci web. Za pomocą niektóre hosty, na przykład DiscountASP.NET, można FTP kopii zapasowej bazy danych lub rzeczywistego `.mdf` plik do witryny sieci Web i następnie w Panelu sterowania, Przywróć plik kopii zapasowej lub dołączyć `.mdf` plików na serwerze bazy danych programu SQL Server. Za pomocą takich narzędzi wdrażania bazy danych jest równie proste jak kopiowanie `App_Data` folder w środowisku produkcyjnym i dołączenie go za pośrednictwem Panelu sterowania. Być może jest to najprostszy i najszybszy sposób Publikuj swoją bazę danych po raz pierwszy.

Innym rozwiązaniem jest użycie Kreatora publikowania bazy danych. Kreator publikowania bazy danych jest Windows aplikacja komputerowa, która spowoduje wygenerowanie polecenia SQL, aby utworzyć schemat bazy danych s - tabele, procedury składowane, widoki, funkcje zdefiniowane przez użytkownika i tak dalej - i, opcjonalnie, dane w jego tabel. Można następnie połączenie z serwerem sieci web hosta dostawcy s bazy danych programu SQL Server Management Studio, a następnie wykonaj ten skrypt, aby zduplikować bazy danych w środowisku produkcyjnym. Jeszcze lepsza, jeśli Twój dostawca hosta sieci web obsługuje Microsoft s [usługi publikowania bazy danych](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) może mieć skrypt wygenerowany przez Kreator publikowania bazy danych, automatycznie są wykonywane na serwerze bazy danych w Twoim imieniu. Ponieważ Database Publishing Wizard generuje skrypt, który tworzy bazę danych s schematu i danych, będzie ona działać niezależnie od tego, czy Twój dostawca hosta sieci web oferuje funkcje, takie jak dołączanie przekazanych `.mdf` pliku.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generowanie poleceń SQL, aby utworzyć schemat bazy danych i danych przy użyciu bazy danych, Kreator publikacji

Pozwól s przeprowadzenie wdrażania bazy danych przeglądy książki w środowisku produkcyjnym za pomocą Kreatora publikowania bazy danych. Jeśli używasz programu Visual Studio 2008 lub nowszych, Kreator publikowania bazy danych jest już zainstalowana. Jeśli używasz programu Visual Studio 2005, wówczas konieczne będzie pierwszy [Pobierz i zainstaluj](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) kreatora.

Otwórz program Visual Studio i przejdź do `Reviews.mdf` bazy danych. Jeśli używasz Visual Web Developer, przejdź do Eksploratora bazy danych; Jeśli używasz programu Visual Studio, skorzystaj z Eksploratora serwera. Rysunek 4 przedstawia `Reviews.mdf` bazy danych w Eksploratorze bazy danych w Visual Web Developer. Jak pokazano na rysunku 4, `Reviews.mdf` bazy danych składa się z czterech tabel, trzech procedur składowanych i funkcji zdefiniowanych przez użytkownika.


[![Zlokalizuj bazę danych w Eksploratorze bazy danych lub w Eksploratorze serwera](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**Rysunek 4**: Zlokalizuj bazę danych w Eksploratorze bazy danych lub w Eksploratorze serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image12.jpg))


Kliknij prawym przyciskiem myszy nazwę bazy danych, a następnie wybierz opcję "Publikuj do dostawcy" z menu kontekstowego. Spowoduje to uruchomienie Kreatora publikowania bazy danych (zobacz rysunek 5). Kliknij obok wcześniejszym ekran powitalny.


[![Ekran powitalny Kreatora publikowania bazy danych](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**Rysunek 5**: ekran powitalny Kreatora publikowania bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image15.jpg))


Drugi ekran kreatora zawiera listę baz danych dostępne dla Kreatora publikowania bazy danych i umożliwia wybranie, czy wszystkie obiekty w wybranej bazie danych skryptu lub wybrać obiekty, które do skryptu. Wybierz odpowiednią bazę danych i pozostaw zaznaczoną opcją "Skrypt wszystkie obiekty w wybranej bazie danych".

> [!NOTE]
> Jeśli zostanie wyświetlony błąd "nie ma żadnych obiektów w bazie danych *databaseName* typów za pomocą skryptów przez tego kreatora" po kliknięciu przycisku Dalej na ekranie pokazano na rysunku 6, upewnij się, że ścieżka do pliku bazy danych nie jest zbyt długa. Jak wspomniano w [ten element dyskusji](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014) na stronie projektu Kreator publikowania bazy danych, ten błąd może wystąpić, jeśli ścieżka do pliku bazy danych jest za długa.


[![Ekran powitalny Kreatora publikowania bazy danych](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**Rysunek 6**: ekran powitalny Kreatora publikowania bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image18.jpg))


Z następnego ekranu można wygenerować plik skryptu lub, jeśli hosta sieci web obsługuje, opublikować bazę danych bezpośrednio na serwerze sieci web hosta dostawcy s bazy danych. Jak pokazano na rysunku 7, mam skryptu zapisywane w pliku `C:\REVIEWS.MDF.sql`.


[![Skrypt bazy danych do pliku lub opublikować ją bezpośrednio na Twój dostawca hosta sieci Web](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**Rysunek 7**: skrypt bazy danych do pliku lub opublikować ją bezpośrednio na Twój dostawca hosta sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image21.jpg))


Kolejne ekran jest wyświetlany monit dla różnych opcji obsługi skryptów. Można określić, czy skrypt powinien zawierać instrukcje usuwania, aby usunąć te istniejących obiektów. To wartość domyślna to True, co jest dobrym rozwiązaniem w przypadku wdrażania bazy danych po raz pierwszy. Można również określić, czy docelowa baza danych programu SQL Server 2000, SQL Server 2005 lub SQL Server 2008. Na koniec można wskazać, czy do skryptu schemat i dane, po prostu dane lub po prostu schematu. Schemat jest kolekcją obiektów bazy danych, tabel, procedur składowanych, widoków i tak dalej. Dane są informacje znajdujące się w tabelach.

Tak jak pokazano w rysunek 8 I ve stało się Kreator skonfigurowany tak, aby porzucić istniejące obiekty bazy danych, można wygenerować skryptu dla bazy danych programu SQL Server 2008 i opublikować schematu i danych.


[![Podczas publikowania Określ opcje](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**Rysunek 8**: Określ opcje publikowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image24.jpg))


Końcowe dwa ekrany podsumowanie akcje, które zamierzasz podjęte, a następnie Wyświetl stan obsługę skryptów. Wynikiem działania kreatora to, że mamy plik skryptu, który zawiera polecenia SQL potrzebne do utworzenia bazy danych w środowisku produkcyjnym i wypełnić ją przy użyciu tych samych danych na rozwój.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Wykonywanie poleceń SQL w bazie danych środowiska produkcyjnego

Skoro mamy już skrypt, który zawiera polecenia SQL, aby utworzyć bazę danych i jej dane wszystkich i nadal będzie to można wykonać skryptu na produkcyjnej bazy danych. Niektórzy dostawcy hosta sieci web oferują pole tekstowe w ich Panelu sterowania, w którym możesz wprowadzić poleceń SQL do wykonania w bazie danych. Jeśli masz plik skryptu bardzo duże, wówczas ta opcja może nie działać ( `REVIEWS.MDF.sql` pliku skryptu jest ponad 425 KB rozmiaru, na przykład).

Lepszym rozwiązaniem jest aby bezpośrednio połączyć się z serwera bazy danych produkcyjnych przy użyciu programu SQL Server Management Studio (SSMS). Jeśli masz inne niż - Express programu SQL Server zainstalowana na danym komputerze następnie prawdopodobnie masz już zainstalowany program SSMS. W przeciwnym razie możesz [Pobierz i zainstaluj](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) uzyskać bezpłatną kopię programu SQL Server Management Studio Express Edition.

Uruchom program SSMS i nawiązać połączenie z serwerem bazy danych s hosta sieci web korzystając z informacji podanych przez dostawcę hosta sieci web.


[![Łączenie się z serwerem sieci Web hosta dostawcy s bazy danych](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**Rysunek 9**: połączyć się z usługi sieci Web hosta dostawcy s serwera bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image27.jpg))


Rozwiń kartę baz danych i Znajdź bazy danych. Kliknij przycisk Nowe zapytanie w lewym górnym rogu paska narzędzi, Wklej poleceń SQL z pliku skryptu, tworzone przez Kreatora publikowania bazy danych i kliknij przycisk Execute, do uruchamiania tych poleceń na serwerze bazy danych w środowisku produkcyjnym. Jeśli plik skryptu jest szczególnie dużych go może potrwać kilka minut można wykonać polecenia.


[![Łączenie się z serwerem sieci Web hosta dostawcy s bazy danych](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**Na rysunku nr 10**: połączyć się z usługi sieci Web hosta dostawcy s serwera bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image30.jpg))


Wszystkie dostępne tego s jest! W tym momencie rozwoju bazy danych został zduplikowany w środowisku produkcyjnym. Jeśli odświeżanie bazy danych w programie SSMS powinny pojawić się nowe obiekty bazy danych. Na ilustracji 11 pokazano tabel bazy danych s produkcyjnych, procedur składowanych i funkcji zdefiniowanych przez użytkownika, które takie same jak stosowane w bazie danych rozwoju. A ponieważ możemy nakazał Kreator publikowania bazy danych, aby opublikować dane, tabele s produkcyjne bazy danych ma te same dane jako tabele s rozwoju bazy danych w czasie, który został wykonany przez kreatora. Rysunek 12 pokazuje dane w `Books` tabeli w bazie danych produkcyjnych.


[![Obiekty bazy danych ma został zduplikowany w produkcyjnej bazy danych](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**Rysunek 11**: bazy danych obiekty mają został zduplikowany w produkcyjnej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image33.jpg))


[![Produkcyjnej bazy danych zawiera te Same dane co w rozwoju bazy danych](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**Rysunek 12**: produkcyjnej bazy danych zawiera te Same dane ponieważ w bazie danych rozwoju ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-vb/_static/image36.jpg))


W tym momencie możemy wdrożona tylko rozwoju bazy danych do środowiska produkcyjnego. Firma Microsoft nie zostały jeszcze przyjrzano się wdrażanie aplikacji sieci web, samego lub sprawdzić, jakie zmiany konfiguracji są potrzebne do aplikacji w środowisku produkcyjnym, użyj produkcyjnej bazy danych. W następnym samouczku omówiono te problemy!

## <a name="summary"></a>Podsumowanie

Wdrażanie aplikacji sieci web opartej na danych wymaga kopiowania bazy danych używanej podczas programowania do środowiska produkcyjnego. Wielu dostawców usług hosta sieci web oferuje narzędzia w celu uproszczenia procesu wdrażania bazy danych. Na przykład za pomocą DiscountASP.NET można FTP bazy danych `.mdf` pliku (lub kopii zapasowej) i następnie dołączyć bazę danych na serwerze bazy danych z poziomu Panelu sterowania. Inną opcję, która działa niezależnie od tego, jakie funkcje ofert dostawcy hosta sieci web jest Microsoft s Database Publishing Wizard narzędzia, które generuje skrypt poleceń SQL, aby utworzyć schemat s bazy danych rozwoju i danych. Po wygenerowaniu ten skrypt można ją wykonać na produkcyjnej bazy danych.

Teraz, gdy przeglądy książki sieci web aplikacji s bazy danych znajduje się w produkcji możemy wdrożyć aplikację. Jednak informacje o konfiguracji sieci web aplikacji s określa parametry połączenia z bazą danych, a te parametry połączenia odwołuje się do rozwoju bazy danych. Musimy zaktualizować to informacje o parametrach połączenia, w przypadku wdrażania w witrynie do środowiska produkcyjnego. Następny samouczek patrzy na te różnice konfiguracji i opisuje etapy potrzebne do opublikowania witryny opartej na danych przeglądy książki do środowiska produkcyjnego.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Pobierz bazy danych programu Microsoft SQL Server 1.1 Kreator publikacji](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Pobierz program Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Poprzednie](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [dalej](configuring-the-production-web-application-to-use-the-production-database-vb.md)
