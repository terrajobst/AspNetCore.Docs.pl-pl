---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Tworzenie procedur składowanych i funkcji zdefiniowanych przez użytkownika z zarządzanego kodu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Microsoft SQL Server 2005 jest zintegrowany z .NET środowiska uruchomieniowego języka wspólnego umożliwiają deweloperom tworzenie obiektów bazy danych przy użyciu kodu zarządzanego. W tym samouczku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 93c1496f28e6a4e29b5fe46c6c20635c05ce78c3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388955"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Tworzenie procedur składowanych i funkcji zdefiniowanych przez użytkownika z kodem zarządzanym (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) lub [Pobierz plik PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 jest zintegrowany z .NET środowiska uruchomieniowego języka wspólnego umożliwiają deweloperom tworzenie obiektów bazy danych przy użyciu kodu zarządzanego. W tym samouczku przedstawiono sposób tworzenia zarządzanej procedury składowane i zarządzane funkcje zdefiniowane przez użytkownika z kodem języka Visual Basic lub C#. Widzimy także, jak te wersje programu Visual Studio umożliwiają debugowanie takie obiekty zarządzanej bazy danych.


## <a name="introduction"></a>Wprowadzenie

Użyj bazy danych, takich jak Microsoft SQL Server 2005-s [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) do wstawiania, modyfikowania i pobierania danych. Większość systemów bazy danych zawiera konstrukcje grupujące serię instrukcji SQL, które następnie mogą być wykonywane jako zespół jednego, wielokrotnego użytku. Procedury składowane są jednym z przykładów. Innym jest *funkcje zdefiniowane przez użytkownika*przez użytkownika (UDF), konstrukcja, która zostanie omówiony bardziej szczegółowo w kroku 9.

Zasadniczo SQL jest przeznaczona do pracy z zestawami danych. `SELECT`, `UPDATE`, I `DELETE` instrukcji założenia mają zastosowanie do wszystkich rekordów w odpowiedniej tabeli i jest ograniczona tylko ich `WHERE` klauzul. Jeszcze ma wiele funkcji języka, zaprojektowane do pracy z jednego rekordu w danym momencie i manipulowanie danymi skalarne. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) umożliwiają dla zestawu rekordów można zapętlonego po jednym naraz. Manipulowania ciągami takich jak `LEFT`, `CHARINDEX`, i `PATINDEX` pracować z danymi skalarne. SQL zawiera również instrukcjach przepływu sterowania, takich jak `IF` i `WHILE`.

