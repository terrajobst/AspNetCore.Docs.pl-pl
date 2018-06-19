---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Tworzenie procedury składowane i funkcje zdefiniowane przez użytkownika za pomocą zarządzanego kodu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Microsoft SQL Server 2005 integruje się z .NET środowisko uruchomieniowe języka wspólnego umożliwia deweloperom tworzenie obiektów bazy danych za pomocą kodu zarządzanego. W tym samouczku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5a860c8ab6ad7ff04de2175900491d532db782d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877440"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Tworzenie procedury składowane i funkcje zdefiniowane przez użytkownika za pomocą kodu zarządzanego (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) lub [pobierania plików PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 integruje się z .NET środowisko uruchomieniowe języka wspólnego umożliwia deweloperom tworzenie obiektów bazy danych za pomocą kodu zarządzanego. W tym samouczku przedstawiono sposób tworzenia zarządzanej procedury składowane i zarządzane funkcje zdefiniowane przez użytkownika za pomocą kodu języka Visual Basic lub C#. Również sprawdzić, jak te wersje programu Visual Studio umożliwiają debugowania takie obiekty zarządzane bazy danych.


## <a name="introduction"></a>Wprowadzenie

Użyj bazy danych, takich jak Microsoft SQL Server 2005-s [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) do wstawiania, modyfikowania i pobierania danych. Większość systemów bazy danych zawiera konstrukcji grupowania serii instrukcji SQL, które następnie mogą być wykonywane jako jednostka jednej, wielokrotnego użytku. Procedury składowane są jednym z przykładów. Inny jest *funkcje zdefiniowane przez użytkownika*(UDF) konstrukcję omówione bardziej szczegółowo w kroku 9.

Zasadniczo SQL jest przeznaczony do pracy z zestawów danych. `SELECT`, `UPDATE`, I `DELETE` instrukcje dotyczą wszystkich rekordów w tej tabeli z założenia i tylko są ograniczone przez ich `WHERE` klauzul. Brak jeszcze wiele funkcji języka zaprojektowane do pracy z jeden rekord w czasie i manipulowania danymi skalarne. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) umożliwiają dla zestawu rekordów można zapętlonego za pomocą jednego naraz. Ciąg manipulowania funkcji, takich jak `LEFT`, `CHARINDEX`, i `PATINDEX` pracować z danymi skalarne. SQL zawiera również instrukcje przepływu sterowania, takie jak `IF` i `WHILE`.

