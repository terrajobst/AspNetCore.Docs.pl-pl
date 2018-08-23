---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Strategie projektowania baz danych i wdrażania (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas wdrażania aplikacji opartych na danych po raz pierwszy może skutkować nieświadomym skopiować bazę danych w środowisku programistycznym do środowiska produkcyjnego. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: e07da4b5263ac3c6db19c375ca00cbcf87e0b35a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756991"
---
<a name="strategies-for-database-development-and-deployment-vb"></a>Strategie projektowania baz danych i wdrażania (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Podczas wdrażania aplikacji opartych na danych po raz pierwszy może skutkować nieświadomym skopiować bazę danych w środowisku programistycznym do środowiska produkcyjnego. Ale wykonywanie blind kopiowania w przypadku kolejnych wdrożeń zastąpią wszelkie dane wprowadzane do produkcyjnej bazy danych. Zamiast tego wdrażania bazy danych polega na stosowaniu zmiany wprowadzone do tworzenia bazy danych od czasu ostatniego wdrażania na produkcyjną bazę danych. W tym samouczku sprawdza, czy te problemy i oferuje różne strategie uzyskanymi chronicling i stosowanie zmian wprowadzonych w bazie danych od czasu ostatniego wdrożenia.


## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w poprzednich samouczkach, wdrażanie aplikacji ASP.NET pociąga za sobą kopiowanie odpowiednie zawartości ze środowiska projektowego do środowiska produkcyjnego. Wdrożenia nie jest to operacja jednorazowa, ale raczej coś, co się dzieje w każdym wydaniu nowej wersji oprogramowania lub kiedy usterek lub obawy związane z bezpieczeństwem zostały zidentyfikowane i które zostały rozwiązane. Podczas kopiowania strony ASP.NET, obrazów, plików JavaScript i inne takie pliki do środowiska produkcyjnego, nie trzeba dotyczą samodzielnie z tych plików zostały zmienione od czasu ostatniego wdrożenia. Może skutkować nieświadomym skopiuj plik do środowiska produkcyjnego, zastępując istniejącą zawartość. Niestety ta uproszczenia nie obejmuje wdrażanie bazy danych.

Podczas wdrażania aplikacji opartych na danych po raz pierwszy może skutkować nieświadomym skopiować bazę danych w środowisku programistycznym do środowiska produkcyjnego. Ale wykonywanie blind kopiowania w przypadku kolejnych wdrożeń zastąpią wszelkie dane wprowadzane do produkcyjnej bazy danych. Zamiast tego wdrażania bazy danych polega na stosowaniu *zmiany* wprowadzone do tworzenia bazy danych od czasu ostatniego wdrażania na produkcyjną bazę danych. W tym samouczku sprawdza, czy te problemy i oferuje różne strategie uzyskanymi chronicling i stosowanie zmian wprowadzonych w bazie danych od czasu ostatniego wdrożenia.

## <a name="the-challenges-of-deploying-a-database"></a>Wyzwania związane z wdrażanie bazy danych

Zanim opartych na danych aplikacja została wdrożona po raz pierwszy, istnieje tylko jedna baza danych, a mianowicie bazy danych w środowisku deweloperskim, dlatego po wdrażanie aplikacji opartych na danych po raz pierwszy możesz bezrefleksyjne skopiować bazę danych w Środowisko deweloperskie do środowiska produkcyjnego. Jednak po wdrożeniu aplikacji są dwie kopie bazy danych: jeden w rozwoju, a drugi w środowisku produkcyjnym.

Między wdrożeniami deweloperskich i produkcyjnych baz danych może stać się zsynchronizowane. Schemat s bazy danych w środowisku produkcyjnym pozostaje bez zmian, schemat s rozwoju bazy danych mogą ulec zmianie w miarę dodawania nowych funkcji. Mogą dodać lub usunąć kolumny, tabele, widoki lub procedur składowanych. Może również być ważnych danych, który pobiera dodany do tworzenia bazy danych. Wiele aplikacji opartych na danych zawierają tabele odnośników wypełniony danymi zakodowane, specyficzne dla aplikacji, których nie można edytować użytkownika. Na przykład w witrynie sieci Web aukcji może mieć listy rozwijanej, opcje, które opisują warunkiem, że element jest poddane licytacji: nowy, takie jak nowe, dobre i uczciwego. Zamiast kodować te opcje bezpośrednio na liście rozwijanej jest zazwyczaj lepiej jest umieścić je w bazie danych. Jeśli podczas tworzenia aplikacji, o nazwie niska nowy warunek jest dodawane do tabeli następnie podczas wdrażania aplikacji to ten sam rekord musi zostać dodane do tabeli odnośników w produkcyjnej bazie danych.

W idealnym przypadku wdrażanie bazy danych obejmowałaby kopiowanie bazy danych, od projektowania do produkcji. Jednak należy pamiętać o tym, że po wdrożeniu aplikacji i wznowić rozwoju produkcyjnej bazy danych trwa wypełnianie dane od rzeczywistych użytkowników. W związku z tym gdyby po prostu skopiować bazę danych od projektowania do produkcji w następnym wdrożenia może zastąpić produkcyjnej bazy danych i utracone jego dane. Wynikiem jest, że wdrażanie bazy danych wrzała w dół w celu zastosowania zmian wprowadzonych w bazie danych rozwoju od czasu ostatniego wdrożenia.

Ponieważ wdrażanie bazy danych wymaga zastosowania zmian w schemacie i ewentualnie danych od czasu poprzedniego wdrożenia, historię zmian musi być utrzymywana (lub określane w czasie wdrażania) tak, aby te zmiany mogą być stosowane w środowisku produkcyjnym. Istnieją różne techniki do zarządzania i stosowania zmian do modelu danych.

### <a name="defining-the-baseline"></a>Definiowanie linii bazowej

Aby zachować zmiany do bazy danych s aplikacji, musisz mieć pewne początkowy stan punktu odniesienia, do której zmiany są stosowane do. W jednym extreme początkowy stan może być pustą bazę danych bez tabel, widoków i procedur składowanych. Plan bazowy wyników w dzienniku jest wprowadzenie znaczącej zmiany, ponieważ musi on zawierać tworzenia wszystkie tabele s bazy danych, widoków i procedur składowanych, wraz z wszelkich zmian wprowadzonych po początkowym wdrożeniu. Na drugim końcu spektrum można ustawić punktu odniesienia, co wersja bazy danych, która początkowo jest wdrożony w środowisku produkcyjnym. Ten wybór skutkuje dużo mniejsze dziennik zmian, ponieważ zawiera on tylko zmiany wprowadzone do bazy danych, zgodnie z pierwszym wdrożeniu. To podejście, które chcę. I oczywiście możesz wybrać więcej środka podejście drogi, definiowanie linii bazowej w pewnym momencie, między wstępne tworzenie bazy danych, a podczas pierwszego wdrożenia bazy danych.

Po utworzeniu wybrany punkt odniesienia należy wziąć pod uwagę Generowanie skryptu SQL, które mogą być wykonywane w celu ponownego utworzenia wersji linii bazowej. Utworzenia takiego skryptu umożliwia szybkie ponowne utworzenie bazy danych w wersji linii bazowej. Ta funkcja jest szczególnie przydatne w dużych projektach, których może istnieć wiele deweloperzy pracujący nad projektu lub dodatkowych środowisk, takich jak testowania i przemieszczania, należy każdy własnej kopii bazy danych.

Istnieją różne narzędzia do dyspozycji użytkownika można wygenerować skryptu SQL w wersji linii bazowej. Z programu SQL Server Management Studio (SSMS) kliknąć prawym przyciskiem myszy w bazie danych, przejdź do podmenu zadań i wybierz opcję generowania skryptów. Spowoduje to uruchomienie Kreatora skryptu, który możesz wydać polecenie, aby wygenerować plik, który zawiera polecenia SQL, aby utworzyć bazę danych z obiektów s. Innym rozwiązaniem jest Kreator publikowania bazy danych, który można wygenerować polecenia SQL, aby tworzyć nie tylko schemat bazy danych, ale dane w tabelach bazy danych. Kreator publikowania bazy danych został szczegółowo zbadane w *wdrażania bazy danych* samouczka. Niezależnie od tego, jakiego narzędzia używasz, na koniec powinny mieć plik skryptu, który można użyć w celu ponownego utworzenia bazy danych w wersji linii bazowej należy wystąpić.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentowanie zmian w bazie danych w Prozie

Najprostszym sposobem Obsługa dziennika zmian do modelu danych w fazie opracowywania jest zapisanie zmian w prozie. Na przykład, jeśli podczas tworzenia aplikacji już wdrożony Dodaj nową kolumnę, aby `Employees` tabeli, należy usunąć kolumny z `Orders` tabeli, a następnie dodaj nową tabelę (`ProductCategories`), czy to Ty masz plik tekstowy lub dokumentu programu Microsoft Word Dzięki danym historycznym następujące:

<a id="0.8_table01"></a>


| **Data zmiany** | **Szczegóły zmiany** |
| --- | --- |
| 2009-02-03: | Dodana kolumna `DepartmentID` (`int`, NOT NULL) do `Employees` tabeli. Dodane ograniczenie klucza obcego z `Departments.DepartmentID` do `Employees.DepartmentID`. |
| 2009-02-05: | Usunięto kolumny `TotalWeight` z `Orders` tabeli. Skojarzone dane już przechwycone w `OrderDetails` rekordów. |
| 2009-02-12: | Utworzone `ProductCategories` tabeli. Istnieją trzy kolumny: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), a `Active` (`bit`, `NOT NULL`). Ograniczenia klucza podstawowego można dodać `ProductCategoryID`oraz domyślną wartość 1, aby `Active`. |


Brak liczby wad tego podejścia. Po pierwsze nie ma żadnych mamy nadzieję, że w przypadku usługi automation. Dowolnym te zmiany, należy zastosować do bazy danych — np. gdy aplikacja jest wdrażana — Deweloper ręcznie musi implementować każdy zmienić, pojedynczo. Ponadto jeśli musisz odtworzyć konkretnej wersji bazy danych z linii bazowej, za pomocą dziennika zmian spowoduje więc potrwa coraz więcej wraz ze wzrostem natężenia rozmiar dziennika. Inny wadą tej metody jest, przejrzystości i poziomie szczegółowości każdego wpisu dziennika zmian pozostało do osoby, rejestrując zmiany. W zespole przy użyciu wielu deweloperów niektóre mogą mieć bardziej szczegółowe bardziej czytelne i bardziej precyzyjne wpisy niż inne. Inne dane dotyczące człowieka wpis błędów są możliwe.

Główną Korzyścią płynącą dokumentowania zmian w bazie danych w prozie jest prostotę. Komputer t konieczność znajomości składni SQL do tworzenia i modyfikowania obiektów bazy danych. Można rejestrować zmiany w prozie i ich wdrażania za pomocą programu SQL Server Management Studio s graficznego interfejsu użytkownika.

Utrzymywanie dziennika zmian w prozie, niewątpliwie, nie jest bardzo zaawansowane i uzyskany działa dobrze w przypadku niektórych projektów, takich jak te, które mają duży zakres, częstych zmian do modelu danych lub obejmować wielu deweloperów. Jednak umieścić takie podejście, które działają bardzo dobrze w małych one-man projektów tylko sporadycznie zmian do modelu danych, które mają i gdzie zapoznają deweloper nie ma silnej tła przy użyciu składni SQL do tworzenia i modyfikowania obiektów bazy danych.

> [!NOTE]
> Informacje przedstawione w dzienniku zmian jest technicznie rzecz biorąc, tylko potrzebne do wdrożenia — czasu I zalecamy przechowywanie historii zmian. Jednak zamiast utrzymania pojedynczej, nigdy nie rośnie pliku dziennika zmian, należy rozważyć utworzenie inny plik dziennika dla każdej wersji bazy danych. Zazwyczaj należy do wersji bazy danych każdorazowo, gdy jest wdrożony. Poprzez utrzymywanie dziennika zmian dzienników można zaczynając od linii bazowej, odtworzyć dowolnej wersji bazy danych, wykonując skrypty dziennika zmian, począwszy od wersji 1 i kontynuowanie aż do wersji należy ponownie utworzyć.


## <a name="recording-the-sql-change-statements"></a>Rejestrowanie oświadczeń zmian SQL

Podstawowy wadą utrzymywania dziennik zmian w prozie jest brak automatyzacji. W idealnym przypadku implementowania zmian w bazie danych do produkcyjnej bazy danych w czasie wdrażania będzie tak łatwe jak kliknięcie przycisku do uruchomienia skryptu, niż musieć go ręcznie przeprowadzić listę instrukcji. Takie usługi automation jest możliwe poprzez utrzymywanie dziennika zmian, zawierający tych poleceń SQL, które służy do zastępowania modelu danych.

Składania SQL zawiera wiele instrukcji do tworzenia i modyfikowania różnych obiektów bazy danych. Na przykład [ *instrukcji CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), po wykonaniu, tworzy nową tabelę przy użyciu określonych kolumn i ograniczeń. [ *Instrukcji ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modyfikuje istniejącej tabeli, dodanie, usunięcie lub zmodyfikowanie jej kolumn lub ograniczenia. Istnieją również instrukcje tworzenia, modyfikacji i Porzuć indeksy, widoki, funkcje zdefiniowane przez użytkownika, procedur składowanych, wyzwalaczy i innych obiektów bazy danych.

Wracając do naszego poprzedniego przykładu, obrazu podczas tworzenia aplikacji już wdrożony, możesz dodać nową kolumnę, aby `Employees` tabeli, należy usunąć kolumny z `Orders` tabeli, a następnie dodaj nową tabelę (`ProductCategories`). Takie działania mogłoby spowodować pliku dziennika zmian za pomocą następujących poleceń SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Wypychanie zmiany do produkcyjnej bazy danych w czasie wdrażania jest operacją jednym kliknięciem: Otwórz program SQL Server Management Studio, połączyć do produkcyjnej bazy danych, Otwórz okno nowego zapytania, Wklej zawartość dziennik zmian i kliknij polecenie Wykonaj do uruchomienia skryptu.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Aby zsynchronizować modele danych przy użyciu narzędziem do porównywania

Dokumentowanie zmian w bazie danych w prozie jest łatwe, ale implementowania zmian wymaga deweloperem każda zmiana na produkcyjną bazę danych, z jednym jednocześnie. Dokumentowanie poleceń SQL zmiana sprawia, że implementowania tych zmian na produkcyjnej bazy danych, jak łatwo i szybko, jak kliknięcie przycisku, ale wymaga nauki i doskonalenie znajomości instrukcji SQL i składnia dla tworzenia i modyfikowania obiektów bazy danych. Narzędzia do porównywania bazy danych wykonaj najlepsze z obu metod i odrzucić najgorszą.

Narzędziem do porównywania bazy danych porównuje schematu lub dane z dwóch baz danych i wyświetla podsumowanie raport pokazujący, jak różnią się bazy danych. A następnie kliknąć przycisk, można wygenerować polecenia SQL do synchronizowania jeden lub więcej obiektów bazy danych. Mówiąc, narzędziem do porównywania bazy danych służy do porównywania rozwoju i produkcyjne bazy danych w czasie wdrażania, generowanie pliku, który zawiera SQL polecenia, który, po wykonaniu zastosuje zmiany schematu bazy danych s produkcji tak że odzwierciedla schemat bazy danych s rozwoju.

Istnieją różne narzędzia porównania bazy danych z innych firm oferowane przez wielu różnych dostawców. Jednym z przykładów jest [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), [ *oprogramowania bramy Red*](http://www.red-gate.com/). Pozwól s zaprezentowania procesu przy użyciu SQL Compare porównania i zsynchronizować schematów baz danych deweloperskim i produkcyjnym.

> [!NOTE]
> W momencie pisania tego dokumentu bieżącej wersji programu SQL Compare była w wersji 7.1, wersji Standard Edition wyceny 395 $. Możesz kontynuować pracę przez pobranie z bezpłatnej wersji próbnej 14-dniowego.


Po uruchomieniu SQL Compare zostanie otwarte okno dialogowe projektów porównania, przedstawiający zapisanych SQL Compare projektów. Utwórz nowy projekt. Spowoduje to uruchomienie Kreatora konfiguracji projektu, który wyświetla monit dotyczący informacji na temat baz danych, aby porównać (patrz rysunek 1). Wprowadź informacje dla baz danych środowisku deweloperskim i produkcyjnym.


[![Porównaj programowania i produkcyjnych bazach danych](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Rysunek 1**: porównanie programowania i produkcyjnych bazach danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> Jeśli bazy danych środowiska projektowania jest plikiem bazy danych programu SQL Express Edition w `App_Data` folderu witryny sieci Web, musisz zarejestrować bazy danych na serwerze bazy danych programu SQL Server Express, aby móc wybrać go z okna dialogowego przedstawionej na rysunku 1. W tym celu najłatwiej Otwórz program SQL Server Management Studio (SSMS), połączyć się z serwerem bazy danych programu SQL Server Express i dołączyć bazy danych. Jeśli nie masz zainstalowany na komputerze programu SSMS możesz pobrać i zainstaluj bezpłatną [ *wersji programu SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Wybierz opcję bazy danych do porównania, można również określić różne ustawienia porównania na karcie Opcje. Jedną z opcji, które chcesz włączyć jest "Ignoruj ograniczenie i indeks nazwy." Pamiętaj, że w poprzednim samouczku dodana aplikacja usługi obiektów bazy danych do baz danych deweloperskim i produkcyjnym. Jeśli użyto `aspnet_regsql.exe` narzędzia do tworzenia tych obiektów w bazie danych produkcyjnych, a następnie można zauważyć, że klucz podstawowy i ograniczenie unikatowe nazwy różnią się między deweloperskich i produkcyjnych baz danych. W związku z tym SQL Compare będzie Flaga wszystkie tabele usług aplikacji jako różne. Można pozostawić "Ignoruj ograniczenie indeksu nazwy i" niezaznaczone i synchronizować nazwy ograniczenia, ale można też SQL Compare ignoruje te różnice.

Po wybraniu baz danych w celu porównania i przeglądając opcje porównywania, kliknij przycisk Porównaj teraz, aby rozpocząć porównanie. Za pośrednictwem dalej kilka sekund Porównaj SQL sprawdza schematy dwóch baz danych i generuje raport o różnicach. Czy mogę ve celowo zmodyfikowana do tworzenia bazy danych pokazują, jak podano w interfejsie SQL Compare występowanie takich niezgodności. Jak pokazano na rysunku 2, I ve dodane `BirthDate` kolumny `Authors` tabeli, usunąć `ISBN` kolumny z `Books` tabeli i dodano nową tabelę `Ratings`, która jest przeznaczona do umożliwiają użytkowników odwiedzających szybkość, z witryny, książki przeglądu.

> [!NOTE]
> Zmiany danych w modelu w ramach tego samouczka zostały wykonane w celu zilustrowania przy użyciu narzędzia do porównywania bazy danych. Te zmiany w bazie danych nie będą dostępne w przyszłości samouczków.


[![SQL Compare przedstawiono różnice między środowiskami deweloperskim i produkcyjne bazy danych](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Rysunek 2**: SQL Compare wyświetla różnice między programowania i produkcyjnych bazach danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


SQL Compare dzieli obiekty bazy danych w grupach, szybkie wyświetlanie obiektów, które istnieją w obu bazach danych, ale są różne, obiekty istniejące w jednej bazie danych, ale nie drugiej, a obiekty, które są identyczne. Jak widać, istnieją dwa obiekty, które istnieją w obu bazach danych, ale różnią się: `Authors` tabeli, która ma kolumny dodane, a `Books` tabeli, która ma jeden usunięty. Jest jeden obiekt, który istnieje tylko w przypadku tworzenia bazy danych, czyli nowo utworzony `Ratings` tabeli. Wiąże się 117 obiektów, które są takie same w obu bazach danych.

Wybieranie obiektu bazy danych wyświetla oknie różnice w języku SQL, które pokazuje, jak te obiekty się różnią. Wyróżnia oknie różnice w języku SQL, wyświetlane u dołu na rysunku 2, który `Authors` tabeli w bazie danych rozwoju `BirthDate` kolumny, która nie znajduje się w `Authors` tabeli w bazie danych produkcyjnych.

Po przejrzeniu różnice i wybraniu obiekty, które chcesz synchronizować, następnym krokiem jest generowanie polecenia SQL wymagane do aktualizacji schematu s bazy danych w środowisku produkcyjnym, aby dopasować rozwoju bazy danych. Jest to realizowane za pośrednictwem Kreatora synchronizacji. Kreator synchronizacji potwierdza, jakie obiekty do synchronizowania, a także podsumowano akcji plan (zobacz rysunek 3). Możesz natychmiast zsynchronizować bazy danych lub generowania skryptu za pomocą poleceń SQL, które mogą być uruchamiane w wolnym czasie.


[![Za pomocą Kreatora synchronizacji, aby zsynchronizować swoje schematy bazy danych](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Rysunek 3**: Użyj Kreatora synchronizacji, aby zsynchronizować Your schematów baz danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


Narzędzia do porównywania bazy danych, takich jak Red bramy oprogramowania s SQL Compare upewnij się, zastosowanie zmiany schematu bazy danych jest rozwoju tak łatwe, jak wskazanie i kliknięcie w produkcyjnej bazie danych.

> [!NOTE]
> SQL Compare porównuje i dwie bazy danych synchronizuje *schematów*. Niestety nie porównania i synchronizowanie danych w tabelach dwie bazy danych. Czerwony bramy oprogramowania oferują produkt o nazwie [ *porównywania danych SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) , porównuje i synchronizuje dane między dwiema bazami danych, ale jest oddzielny produkt od SQL Compare i koszty innego $395.


## <a name="taking-the-application-offline-during-deployment"></a>Przełączania aplikacji w trybie Offline podczas wdrażania

Ponieważ ve widoczny w tych samouczkach wdrażanie to proces, który obejmuje wiele kroków: kopiowanie strony ASP.NET, strony wzorcowe, CSS pliki, JavaScript pliki, obrazy i inne niezbędną zawartość ze środowiska projektowego do produkcji środowiska; Kopiowanie informacji konfiguracyjnych środowisku produkcyjnym, w razie potrzeby; i stosowania zmian do modelu danych od czasu ostatniego wdrożenia. W zależności od liczby plików i złożoność zmian w bazie danych te kroki może potrwać od kilku sekund do kilku minut do ukończenia. W tym oknie aplikacji sieci web jest strumień i użytkowników w witrynie mogą wystąpić błędy lub nieoczekiwane zachowanie.

Wdrażanie witryny sieci Web najlepiej wykonać aplikacji sieci web "offline", ukończenie wdrożenia. Przełączania aplikacji w trybie offline (i przywracanie kopii zapasowej, po zakończeniu procesu wdrażania) jest bardzo proste — wystarczy przekazać plik, a następnie usuwając go. Począwszy od programu ASP.NET 2.0 zwykła obecność pliku o nazwie `app_offline.htm` w s aplikacji katalog główny przyjmuje całej witryny sieci Web "w trybie offline." Wszelkie żądania do strony ASP.NET w danej lokacji jest automatycznie wypełniona przy użyciu zawartości `app_offline.htm` pliku. Po usunięciu tego pliku aplikacja powróci do trybu online.

Przełączenie aplikacji w trybie offline podczas wdrażania, a następnie jest proste i polega na przekazaniu `app_offline.htm` pliku do środowiska produkcyjnego s katalogu przed rozpoczęciem procesu wdrażania i usunięcie go (lub jej nazwa zostanie zmieniona na inny) głównego po wdrożeniu zostało ukończone. Aby uzyskać więcej informacji na temat tej techniki można znaleźć w artykule s John Peterson, biorąc [ *ASP.NET aplikacji w trybie Offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Podsumowanie

Głównym wyzwaniem w wdrażanie aplikacji opartych na danych koncentruje się wokół wdrażania bazy danych. Ponieważ istnieją dwie wersje bazy danych — jedną w środowisku programistycznym -, a jeden w środowisku produkcyjnym schematy te dwie bazy danych może stać się zsynchronizowane miarę dodawania nowych funkcji w trakcie opracowywania. Jakie więcej, ponieważ w produkcyjnej bazie danych jako trwa wypełnianie listy przy użyciu prawdziwe dane pochodzące z rzeczywistych użytkowników, nie można zastąpić produkcyjną bazę danych z bazą danych modyfikacji rozwoju jak w przypadku wdrażania plików, które tworzą aplikacji (strony ASP.NET, s pliki obrazów i tak dalej). Zamiast tego wdrażania bazy danych pociąga za sobą implementacji dokładny zestaw zmian wprowadzonych do tworzenia bazy danych na produkcyjną bazę danych od czasu ostatniego wdrożenia.

W tym samouczku przyjrzano się trzy technik za utrzymanie i stosowanie dziennik zmian w bazie danych. Najprostszą metodą jest zapisanie zmian w prozie. Chociaż takie podejście ułatwia wdrażanie tych zmian w bazie danych produkcyjnych ręczny proces, nie wymaga znajomości poleceń SQL do tworzenia i modyfikowania obiektów bazy danych. Bardziej wyszukane metody i jest znacznie bardziej palatable w większe projekty lub projektów za pomocą wielu deweloperów ma rejestrować zmiany jako serię poleceń SQL. To znacznie hastens wprowadza te zmiany z docelową bazą danych. Najlepsze cechy obu metod można osiągnąć za pomocą narzędzia do porównywania bazy danych, np. Red bramy oprogramowania s SQL Compare.

W tym samouczku kończy się naszym głównym celem na wdrażanie aplikacji opartych na danych. Kolejny zbiór samouczki patrzy na sposób reagowania na błędy w środowisku produkcyjnym. Omówimy sposób wyświetlania strony błędu przyjazna zamiast zamiast ekranu żółty śmierci. Teraz poczekamy sposób rejestrować szczegóły błędów s i wysyłania alertów, gdy występują takie błędy.

Wszystkiego najlepszego programowania!

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-website-that-uses-application-services-vb.md)
> [dalej](displaying-a-custom-error-page-vb.md)
