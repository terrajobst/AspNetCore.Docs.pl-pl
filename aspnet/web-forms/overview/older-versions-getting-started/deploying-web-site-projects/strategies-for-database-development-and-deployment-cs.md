---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: "Strategie programowania baz danych i wdrażania (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W przypadku wdrażania aplikacji opartych na danych po raz pierwszy ślepo można skopiować bazy danych w środowisku programistycznym do środowiska produkcyjnego. B..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 551a04296ff92e174a14bd9d2636714e823397e1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="strategies-for-database-development-and-deployment-c"></a>Strategie programowania baz danych i wdrażania (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> W przypadku wdrażania aplikacji opartych na danych po raz pierwszy ślepo można skopiować bazy danych w środowisku programistycznym do środowiska produkcyjnego. Ale blind wykonywania kopii w kolejnych wdrożeń spowoduje zastąpienie żadnych danych zawartych w produkcyjnej bazie danych. Zamiast tego wdrażania bazy danych polega na stosowaniu zmiany wprowadzone w projektowej bazie danych od czasu ostatniego wdrożenia na produkcyjną bazę danych. W tym samouczku sprawdza, czy te problemy, a także daje różne strategie, aby pomóc chronicling i zastosowania zmian wprowadzonych w bazie danych od czasu ostatniego wdrożenia.


## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w poprzedniej samouczki, wdrażanie aplikacji ASP.NET wymaga kopiowania stosowne zawartości ze środowiska projektowego do środowiska produkcyjnego. Wdrożenie nie jest zdarzeniem jednorazowe, ale raczej coś, co dzieje każdorazowo po wydaniu nowej wersji oprogramowania lub kiedy usterek lub bezpieczeństwem zostały zidentyfikowane i problemu. Podczas kopiowania strony ASP.NET, obrazów, plików JavaScript i inne takie pliki do środowiska produkcyjnego, nie trzeba samodzielnie dotyczą z tych plików zostały zmienione od czasu ostatniego wdrożenia. Ślepo można skopiować plik do środowiska produkcyjnego, zastępowanie istniejącej zawartości. Niestety ta prostota nie rozszerza wdrażania bazy danych.

W przypadku wdrażania aplikacji opartych na danych po raz pierwszy ślepo można skopiować bazy danych w środowisku programistycznym do środowiska produkcyjnego. Ale blind wykonywania kopii w kolejnych wdrożeń spowoduje zastąpienie żadnych danych zawartych w produkcyjnej bazie danych. Zamiast tego wdrażania bazy danych polega na stosowaniu *zmiany* wprowadzone w projektowej bazie danych od czasu ostatniego wdrożenia na produkcyjną bazę danych. W tym samouczku sprawdza, czy te problemy, a także daje różne strategie, aby pomóc chronicling i zastosowania zmian wprowadzonych w bazie danych od czasu ostatniego wdrożenia.

## <a name="the-challenges-of-deploying-a-database"></a>Wyzwania wdrażania bazy danych

Przed aplikacji opartych na danych został wdrożony po raz pierwszy, jest tylko jedną bazę danych, czyli bazy danych w środowisku programistycznym, dlatego po wdrażanie aplikacji opartych na danych po raz pierwszy można ślepo skopiować bazę danych w Środowisko deweloperskie do środowiska produkcyjnego. Jednak po wdrożeniu aplikacji są dwie kopie bazy danych: w rozwoju i jeden w środowisku produkcyjnym.

Między wdrożeniami programowanie i produkcyjne bazy danych może zostać zsynchronizowane. S schemat bazy danych produkcyjnej pozostaje niezmieniony, schemat s programowanie bazy danych mogą ulec zmianie w miarę dodawania nowych funkcji. Może dodać lub usunąć kolumny, tabel, widoków lub procedur składowanych. Można również ważnych danych, który zostanie dodany do projektowej bazie danych. Wiele aplikacji opartych na danych obejmują tabele odnośników wypełniane przy użyciu dane zakodowane, specyficzne dla aplikacji, które nie są można edytować użytkownika. Na przykład aukcji witryny sieci Web może mieć listy rozwijanej z opcji, które opisują stan elementu trwa poddane licytacji: nowy, takie jak nowe dobrej i targach. Zamiast kodować te opcje bezpośrednio na liście rozwijanej zwykle lepszym rozwiązaniem jest umieszczenie ich w bazie danych. Jeśli podczas tworzenia, nowy warunek o nazwie słaby jest dodawane do tabeli następnie podczas wdrażania aplikacji tego samego rekordu musi zostać dodany do tabeli odnośników w produkcyjnej bazie danych.