Przed Microsoft SQL Server 2005 procedury składowane i funkcje UDF tylko można zdefiniować jako kolekcja instrukcje T-SQL. SQL Server 2005, jednak zaprojektowano tak, aby zapewnić integrację z [środowiska uruchomieniowego języka wspólnego (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), który jest używany przez wszystkie zestawy .NET środowiska uruchomieniowego. W związku z tym procedury składowane i funkcje UDF w bazie danych programu SQL Server 2005 mogą być tworzone za pomocą kodu zarządzanego. Oznacza to można utworzyć procedury składowanej lub funkcji zdefiniowanej przez użytkownika jako metodę w klasie C#. Dzięki temu te procedury składowane i funkcje UDF mogą korzystać z funkcji w programie .NET Framework i z klas niestandardowych.

W tym samouczku, który zostanie tworzenie zarządzanych procedury składowane i funkcje zdefiniowane przez użytkownika i sposobu integracji je w naszej bazie danych Northwind. Rozpoczynanie pracy dzięki s!

> [!NOTE]
> Obiekty zarządzane bazy danych oferuje kilka zalet w porównaniu z ich odpowiedniki SQL. Siłę języka i znajomości i możliwość ponownego używania istniejącego kodu i logikę są główne zalety. Ale obiekty zarządzane bazy danych może być mniej wydajne, podczas pracy z zestawami danych, które nie obejmują wiele procedur logiki. Aby uzyskać dokładniejsze omówienie o zaletach korzystania z kodu zarządzanego lub T-SQL, zapoznaj się [korzyści z za pomocą kodu zarządzanego do tworzenia obiektów bazy danych](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Krok 1: Przenoszenie bazy danych Northwind poza`App_Data`

Wszystkie z naszych samouczków dotychczasowych używanych pliku bazy danych programu Microsoft SQL Server 2005 Express Edition w aplikacji sieci web s `App_Data` folderu. Wprowadzenie do bazy danych w `App_Data` uproszczony dystrybucji i uruchomione te samouczki, wszystkie pliki zostały zlokalizowane w jednym katalogu, a wymagane nie dodatkowe czynności konfiguracyjne do testowania w samouczku.

Jednak w tym samouczku let s przenoszenia bazy danych Northwind poza `App_Data` i zarejestruj go jawnie wystąpienie bazy danych programu SQL Server 2005 Express Edition. Gdy firma Microsoft wykonaj kroki w tym samouczku z bazy danych w `App_Data` folderu, liczba kroków zostają znacznie prostsza jawnie rejestrując bazy danych w wystąpieniu bazy danych programu SQL Server 2005 Express Edition.

Pobierania w tym samouczku ma dwa plików bazy danych programu - `NORTHWND.MDF` i `NORTHWND_log.LDF` — umieszczony w folderze o nazwie `DataFiles`. Jeśli wykonujesz wraz z implementacji samouczki, zamknij program Visual Studio i Przenieś `NORTHWND.MDF` i `NORTHWND_log.LDF` plików z witryny sieci Web s `App_Data` folderu do folderu poza witryny sieci Web. Po przeniesieniu plików bazy danych do innego folderu, należy zarejestrować bazy danych Northwind wystąpienie bazy danych programu SQL Server 2005 Express Edition. Można to zrobić z programu SQL Server Management Studio. Jeśli masz z systemem innym niż - Express Edition programu SQL Server 2005 zainstalowana na danym komputerze następnie prawdopodobnie masz już Management Studio zainstalowany. Jeśli tylko na komputerze znajduje się program SQL Server 2005 Express Edition, Poświęć chwilę, aby pobrać i zainstalować [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Uruchom program SQL Server Management Studio. Jak pokazano na rysunku 1, Management Studio rozpoczyna się od pytaniem, jakiego serwera do nawiązania połączenia. Wprowadź localhost\SQLExpress dla nazwy serwera, wybierz z listy rozwijanej uwierzytelniania uwierzytelniania systemu Windows, a następnie kliknij przycisk Połącz.


![Połącz się z wystąpieniem odpowiednią bazę danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Rysunek 1**: Połącz się z wystąpieniem odpowiednią bazę danych


Po nawiązaniu połączenia był okna Eksplorator obiektów spowoduje wyświetlenie listy informacji na temat tego wystąpienia bazy danych programu SQL Server 2005 Express Edition, łącznie z jej baz danych, informacje o zabezpieczeniach, opcji zarządzania i tak dalej.

Musimy dołączyć bazy danych Northwind w `DataFiles` folderu (lub wszędzie tam, gdzie może przeniesiono go) w wystąpieniu bazy danych programu SQL Server 2005 Express Edition. Kliknij prawym przyciskiem folder baz danych i wybierz opcję dołączenia z menu kontekstowego. Zostanie wyświetlone okno dialogowe dołączanie bazy danych. Kliknij przycisk Dodaj, następnie przejść do odpowiedniej `NORTHWND.MDF` pliku, a następnie kliknij przycisk OK. W tym momencie ekranie powinien wyglądać podobnie do rysunku 2.


[![Połącz się z wystąpieniem odpowiednią bazę danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Rysunek 2**: połączyć się z odpowiednim wystąpieniem bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> Podczas nawiązywania połączenia z wystąpieniem programu SQL Server 2005 Express Edition, za pomocą Management Studio w oknie dialogowym dołączanie bazy danych nie pozwalają na przechodzenie do katalogów profilu użytkownika, takie jak Moje dokumenty. W związku z tym, upewnij się umieścić `NORTHWND.MDF` i `NORTHWND_log.LDF` pliki w katalogu profilu niezwiązanych z użytkownikiem.


Kliknij przycisk OK, aby dołączyć bazę danych. Eksplorator obiektów powinny teraz lista just dołączonej bazy danych i zostanie zamknięte okno dialogowe dołączanie bazy danych. Prawdopodobnie Northwind baza danych ma nazwę, takich jak `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Zmień nazwę bazy danych do Northwind prawym przyciskiem myszy w bazie danych i wybierając polecenie zmiany nazwy.


![Zmień nazwę bazy danych na Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Rysunek 3**: Zmień nazwę bazy danych na Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Krok 2: Tworzenie nowego rozwiązania i projektu serwera SQL w programie Visual Studio

Do tworzenia zarządzanego procedury składowanej lub funkcji UDF w programie SQL Server 2005 firma Microsoft będzie zapisywać procedur składowanych i logiki funkcji zdefiniowanej przez użytkownika jako kod języka C# w klasie. Po zapisaniu kodu są wymagane do kompilacji tej klasy w zestawie ( `.dll` pliku), zarejestruj zestawu z bazą danych programu SQL Server, a następnie utworzyć procedury składowanej lub funkcji zdefiniowanej przez użytkownika w bazie danych, który wskazuje odpowiedniej metody w zestaw. Te czynności wszystkie można wykonać ręcznie. Firma Microsoft można utworzyć kod w dowolny tekst, edytor, skompiluj go z poziomu wiersza polecenia za pomocą kompilatora C# ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), zarejestruj go w bazie danych przy użyciu [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) polecenia lub z zarządzania Studio i Dodaj procedura składowana lub obiekt UDF w podobny sposób. Na szczęście wersje Professional i systemy zespołu programu Visual Studio obejmują typ Projekt serwera SQL, który zautomatyzuje tych zadań. W tym samouczku opisano tworzenie zarządzanej procedury składowanej i funkcji zdefiniowanej przez użytkownika za pomocą typu Projekt serwera SQL.

> [!NOTE]
> Jeśli używasz programu Visual Web Developer lub wersja Standard programu Visual Studio, następnie należy zamiast tego użyj metody ręcznego. Krok 13 zawiera szczegółowe instrukcje dotyczące wykonywania tych kroków ręcznie. I zachęca przed odczytaniem krok 13, ponieważ te kroki obejmują ważne instrukcje konfiguracji programu SQL Server, które muszą być stosowane niezależnie od tego, jakie wersja programu Visual Studio jest używana do odczytu kroki 2 do 12.


Uruchom po otwarciu programu Visual Studio. Z menu Plik wybierz nowy projekt, aby wyświetlić okno dialogowe Nowy projekt (zobacz rysunek 4). Następnie przejść do projektu typu bazy danych, a następnie wybierz z szablonów na liście po prawej stronie utworzyć nowy projekt serwera SQL. Wybrano I nazwy tego projektu `ManagedDatabaseConstructs` i umieścić go w obrębie rozwiązania o nazwie `Tutorial75`.


[![Utwórz nowy projekt serwera SQL](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Rysunek 4**: Utwórz nowy projekt serwera SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


Kliknij przycisk OK w oknie dialogowym Nowy projekt do tworzenia rozwiązania i projektu serwera SQL.

Określona baza danych jest powiązany projekt serwera SQL. W związku z tym po utworzeniu nowego projektu programu SQL Server firma Microsoft niezwłocznie monit o określenie tych informacji. Rysunek 5. Pokazuje okno dialogowe Nowe odwołanie do bazy danych, które zostały wypełnione wskaż bazę danych Northwind, który mamy zarejestrowany w wystąpieniu bazy danych programu SQL Server 2005 Express Edition w kroku 1.


![Skojarz projekt serwera SQL z bazą danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Rysunek 5**: Skojarz projekt serwera SQL z bazą danych Northwind


Aby debugować zarządzanego procedury składowane i funkcje UDF, zostanie utworzony w ramach tego projektu, należy włączyć debugowanie obsługę połączenia SQL/CLR. Zawsze, gdy skojarzenie Projekt serwera SQL z nową bazę danych (jak robiliśmy na rysunku 5), programu Visual Studio zapyta, nam Jeśli chcemy włączyć debugowanie SQL/CLR dla połączenia (patrz rysunek 6). Kliknij przycisk Tak.


![Włącz debugowanie SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Rysunek 6**: Włącz debugowanie SQL/CLR


W tym momencie nowy projekt serwera SQL dodano do rozwiązania. Zawiera folder o nazwie `Test Scripts` przy użyciu pliku o nazwie `Test.sql`, który jest używany do debugowania obiekty zarządzane bazy danych utworzone w projekcie. Zajmiemy debugowania w kroku 12.

Można teraz dodać nowe zarządzanych procedury składowane i funkcje UDF do tego projektu, ale przed nie możemy udostępnić s najpierw obejmuje naszych istniejącą aplikację sieci web w rozwiązaniu. W menu Plik wybierz opcję Dodaj i wybierz istniejącą witrynę sieci Web. Przejdź do folderu odpowiedniej witryny sieci Web, a następnie kliknij przycisk OK. Jak pokazano na rysunku 7, spowoduje to zaktualizowanie rozwiązania, aby uwzględnić dwa projekty: witryny sieci Web i `ManagedDatabaseConstructs` Projekt serwera SQL.


![Eksplorator rozwiązań zawiera teraz dwa projekty](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Rysunek 7**: Eksploratora rozwiązań zawiera teraz dwa projekty


`NORTHWNDConnectionString` Wartość w `Web.config` obecnie `NORTHWND.MDF` w pliku `App_Data` folderu. Ponieważ firma Microsoft usunęła tę bazę danych z `App_Data` i go jawnie zarejestrowane w wystąpieniu bazy danych programu SQL Server 2005 Express Edition, należy zaktualizować odpowiednio `NORTHWNDConnectionString` wartość. Otwórz `Web.config` plik w witrynie sieci Web i zmiany `NORTHWNDConnectionString` tak, aby odczytuje parametry połączenia: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Po tej zmianie Twojej `<connectionStrings>` sekcji `Web.config` powinien wyglądać podobnie do następującego:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Zgodnie z opisem w [poprzedniego samouczek](debugging-stored-procedures-cs.md), podczas debugowania obiekt serwera SQL z aplikacji klienta, takich jak witryny sieci Web ASP.NET należy wyłączyć puli połączeń. Parametry połączenia pokazanym powyżej wyłącza buforowanie połączeń ( `Pooling=false` ). Jeśli nie planujesz debugowania zarządzanego procedury składowane i funkcje UDF z witryny sieci Web ASP.NET, należy włączyć puli połączeń.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Krok 3: Tworzenie zarządzanej procedury składowanej

Aby dodać zarządzanej procedury składowanej do bazy danych Northwind, najpierw należy utworzyć procedurę składowaną jako metody w projekcie programu SQL Server. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` Nazwa projektu i wybierz opcję Dodaj nowy element. Spowoduje to wyświetlenie okna dialogowego Dodaj nowy element, które wyświetla listę typów obiektów zarządzanych bazy danych, które mogą zostać dodane do projektu. Jak pokazano na rysunku 8, w tym procedury składowane i funkcje zdefiniowane przez użytkownika, między innymi.

Let s, Rozpocznij od dodania procedury przechowywanej, która po prostu zwraca wszystkie produkty, które zostały anulowane. Nazwa nowego pliku procedury składowanej `GetDiscontinuedProducts.cs`.


[![Dodaj nową procedurę składowaną o nazwie GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Rysunek 8**: dodawanie nowych przechowywane procedury nazwanym `GetDiscontinuedProducts.cs` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Spowoduje to utworzenie nowego pliku klasy C# o następującej treści:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Należy pamiętać, że procedura składowana jest zaimplementowany jako `static` metodę `partial` pliku klasy o nazwie `StoredProcedures`. Ponadto `GetDiscontinuedProducts` zostanie nadany metody `SqlProcedure attribute`, która oznacza metodę jako procedury składowanej.

Poniższy kod tworzy `SqlCommand` obiekt i ustawia jej `CommandText` do `SELECT` zapytanie zwracające wszystkie kolumny z `Products` tabeli produktów, których `Discontinued` pole jest równa 1. Następnie wykonuje polecenie i wysyła wyniki z powrotem do aplikacji klienckiej. Dodaj ten kod do `GetDiscontinuedProducts` metody.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Wszystkie obiekty zarządzane bazy danych mają dostęp do [ `SqlContext` obiektu](https://msdn.microsoft.com/library/ms131108.aspx) reprezentujący kontekst obiektu wywołującego. `SqlContext` Zapewnia dostęp do [ `SqlPipe` obiektu](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) za pośrednictwem jego [ `Pipe` właściwości](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). To `SqlPipe` obiekt jest używany do prom informacji między bazą danych programu SQL Server i aplikacja wywołująca. Jak jego nazwa wskazuje, [ `ExecuteAndSend` metody](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) wykonuje przekazany programu `SqlCommand` obiektów i wysyła wyniki z powrotem do aplikacji klienckiej.

> [!NOTE]
> Zarządzane obiekty są najbardziej odpowiednie dla procedury składowane i funkcje UDF, korzystających z procedurach logiki, a nie na podstawie zestawu logiki. Logika proceduralna obejmuje Praca z zestawami danych na podstawie wiersz po wierszu lub Praca z danymi skalarne. `GetDiscontinuedProducts` Metoda właśnie utworzyliśmy, jednak obejmuje nie procedurach logiki. W związku z tym go będzie najlepiej można zaimplementować jako procedury składowane T-SQL. Zaimplementowano jako procedury składowane zarządzany zarządzanej procedury składowanej, aby zademonstrować kroki niezbędne do tworzenia i wdrażania.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Krok 4: Wdrażanie zarządzanej procedury składowanej

Pełny kod możemy przystąpić do wdrażania go do bazy danych Northwind. Wdrażanie projektu serwera SQL kompiluje kod w zestawie, rejestruje zestaw z bazą danych i tworzy odpowiednich obiektów w bazie danych, łączenie ich z odpowiedniej metody w zestawie. Dokładny zestaw zadań wykonywanych przez opcję wdrażania jest bardziej precyzyjnie określić w kroku 13. Kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` projekt nazwę w Eksploratorze rozwiązań i opcji wdrażania. Jednakże, wdrożenie zakończy się niepowodzeniem z powodu następującego błędu: Błędna składnia w pobliżu "Zewnętrzne". Konieczne może być ustawiony poziom zgodności bieżącej bazy danych na wartość większą, aby włączyć tę funkcję. Zobacz temat pomocy dla procedury składowanej `sp_dbcmptlevel`.

Ten błąd występuje podczas próby zarejestrowania zestawu z bazą danych Northwind. W celu zarejestrowania zestawu z bazą danych programu SQL Server 2005, poziom zgodności bazy danych s musi mieć ustawioną w 90. Domyślnie nowe bazy danych programu SQL Server 2005 mają poziom zgodności 90. Jednak baz danych utworzonych przy użyciu programu Microsoft SQL Server 2000 jest domyślny poziom zgodności 80. Ponieważ bazy danych Northwind była początkowo bazy danych programu Microsoft SQL Server 2000, jego poziom zgodności ma obecnie ustawioną 80 i w związku z tym wymaga zwiększenia do 90, aby zarejestrować obiekty zarządzane bazy danych.

Aby zaktualizować poziom zgodności bazy danych s, Otwórz okno nowego zapytania w programie Management Studio i wprowadź:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Kliknij ikonę Execute na pasku narzędzi, aby uruchomić kwerendę powyżej.


[![Aktualizacja poziomu zgodności bazy danych Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Rysunek 9**: Aktualizacja bazy danych Northwind s poziom zgodności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


Po zaktualizowaniu poziom zgodności, należy ponownie wdrożyć Projekt serwera SQL. Teraz wdrożenie powinno zakończyć się bez błędów.

Wróć do programu SQL Server Management Studio, kliknij prawym przyciskiem myszy w Eksploratorze obiektów bazy danych Northwind i kliknij przycisk Odśwież. Następnie należy przejść do folderu programowania, a następnie rozwiń folder zestawów. Jak pokazano na rysunku nr 10, bazy danych Northwind zawiera teraz zestawu wygenerowanych przez `ManagedDatabaseConstructs` projektu.


![Zestaw ManagedDatabaseConstructs jest teraz zarejestrowane z bazą danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Na rysunku nr 10**: `ManagedDatabaseConstructs` zestaw jest rejestrowany z bazą danych Northwind


Również rozwiń folder procedur składowanych. Zobaczą procedury składowanej o nazwie `GetDiscontinuedProducts`. Ta procedura składowana została utworzona przez proces wdrażania i wskazuje na `GetDiscontinuedProducts` metoda `ManagedDatabaseConstructs` zestawu. Gdy `GetDiscontinuedProducts` procedury składowanej jest wykonywane, z kolei wykonuje `GetDiscontinuedProducts` metody. Ponieważ jest to zarządzanej procedury składowanej nie można edytować za pomocą Management Studio (stąd ikoną kłódki obok nazwy procedury składowanej).


![Procedura składowana GetDiscontinuedProducts znajduje się w folderze procedury składowane](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Rysunek 11**: `GetDiscontinuedProducts` procedury składowanej znajduje się w folderze procedury składowane


Co więcej próg wykluczający mamy usunie przed nazywamy zarządzanej procedury składowanej występuje nadal: skonfigurowano bazę danych, aby zapobiec wykonaniu kodu zarządzanego. To sprawdzić, otwierając okno nowego zapytania i wykonywania `GetDiscontinuedProducts` procedury składowanej. Zostanie wyświetlony następujący komunikat o błędzie: wykonywanie kodu użytkownika w programie .NET Framework jest wyłączone. Włącz opcję konfiguracji clr enabled.

Sprawdź informacje o konfiguracji s bazy danych Northwind, wpisz i wykonaj polecenie `exec sp_configure` w oknie zapytania. Oznacza to, że środowisko clr włączone ustawienie obecnie jest równa 0.


[![Clr włączone ustawienie jest aktualnie ustawione na 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Rysunek 12**: clr włączone ustawienie jest aktualnie ustawione na 0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Należy zauważyć, że każdego ustawienia konfiguracji w rysunek 12 ma cztery wartości na liście z nim: minimalnej i maksymalnej wartości i config i wykonywania wartości. Aby zaktualizować wartości konfiguracji ustawienia clr włączona, uruchom następujące polecenie:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Jeśli zostanie uruchomiony ponownie `exec sp_configure` zobaczysz, że powyższe stwierdzenie zaktualizowany wartość konfiguracyjna clr włączone ustawienie s, 1, ale wartość wykonywania nadal jest ustawione na 0. Zmiany konfiguracji zostały wprowadzone musimy wykonać [ `RECONFIGURE` polecenia](https://msdn.microsoft.com/library/ms176069.aspx), który ustawia wartość wykonywania do bieżącej wartości konfiguracji. Wystarczy wprowadzić `RECONFIGURE` w oknie zapytania i kliknij ikonę Execute na pasku narzędzi. Po uruchomieniu `exec sp_configure` teraz powinni widzieć wartość 1 dla clr włączone ustawienie s config i uruchom wartości.

Kończenie konfiguracji clr włączone możemy przystąpić do uruchamiania zarządzanego `GetDiscontinuedProducts` procedury składowanej. W oknie zapytania wprowadź i wykonaj polecenie `exec` `GetDiscontinuedProducts`. Wywoływanie procedury składowanej powoduje, że odpowiedni kod zarządzany w `GetDiscontinuedProducts` metody do wykonania. Ten kod wystawia `SELECT` zapytania do zwrócenia wszystkich produktów, które są przerywane i zwraca dane do aplikacji wywołującej jest w tym wystąpieniu programu SQL Server Management Studio. Management Studio te wyniki odbiera i wyświetla je w oknie wyników.


[![GetDiscontinuedProducts procedury składowanej zwraca wszystkie wycofane produktów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Rysunek 13**: `GetDiscontinuedProducts` przechowywane procedury zwraca wszystkie wycofane produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Krok 5: Tworzenie zarządzanych przechowywane procedury, które akceptują parametrów wejściowych

Wiele zapytań i procedur składowanych został utworzony w całym te samouczki zostały użyte *parametry*. Na przykład w [Tworzenie nowej procedury składowanej s wpisane DataSet TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczek utworzyliśmy procedury składowanej o nazwie `GetProductsByCategoryID` który zaakceptowane parametru wejściowego o nazwie `@CategoryID`. Procedura składowana zwracany wszystkich produktów których `CategoryID` pola dopasowano wartości podane `@CategoryID` parametru.

Do utworzenia zarządzanej procedury składowanej, który akceptuje parametr wejściowy, wystarczy określić tych parametrów w definicji metody s. Na przykład umożliwiają s dodać inny zarządzanej procedury składowanej do `ManagedDatabaseConstructs` projektu o nazwie `GetProductsWithPriceLessThan`. Tę procedurę składowaną zarządzanych przyjmuje parametr wejściowy cenę i zwróci wszystkie produkty których `UnitPrice` pole jest mniejsza niż wartość parametru s.

Aby dodać nową procedurę składowaną do projektu, kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` Nazwa projektu i wybierz opcję Dodaj nową procedurę składowaną. Nadaj nazwę plikowi `GetProductsWithPriceLessThan.cs`. Jak widzieliśmy w kroku 3, spowoduje to utworzenie nowego pliku klasy C# za pomocą metody o nazwie `GetProductsWithPriceLessThan` umieszczony `partial` klasy `StoredProcedures`.

Aktualizacja `GetProductsWithPriceLessThan` definicję metody s, tak aby akceptowała [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) parametru wejściowego o nazwie `price` i napisać kod do wykonania, a następnie zwracają wyniki zapytania:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan` Definicję metody s i kod przypomina definicji i kod `GetDiscontinuedProducts` metody utworzony w kroku 3. Są tylko różnice, które `GetProductsWithPriceLessThan` metoda przyjmuje jako parametr wejściowy (`price`), `SqlCommand` s zapytanie zawiera parametr (`@MaxPrice`), a parametr jest dodawany do `SqlCommand` s `Parameters` jest kolekcja i przypisana wartość `price` zmiennej.

Po dodaniu tego kodu, należy ponownie wdrożyć Projekt serwera SQL. Następnie wróć do programu SQL Server Management Studio i Odśwież folder procedur składowanych. Powinien zostać wyświetlony nowy wpis `GetProductsWithPriceLessThan`. W oknie zapytania, wprowadź i wykonaj polecenie `exec GetProductsWithPriceLessThan 25`, która będzie listy wszystkich produktów mniej niż 25, jak pokazano na rysunku 14.


[![Produkty w obszarze 25 są wyświetlane](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Rysunek 14**: produktów w obszarze 25 są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Krok 6: Wywoływanie zarządzanej procedury składowanej z Warstwa dostępu do danych

W tym momencie dodaliśmy `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedur składowanych do `ManagedDatabaseConstructs` projektu i zarejestrować je z bazą danych Northwind programu SQL Server. Możemy również wywołać te zarządzanych procedur składowanych z programu SQL Server Management Studio (patrz rysunek s 13 i 14). Aby naszych ASP.NET aplikacji do korzystania z tych zarządzane procedur składowanych, jednak należy dodać je do dostępu do danych i warstwy logiki biznesowej w architekturze. W tym kroku zostanie dodany dwóch nowych metod `ProductsTableAdapter` w `NorthwindWithSprocs` wpisane DataSet, która została początkowo utworzona w [Tworzenie nowej procedury składowanej s wpisane DataSet TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka. W kroku 7 dodamy odpowiednich metod do logiki warstwy Biznesowej.

Otwórz `NorthwindWithSprocs` wpisane zestaw danych z programu Visual Studio, a następnie rozpocznij, dodając nową metodę do `ProductsTableAdapter` o nazwie `GetDiscontinuedProducts`. Aby dodać nową metodę TableAdapter, kliknij prawym przyciskiem myszy nazwę s TableAdapter w projektancie, a następnie wybierz opcję Dodaj zapytanie z menu kontekstowego.

> [!NOTE]
> Ponieważ przenieśliśmy z bazy danych Northwind `App_Data` folderu do wystąpienia bazy danych programu SQL Server 2005 Express Edition, jest konieczne, że odpowiednie parametry połączenia w pliku Web.config można zaktualizować w celu odzwierciedlenia tej zmiany. W kroku 2 Rozmawialiśmy aktualizowanie `NORTHWNDConnectionString` wartość w `Web.config`. Jeśli nie pamiętasz ta aktualizacja będzie zobacz komunikat o błędzie nie powiodło się dodanie zapytania. Nie można odnaleźć połączenia `NORTHWNDConnectionString` dla obiekt `Web.config` w oknie dialogowym podczas próby dodania nowej metody do TableAdapter. Aby rozwiązać ten problem, kliknij przycisk OK, a następnie przejdź do `Web.config` i zaktualizuj `NORTHWNDConnectionString` wartości zgodnie z opisem w kroku 2. Spróbuj ponownie Dodawanie metody do TableAdapter. Teraz powinien działać bez błędów.


Dodawanie nowej metody uruchamia Kreatora konfiguracji zapytania TableAdapter użyliśmy wiele razy w ciągu ostatnich samouczkach. Pierwszym krokiem zapyta firmie Microsoft w celu określenia sposobu TableAdapter powinien uzyskiwać dostęp do bazy danych: za pomocą instrukcji SQL ad hoc lub za pomocą nowego lub istniejącego procedury składowanej. Ponieważ firma Microsoft już utworzona i zarejestrowana `GetDiscontinuedProducts` zarządzanej procedury składowanej z bazą danych, wybierz istniejące użyj opcji procedury przechowywane i kliknij przycisk Dalej.


[![Wybierz istniejącą procedurę składowaną opcja użycia](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Rysunek 15**: wybierz opcję użycia istniejącej przechowywane procedury opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


Następnym ekranie nam wyświetli monit dla procedury składowanej, który wywoła metodę. Wybierz `GetDiscontinuedProducts` zarządzanych procedurę składowaną z listy rozwijanej i kliknij przycisk Dalej.


[![Wybierz procedurę składowaną zarządzanego GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Rysunek 16**: Wybierz `GetDiscontinuedProducts` zarządzane procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Następnie zażądano Określ, czy procedura składowana ma zwracać wiersze, pojedynczą wartość lub nothing. Ponieważ `GetDiscontinuedProducts` zwraca zestaw wierszy produktu wycofane z pierwszej opcji (dane tabelaryczne), a następnie kliknij przycisk Dalej.


[![Wybierz opcję dane tabelaryczne](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Rysunek 17**: wybierz opcję danych tabelarycznych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


Ekran końcowy kreatora pozwala określić wzorce dostępu do danych, które są używane i nazwy metod wynikowy. Pozostaw zaznaczone pola wyboru jak również nazwę metody `FillByDiscontinued` i `GetDiscontinuedProducts`. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Nazwa metody FillByDiscontinued i GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Rysunek 18**: Nazwa metody `FillByDiscontinued` i `GetDiscontinuedProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Powtórz te kroki, aby utworzyć metody o nazwie `FillByPriceLessThan` i `GetProductsWithPriceLessThan` w `ProductsTableAdapter` dla `GetProductsWithPriceLessThan` zarządzane procedury składowanej.

19 rysunku przedstawiono zrzut ekranu projektanta obiektów DataSet po dodaniu metody służące do `ProductsTableAdapter` dla `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedur składowanych.


[![ProductsTableAdapter obejmuje nowych metod dodane w tym kroku](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Rysunek 19**: `ProductsTableAdapter` obejmuje nowych metod dodane w tym kroku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Krok 7: Dodawanie odpowiednich metod do warstwy logiki biznesowej

Teraz, gdy zaktualizowano Warstwa dostępu do danych w celu uwzględnienia metod wywoływania zarządzanych procedur składowanych dodać kroki 4 i 5, należy dodać odpowiednich metod do warstwy logiki biznesowej. Dodaj następujące dwie metody `ProductsBLLWithSprocs` klasy:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Obie metody po prostu Wywołaj odpowiedniej metody DAL i zwracać `ProductsDataTable` wystąpienia. `DataObjectMethodAttribute` Znaczników powyżej każda metoda powoduje, że te metody mają zostać uwzględnione na liście rozwijanej wybierz kartę kreatora skonfiguruj źródło danych elementu ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Krok 8: Wywoływanie zarządzanej procedur składowanych z warstwy prezentacji

Przy użyciu logiki biznesowej i warstwy dostępu do danych rozszerzony do obejmują obsługę wywoływania `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedur składowanych, można teraz wyświetlić te przechowywane procedury wyniki za pośrednictwem strony platformy ASP.NET.

Otwórz `ManagedFunctionsAndSprocs.aspx` strony `AdvancedDAL` folderu i z przybornika, przeciągnij element GridView do projektanta. Ustaw GridView s `ID` właściwości `DiscontinuedProducts` i z jego tagów inteligentnych powiązać go z nowego elementu ObjectDataSource o nazwie `DiscontinuedProductsDataSource`. Skonfiguruj ObjectDataSource jego dane z `ProductsBLLWithSprocs` klasy s `GetDiscontinuedProducts` metody.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Rysunek 20**: Konfigurowanie ObjectDataSource użyć `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Wybierz metodę GetDiscontinuedProducts z listy rozwijanej wybierz kartę](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Rysunek 21**: Wybierz `GetDiscontinuedProducts` metodę z listy rozwijanej wybierz kartę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Ponieważ ta siatka będzie można użyć tylko wyświetlania informacji o produkcie, ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak), a następnie kliknij przycisk Zakończ.

Po zakończeniu działania kreatora programu Visual Studio spowoduje automatyczne dodanie elementu BoundField lub CheckBoxField dla każdego pola danych w `ProductsDataTable`. Poświęć chwilę, aby usunąć wszystkie te pola z wyjątkiem `ProductName` i `Discontinued`, w którym punktu z widoku GridView i znaczników deklaracyjne element ObjectDataSource s powinien wyglądać podobnie do następującego:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Gdy odwiedzoną stronę, wywołania elementu ObjectDataSource `ProductsBLLWithSprocs` klasy s `GetDiscontinuedProducts` metody. Jak widzieliśmy w kroku 7, ta metoda wywołuje w dół do DAL s `ProductsDataTable` klasy s `GetDiscontinuedProducts` metodę, która wywołuje `GetDiscontinuedProducts` procedury składowanej. Procedura składowana jest zarządzane procedury składowanej i wykonuje kod utworzonego w kroku 3, zwracając obejmowałyby produkty.

Wyniki zwrócone przez procedurę składowaną zarządzane są pakowane w w `ProductsDataTable` przez warstwę DAL, a następnie zwracany do logiki warstwy Biznesowej, które zwraca je do warstwy prezentacji, gdzie są powiązane z widoku GridView i wyświetlane. Zgodnie z oczekiwaniami, siatki zawiera listę produktów, które zostały wycofane.


[![Produkty zaprzestać wymienione](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Rysunek 22**: produkty zaprzestać wymienione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Dla dalszego rozwiązaniem Dodaj pole tekstowe i innego widoku GridView do strony. Ma to GridView wyświetlić produktów, mniejszą niż wartość wprowadzona w polu tekstowym, wywołując `ProductsBLLWithSprocs` klasy s `GetProductsWithPriceLessThan` metody.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Krok 9: Tworzenia i wywoływania funkcji UDF T-SQL

Funkcje zdefiniowane przez użytkownika lub funkcji UDF, czy obiekty bazy danych ściśle mimic semantykę funkcji w językach programowania. Jak funkcja w języku C# funkcji UDF można uwzględnić zmienną liczbą parametrów wejściowych i zwracać wartość określonego typu. UDF może zwrócić dane skalarne — ciąg, liczbę całkowitą z zakresu i tak dalej — lub dane tabelaryczne. Umożliwiają szybkie Spójrz na obu typów funkcji UDF, począwszy od UDF, która zwraca typ danych skalarnych s.

Następujące UDF oblicza szacowaną wartość spisu dla określonego produktu. Robi to biorąc w trzech parametrów wejściowych - `UnitPrice`, `UnitsInStock`, i `Discontinued` wartości dla określonego produktu - i zwraca wartość typu `money`. Oblicza iloczyn szacowaną wartość spisu `UnitPrice` przez `UnitsInStock`. Wycofane elementów wartość ta jest dwukrotnie mniejsza.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Po dodaniu tej funkcji zdefiniowanej przez użytkownika do bazy danych można znaleźć za pomocą Management Studio rozwijając programowania folder, a następnie funkcje, a następnie funkcji wartość skalarną. Mogą być używane w `SELECT` zapytania w następujący sposób:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Po dodaniu `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika do bazy danych Northwind. 23 rysunek przedstawia dane wyjściowe z powyższych `SELECT` zapytania, wyświetlony za pomocą Management Studio. Należy również zauważyć, że funkcję zdefiniowaną przez użytkownika znajduje się w folderze funkcje wartość skalarną w Eksploratorze obiektów.


[![Każdy produkt s wartości zapasów ma na liście](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Rysunek 23**: s każdego produktu znajduje się spis wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Funkcje UDF można również zwracające dane tabelaryczne. Na przykład można utworzyć UDF, która zwraca produktów, które należą do określonej kategorii:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` Akceptuje UDF `@CategoryID` parametr wejściowy oraz zwraca wyniki z określonym `SELECT` zapytania. Po utworzeniu tej funkcji zdefiniowanej przez użytkownika może być przywoływany w `FROM` (lub `JOIN`) klauzuli `SELECT` zapytania. Poniższy przykład powoduje zwrócenie `ProductID`, `ProductName`, i `CategoryID` wartości dla każdego napoje.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Po dodaniu `udf_GetProductsByCategoryID` funkcji zdefiniowanej przez użytkownika do bazy danych Northwind. 24 rysunek przedstawia dane wyjściowe z powyższych `SELECT` zapytania, wyświetlony za pomocą Management Studio. Funkcje UDF, które zwracają dane tabelaryczne znajdują się w folderze Eksplorator obiektów s funkcji z wartościami przechowywanymi w tabeli.


[![ProductID ProductName i CategoryID są wyświetlane dla każdego napoju](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Rysunek 24**: `ProductID`, `ProductName`, i `CategoryID` są wyświetlane dla każdego napoju ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Aby uzyskać więcej informacji dotyczących tworzenia i używania funkcji UDF, zapoznaj się z [wprowadzenie do funkcji zdefiniowanych przez użytkownika](http://www.sqlteam.com/item.asp?ItemID=1955). Sprawdź również [korzyści i funkcje Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Krok 10: Tworzenie zarządzanych UDF

`udf_ComputeInventoryValue` i `udf_GetProductsByCategoryID` utworzone w powyższych przykładach funkcje UDF są obiektami bazy danych T-SQL. SQL Server 2005 obsługuje również funkcje UDF zarządzanych, które mogą być dodawane do `ManagedDatabaseConstructs` projektu tak samo, jak zarządzanej procedury składowane z kroki 3 i 5. W tym kroku let s zaimplementować `udf_ComputeInventoryValue` UDF w kodzie zarządzanym.

Aby dodać zarządzane UDF do `ManagedDatabaseConstructs` projektu, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz opcję Dodaj nowy element. Wybierz szablon zdefiniowane przez użytkownika w oknie dialogowym Dodawanie nowego elementu, a nazwa nowego pliku UDF `udf_ComputeInventoryValue_Managed.cs`.


[![Dodaj nowy UDF zarządzanych do projektu ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Rysunek 25**: dodawanie nowych UDF zarządzanych do `ManagedDatabaseConstructs` projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Tworzy szablon funkcji zdefiniowanych przez użytkownika `partial` klasy o nazwie `UserDefinedFunctions` za pomocą metody, której nazwa jest taka sama jak nazwa pliku s klasy (`udf_ComputeInventoryValue_Managed`, w tym wystąpieniu). Ta metoda jest dekorowane za pomocą [ `SqlFunction` atrybut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), który flagi metodę jako zarządzane UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue` Metoda obecnie zwraca [ `SqlString` obiektu](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) i nie akceptuje parametrów wejściowych. Należy zaktualizować definicję metody, aby przyjmuje trzy parametry - wejściowe `UnitPrice`, `UnitsInStock`, i `Discontinued` - i zwraca `SqlMoney` obiektu. Logika do obliczania wartości magazynu jest identyczna jak T-SQL `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Należy pamiętać, że parametry wejściowe metody s UDF są jako odpowiednie typy SQL: `SqlMoney` dla `UnitPrice` pola [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) dla `UnitsInStock`, i [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) Aby uzyskać `Discontinued`. Te typy danych odzwierciedlają typów zdefiniowanych w `Products` tabeli: `UnitPrice` kolumny jest typu `money`, `UnitsInStock` kolumny typu `smallint`i `Discontinued` kolumny typu `bit`.

Kod uruchamiany przez utworzenie `SqlMoney` wystąpienia o nazwie `inventoryValue` przypisany wartość 0. `Products` Umożliwia tabeli bazy danych `NULL` wartości w `UnitsInPrice` i `UnitsInStock` kolumn. W związku z tym musimy pierwszy wyboru, aby sprawdzić, czy te wartości zawiera `NULL` s, który przejdziemy przez `SqlMoney` obiektu s [ `IsNull` właściwości](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Jeśli oba `UnitPrice` i `UnitsInStock` zawiera inną niż`NULL` wartości, a następnie możemy obliczeniowe `inventoryValue` jako iloczyn dwóch. Następnie, jeśli `Discontinued` ma wartość true, a następnie możemy połowę wartość.

> [!NOTE]
> `SqlMoney` Obiekt tylko dwa umożliwia `SqlMoney` wystąpień mnoży się ze sobą. Nie zezwalaj `SqlMoney` wystąpienia pomnożona przez wartość zmiennoprzecinkowa literału. W związku z tym o połowę `inventoryValue` możemy on mnożony nowy `SqlMoney` wystąpienia, który ma wartość 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Krok 11: Wdrażanie zarządzanych UDF

Teraz, gdy utworzono zarządzanych UDF, możemy są gotowe do wdrożenia go do bazy danych Northwind. Jak widzieliśmy w kroku 4 zarządzanych obiektów w projekcie programu SQL Server są wdrażane przez kliknięcie prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybranie opcji wdrażania z menu kontekstowego.

Po wdrożeniu projektu, wróć do programu SQL Server Management Studio i Odśwież folder skalarne. Powinien zostać wyświetlony dwie pozycje:

- `dbo.udf_ComputeInventoryValue` -UDF T-SQL utworzony w kroku 9 i
- `dbo.udf ComputeInventoryValue_Managed` -zarządzanych UDF utworzony w kroku 10, która właśnie została wdrożona.

Do testowania tej funkcji zdefiniowanej przez użytkownika zarządzanych, wykonaj następujące zapytanie w programie Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

To polecenie używa zarządzanej `udf ComputeInventoryValue_Managed` UDF zamiast T-SQL `udf_ComputeInventoryValue` funkcji zdefiniowanej przez użytkownika, ale dane wyjściowe są takie same. Odwołaj się do 23 rysunek, aby zobaczyć zrzutu ekranu zawierającego dane wyjściowe s funkcji zdefiniowanej przez użytkownika.

## <a name="step-12-debugging-the-managed-database-objects"></a>Krok 12: Debugowanie zarządzane obiekty

W [debugowanie procedur składowanych](debugging-stored-procedures-cs.md) samouczku omówiono trzy opcje do debugowania programu SQL Server za pomocą programu Visual Studio: bezpośrednie debugowania bazy danych, debugowania aplikacji i debugowanie z projektu programu SQL Server. Zarządzane bazy danych obiektów, nie można debugować za pośrednictwem bezpośredniego debugowania bazy danych, ale może być debugowany od aplikacji klienckiej i bezpośrednio z Projekt serwera SQL. Aby debugowania do pracy jednak bazy danych programu SQL Server 2005 muszą zezwalać na debugowanie SQL/CLR. Odwołania, który po pierwszym utworzeniu `ManagedDatabaseConstructs` projektu programu Visual Studio pytanie czy trzeba włączyć debugowanie (patrz rysunek 6 w kroku 2) SQL/CLR. To ustawienie może być modyfikowany przez kliknięcie prawym przyciskiem myszy bazę danych z okna Eksploratora serwera.


![Upewnij się, że bazy danych umożliwia debugowanie SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Rysunek 26**: Upewnij się, że bazy danych umożliwia debugowanie SQL/CLR


Załóżmy, że możemy debugowania `GetProductsWithPriceLessThan` zarządzane procedury składowanej. Rozpoczniemy ustawiając punkt przerwania w kodzie `GetProductsWithPriceLessThan` metody.


[![Ustaw punkt przerwania w metodzie GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Rysunek 27**: Ustaw punkt przerwania `GetProductsWithPriceLessThan` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Let s Pierwsze spojrzenie na debugowanie zarządzane obiekty z projektu programu SQL Server. Ponieważ naszego rozwiązania zawiera dwa projekty — `ManagedDatabaseConstructs` Projekt serwera SQL wraz z naszą witrynę sieci Web — Aby debugować z projektu serwera SQL, potrzebujemy nakazać programowi Visual Studio, aby uruchomić `ManagedDatabaseConstructs` Projekt serwera SQL podczas możemy rozpocząć debugowania. Kliknij prawym przyciskiem myszy `ManagedDatabaseConstructs` projekt w Eksploratorze rozwiązań i wybierz pozycję Ustaw jako projekt startowy opcji z menu kontekstowego.

Gdy `ManagedDatabaseConstructs` projektu jest uruchamiana z debugera wykonywania instrukcji SQL w `Test.sql` pliku, który znajduje się w `Test Scripts` folderu. Na przykład, aby przetestować `GetProductsWithPriceLessThan` zarządzane procedury składowanej zastąpić istniejącą `Test.sql` plików zawartości z następującą instrukcję, która wywołuje `GetProductsWithPriceLessThan` zarządzane procedury składowanej, przekazując `@CategoryID` wartość 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Powyższy skrypt na wejściu był `Test.sql`, Rozpocznij debugowanie, przechodząc do menu Debugowanie i wybierając pozycję Rozpocznij debugowanie lub naciskać klawisz F5 lub zielonego odtwarzania ikonę na pasku narzędzi. Spowoduje to kompilacji projektów w rozwiązaniu, wdrażanie zarządzane obiekty do bazy danych Northwind i wykonujących `Test.sql` skryptu. W tym momencie zostanie trafiony punkt przerwania i będziemy przetwarzać `GetProductsWithPriceLessThan` metody, sprawdź wartości parametrów wejściowych i tak dalej.


[![Został trafiony punkt przerwania w metodzie GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Rysunek 28**: punkt przerwania w `GetProductsWithPriceLessThan` został trafiony — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


Aby dla obiektu bazy danych SQL do debugowania za pośrednictwem aplikacji klienckiej konieczne jest skonfigurowania bazy danych do obsługi debugowania aplikacji. Kliknij prawym przyciskiem myszy w Eksploratorze serwera bazy danych i upewnij się, że zaznaczono opcję debugowania aplikacji. Ponadto należy skonfigurować aplikację ASP.NET do integracji z debugera SQL i wyłączyć puli połączeń. Te kroki opisano szczegółowo w kroku 2 [debugowanie procedur składowanych](debugging-stored-procedures-cs.md) samouczka.

Po skonfigurowaniu aplikacji ASP.NET i bazy danych, Ustaw jako projekt startowy witryny sieci Web ASP.NET i Rozpocznij debugowanie. W przypadku odwiedzenia strony, która wywołuje jeden z obiektów zarządzanych, które ma punkt przerwania, aplikacja zostanie zatrzymany i formantu zostanie włączone za pośrednictwem do debugera, której można przejrzeć kod, jak pokazano na rysunku 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Krok 13: Ręcznie kompilowanie i wdrażanie zarządzanych obiektów bazy danych

SQL Server — projekty ułatwiają tworzenie, kompilowanie i wdrażanie obiektów zarządzanych bazy danych. Niestety program SQL Server — projekty są dostępne tylko w wersjach Professional i systemy zespołu programu Visual Studio. Jeśli używasz programu Visual Web Developer i Standard Edition dla programu Visual Studio i chcesz użyć obiektów zarządzanych bazy danych, należy ręcznie utworzyć i wdrożyć je. Obejmuje to cztery kroki:

1. Utwórz plik zawierający kod źródłowy dla obiekt zarządzany bazy danych
2. Skompiluj obiekt do zestawu
3. Rejestrowanie zestawów z bazą danych programu SQL Server 2005 i
4. Tworzenie obiektu bazy danych w programie SQL Server, który wskazuje odpowiedniej metody w zestawie.

Aby zilustrować te zadania, niech s, Utwórz nową zarządzanych procedury przechowywanej zwracającej tych produktów którego `UnitPrice` jest większa niż określona wartość. Utwórz nowy plik na komputerze o nazwie `GetProductsWithPriceGreaterThan.cs` , a następnie wprowadź poniższy kod do pliku (możesz użyć programu Visual Studio, Notatnik lub edytorze tekstu w tym celu):


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Ten kod jest niemal identyczna ze `GetProductsWithPriceLessThan` metody utworzony w kroku 5. Nazwy metod są tylko różnice `WHERE` klauzul i nazwę parametru użytego w zapytaniu. W `GetProductsWithPriceLessThan` metody `WHERE` klauzuli odczytać: `WHERE UnitPrice < @MaxPrice`. W tym miejscu, w `GetProductsWithPriceGreaterThan`, używamy: `WHERE UnitPrice > @MinPrice` .

Teraz należy skompilować tej klasy w zestawie. W wierszu polecenia przejdź do katalogu, w której zapisano `GetProductsWithPriceGreaterThan.cs` pliku i użyć kompilatora C# (`csc.exe`) do kompilowania pliku klasy do zestawu:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Jeśli folder zawierający `csc.exe` w nie znajduje się w systemie s `PATH`, konieczne będzie pełni odwołania ze ścieżką `%WINDOWS%\Microsoft.NET\Framework\version\`, w następujący sposób:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Kompiluje GetProductsWithPriceGreaterThan.cs się do zestawu](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Rysunek 29**: kompilacja `GetProductsWithPriceGreaterThan.cs` do zestawu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t` Flaga Określa, że plik klasy C# ma być kompilowana do biblioteki DLL (zamiast pliku wykonywalnego). `/out` Flagi Określa nazwę zestawu wynikowego.

> [!NOTE]
> Zamiast kompilowanie `GetProductsWithPriceGreaterThan.cs` pliku klasy z wiersza polecenia, można też użyć [Visual C# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) lub Utwórz oddzielne projektu biblioteki klas w programie Visual Studio, Standard Edition. S ren Lauritsen Jacoba potrzebna udostępnił taki projekt Visual C# Express Edition o kod `GetProductsWithPriceGreaterThan` procedur składowanych i dwa zarządzane procedur składowanych i funkcji zdefiniowanej przez użytkownika utworzony w kroku 3, 5 i 10. S ren s projektu zawiera także polecenia T-SQL, aby dodać odpowiednie obiekty bazy danych.


Kod skompilowany w zestawie możemy przystąpić można zarejestrować zestawu w bazie danych programu SQL Server 2005. Można to wykonać za pomocą T-SQL, za pomocą polecenia `CREATE ASSEMBLY`, lub za pomocą programu SQL Server Management Studio. Let s fokus użyciu Management Studio.

Z Management Studio rozwiń folder programowania bazy danych Northwind. Jednym z jej podfolder, jest zestawów. Aby ręcznie dodać nowego zestawu z bazą danych, kliknij prawym przyciskiem myszy folder na zestawy, a następnie wybierz pozycję nowego zestawu z menu kontekstowego. Ten wyświetla okno dialogowe nowego zestawu polu (patrz rysunek 30). Kliknij przycisk Przeglądaj, wybierz opcję `ManuallyCreatedDBObjects.dll` zestawu możemy właśnie skompilowany, a następnie kliknij OK, aby dodać zestaw do bazy danych. Nie powinien być widoczny `ManuallyCreatedDBObjects.dll` zestawu w Eksploratorze obiektów.


[![Dodaj zestaw ManuallyCreatedDBObjects.dll do bazy danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Rysunek 30**: Dodaj `ManuallyCreatedDBObjects.dll` zestawu do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![ManuallyCreatedDBObjects.dll znajduje się w Eksploratorze obiektów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Rysunek 31**: `ManuallyCreatedDBObjects.dll` znajduje się w Eksploratorze obiektów


Gdy zestaw dodano do bazy danych Northwind, musimy jeszcze skojarzyć procedury składowanej z `GetProductsWithPriceGreaterThan` metody w zestawie. Aby to zrobić, Otwórz nowe okno zapytanie i uruchom następujący skrypt:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

To tworzy nową procedurę składowaną w bazie danych Northwind o nazwie `GetProductsWithPriceGreaterThan` i kojarzy ją z zarządzaną metodę `GetProductsWithPriceGreaterThan` (czyli w klasie `StoredProcedures`, która znajduje się w zestawie `ManuallyCreatedDBObjects`).

Po wykonaniu powyższych skryptu, Odśwież folder procedur składowanych w Eksploratorze obiektów. Powinien zostać wyświetlony nowy wpis procedury składowanej - `GetProductsWithPriceGreaterThan` -mającego ikoną kłódki, obok niej. Aby przetestować tę procedurę składowaną, wprowadź i uruchom następujący skrypt w oknie zapytania:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Jak pokazano na rysunku 32, powyższe polecenie wyświetla informacje dotyczące tych produktów z `UnitPrice` większa niż 24.95 $.


[![ManuallyCreatedDBObjects.dll znajduje się w Eksploratorze obiektów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Rysunek 32**: `ManuallyCreatedDBObjects.dll` znajduje się w Eksploratorze obiektów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Podsumowanie

Microsoft SQL Server 2005 zapewnia integrację z języka wspólnego środowiska uruchomieniowego (CLR), dzięki czemu obiekty bazy danych ma zostać utworzony przy użyciu kodu zarządzanego. Wcześniej tylko można utworzyć te obiekty bazy danych przy użyciu T-SQL, ale teraz utworzymy tych obiektów przy użyciu platformy .NET Programowanie w językach C#. W tym samouczku utworzyliśmy dwa zarządzane procedur przechowywanych i zarządzanych funkcji zdefiniowanych przez użytkownika.

Visual Studio s Projekt serwera SQL typu ułatwia tworzenie, kompilowanie i wdrażanie bazy danych zarządzanych obiektów. Ponadto oferuje rozbudowane obsługę debugowania. Jednak typów projektów serwera SQL są dostępne tylko w wersjach Professional i systemy zespołu programu Visual Studio. Dla tych przy użyciu programu Visual Web Developer lub Standard Edition dla programu Visual Studio, tworzenia, kompilowania i kroki wdrażania należy wykonać ręcznie, jak widzieliśmy w kroku 13.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zalety i wady funkcji zdefiniowanych przez użytkownika](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Tworzenie obiekty programu SQL Server 2005 w kodzie zarządzanym](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Tworzenie Wyzwalacze w programie SQL Server 2005 za pomocą kodu zarządzanego](http://www.15seconds.com/issue/041006.htm)
- [Porady: Tworzenie i uruchamianie CLR SQL Server procedury składowanej](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Porady: Tworzenie i uruchamianie funkcji zdefiniowanej przez użytkownika serwera CLR SQL](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Porady: Edytowanie `Test.sql` skryptu obiekty SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Funkcje zdefiniowane przez wprowadzenie do użytkownika](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Zarządzany kod i SQL Server 2005 (klip wideo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Dokumentacja języka Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Wskazówki: Tworzenie procedury składowanej w kodzie zarządzanym](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został ren S Lauritsen Jacoba. Oprócz przeglądania tego artykułu, S ren również utworzony projekt Visual C# Express Edition, zawarte w tym pobieranie artykułu s dla ręcznie kompilowania obiektów zarządzanych bazy danych. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](debugging-stored-procedures-cs.md)
> [dalej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