Przed Microsoft SQL Server 2005 procedury składowane i funkcje zdefiniowane przez użytkownika tylko wtedy można zdefiniować jako zbiór instrukcji języka T-SQL. SQL Server 2005, jednak zaprojektowano tak, aby zapewnić integrację z usługą [środowiska uruchomieniowego języka wspólnego (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), czyli środowiska uruchomieniowego użytego przez wszystkie zestawy .NET. W związku z tym procedur przechowywanych i funkcji zdefiniowanych przez użytkownika w bazie danych programu SQL Server 2005 mogą być tworzone przy użyciu kodu zarządzanego. Oznacza to można utworzyć procedury składowanej lub funkcji definiowanych przez użytkownika jako metodę w klasie języka C#. Dzięki temu tych procedur przechowywanych i funkcji zdefiniowanych przez użytkownika, korzystanie z funkcji w programie .NET Framework i z własnych niestandardowych klas.

W tym samouczku, który zostanie omówiony sposób tworzenia zarządzanej procedur składowanych i funkcje zdefiniowane przez użytkownika i jak zintegrować ją w naszej bazie danych Northwind. Rozpocznij pracę dzięki s!

> [!NOTE]
> Obiekty zarządzane bazy danych oferuje kilka zalet w porównaniu z ich odpowiedniki SQL. Język złożonością i znajomości języka i możliwość ponownego używania istniejącego kodu i logiki są głównymi zaletami. Ale obiektami zarządzanej bazy danych może być mniej wydajne, podczas pracy z zestawami danych, które nie obejmują wiele procedur logiki. Aby uzyskać bardziej szczegółowe omówienie o zaletach przy użyciu kodu zarządzanego i T-SQL, zapoznaj się [korzyści wynikające z przy użyciu kodu zarządzanego do tworzenia obiektów bazy danych](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Krok 1: Przenoszenie bazy danych Northwind, poza`App_Data`

Wszystkie z naszych samouczków dotychczasowych użyto pliku bazy danych programu Microsoft SQL Server 2005 Express Edition, w tym s aplikacji sieci web `App_Data` folderu. Wprowadzenie do bazy danych w `App_Data` uproszczony, dystrybucja i uruchamianie tych samouczków, wszystkie pliki znajdowały się w jednym katalogu, a wymagane nie dodatkowe czynności konfiguracyjne do przetestowania tego samouczka.

Jednak w tym samouczku umożliwiają s przeniesienie bazy danych Northwind poza `App_Data` i jawnie zarejestrować ją w wystąpieniu bazy danych programu SQL Server 2005 Express Edition. Chociaż można wykonać kroki opisane w ramach tego samouczka przy użyciu bazy danych w `App_Data` folderu, szereg kroków zostają znacznie prostsze jawnie rejestrując bazy danych w wystąpieniu bazy danych programu SQL Server 2005 Express Edition.

Pobieranie na potrzeby tego samouczka znajdują się pliki dwie bazy danych — `NORTHWND.MDF` i `NORTHWND_log.LDF` — jest umieszczane w folderze o nazwie `DataFiles`. Jeśli wykonujesz wraz z Twojej własnej implementacji samouczków, zamknij program Visual Studio i Przenieś `NORTHWND.MDF` i `NORTHWND_log.LDF` plików z witryny sieci Web s `App_Data` folderu do folderu poza witryny sieci Web. Po przeniesieniu plików bazy danych do innego folderu należy zarejestrować bazy danych Northwind za pomocą wystąpienia bazy danych programu SQL Server 2005 Express Edition. Można to zrobić z programu SQL Server Management Studio. Jeśli masz inne niż - Express Edition z programu SQL Server 2005 zainstalowana na danym komputerze następnie prawdopodobnie masz już zainstalowany program Management Studio. Jeśli tylko na komputerze znajduje się program SQL Server 2005 Express Edition, Poświęć chwilę, aby pobrać i zainstalować [programu Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Uruchom program SQL Server Management Studio. Jak pokazano na rysunku 1, Management Studio rozpoczyna się od pytaniem, jakie serwera, aby nawiązać połączenie. Wprowadź localhost\SQLExpress dla nazwy serwera, na liście rozwijanej uwierzytelniania wybierz opcję uwierzytelniania Windows i kliknij przycisk Połącz.


![Połącz się z wystąpieniem odpowiednią bazą danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Rysunek 1**: nawiąż połączenie z wystąpieniem odpowiednią bazą danych


Po nawiązaniu połączenia możesz ve okno Eksplorator obiektów spowoduje wyświetlenie listy informacji na temat wystąpienia bazy danych programu SQL Server 2005 Express Edition, w tym baz danych, informacje o zabezpieczeniach, opcji zarządzania i tak dalej.

Musimy dołączyć bazy danych Northwind w `DataFiles` folderu (lub wszędzie tam, gdzie może przeniesiono go) do wystąpienia bazy danych programu SQL Server 2005 Express Edition. Kliknij prawym przyciskiem myszy na folder baz danych i wybierz opcję Dołącz z menu kontekstowego. Zostanie wyświetlone okno dialogowe dołączanie bazy danych. Kliknij przycisk Dodaj, przejdź do szczegółów odpowiedniego `NORTHWND.MDF` pliku, a następnie kliknij przycisk OK. W tym momencie ekran powinien wyglądać podobnie jak na rysunku 2.


[![Połącz się z wystąpieniem odpowiednią bazą danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Rysunek 2**: łączenie się z odpowiednim wystąpieniem bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> Podczas nawiązywania połączenia z wystąpieniem programu SQL Server 2005 Express Edition, za pomocą programu Management Studio w oknie dialogowym Dołącz bazę danych nie pozwala na przechodzenie do katalogów profilu użytkownika, takie jak Moje dokumenty. Dlatego upewnij się umieścić `NORTHWND.MDF` i `NORTHWND_log.LDF` pliki w katalogu profilu niezwiązanych z użytkownikiem.


Kliknij przycisk OK, aby dołączyć bazę danych. Dołącz bazę danych, okno dialogowe zostanie zamknięte i Eksplorator obiektów teraz wyświetlić listę po prostu dołączyć bazy danych. Jest szansa, Northwind baza danych ma nazwę, takich jak `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Zmień nazwę bazy danych Northwind, klikając prawym przyciskiem myszy w bazie danych i wybierając pozycję zmiany nazwy.


![Zmiana nazwy bazy danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Rysunek 3**: zmiana nazwy bazy danych Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Krok 2: Tworzenie nowego rozwiązania i projekt programu SQL Server w programie Visual Studio

Do utworzenia zarządzanej procedury składowanej lub funkcji zdefiniowanych przez użytkownika w programie SQL Server 2005 napiszemy procedur składowanych i funkcji zdefiniowanej przez użytkownika logiki jako kod C# w klasie. Napisana kod firma Microsoft będzie należy skompilować tej klasy w zestawie ( `.dll` pliku), zarejestrować zestaw z bazą danych programu SQL Server, a następnie utworzyć procedury składowanej lub funkcji definiowanych przez użytkownika obiekt w bazie danych, który wskazuje na odpowiedniej metody w zestaw. Te kroki wszystkie można wykonać ręcznie. Firma Microsoft można tworzyć kod w dowolnym tekście edytora, skompilować go w wierszu polecenia za pomocą kompilatora C# ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), zarejestruj je przy użyciu bazy danych przy użyciu [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) polecenia lub z zarządzania Studio i Dodaj procedury składowanej lub funkcji definiowanych przez użytkownika obiekt w podobny sposób. Na szczęście wersje Professional i systemy zespołu programu Visual Studio obejmują typ projekt programu SQL Server, który automatyzuje następujące zadania. W tym samouczku opisano tworzenie zarządzanych procedur składowanych i funkcji definiowanych przez użytkownika przy użyciu typu Projekt programu SQL Server.

> [!NOTE]
> Jeśli używasz Visual Web Developer lub standardową edycję programu Visual Studio, następnie należy zamiast tego użyj metody ręcznego. Krok 13 zawiera szczegółowe instrukcje dotyczące ręcznego wykonywania tych kroków. Zachęcam Cię do przeczytania 2 kroki 12 przed odczytaniem 13 krok, ponieważ te kroki obejmują ważne instrukcje konfiguracji programu SQL Server, które muszą być stosowane niezależnie od tego, jakie wersje programu Visual Studio, którego używasz.


Uruchom po otwarciu programu Visual Studio. W menu Plik wybierz nowy projekt, aby wyświetlić okno dialogowe Nowy projekt (zobacz rysunek 4). Przejdź do szczegółów typ projektu bazy danych, a następnie wybierz za pomocą szablonów wymienione po prawej stronie utworzyć nowy projekt programu SQL Server. Wybrano I nazwij ten projekt `ManagedDatabaseConstructs` i umieścić go w ramach rozwiązania o nazwie `Tutorial75`.


[![Utwórz nowy projekt programu SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Rysunek 4**: Utwórz nowy projekt programu SQL Server ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


Kliknij przycisk OK w oknie dialogowym Nowy projekt, aby utworzyć rozwiązanie i projekt programu SQL Server.

Projekt programu SQL Server jest powiązany z określoną bazą danych. W związku z tym po utworzeniu projektu na nowy serwer SQL firma Microsoft niezwłocznie prośba o określenie tych informacji. Rysunek 5. Pokazuje okno dialogowe Nowe odwołanie do bazy danych, które zostały wprowadzone do punktu z bazą danych Northwind, firma Microsoft jest zarejestrowana w wystąpieniu bazy danych programu SQL Server 2005 Express Edition w kroku 1.


![Skojarz projekt programu SQL Server z bazą danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Rysunek 5**: skojarzyć projekt programu SQL Server z bazą danych Northwind


Aby debugować zarządzanego procedur przechowywanych i funkcji zdefiniowanych przez użytkownika, zostanie utworzona w ramach tego projektu, należy włączyć obsługę debugowania dla połączenia SQL/CLR. Zawsze, gdy skojarzenie projekt programu SQL Server przy użyciu nowej bazy danych (jak zrobiliśmy na rysunku 5), Visual Studio Wyświetla prośbę nam Jeśli chcemy włączyć debugowanie SQL/CLR dla połączenia (patrz rysunek 6). Kliknij przycisk Tak.


![Włącz debugowanie SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Rysunek 6**: Włącz debugowanie SQL/CLR


W tym momencie nowy projekt serwera SQL dodano do rozwiązania. Zawiera folder o nazwie `Test Scripts` przy użyciu pliku o nazwie `Test.sql`, która jest używana do debugowania obiektami zarządzanej bazy danych utworzone w projekcie. Przyjrzymy się debugowanie w kroku 12.

Firma Microsoft jest teraz dodać nowe zarządzanej procedury składowane i funkcje zdefiniowane przez użytkownika do tego projektu, ale przed umożliwialiśmy s najpierw dołączyć naszej istniejącej aplikacji sieci web w rozwiązaniu. W menu Plik wybierz opcję Dodaj, a następnie wybierz istniejącą witrynę sieci Web. Przejdź do folderu odpowiedniej witryny sieci Web, a następnie kliknij przycisk OK. Jak pokazano na rysunku 7, spowoduje to zaktualizowanie rozwiązania, aby uwzględnić dwa projekty: witryny sieci Web i `ManagedDatabaseConstructs` projekt programu SQL Server.


![Eksplorator rozwiązań zawiera teraz dwa projekty](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Rysunek 7**: Eksplorator rozwiązań zawiera teraz dwa projekty


`NORTHWNDConnectionString` Wartość w `Web.config` odwołuje się do aktualnie `NORTHWND.MDF` w pliku `App_Data` folderu. Ponieważ firma Microsoft usunęła tę bazę danych z `App_Data` i jawnie on zarejestrowany w wystąpieniu bazy danych programu SQL Server 2005 Express Edition, należy odpowiednio zaktualizować `NORTHWNDConnectionString` wartość. Otwórz `Web.config` pliku w witrynie sieci Web i zmień `NORTHWNDConnectionString` wartości, tak aby odczytuje parametry połączenia: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Po tej zmianie swoje `<connectionStrings>` sekcji `Web.config` powinien wyglądać podobnie do poniższego:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Zgodnie z opisem w [poprzedni Samouczek](debugging-stored-procedures-cs.md), podczas debugowania obiektu serwera SQL z aplikacji klienckiej, takich jak witryny sieci Web programu ASP.NET należy wyłączyć buforowanie połączeń. Parametry połączenia, powyżej wyłącza buforowanie połączeń ( `Pooling=false` ). Jeśli nie planujesz debugowania zarządzanego procedur przechowywanych i funkcji zdefiniowanych przez użytkownika z witryny sieci Web platformy ASP.NET, należy włączyć buforowanie połączeń.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Krok 3: Tworzenie zarządzanej procedury składowanej

Aby dodać zarządzanej procedury składowanej do bazy danych Northwind, najpierw należy utworzyć procedurę składowaną jako metoda w projekcie programu SQL Server. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` nazwy projektu i wybierz opcję Dodaj nowy element. Spowoduje to wyświetlenie okna dialogowego Dodaj nowy element, które zawiera listę typów obiektów zarządzanej bazy danych, które można dodać do projektu. Jak pokazano na rysunku 8, w tym procedury składowane i funkcje zdefiniowane przez użytkownika, między innymi.

Pozwól, s, Rozpocznij od dodania procedury przechowywanej, która po prostu zwraca wszystkie produkty, które zostały anulowane. Nadaj nowemu plikowi procedury składowanej `GetDiscontinuedProducts.cs`.


[![Dodaj nową procedurę składowaną o nazwie GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Rysunek 8**: Dodaj nowy przechowywane procedury o nazwie `GetDiscontinuedProducts.cs` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Spowoduje to utworzenie nowego pliku klasy C# o następującej zawartości:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Należy zauważyć, że procedura składowana jest implementowany jako `static` metodę w ramach `partial` plik klasy o nazwie `StoredProcedures`. Ponadto `GetDiscontinuedProducts` zostanie nadany metoda `SqlProcedure attribute`, która oznacza metodę jako procedurę przechowywaną.

Poniższy kod tworzy `SqlCommand` i ustawia jego `CommandText` do `SELECT` zapytania zwracającego wszystkie kolumny z `Products` Tabela produktów, których `Discontinued` pola jest równa 1. Następnie wykonuje polecenie i wysyła wyniki do aplikacji klienckiej. Dodaj następujący kod do `GetDiscontinuedProducts` metody.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Wszystkie obiekty zarządzane bazy danych mają dostęp do [ `SqlContext` obiektu](https://msdn.microsoft.com/library/ms131108.aspx) reprezentujący kontekst obiektu wywołującego. `SqlContext` Zapewnia dostęp do [ `SqlPipe` obiektu](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) za pośrednictwem jego [ `Pipe` właściwość](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). To `SqlPipe` obiekt jest używany do promy informacji między bazą danych programu SQL Server i aplikacji wywołującej. Jak sugeruje jej nazwa, [ `ExecuteAndSend` metoda](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) wykonuje przekazanego `SqlCommand` obiektu i wysyła wyniki z powrotem do aplikacji klienckiej.

> [!NOTE]
> Obiekty zarządzane bazy danych są najbardziej odpowiednie dla procedur przechowywanych i funkcji zdefiniowanych przez użytkownika, korzystających z procedurach logiki, a nie logiki opartej na zestawie. Proceduralne logiki obejmuje Praca z zestawami danych na podstawie wiersz po wierszu lub Praca z danymi skalarne. `GetDiscontinuedProducts` Metodę, którą właśnie utworzyliśmy, jednak pociąga za sobą nie procedur logiki. W związku z tym jego będzie najlepiej można zaimplementować jako procedury przechowywanej T-SQL. Zaimplementowano jako zarządzanej procedury składowanej, aby zademonstrować kroki niezbędne do tworzenia i wdrażania zarządzanego procedur składowanych.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Krok 4: Wdrażanie zarządzanej procedury składowanej

Przy użyciu tego kodu jest pełna jesteśmy gotowi wdrożyć ją z bazą danych Northwind. Wdrożenia projektu serwera SQL kompiluje kod w zestawie, rejestruje zestaw z bazą danych i tworzy odpowiednimi obiektami w bazie danych, łącząc je do odpowiedniej metody w zestawie. Dokładny zestaw zadań wykonywanych za pomocą opcji wdrażania jest bardziej precyzyjne województw w kroku 13. Kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` projektu nazwę w Eksploratorze rozwiązań, a następnie wybierz opcję wdrażania. Jednakże, wdrożenie zakończy się niepowodzeniem z następującym błędem: Błędna składnia w pobliżu "Zewnętrzny". Konieczne może być ustawiony poziom zgodności bieżącej bazy danych na wartość większą, aby włączyć tę funkcję. Zobacz Pomoc dla procedury składowanej `sp_dbcmptlevel`.

Ten błąd występuje podczas próby zarejestrowania zestawu z bazą danych Northwind. W celu zarejestrowania zestawu z bazą danych programu SQL Server 2005, poziom zgodności bazy danych s musi być równa 90. Domyślnie nowe bazy danych programu SQL Server 2005 mają poziom zgodności 90. Jednak bazy danych utworzone za pomocą programu Microsoft SQL Server 2000 mają domyślny poziom zgodności 80. Ponieważ bazy danych Northwind początkowo był bazę danych Microsoft SQL Server 2000, jej poziom zgodności jest obecnie ustawiony na 80 i dlatego musi być zwiększony do 90 do zarejestrowania obiektami zarządzanej bazy danych.

Aby zaktualizować poziom zgodności bazy danych s, Otwórz okno nowego zapytania w programie Management Studio i wprowadź:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Kliknij ikonę wykonywania na pasku narzędzi, aby uruchomić zapytanie powyżej.


[![Zaktualizuj poziom zgodności bazy danych Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Rysunek 9**: aktualizowanie s poziom zgodności bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


Po zaktualizowaniu poziom zgodności, należy ponownie wdrożyć projekt programu SQL Server. Tym razem wdrożenie powinno zakończyć się bez błędów.

Wróć do programu SQL Server Management Studio, kliknij prawym przyciskiem myszy w bazie danych Northwind w Eksploratorze obiektów i wybierz polecenie Odśwież. Następnie przejść do folderu programowania, a następnie rozwiń folder zestawów. Jak pokazano na rysunku nr 10, bazy danych Northwind zawiera teraz zestaw wygenerowany przez `ManagedDatabaseConstructs` projektu.


![Zestaw ManagedDatabaseConstructs jest teraz zarejestrowane w usłudze bazy danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Na rysunku nr 10**: `ManagedDatabaseConstructs` zestaw jest teraz zarejestrowane w usłudze bazy danych Northwind


Również rozwinąć folder procedur składowanych. Zobaczysz procedury składowanej o nazwie `GetDiscontinuedProducts`. Ta procedura składowana została utworzona przez proces wdrażania i wskazuje na `GetDiscontinuedProducts` method in Class metoda `ManagedDatabaseConstructs` zestawu. Gdy `GetDiscontinuedProducts` procedury składowanej jest wykonywane, z kolei wykonuje `GetDiscontinuedProducts` metody. Ponieważ jest to zarządzanej procedury składowanej nie można edytować za pomocą programu Management Studio (dlatego ikoną kłódki obok nazwy procedury składowanej).


![Procedura składowana GetDiscontinuedProducts znajduje się w folderze procedur przechowywanych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Rysunek 11**: `GetDiscontinuedProducts` procedury składowanej znajduje się w folderze procedur przechowywanych


Nadal jest jeden więcej próg wykluczający mamy do pokonania przed nazywamy zarządzanej procedury składowanej: baza danych jest skonfigurowana, aby uniemożliwić wykonywanie kodu zarządzanego. To sprawdzić, otwierając okno nowego zapytania i wykonywanie `GetDiscontinuedProducts` procedury składowanej. Zostanie wyświetlony następujący komunikat o błędzie: wykonywanie kodu użytkownika w programie .NET Framework jest wyłączone. Włącz opcję konfiguracji clr enabled.

Aby sprawdzić informacje o konfiguracji s bazy danych Northwind, wprowadź i wykonaj polecenie `exec sp_configure` w oknie zapytania. Pokazuje to, że środowisko clr włączone ustawienie aktualnie jest ustawiona na 0.


[![Środowisko clr, włączone ustawienie jest aktualnie ustawiona na 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Rysunek 12**: clr włączone ustawienie jest aktualnie ustawiona na wartość 0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Należy zauważyć, że każdego ustawienia konfiguracji w rysunek 12 ma cztery wartości na liście z nią: minimalne i maksymalne wartości i config oraz wykonywania wartości. Aby zaktualizować wartość konfiguracji ustawienia środowiska clr jest włączone, wykonaj następujące polecenie:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Jeśli ponownie uruchomisz `exec sp_configure` zobaczysz, że powyższe stwierdzenie zaktualizowana wartość konfiguracji clr włączone ustawienie s 1, ale czy wykonywania wartość nadal jest równa 0. Ta zmiana konfiguracji zaczną obowiązywać musimy wykonać [ `RECONFIGURE` polecenia](https://msdn.microsoft.com/library/ms176069.aspx), która ustawia wartość wykonywania do bieżącej wartości konfiguracji. Po prostu wprowadź `RECONFIGURE` w oknie zapytania i kliknij ikonę wykonywania na pasku narzędzi. Jeśli uruchamiasz `exec sp_configure` teraz powinno wyświetlić wartość 1 dla konfiguracji clr włączone ustawienie s i uruchomimy wartości.

Za pomocą Kończenie konfiguracji clr włączone możemy przystąpić do uruchomienia zarządzanej `GetDiscontinuedProducts` procedury składowanej. W oknie zapytania wprowadź, a następnie wykonaj polecenie `exec` `GetDiscontinuedProducts`. Wywoływanie procedury składowanej powoduje, że odpowiedni kod zarządzany w `GetDiscontinuedProducts` wykonać metodę. Ten kod generuje `SELECT` zapytanie, aby zwrócić wszystkie produkty, które zostaną przerwane i zwraca dane do aplikacji wywołującej jest program SQL Server Management Studio, w tym wystąpieniu. Management Studio otrzymuje te wyniki i wyświetla je w oknie wyników.


[![GetDiscontinuedProducts procedura składowana ma zwracać wszystkie wycofane produktów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Rysunek 13**: `GetDiscontinuedProducts` przechowywane procedury zwraca wszystkie wycofane produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Krok 5: Tworzenie zarządzanej procedury składowane, które akceptują parametry wejściowe

Wiele zapytań i procedur składowanych, które utworzyliśmy w tych samouczkach używano *parametry*. Na przykład w [tworzenie nowych procedur składowanych dla s wpisany zestaw danych TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczku utworzyliśmy procedury składowanej o nazwie `GetProductsByCategoryID` zaakceptowane, parametr wejściowy o nazwie `@CategoryID`. Procedura składowana zwróciła następnie wszystkie produkty którego `CategoryID` pola pasowało do wartości z podanym `@CategoryID` parametru.

Aby utworzyć zarządzanej procedury składowanej, która akceptuje parametry wejściowe, wystarczy określić tych parametrów w definicji metody s. Na przykład umożliwiają s dodać innego zarządzanego procedurę przechowywaną, aby `ManagedDatabaseConstructs` projektu o nazwie `GetProductsWithPriceLessThan`. Tę procedurę składowaną zarządzanych przyjmuje parametr wejściowy, określając cenę i zwróci wszystkie produkty którego `UnitPrice` pole jest mniejsza niż wartość parametru s.

Aby dodać nową procedurę składowaną do projektu, kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` nazwy projektu i dodać nową procedurę składowaną. Nadaj plikowi nazwę `GetProductsWithPriceLessThan.cs`. Jak widzieliśmy w kroku 3, spowoduje to utworzenie nowego pliku klasy C# za pomocą metody o nazwie `GetProductsWithPriceLessThan` umieszczony w `partial` klasy `StoredProcedures`.

Aktualizacja `GetProductsWithPriceLessThan` definicję metody s, tak że akceptuje [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) parametr wejściowy `price` i napisać kod, który może działać i zwracać wyniki zapytania:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan` Definicję metody s i kod przypomina definicją i kodem `GetDiscontinuedProducts` metoda utworzonego w kroku 3. Jedyne różnice są, `GetProductsWithPriceLessThan` metoda przyjmuje jako parametr wejściowy (`price`), `SqlCommand` s zapytanie zawiera parametr (`@MaxPrice`), a parametr jest dodawany do `SqlCommand` s `Parameters` kolekcja jest i przypisana wartość `price` zmiennej.

Po dodaniu tego kodu, należy ponownie wdrożyć projekt programu SQL Server. Następnie wróć do programu SQL Server Management Studio i Odśwież folder procedur składowanych. Powinien zostać wyświetlony nowy wpis `GetProductsWithPriceLessThan`. W oknie zapytania wprowadź, a następnie wykonaj polecenie `exec GetProductsWithPriceLessThan 25`, która wyświetli wszystkie produkty mniej niż 25 USD, jak pokazano na rysunku 14.


[![Produkty w wysokości 25 USD są wyświetlane](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Rysunek 14**: produktów w wysokości 25 USD są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Krok 6: Wywoływanie procedury składowanej zarządzanych z warstwy dostępu do danych

W tym momencie dodaliśmy `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedur składowanych na potrzeby `ManagedDatabaseConstructs` projektu i zarejestrować je z bazą danych Northwind programu SQL Server. Możemy również wywoływane tych procedurach składowanych zarządzanych z programu SQL Server Management Studio (patrz rysunek s 13 i 14). Aby naszej platformy ASP.NET aplikacji do korzystania z tych zarządzane procedur składowanych, jednak należy dodać je do dostępu do danych i warstwy logiki biznesowej w architekturze. W tym kroku zostanie dodany dwie nowe metody `ProductsTableAdapter` w `NorthwindWithSprocs` wpisana zestawu danych, która została początkowo utworzona w [tworzenie nowych procedur składowanych dla s wpisany zestaw danych TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka. W kroku 7 dodamy odpowiednie metody do LOGIKI.

Otwórz `NorthwindWithSprocs` wpisana zestawu danych w programie Visual Studio i Rozpocznij, dodając nową metodę `ProductsTableAdapter` o nazwie `GetDiscontinuedProducts`. Aby dodać nowe metody TableAdapter, kliknij prawym przyciskiem myszy nazwę s TableAdapter w projektancie, a następnie wybierz opcję Dodaj zapytanie z menu kontekstowego.

> [!NOTE]
> Ponieważ przeszliśmy z bazy danych Northwind `App_Data` folderu z wystąpieniem bazy danych programu SQL Server 2005 Express Edition, konieczne jest zaktualizowana odpowiednie parametry połączenia w pliku Web.config w celu odzwierciedlenia tej zmiany. W kroku 2 opisano sposób aktualizowania `NORTHWNDConnectionString` wartość w `Web.config`. Jeśli nie pamiętasz dokonać tej aktualizacji, następnie zobaczysz komunikat o błędzie można dodać zapytania. Nie można odnaleźć połączenia `NORTHWNDConnectionString` dla obiektu `Web.config` w oknie dialogowym w trakcie próby Dodaj nową metodę do TableAdapter. Aby rozwiązać ten problem, kliknij przycisk OK, a następnie przejdź do `Web.config` i zaktualizuj `NORTHWNDConnectionString` wartość zgodnie z opisem w kroku 2. Spróbuj ponowne dodanie metody do TableAdapter. Teraz powinien działać bez błędów.


Dodawanie nowej metody powoduje uruchomienie Kreatora konfiguracji zapytania TableAdapter, w którym użyto wiele razy w ciągu ostatnich samouczków. Pierwszym krokiem prosi NAS, aby określić, jak TableAdapter powinien uzyskiwać dostęp do bazy danych: za pomocą instrukcji SQL zapytań ad-hoc lub za pomocą nowej lub istniejącej procedury składowanej. Ponieważ firma Microsoft wcześniej utworzony i zarejestrowany `GetDiscontinuedProducts` zarządzanej procedury składowanej z bazą danych, wybierz pozycję Użyj istniejącej opcji procedury przechowywane i trafień dalej.


[![Wybierz opcję użycia istniejącej procedury składowanej — opcja](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Rysunek 15**: wybierz opcję Użyj istniejącej przechowywane procedury opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


Następny ekran nam monituje o podanie procedury składowanej, który będzie wywoływał metodę. Wybierz `GetDiscontinuedProducts` zarządzane procedurę składowaną z listy rozwijanej, a następnie kliknij przycisk Dalej.


[![Wybierz GetDiscontinuedProducts zarządzanego procedury składowanej](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Rysunek 16**: Wybierz `GetDiscontinuedProducts` zarządzane Stored Procedure ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Firma Microsoft następnie monit o określenie, czy procedura składowana ma zwracać wiersze, pojedynczą wartość lub nie rób. Ponieważ `GetDiscontinuedProducts` zwraca zestaw wierszy nieobsługiwane produktu z pierwszej opcji (dane tabelaryczne), a następnie kliknij przycisk Dalej.


[![Wybierz opcję dane tabelaryczne](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Rysunek 17**: wybierz opcję danych tabelarycznych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


Ekran końcowy Kreator pozwala określać wzorce dostępu do danych, które są używane, jak i nazwy metod wynikowe. Pozostaw zaznaczone pola wyboru i nazwę metody `FillByDiscontinued` i `GetDiscontinuedProducts`. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Nazwa FillByDiscontinued metod i GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Rysunek 18**: Nazwa metody `FillByDiscontinued` i `GetDiscontinuedProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Powtórz te kroki, aby utworzyć metody o nazwie `FillByPriceLessThan` i `GetProductsWithPriceLessThan` w `ProductsTableAdapter` dla `GetProductsWithPriceLessThan` zarządzane procedury składowanej.

Rysunek 19 przedstawiono zrzut ekranu przedstawiający Projektanta obiektów DataSet, po dodaniu metody służące do `ProductsTableAdapter` dla `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedur składowanych.


[![ProductsTableAdapter obejmuje nowe metody, które są dodawane w tym kroku](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Rysunek 19**: `ProductsTableAdapter` obejmuje nowe metody dodane, w tym kroku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Krok 7: Dodawanie odpowiadających im metod do warstwy logiki biznesowej

Teraz, gdy zaktualizowano warstwy dostępu do danych w celu uwzględnienia metod wywoływania zarządzanego procedur składowanych, dodać kroki 4 i 5, należy dodać odpowiednie metody do warstwy logiki biznesowej. Dodaj następujące dwie metody `ProductsBLLWithSprocs` klasy:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Obie metody po prostu wywołanie odpowiedniej metody DAL i zwracają `ProductsDataTable` wystąpienia. `DataObjectMethodAttribute` Znaczników powyżej każdej metody powoduje, że te metody, które mają zostać uwzględnione na liście rozwijanej w karcie Wybierz kreatora skonfiguruj źródło danych elementu ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Krok 8: Wywoływanie zarządzanej procedury składowane z warstwy prezentacji

Przy użyciu logiki biznesowej i warstwy dostępu do danych rozszerzony o obsługę wywoływania `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedur składowanych, możemy teraz wyświetlić tych przechowywanych procedur wyniki za pośrednictwem strony ASP.NET.

Otwórz `ManagedFunctionsAndSprocs.aspx` stronie `AdvancedDAL` folder i z przybornika przeciągnij GridView do konstruktora. Ustaw GridView s `ID` właściwości `DiscontinuedProducts` i z jego tag inteligentny powiązać go do nowego elementu ObjectDataSource, o nazwie `DiscontinuedProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource swoich danych z `ProductsBLLWithSprocs` klasy s `GetDiscontinuedProducts` metody.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Rysunek 20**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Wybierz metodę GetDiscontinuedProducts z listy rozwijanej wybierz kartę](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Rysunek 21**: Wybierz `GetDiscontinuedProducts` metodę z listy rozwijanej wybierz karcie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Ponieważ ta siatka będzie służyć do tylko wyświetlanie informacji o produkcie, ustawić list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak), a następnie kliknij przycisk Zakończ.

Po ukończeniu kreatora, Visual Studio spowoduje automatyczne dodanie elementu BoundField lub CheckBoxField dla każdego pola danych w `ProductsDataTable`. Poświęć chwilę, aby usunąć wszystkie te pola z wyjątkiem `ProductName` i `Discontinued`, jaką punktu Usługi GridView i oznaczeniu deklaracyjnym s ObjectDataSource powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Gdy odwiedzenia strony, wywołań elementu ObjectDataSource `ProductsBLLWithSprocs` klasy s `GetDiscontinuedProducts` metody. Jak widzieliśmy w kroku 7, ta metoda wywołuje w dół do DAL s `ProductsDataTable` klasy s `GetDiscontinuedProducts` metody, która wywołuje `GetDiscontinuedProducts` procedury składowanej. Procedura składowana jest zarządzanej procedury składowanej i wykonuje kod, utworzonego w kroku 3, zwracając uwzględniałyby produkty.

Wyniki zwrócone przez procedurę składowaną zarządzane są pakowane w w `ProductsDataTable` przez warstwę DAL a później wrócili do LOGIKI, która zwraca je do warstwy prezentacji, gdzie są powiązane z kontrolki GridView i wyświetlane. Zgodnie z oczekiwaniami, siatka zawiera listę tych produktów, które zostały anulowane.


[![Produkty wycofane](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Rysunek 22**: produkty wycofane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Dla dalszych rozwiązaniem Dodaj pole tekstowe i innej kontrolki GridView do strony. Ma to GridView Wyświetl produkty mniejsza niż wartość wprowadzona w polu tekstowym, wywołując `ProductsBLLWithSprocs` klasy s `GetProductsWithPriceLessThan` metody.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Krok 9: Tworzenie i wywoływanie funkcji UDF języka T-SQL

Funkcje zdefiniowane przez użytkownika lub funkcje zdefiniowane przez użytkownika, czy obiekty bazy danych ściśle mimic semantykę funkcji w językach programowania. Takich jak funkcji w języku C# funkcje zdefiniowane przez użytkownika można uwzględnić zmienną liczbę parametrów wejściowych i zwraca wartość określonego typu. Funkcji zdefiniowanej przez użytkownika może zwracać albo danych skalarnych — ciąg, liczba całkowita i tak dalej — lub dane tabelaryczne. Pozwól, s, Przyjrzyj się szybkie oba rodzaje funkcje zdefiniowane przez użytkownika, zaczynając od funkcji zdefiniowanej przez użytkownika, który zwraca typ danych skalarnych.

Następujące UDF oblicza wartość szacunkową spisu dla określonego produktu. Robi to przy podejmowaniu w trzech parametrów wejściowych - `UnitPrice`, `UnitsInStock`, i `Discontinued` wartości dla konkretnego produktu — i zwraca wartość typu `money`. Oblicza wartość szacunkową magazynu przez pomnożenie `UnitPrice` przez `UnitsInStock`. Nieobsługiwane elementów ta wartość jest o połowę.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Po dodaniu tej funkcji zdefiniowanej przez użytkownika w bazie danych można znaleźć za pomocą programu Management Studio, rozszerzając programowania folder, a następnie funkcji, a następnie wartości skalarnych. Mogą być używane w `SELECT` zapytanie w następujący sposób:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Po dodaniu `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika w bazie danych Northwind; Ilustracja 23 zawierają dane wyjściowe powyższych `SELECT` zapytania po wyświetleniu za pomocą programu Management Studio. Należy również zauważyć, że funkcji zdefiniowanej przez użytkownika znajduje się w folderze wartości skalarnych w Eksploratorze obiektów.


[![Każdy produkt s wartości magazynu znajduje się na liście](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Ilustracja 23**: s każdego produktu znajduje się na liście wartości magazynu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Funkcje zdefiniowane przez użytkownika może również zwracać dane tabelaryczne. Na przykład możemy utworzyć funkcji zdefiniowanej przez użytkownika, która zwraca produkty, które należą do określonej kategorii:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` Akceptuje UDF `@CategoryID` parametr wejściowy i zwraca wyniki z określonym `SELECT` zapytania. Po utworzeniu tej funkcji zdefiniowanej przez użytkownika może być przywoływany w `FROM` (lub `JOIN`) klauzuli `SELECT` zapytania. Poniższy przykład zwróci `ProductID`, `ProductName`, i `CategoryID` wartości dla każdej beverages.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Po dodaniu `udf_GetProductsByCategoryID` funkcji zdefiniowanej przez użytkownika w bazie danych Northwind; 24 rysunek przedstawia dane wyjściowe powyższych `SELECT` zapytania po wyświetleniu za pomocą programu Management Studio. Funkcje zdefiniowane przez użytkownika, które zwracają dane tabelaryczne znajdują się w folderze funkcji wartości tabeli s Eksplorator obiektów.


[![ProductID, ProductName i CategoryID są wyświetlane dla każdego spożywczy](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Rysunek 24**: `ProductID`, `ProductName`, i `CategoryID` są wyświetlane dla każdego spożywczy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Aby uzyskać więcej informacji dotyczących tworzenia i używania funkcji UDF, zapoznaj się z [wprowadzenie do funkcji User-defined](http://www.sqlteam.com/item.asp?ItemID=1955). Sprawdź również [korzyści i funkcje Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Krok 10. tworzenie zarządzanych funkcji zdefiniowanej przez użytkownika

`udf_ComputeInventoryValue` i `udf_GetProductsByCategoryID` funkcje zdefiniowane przez użytkownika, utworzony w powyższych przykładach są obiektami bazy danych języka T-SQL. SQL Server 2005 obsługuje również zarządzanych funkcje zdefiniowane przez użytkownika, które mogą być dodawane do `ManagedDatabaseConstructs` projektu tak samo, jak zarządzany procedur składowanych z kroki 3 i 5. W tym kroku umożliwić s zaimplementować `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika w kodzie zarządzanym.

Aby dodać zarządzane UDF do `ManagedDatabaseConstructs` projektu, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz opcję dodać nowy element. Wybierz szablon zdefiniowane przez użytkownika w oknie dialogowym Dodaj nowy element i nadaj nowemu plikowi UDF `udf_ComputeInventoryValue_Managed.cs`.


[![Dodaj nowe UDF zarządzanego projektu ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Rysunek 25**: Dodaj nowy UDF zarządzane do `ManagedDatabaseConstructs` projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Tworzy szablon funkcji User-defined `partial` klasę o nazwie `UserDefinedFunctions` przy użyciu metody, których nazwa jest taka sama jak nazwa pliku s klasy (`udf_ComputeInventoryValue_Managed`, w tym wystąpieniu). Ta metoda jest urządzony przy użyciu [ `SqlFunction` atrybut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), który flagi metodę jako zarządzanej funkcji zdefiniowanej przez użytkownika.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue` Metoda obecnie zwraca [ `SqlString` obiektu](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) i nie akceptuje dowolne parametry wejściowe. Należy zaktualizować definicję metody, dzięki czemu przyjmuje trzy parametry - wejściowe `UnitPrice`, `UnitsInStock`, i `Discontinued` — i zwraca `SqlMoney` obiektu. Logika do obliczania wartości magazynu jest identyczna z języka T-SQL `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Należy zauważyć, że parametry wejściowe metoda s funkcji zdefiniowanej przez użytkownika są jako odpowiednie typy SQL: `SqlMoney` dla `UnitPrice` pola [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) dla `UnitsInStock`, i [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) Aby uzyskać `Discontinued`. Te typy danych odzwierciedlają typów zdefiniowanych w `Products` tabeli: `UnitPrice` kolumna jest kolumną typu `money`, `UnitsInStock` kolumny typu `smallint`i `Discontinued` kolumny typu `bit`.

Zaczyna się kod, tworząc `SqlMoney` wystąpienia o nazwie `inventoryValue` przypisany wartość 0. `Products` Umożliwia tabeli bazy danych `NULL` wartości w `UnitsInPrice` i `UnitsInStock` kolumn. W związku z tym, musimy najpierw Jeśli te wartości zawierają `NULL` s, co możemy zrobić za pomocą `SqlMoney` obiektu s [ `IsNull` właściwość](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Jeśli oba `UnitPrice` i `UnitsInStock` zawierają non -`NULL` wartości, a następnie możemy obliczyć `inventoryValue` jako produktu dwie. Następnie, jeśli `Discontinued` ma wartość true, a następnie możemy połowę wartość.

> [!NOTE]
> `SqlMoney` Obiektu może składać się dwa `SqlMoney` wystąpień mnoży się ze sobą. Nie zezwalaj `SqlMoney` wystąpienia pomnożona przez literału liczba zmiennoprzecinkowa. W związku z tym o połowę `inventoryValue` firma Microsoft należy go pomnożyć przez nową `SqlMoney` wystąpienia, który ma wartość 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Krok 11: Wdrażanie zarządzanych UDF

Skoro zarządzanych UDF została już utworzona, jesteśmy gotowi wdrożyć ją z bazą danych Northwind. Jak widzieliśmy w kroku 4 obiektów zarządzanych w projekcie programu SQL Server są wdrażane, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierając opcję wdrażania z poziomu menu kontekstowego.

Wdrożenie projektu wróć do programu SQL Server Management Studio i Odśwież folder skalarne. Zostaną wyświetlone dwie pozycje:

- `dbo.udf_ComputeInventoryValue` -UDF języka T-SQL utworzony w kroku 9 i
- `dbo.udf ComputeInventoryValue_Managed` -zarządzanych UDF utworzony w kroku 10, który właśnie został wdrożony.

Aby przetestować ten zarządzanych funkcji zdefiniowanej przez użytkownika, wykonaj następujące zapytanie z poziomu programu Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

To polecenie używa zarządzanej `udf ComputeInventoryValue_Managed` funkcji zdefiniowanej przez użytkownika zamiast języka T-SQL `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika, ale dane wyjściowe jest taka sama. Odwołaj się do 23 rysunku, aby wyświetlić zrzut ekranu danych wyjściowych s funkcji zdefiniowanej przez użytkownika.

## <a name="step-12-debugging-the-managed-database-objects"></a>Krok 12: Debugowanie obiektami zarządzanej bazy danych

W [debugowanie procedur składowanych](debugging-stored-procedures-cs.md) samouczku Omówiliśmy trzy opcje debugowania programu SQL Server za pomocą programu Visual Studio: bezpośredniego debugowania bazy danych, debugowania aplikacji i debugowanie z projektu programu SQL Server. Zarządzane bazy danych obiekty nie można debugować za pomocą bezpośredniego debugowania bazy danych, ale może być debugowany, od aplikacji klienckiej i bezpośrednio z projekt programu SQL Server. Aby debugowania do pracy jednak bazy danych programu SQL Server 2005 muszą zezwalać na debugowanie SQL/CLR. Pamiętamy, podczas pierwszego utworzenia `ManagedDatabaseConstructs` projektu programu Visual Studio pytanie, nam czy Chcieliśmy, aby włączyć debugowanie (patrz rysunek 6 w kroku 2) SQL/CLR. To ustawienie można zmodyfikować, klikając prawym przyciskiem myszy w bazie danych z okna Eksploratora serwera.


![Upewnij się, że baza danych umożliwia debugowanie SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Rysunek 26**: Upewnij się, że baza danych umożliwia debugowanie SQL/CLR


Wyobraź sobie, że Chcieliśmy, aby debugować `GetProductsWithPriceLessThan` zarządzane procedury składowanej. Zaczniemy przez ustawienie punktu przerwania w kodzie `GetProductsWithPriceLessThan` metody.


[![Ustaw punkt przerwania w metodzie GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Rysunek 27**: Ustaw punkt przerwania `GetProductsWithPriceLessThan` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Pozwól s Pierwsze spojrzenie na profilowanie obiektami zarządzanej bazy danych z projektu programu SQL Server. Ponieważ rozwiązanie zawiera dwa projekty — `ManagedDatabaseConstructs` projekt programu SQL Server wraz z naszej witryny sieci Web — Aby debugować z projektu serwera SQL, należy poinstruować Visual Studio, aby uruchomić `ManagedDatabaseConstructs` Projekt serwera SQL, gdy Rozpoczniemy debugowania. Kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` projektu w Eksploratorze rozwiązań, a następnie wybierz zestaw jako projekt startowy opcję z menu kontekstowego.

Gdy `ManagedDatabaseConstructs` projektu zostanie uruchomione z debugera, wykonuje instrukcje SQL w `Test.sql` pliku, który znajduje się w `Test Scripts` folderu. Na przykład, aby przetestować `GetProductsWithPriceLessThan` zarządzane procedury składowanej, Zastąp istniejące `Test.sql` plików zawartości przy użyciu następujących instrukcji, które wywołuje `GetProductsWithPriceLessThan` zarządzane procedury składowanej, przekazując `@CategoryID` wartość 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Powyższy skrypt do wprowadzone był `Test.sql`, rozpocząć debugowanie, przechodząc do menu Debuguj i wybierając Rozpocznij debugowanie lub naciskać klawisz F5 lub zielony odtwarzania ikonę na pasku narzędzi. To będzie kompilować projekty w rozwiązaniu, wdrażanie obiektami zarządzanej bazy danych w bazie danych Northwind, a następnie wykonywania `Test.sql` skryptu. W tym momencie punkt przerwania zostanie osiągnięty, a firma Microsoft może przejść przez `GetProductsWithPriceLessThan` metody, sprawdź wartości parametrów wejściowych i tak dalej.


[![Został trafiony punkt przerwania w metodzie GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Rysunek 28**: punkt przerwania w `GetProductsWithPriceLessThan` został trafiony — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


Aby dla obiektu bazy danych SQL do debugowania za pomocą aplikacji klienckiej konieczne jest skonfigurowania bazy danych do obsługi debugowania aplikacji. Kliknij prawym przyciskiem myszy w bazie danych w Eksploratorze serwera i upewnij się, że zaznaczono opcję debugowania aplikacji. Ponadto należy skonfigurować aplikację ASP.NET do integracji z debugerem SQL i wyłączenie buforowania połączeń. Te kroki zostały szczegółowo omówione w kroku 2 [debugowanie procedur składowanych](debugging-stored-procedures-cs.md) samouczka.

Po skonfigurowaniu aplikacji ASP.NET i bazy danych, ustaw witryny sieci Web platformy ASP.NET jako projekt startowy i Rozpocznij debugowanie. Jeśli strona wywołuje jedną zarządzanych obiektów, które ma punkt przerwania, aplikacja zostanie zatrzymany i sterowania zostanie włączony za pośrednictwem do debugera, gdzie możesz przejrzeć kod pokazany na rysunku 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Krok 13: Ręcznie kompilowanie i wdrażanie zarządzane obiekty bazy danych

Projekty z serwera SQL ułatwiają tworzenie, kompilowanie i wdrażanie obiektami zarządzanej bazy danych. Niestety projekty z serwera SQL są dostępne tylko w wersjach Professional i systemy zespołu programu Visual Studio. Jeśli używasz Visual Web Developer i Standard Edition z programu Visual Studio i ma być używany z obiektami zarządzanej bazy danych, należy ręcznie utworzyć i wdrożyć je. Obejmuje to cztery kroki:

1. Utwórz plik, który zawiera kod źródłowy dla obiektu zarządzanej bazy danych
2. Skompiluj obiekt do zestawu
3. Rejestrowanie zestawów z bazą danych programu SQL Server 2005 i
4. Utwórz obiekt bazy danych w programie SQL Server, który wskazuje na odpowiedniej metody w zestawie.

Aby zilustrować tych zadań, niech s, Utwórz nową zarządzanych przez procedury przechowywanej zwracającej tych produktów którego `UnitPrice` jest większa niż określona wartość. Utwórz nowy plik na komputerze o nazwie `GetProductsWithPriceGreaterThan.cs` i wprowadź następujący kod do pliku (umożliwia Visual Studio, Notatnik lub dowolnego edytora tekstów w tym):


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Ten kod jest niemal identyczny jak w przypadku `GetProductsWithPriceLessThan` metoda utworzony w kroku 5. Jedyne różnice są nazwy metod `WHERE` klauzula i nazwę parametru użytego w zapytaniu. Ponownie `GetProductsWithPriceLessThan` metody `WHERE` klauzuli odczytu: `WHERE UnitPrice < @MaxPrice`. W tym miejscu, w `GetProductsWithPriceGreaterThan`, używamy: `WHERE UnitPrice > @MinPrice` .

Musimy teraz skompilować tej klasy w zestawie. W wierszu polecenia przejdź do katalogu, w którym został zapisany `GetProductsWithPriceGreaterThan.cs` pliku i użyć kompilatora języka C# (`csc.exe`) aby skompilować plik klasy do zestawu:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Jeśli folder zawierający `csc.exe` w nie w systemie s `PATH`, trzeba będzie w pełni odwoływać się do jego ścieżki `%WINDOWS%\Microsoft.NET\Framework\version\`, w następujący sposób:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Skompilować GetProductsWithPriceGreaterThan.cs do zestawu](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Rysunek 29**: kompilowanie `GetProductsWithPriceGreaterThan.cs` do zestawu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t` Flaga Określa, czy plik klasy C# mają być łączone w Biblioteka DLL (a nie plikiem wykonywalnym). `/out` Flagi Określa nazwę zestawu wynikowego.

> [!NOTE]
> Zamiast kompilowanie `GetProductsWithPriceGreaterThan.cs` plik klasy z wiersza polecenia, można też użyć [Visual C# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) lub utworzyć oddzielny projekt biblioteki klas w Visual Studio Standard Edition. S ren Jacob Lauritsen uprzejmie udostępnił taki projekt Visual C# Express Edition z kodem dla `GetProductsWithPriceGreaterThan` procedur składowanych i dwa zarządzane procedur składowanych i funkcji definiowanych przez użytkownika utworzony w kroku 3, 5 i 10. S ren s projektu obejmuje również polecenia T-SQL, aby dodać odpowiednie obiekty bazy danych.


Kod skompilowany w zestawie jesteśmy gotowi zarejestrować zestaw w bazie danych programu SQL Server 2005. Można to wykonać za pomocą języka T-SQL, za pomocą polecenia `CREATE ASSEMBLY`, lub za pomocą programu SQL Server Management Studio. Pozwól s fokus za pomocą programu Management Studio.

W programie Management Studio rozwiń folder programowania w bazie danych Northwind. Jednym z jego podfolderów jest zestawów. Aby ręcznie dodać nowego zestawu z bazą danych, kliknij prawym przyciskiem myszy w folderze zestawy, a następnie wybierz nowego zestawu z menu kontekstowego. Ten wyświetla okno dialogowe Nowy zestaw polu (zobacz rysunek 30). Kliknij przycisk przeglądania, wybierz opcję `ManuallyCreatedDBObjects.dll` zestawu możemy po prostu skompilowany, a następnie kliknij OK, aby dodać zestaw do bazy danych. Nie powinien zostać wyświetlony `ManuallyCreatedDBObjects.dll` zestawu w Eksploratorze obiektów.


[![Dodaj zestaw ManuallyCreatedDBObjects.dll do bazy danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**30 rysunek**: Dodaj `ManuallyCreatedDBObjects.dll` zestawu do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![ManuallyCreatedDBObjects.dll znajduje się w Eksploratorze obiektów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Rysunek 31**: `ManuallyCreatedDBObjects.dll` znajduje się w Eksploratorze obiektów


Dodaliśmy zestawu z bazą danych Northwind, mamy jeszcze do skojarzenia z procedury składowanej z `GetProductsWithPriceGreaterThan` metody w zestawie. Aby to zrobić, Otwórz nowe okno zapytania i uruchom następujący skrypt:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Tworzy nową procedurę składowaną w bazie danych Northwind, o nazwie `GetProductsWithPriceGreaterThan` i kojarzy ją z zarządzaną metodą `GetProductsWithPriceGreaterThan` (która znajduje się w klasie `StoredProcedures`, która znajduje się w zestawie `ManuallyCreatedDBObjects`).

Po wykonaniu powyższych skryptu, Odśwież folder procedur składowanych w Eksploratorze obiektów. Powinien zostać wyświetlony nowy wpis procedury składowanej — `GetProductsWithPriceGreaterThan` -mającego ikoną kłódki obok niej. Aby przetestować tę procedurę składowaną, wprowadź i uruchom następujący skrypt w oknie zapytania:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Jak pokazano na rysunku 32, powyższe polecenie wyświetla informacje dotyczące tych produktów z `UnitPrice` większa niż 24.95 $.


[![ManuallyCreatedDBObjects.dll znajduje się w Eksploratorze obiektów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Ilustracja 32**: `ManuallyCreatedDBObjects.dll` znajduje się w Eksploratorze obiektów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Podsumowanie

Microsoft SQL Server 2005 zapewnia integrację z języka wspólnego środowiska uruchomieniowego (CLR), co pozwala obiekty bazy danych ma zostać utworzony przy użyciu kodu zarządzanego. Wcześniej tylko można utworzyć te obiekty bazy danych przy użyciu języka T-SQL, ale teraz możemy utworzyć te obiekty przy użyciu platformy .NET, Programowanie w językach C#. W tym samouczku, którą utworzyliśmy dwa zarządzane procedur przechowywanych i funkcji zarządzanej zdefiniowane przez użytkownika.

Program Visual Studio s typu Projekt programu SQL Server ułatwia tworzenie, kompilowanie i wdrażanie obiektami zarządzanej bazy danych. Ponadto oferuje ona zaawansowane obsługę debugowania. Jednak typów projekt programu SQL Server są dostępne tylko w wersjach Professional i systemy zespołu programu Visual Studio. Dla osób, przy użyciu programu Visual Web Developer lub Standard Edition z programu Visual Studio, tworzenia, kompilacji i kroki wdrażania muszą być wykonywane ręcznie, jak widzieliśmy w kroku 13.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zalety i wady funkcji zdefiniowanych przez użytkownika](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Tworzenie obiekty programu SQL Server 2005 w kodzie zarządzanym](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Tworzenie wyzwalaczy przy użyciu kodu zarządzanego w programie SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Instrukcje: Tworzenie i uruchamianie środowiska CLR programu SQL Server procedury składowanej](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Instrukcje: Tworzenie i uruchamianie funkcji zdefiniowanej przez użytkownika serwera CLR SQL](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Porady: Edytowanie `Test.sql` skrypt do uruchomienia obiekty SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Funkcje zdefiniowane przez wprowadzenie do użytkownika](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Kodu zarządzanego i programu SQL Server 2005 (wideo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Dokumentacja języka Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Wskazówki: Tworzenie procedury składowanej w kodzie zarządzanym](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został ren S Jacob Lauritsen. Oprócz przeglądania tego artykułu, S ren również utworzony projekt Visual C# Express Edition, zawarte w tym artykule s do kompilowania ręcznie obiektami zarządzanej bazy danych. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](debugging-stored-procedures-cs.md)
> [dalej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