W idealnym przypadku wdrażania bazy danych wymagałoby kopiowanie bazy danych od projektowania do produkcji. Jednak należy pamiętać, że po wdrożeniu aplikacji i wznowić Programowanie w produkcyjnej bazie danych jest są wypełniane przy użyciu prawdziwe dane z rzeczywistych użytkowników. W związku z tym gdyby wystarczy skopiować bazy danych od projektowania do produkcji w następnym wdrażania czy zastąpić produkcyjną bazę danych i utracone jego dane. Wynikiem jest, że wrzała wdrażania bazy danych w dół do zastosowania zmiany wprowadzone w projektowej bazie danych od czasu ostatniego wdrożenia.

Ponieważ wdrażania bazy danych wymaga zastosowania zmian w schemacie i prawdopodobnie danych od czasu ostatniego wdrożenia, historii zmian musi być zachowana (lub określane w czasie wdrażania) tak, aby te zmiany mogą być stosowane w środowisku produkcyjnym. Istnieje wiele metod do zarządzania i stosowania zmian do modelu danych.

### <a name="defining-the-baseline"></a>Definiowanie linii bazowej

Aby zachować zmiany do bazy danych aplikacji s musisz mieć niektóre początkowy stan linii bazowej, do której zmiany są stosowane do. W jednym extreme początkowy stan może być pustą bazę danych bez tabel, widoków i procedur składowanych. Takie linii bazowej wyników w dzienniku wielu zmian, ponieważ musi obejmować tworzenie wszystkich tabel s bazy danych, widoków i procedur składowanych oraz wszelkie zmiany wprowadzone po początkowym wdrożeniu. Na końcu tego spektrum linii bazowej można ustawić jako wersja bazy danych, która początkowo jest wdrożony w środowisku produkcyjnym. Ten wybór wynikiem dużo mniejsze dziennik zmian, ponieważ zawiera on tylko zmiany wprowadzone do bazy danych po ich pierwszym wdrożeniu. To podejście, które wolę. I oczywiście można wybrać więcej środka podejście drogowej Definiowanie linii bazowej w pewnym momencie między początkowe tworzenie bazy danych i podczas pierwszego wdrożenia bazy danych.

Po utworzeniu wybranego planu bazowego należy wziąć pod uwagę Generowanie skryptu SQL, który może zostać wykonany, aby odtworzyć wersji linii bazowej. Skrypt programu pozwala na szybkie ponowne utworzenie wersji bazowej bazy danych. Ta funkcja jest szczególnie przydatna w dużych projektach, których może istnieć wiele deweloperów pracujących na projekt lub dodatkowe środowiska, takich jak testowania lub przemieszczania, należy każdy własnych kopii bazy danych.

Istnieją różne narzędzia Twojej dyspozycji do generowania skrypt SQL w wersji linii bazowej. Z programu SQL Server Management Studio (SSMS) kliknąć prawym przyciskiem myszy w bazie danych, przejdź do menu zadania i wybierz opcję generowania skryptów. Spowoduje to uruchomienie Kreatora skryptu, w którym można zmodyfikować, aby wygenerować plik zawierający poleceń SQL, aby utworzyć bazę danych obiektów s. Innym rozwiązaniem jest Kreatora publikowania bazy danych, który można wygenerować polecenia SQL, aby utworzyć nie tylko schemat bazy danych, ale dane w tabelach bazy danych. Kreator publikowania bazy danych został przeanalizowany szczegółowo w *wdrażania bazy danych* samouczka. Niezależnie od tego, jakiego należy użyć narzędzia, w celu powinny mieć plik skryptu, który służy do odtworzenia wersji linii bazowej bieżącej bazy danych, jeśli będzie to potrzebne wystąpić.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentowanie zmian w bazie danych w Prozie

Najprostszym sposobem, aby zachować dziennika zmian do modelu danych w fazie projektowania jest rejestrowania zmian w prozie. Na przykład, jeśli podczas tworzenia aplikacji już wdrożony Dodaj nową kolumnę do `Employees` tabeli, należy usunąć kolumny z `Orders` tabeli, a następnie dodaj nową tabelę (`ProductCategories`), czy obsługa pliku tekstowego lub dokumentu programu Microsoft Word o następujących historii:

<a id="0.4_table01"></a>


| **Data zmiany** | **Zmień szczegóły** |
| --- | --- |
| 2009-02-03: | Kolumny dodanej `DepartmentID` (`int`, NOT NULL) do `Employees` tabeli. Dodaje ograniczenie klucza obcego z `Departments.DepartmentID` do `Employees.DepartmentID`. |
| 2009-02-05: | Usunięto kolumny `TotalWeight` z `Orders` tabeli. Skojarzone dane już zarejestrowane w `OrderDetails` rekordów. |
| 2009-02-12: | Utworzony `ProductCategories` tabeli. Istnieją trzy kolumny: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), a `Active` (`bit`, `NOT NULL`). Ograniczenie klucza podstawowego, aby dodać `ProductCategoryID`i wartość domyślną 1 do `Active`. |


Istnieje szereg wady tej metody. Po pierwsze nie istnieje żadne rozwiązanie do automatyzacji. W każdej chwili te zmiany, należy zastosować do bazy danych — np. gdy aplikacja jest wdrażana — Projektant ręcznie musi implementować każdej zmiany, pojedynczo. Ponadto jeśli zajdzie potrzeba rekonstrukcji konkretnej wersji bazy danych z linii bazowej, przy użyciu dziennik zmian, to tak potrwa coraz więcej wraz z rozwojem rozmiar dziennika. Inną wadą do tej metody jest jasności i poziom szczegółowości każdego wpisu dziennika zmian pozostało do osoby rejestracji zmian. Zespół z wielu deweloperów niektóre mogą mieć bardziej szczegółowe, bardziej czytelny lub bardziej dokładne wpisy od innych. Innych błędów wejścia dane dotyczące personelu są możliwe.

Główną zaletą Dokumentowanie zmian w bazie danych w prozie jest prostota. Zrobisz t konieczność znajomości składni SQL do tworzenia i modyfikowania obiektów bazy danych. Można zapisać zmian w prozie i ich wdrażania za pomocą programu SQL Server Management Studio s graficznego interfejsu użytkownika.

Obsługa dziennika zmian w prozie, niewątpliwie, nie jest bardzo zaawansowane i wykorzystanej działa prawidłowo w niektórych projektach, takich jak te, które są za duże w zakresie, częstych zmian do modelu danych lub wymagają wielu deweloperów. Jednak umieścić takie podejście dość prawidłowo na małych one-man projektów zawierających tylko okazjonalne zmiany w modelu danych i gdy bez developer nie ma silnej tła w składni SQL do tworzenia i modyfikowania obiektów bazy danych.

> [!NOTE]
> Informacje w dzienniku zmian jest technicznie, tylko wymagane do wdrożenia czasu, najlepiej przechowywania historii zmian. Jednak zamiast utrzymania pojedynczy, kiedykolwiek powiększania zmiany pliku dziennika, należy rozważyć utworzenie inny plik dziennika dla poszczególnych wersji bazy danych. Zazwyczaj należy do wersji bazy danych zawsze, gdy jest wdrożony. Dzięki utrzymywaniu dziennik dzienniki zmian począwszy od linii bazowej, ponownie dowolnej wersji bazy danych przez wykonywanie skryptów dziennika zmian, począwszy od wersji 1 i kontynuowanie aż do wersji należy utworzyć je ponownie.


## <a name="recording-the-sql-change-statements"></a>Rejestrowanie zmian instrukcje SQL

Wadą podstawowej obsługi dziennik zmian w prozie jest brak automatyzacji. W idealnym przypadku implementowania zmian w bazie danych w produkcyjnej bazie danych w czasie wdrażania będzie tak proste, jak kliknięcie przycisku do uruchomienia skryptu zamiast ręcznie wykonać listę instrukcji. Takie automatyzacji jest możliwe dzięki utrzymywaniu dziennik zmian, zawierające te polecenia SQL służy do zastępowania modelu danych.

Składnia SQL zawiera wiele instrukcji do tworzenia i modyfikowania różnych obiektów bazy danych. Na przykład [ *instrukcji CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), gdy wykonywana, tworzy nową tabelę przy użyciu określonych kolumn i ograniczeń. [ *Instrukcji ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modyfikuje istniejącej tabeli, dodawania, usuwania lub modyfikowania jej kolumny lub ograniczenia. Dostępne są także instrukcje do tworzenia, modyfikowania i Porzuć indeksy, widoki, funkcje zdefiniowane przez użytkownika, procedur składowanych, wyzwalaczy i innych obiektów bazy danych.

Wracając do naszych wcześniejszego przykładu obrazu podczas tworzenia aplikacji już wdrożona, Dodaj nową kolumnę do `Employees` tabeli, należy usunąć kolumny z `Orders` tabeli, a następnie dodaj nową tabelę (`ProductCategories`). Takie działania spowoduje w pliku dziennika zmian za pomocą następujących poleceń SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Wypychanie te zmiany w produkcyjnej bazie danych w czasie wdrażania jest operacją jednym kliknięciem: Otwórz program SQL Server Management Studio, połączenia z bazą danych w środowisku produkcyjnym, Otwórz okno nowego zapytania, Wklej zawartość dziennik zmian i kliknij polecenie Wykonaj do uruchomienia skryptu.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Aby zsynchronizować modeli danych przy użyciu narzędzia do porównywania

Dokumentowanie zmian w bazie danych w prozie jest proste, ale implementacja zmiany wymaga dewelopera każdej zmiany na produkcyjną bazę danych, co w czasie; Dokumentowanie zmiany poleceń SQL umożliwia wdrażanie tych zmian na produkcyjną bazę danych jako łatwo i szybko jak kliknięcie przycisku, ale wymaga nauki i opanowanie instrukcji SQL i składnia tworzenia i modyfikowania obiektów bazy danych. Narzędzia do porównywania bazy danych zająć najlepiej z obu podejść i odrzucić najgorszą.

Narzędzie do porównywania bazy danych porównuje schematu lub dane z dwóch baz danych i wyświetla podsumowanie raport pokazujący, czym różni się od bazy danych. A następnie kliknąć przycisk, można wygenerować polecenia SQL do zsynchronizowania jeden lub więcej obiektów bazy danych. Mówiąc, można użyć narzędzia do porównywania bazy danych do porównania rozwoju i produkcyjnych baz danych w czasie wdrażania, generowanie pliku, który zawiera SQL polecenia, gdy wykonywana, zastosuje zmiany do schematu bazy danych s produkcji tak że odzwierciedla Projektowanie schematu bazy danych s.

Istnieją różne narzędzia porównania bazy danych innej firmy oferowanych przez wiele różnych dostawców. Jest jednym z przykładów [ *porównania SQL*](http://www.red-gate.com/products/SQL_Compare/), przez [ *oprogramowania bramy czerwony*](http://www.red-gate.com/). Let s Przeprowadź proces porównania i zsynchronizować rozwoju i produkcji schematów baz danych przy użyciu porównania SQL.

> [!NOTE]
> W momencie pisania tego w bieżącej wersji programu SQL porównania był w wersji 7.1, z wersji Standard wyceny 395 $. Możesz kontynuować pracę, pobierając bezpłatnej wersji próbnej 14 dni.


Po uruchomieniu porównania SQL zostanie otwarte okno dialogowe projekty porównania przedstawiający zapisanych projektów porównania SQL. Utwórz nowy projekt. Spowoduje to uruchomienie Kreatora konfiguracji projektu, który wyświetla monit dotyczący informacji na temat baz danych do porównania (zobacz rysunek 1). Wprowadź informacje dotyczące bazy danych środowisko rozwoju i produkcji.


[![Porównanie programowanie i produkcyjnych baz danych](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Rysunek 1**: porównanie programowanie i produkcyjne bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Jeśli środowisko rozwoju bazy danych jest plikiem bazy danych SQL Express Edition w `App_Data` folderu witryny sieci Web, musisz zarejestrować bazy danych na serwerze bazy danych programu SQL Server Express w celu wybierz ją z okna dialogowego pokazany na rysunku 1. W tym celu najłatwiej Otwórz program SQL Server Management Studio (SSMS), nawiąż połączenie z serwerem bazy danych programu SQL Server Express i dołączyć bazy danych. Jeśli nie masz SSMS zainstalowany na tym komputerze można pobrać i zainstalować bezpłatną [ *wersji programu SQL Server 2008 Management Studio podstawowe*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Oprócz Wybieranie baz danych do porównania, można również określić różne ustawienia porównania na karcie Opcje. Jedną z opcji, które chcesz włączyć jest "Ignoruj ograniczenia i indeksu nazwy." Odwołaj, że w poprzednim samouczek dodano obiektów bazy danych do rozwoju i produkcyjne bazy danych usług aplikacji. Jeśli używasz `aspnet_regsql.exe` narzędzia do tworzenia tych obiektów w produkcyjnej bazie danych, a następnie można zauważyć, że nazwy ograniczenia unique i klucz podstawowy różnią się między bazami danych rozwoju i produkcji. W związku z tym Porównaj SQL zostanie Flaga wszystkie tabele usług aplikacji jako różne. Można pozostawić "Ignoruj ograniczenie indeksu nazwy i" unchecked i synchronizować nazwy ograniczenia, lub poinstruować SQL porównania ignorować te różnice.

Po wybraniu bazy danych do porównania (i opcje porównania recenzowania), kliknij przycisk porównania teraz, aby rozpocząć porównania. Za pośrednictwem dalej kilka sekund Porównaj SQL sprawdza schematów dwóch baz danych i tworzy raport różnic między nimi. I Zapisz celowo zmian niektórych bazą programowanie pokazanie, jak takich różnic są wymienione w interfejsie porównania SQL. Jak pokazano na rysunku 2, I Zapisz dodane `BirthDate` kolumny, która ma `Authors` tabeli usunięte `ISBN` kolumny z `Books` tabeli i dodać nową tabelę, `Ratings`, które mają na celu let użytkowników odwiedzających je przejrzeć książek szybkość, z witryny.

> [!NOTE]
> Zmiany danych w modelu w tym samouczku zostały wykonane w celu zilustrowania przy użyciu narzędzia do porównywania bazy danych. Te zmiany w bazie danych nie będzie zawierał w przyszłości samouczki.


[![Porównaj SQL wymieniono różnice między opracowywania i produkcyjnych baz danych](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Rysunek 2**: porównanie SQL wymieniono różnice między programowanie i produkcyjne bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


Porównanie SQL dzieli obiektów bazy danych w grupach, szybko pokazujące, jakie obiekty istnieją w obu bazach danych, ale są różne, istniejące obiekty w jedną bazę danych, ale nie dla drugiego i obiekty, które są identyczne. Jak widać, istnieją dwa obiekty, które istnieją w obu bazach danych, ale są różne: `Authors` tabeli, które zostały dodane kolumny, oraz `Books` tabeli, która ma jednej usunięta. Istnieje jeden obiekt, który istnieje tylko w projektowej bazie danych, czyli nowo utworzony `Ratings` tabeli. I 117 obiektów są identyczne w obu bazach danych.

Wybieranie obiektu bazy danych wyświetla okno różnice SQL, który pokazuje, jak te obiekty są różne. W oknie SQL różnice, wyświetlane u dołu na rysunku 2 prezentuje który `Authors` tabela w bazie danych Programowanie ma `BirthDate` kolumny, która nie znajduje się w `Authors` tabeli w produkcyjnej bazie danych.

Po zapoznaniu się różnic i wybraniu obiekty, które mają być synchronizowane, następnym krokiem jest generowanie poleceń SQL potrzebnych do aktualizacji schematu s produkcyjnej bazy danych do dopasowania projektowej bazie danych. Jest to realizowane za pośrednictwem Kreatora synchronizacji. Kreator synchronizacji potwierdza co obiekty do synchronizacji i zawiera podsumowanie akcji planowanie (patrz rysunek 3). Można zsynchronizować bazy danych od razu lub generowania skryptu za pomocą poleceń SQL, które mogą być uruchamiane w wolnym czasie.


[![Synchronizuj z schematów baz danych za pomocą Kreatora synchronizacji](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Rysunek 3**: Użyj Kreatora synchronizacji, aby zsynchronizować Your schematów baz danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Narzędzia do porównywania bazy danych, takich jak oprogramowania bramy czerwony s porównania SQL upewnij, wprowadzania zmian schematu bazy danych Programowanie w produkcyjnej bazie danych tak łatwe, jak i kliknij opcję.

> [!NOTE]
> Porównanie SQL porównuje i synchronizuje dwóch baz danych *schematy*. Niestety nie porównania i zsynchronizować dane w tabelach dwie bazy danych. Oprogramowanie bramy czerwony oferują produkt o nazwie [ *porównania danych SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) który porównuje i synchronizuje dane między dwiema bazami danych, ale jest oddzielny produkt z porównania SQL i innym $395 koszty.


## <a name="taking-the-application-offline-during-deployment"></a>Pobieranie aplikacji w tryb Offline podczas wdrażania

Jak możemy kolejnych widoczne w całym te samouczki wdrażanie to proces, który obejmuje wiele kroków: kopiowanie stron ASP.NET, stron wzorcowych CSS pliki, JavaScript pliki, obrazy i inne niezbędne zawartości ze środowiska projektowego do produkcji środowiska; Kopiowanie informacji konfiguracji specyficznych dla środowiska produkcyjnego, w razie potrzeby; i wprowadzania zmian w modelu danych od czasu ostatniego wdrożenia. W zależności od liczby plików i złożoność zmian w bazie danych następujące kroki może potrwać od kilku sekund do kilku minut do ukończenia. W tym oknie aplikacji sieci web jest strumienia i użytkowników w witrynie mogą wystąpić błędy lub nieoczekiwane zachowanie.

W przypadku wdrażania witryny sieci Web najlepiej "offline" zająć aplikacji sieci web dopiero po ukończeniu wdrażania. Przełączanie aplikacji do trybu offline (i przywracanie kopii zapasowej po zakończeniu procesu wdrażania), wystarczy przekazywania pliku, a następnie usuwając go. Począwszy od programu ASP.NET 2.0, obecność pliku o nazwie `app_offline.htm` w aplikacji s katalog główny przyjmuje całej witryny sieci Web "offline". Każde żądanie strony ASP.NET w tej witrynie jest automatycznie odpowiedzi z zawartością `app_offline.htm` pliku. Po usunięciu tego pliku, aplikacja powróci do trybu online.

Pobieranie aplikacji w tryb offline podczas wdrażania, a następnie jest tak proste, jak przekazywanie `app_offline.htm` pliku do środowiska produkcyjnego s katalogu przed rozpoczęciem procesu wdrażania i usunięcie go (lub zmiana jego nazwy na inny) głównego raz wdrożenia zostanie zakończona. Więcej informacji na temat tej metody można znaleźć w artykule s Peterson Jan, biorąc [ *ASP.NET aplikacji w tryb Offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Podsumowanie

Główne żądania w wdrażanie aplikacji opartych na danych koncentruje się wokół wdrażania bazy danych. Ponieważ istnieją dwie wersje bazy danych-do - jednego w środowisku programistycznym - i jeden w środowisku produkcyjnym schematy te dwie bazy danych może być zsynchronizowane miarę dodawania nowych funkcji do rozwoju. Jakie więcej, ponieważ produkcyjną bazę danych jako są wypełniane przy użyciu prawdziwe dane z rzeczywistych użytkowników, nie można zastąpić produkcyjną bazę danych z bazą danych zmodyfikowanych programowanie jak w przypadku wdrażania plików, które tworzą aplikacji (strony ASP.NET, s pliki obrazów i tak dalej). Zamiast tego wdrażania bazy danych wymaga wykonania dokładny zestaw zmian wprowadzonych w projektowej bazie danych w produkcyjnej bazie danych od czasu ostatniego wdrożenia.

W tym samouczku przeglądał trzy metody obsługi i stosowania dziennik zmian w bazie danych. Jest najprostsza metoda rejestrowania zmian w prozie. Gdy takie działanie umożliwia wdrażanie tych zmian w bazie danych produkcyjnych ręczny proces, nie wymaga znajomości poleceń SQL do tworzenia i modyfikowania obiektów bazy danych. Bardziej złożone podejścia i jest bardziej palatable w większych projekty lub projektów z wielu deweloperów, jest do rejestrowania zmian w postaci serii poleceń SQL. To znacznie hastens wprowadzanie tych zmian do docelowej bazy danych. Najlepsze cechy obu podejść można osiągnąć za pomocą narzędzia do porównywania bazy danych, takich jak s oprogramowania bramy czerwony porównania SQL.

Ten samouczek zawiera skupiliśmy na wdrażanie aplikacji opartych na danych. Następny zestaw samouczki analizuje odpowiadanie na błędy w środowisku produkcyjnym. Wyjaśniono, jak należy wyświetlić stronę błędu przyjazną raczej zamiast ekranu żółty śmierci. I zajmiemy się tym sposobu logowania s Szczegóły błędu i alert, gdy występują takie błędy.

Programowanie przyjemność!

>[!div class="step-by-step"]
[Poprzednie](configuring-a-website-that-uses-application-services-cs.md)
[dalej](displaying-a-custom-error-page-cs.md)
