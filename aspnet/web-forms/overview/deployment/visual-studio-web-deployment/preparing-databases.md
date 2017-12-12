---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: "Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: przygotowywanie do wdrożenia bazy danych | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 1f19d54a5f2679f790575d520b28472d4ff3233f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: przygotowywanie do wdrożenia bazy danych
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak można pobrać projektu gotowe do wdrożenia bazy danych. Struktura bazy danych, a niektóre (nie wszystkie) danych w dwóch aplikacji baz danych, należy wdrożyć test przemieszczania i środowisk produkcyjnych.

Zazwyczaj podczas opracowywania aplikacji test dane wprowadzane do bazy danych, których nie chcesz, aby wdrożyć w witrynie na żywo. Jednak może być również części danych produkcyjnych, które chcesz wdrożyć. W tym samouczku możesz skonfigurować projekt Contoso University i przygotowania skryptów SQL danych jest uwzględniane podczas wdrażania.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

Przykładowa aplikacja korzysta z programu SQL Server Express LocalDB. SQL Server Express to bezpłatna wersja programu SQL Server. Najczęściej jest używana podczas tworzenia, ponieważ jest on oparty na tym samym aparatu bazy danych jako pełnej wersji programu SQL Server. Należy przeprowadzić test za pomocą programu SQL Server Express i mieć pewność, że aplikacja będzie działają tak samo w środowisku produkcyjnym, kilka wyjątków dla funkcji, które być różne w różnych wersjach programu SQL Server.

LocalDB jest tryb specjalne wykonanie programu SQL Server Express, który umożliwia pracę z bazami danych jako *.mdf* plików. Zazwyczaj są przechowywane pliki bazy danych LocalDB w *aplikacji\_danych* folderu projektu sieci web. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia również współpracować *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzały; w związku z tym LocalDB jest zalecane w przypadku pracy z *.mdf* plików.

Zazwyczaj programu SQL Server Express nie jest używany dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web ponieważ nie jest przeznaczony do pracy z usługami IIS.

W programie Visual Studio 2012 LocalDB jest instalowany domyślnie z programem Visual Studio. W programie Visual Studio 2010 i wcześniejszych wersjach programu SQL Server Express (bez LocalDB) jest instalowany domyślnie z programem Visual Studio; oznacza to dlaczego zainstalowany jako jeden z warunków wstępnych w [pierwszym samouczku tej serii](introduction.md).

Aby uzyskać więcej informacji o wersjach programu SQL Server, w tym LocalDB, zobacz następujące zasoby [pracy z bazy danych programu SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework i dostawców uniwersalnych

Aby uzyskać dostęp do bazy danych aplikacja Contoso University wymaga następującego oprogramowania, które należy wdrożyć w aplikacji, ponieważ nie znajduje się w programie .NET Framework:

- [Dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umożliwia systemu członkostwa programu ASP.NET użyć usługi Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/en-us/library/gg696172.aspx)

Ponieważ to oprogramowanie, znajduje się w pakietach NuGet, projekt jest już skonfigurowane tak, aby wymagane zestawy są wdrażane w projekcie. (Łącza wskaż bieżące wersje tych pakietów, które może być nowsza niż zainstalowana w projekcie starter pobranego w ramach tego samouczka).

Jeśli wdrażasz do innych firm dostawcy obsługi zamiast Azure, upewnij się, że używasz programu Entity Framework w wersji 5.0 lub nowszej. Starsze niż migracje Code First wymaga pełnego zaufania i większość dostawców usług hostingu uruchomi aplikację w trybie średniego zaufania. Aby uzyskać więcej informacji o trybie średniego zaufania, zobacz [Wdróż w usługach IIS jako środowisko testowe](deploying-to-iis.md) samouczka.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Skonfiguruj migracje Code First dla wdrożenia aplikacji bazy danych

Bazy danych aplikacji Contoso University zarządza Code First i wdrożony za pomocą migracje Code First. Omówienie wdrażania bazy danych przy użyciu migracje Code First, zobacz [pierwszym samouczku tej serii](introduction.md).

Podczas wdrażania bazy danych aplikacji, zwykle nie można po prostu wdrożyć programowanie bazy danych za pomocą wszystkich danych znajdujących się w niej do środowiska produkcyjnego, ponieważ większość danych, prawdopodobnie jest tylko do celów testowych. Na przykład nazwy uczniów w testowej bazy danych są fikcyjne. Z drugiej strony, często nie można wdrożyć tylko struktury bazy danych nie zawiera żadnych danych w niej w ogóle. Niektóre dane w bazie danych test może być prawdziwe dane, a musi być dostępne, gdy użytkownicy rozpocząć korzystanie z aplikacji. Na przykład baza danych może być tabeli, która zawiera nieprawidłowy klasy wartości lub nazwy działów prawdziwe.

Aby symulować ten typowy scenariusz, należy skonfigurować migracje Code First `Seed` metodę, która wstawia do bazy danych, które mają być dostępne w środowisku produkcyjnym. To `Seed` — metoda nie należy wstawić danych testowych, ponieważ będzie uruchamiany w środowisku produkcyjnym po Code First utworzy bazę danych w środowisku produkcyjnym.

We wcześniejszych wersjach programu Code First przed migracje został zwolniony, jest często `Seed` metod, aby wstawić dane testowe również, ponieważ przy każdej zmianie modelu podczas tworzenia bazy danych musiała zostać całkowicie usunięty i utworzony ponownie od początku. Migracje Code First, test dane zostaną zachowane po wprowadzeniu zmian w bazie danych, więc tym dane testowe w `Seed` — metoda nie jest konieczne. Pobrany projekt używa metody, w tym wszystkie dane w `Seed` metody klasy inicjatora. W tym samouczku zostanie wyłączone tej klasy inicjatora i `enable Migrations. Then you'll update the `inicjatora "w konfiguracji migracji metody klasy, aby wstawia tylko dane, które mają być wstawiane w środowisku produkcyjnym.

Na poniższym diagramie przedstawiono schemat bazy danych aplikacji:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Te samouczki będzie założono, że `Student` i `Enrollment` tabel powinien być pusty, jeśli najpierw zostanie wdrożona lokacja. Inne tabele zawierają dane, które ma zostać załadowane aplikacji systemowi na żywo.

### <a name="disable-the-initializer"></a>Wyłącz inicjatora

Ponieważ będziesz używać migracje Code First, nie trzeba używać `DropCreateDatabaseIfModelChanges` inicjatora Code First. Kod dla tego inicjatora jest *SchoolInitializer.cs* plik w projekcie ContosoUniversity.DAL. Ustawienia w `appSettings` elementu *Web.config* tego inicjatora do uruchomienia, gdy aplikacja próbuje dostęp do bazy danych po raz pierwszy powoduje, że plik:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Otwórz aplikację *Web.config* plików i Usuń komentarz `add` element, który określa klasę inicjatora Code First. `appSettings` Element teraz wygląda następująco:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Innym sposobem określania klasy inicjatora jest to zrobić przez wywołanie metody `Database.SetInitializer` w `Application_Start` metody w *Global.asax* pliku. Jeśli włączasz migracje w projekcie, który używa tej metody można określić inicjatora, usuń go.


> [!NOTE]
> Jeśli używasz programu Visual Studio 2013, należy dodać następujące kroki między kroki 2 i 3: Wprowadź PMC () w "entityframework pakiet aktualizacji — wersja 6.1.1" Aby uzyskać bieżącą wersję programu EF. Następnie (b) kompilacji projektu, aby uzyskać listę błędów kompilacji i popraw je. Usuwanie przy użyciu instrukcji dla przestrzeni nazw, która już istnieje, kliknij prawym przyciskiem myszy i kliknij przycisk Rozwiąż, aby dodać za pomocą instrukcji, gdy są potrzebne i Zmień wystąpienia System.Data.EntityState System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Włącz migracje Code First

1. Upewnij się, że projekt ContosoUniversity (nie ContosoUniversity.DAL) jest ustawiony jako projekt startowy. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz **Ustaw jako projekt startowy**. Migracje Code First będzie wyszukiwał projekt startowy można znaleźć ciąg połączenia bazy danych.
2. Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** (lub **Menedżera pakietów NuGet**), a następnie **Konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. W górnej części **Konsola Menedżera pakietów** oknie Wybierz ContosoUniversity.DAL jako domyślny projekt, a następnie at `PM>` wierszu wprowadź "enable-migrations".

    ![polecenia enable-migrations](preparing-databases/_static/image4.png)

    (Jeśli zostanie wyświetlony błąd informacją o tym *enable-migrations,* nie rozpoznano polecenia, wpisz polecenie *EntityFramework pakiet aktualizacji-Zainstaluj ponownie* i spróbuj ponownie.)

    To polecenie tworzy *migracje* folderu projektu ContosoUniversity.DAL i umieszcza w tym folderze dwa pliki: *Configuration.cs* pliku, który służy do konfigurowania migracji i *InitialCreate.cs* pierwszej migracji, które utworzy bazę danych w pliku.

    ![Migrations folder](preparing-databases/_static/image5.png)

    Wybrany projekt DAL w **domyślny projekt** listy rozwijanej **Konsola Menedżera pakietów** ponieważ `enable-migrations` polecenie musi zostać wykonana w projekcie, który zawiera Code First Klasa kontekstu. W przypadku tej klasy w projektu biblioteki klas, migracje Code First szuka parametry połączenia bazy danych w projekcie uruchamiania dla rozwiązania. W rozwiązaniu ContosoUniversity projektu sieci web został ustawiony jako projekt startowy. Jeśli nie chcesz wyznaczyć projektu, który ma parametry połączenia jako projekt startowy w programie Visual Studio, można określić projekt startowy w polecenia programu PowerShell. Aby wyświetlić składnię polecenia, wpisz polecenie `get-help enable-migrations`.

    `enable-migrations` Polecenie automatycznie utworzyć pierwszy migracji, ponieważ baza danych już istnieje. Alternatywą jest migracje utworzyć bazę danych. Aby to zrobić, użyj **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** można usunąć bazy danych ContosoUniversity przed włączeniem migracji. Po włączeniu migracji, należy ręcznie utworzyć pierwszy migracji przez wprowadzenie polecenia "Dodaj migracji InitialCreate". Następnie można utworzyć bazy danych, wpisując polecenie "update-database".

### <a name="set-up-the-seed-method"></a>Konfigurowanie Seed — metoda

W tym samouczku dodasz stałych z danymi, przez dodawanie kodu do `Seed` metodzie migracje Code First `Configuration` klasy. Kod wywoła migracje First `Seed` metody po każdej migracji.

Ponieważ `Seed` metoda jest uruchamiana po każdym migracji, danych już istnieje w tabeli po pierwszej migracji. Aby obsłużyć taką sytuację użyjesz `AddOrUpdate` metodę aktualizowania wierszy, które już zostały wstawione lub wstawić je, jeśli nie istnieje jeszcze. `AddOrUpdate` — Metoda nie może być najlepszym rozwiązaniem dla danego scenariusza. Aby uzyskać więcej informacji, zobacz [zajmie się za pomocą metody AddOrUpdate EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

1. Otwórz *Configuration.cs* plików i Zastąp komentarzy w `Seed` metodę z następującym kodem:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Odwołania do `List` mieć czerwone dowolnym kształcie linie w nich, ponieważ nie masz `using` jeszcze instrukcji dla jego przestrzeni nazw. Kliknij prawym przyciskiem myszy jednego wystąpienia `List` i kliknij przycisk **rozwiązać**, a następnie kliknij przycisk **przy użyciu system.Collections.Generic —**.

    ![Rozwiązania z wykorzystaniem instrukcji](preparing-databases/_static/image6.png)

    Wybranie tej opcji menu dodaje następujący kod do `using` instrukcje górnej części pliku.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.

Projekt jest gotowy do wdrożenia *ContosoUniversity* bazy danych. Po wdrożeniu aplikacji, podczas pierwszego uruchomienia go i przejdź do strony, który uzyskuje dostęp do bazy danych, Code First będzie utworzyć bazę danych i uruchom to `Seed` metody.

> [!NOTE]
> Dodawanie kodu do `Seed` metoda jest wiele sposobów wstawiany stałych z danymi w bazie danych. Alternatywą jest Dodaj kod, aby `Up` i `Down` metody każdej klasy migracji. `Up` i `Down` metody zawiera kod, który implementuje zmian w bazie danych. Zobaczysz przykłady je w [wdrażanie aktualizacji bazy danych](deploying-a-database-update.md) samouczka.
> 
> Można również napisać kod, który wykonuje instrukcje SQL przy użyciu `Sql` metody. Na przykład dodawania kolumny budżetu do tabeli działu, aby zainicjować wszystkich budżetów działów do $1000,00 w ramach migracji należy dodać wierszy folllowing kodu `Up` metodę dla tej migracji:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Tworzenie skryptów dla wdrożenia bazy danych członkostwa

Aplikacja Contoso University używa uwierzytelniania formularzy i systemu członkostwa programu ASP.NET do uwierzytelniania i autoryzacji użytkowników. **Środków aktualizacji** strona jest dostępna tylko dla użytkowników, którzy są w roli administratora.

Uruchom aplikację, a następnie kliknij przycisk **kursów**, a następnie kliknij przycisk **środków aktualizacji**.

![Kliknij przycisk środków aktualizacji](preparing-databases/_static/image7.png)

**Zaloguj** zostanie wyświetlona strona, ponieważ **środków aktualizacji** strona wymaga uprawnień administracyjnych.

Wprowadź *admin* jako nazwy użytkownika i *devpwd* jako hasło i kliknij przycisk **Zaloguj**.

![Zaloguj się na stronie](preparing-databases/_static/image8.png)

**Środków aktualizacji** zostanie wyświetlona strona.

![Strona środki na korzystanie z aktualizacji](preparing-databases/_static/image9.png)

Informacje użytkownika i roli znajdują się w *aspnet ContosoUniversity* bazy danych, który jest określony przez **połączenia DefaultConnection** parametry połączenia w *Web.config* pliku.

Ta baza danych nie jest zarządzany przez Entity Framework Code First, więc nie można wdrożyć za pomocą migracji. Dostawcy dbDacFx zostanie użyty do wdrożenia schemat bazy danych, a następnie należy skonfigurować profil publikowania, aby uruchomić skrypt, który powoduje wstawienie początkowe dane w tabelach bazy danych.

> [!NOTE]
> Nowego systemu członkostwa programu ASP.NET (teraz nazwę ASP.NET Identity) programu Microsoft Visual Studio 2013. Nowy system umożliwia utrzymanie zarówno aplikacji, jak i tabele członkostwa w tej samej bazy danych, a można wdrożyć obydwa migracje Code First. Przykładowa aplikacja korzysta z wcześniej systemu członkostwa programu ASP.NET, której nie można wdrożyć przy użyciu migracje Code First. Procedury dotyczące wdrażania tej bazy danych członkostwa również zastosowanie do każdej innej sytuacji, w którym aplikacja ma wdrażania bazy danych programu SQL Server, który nie jest tworzony przez Entity Framework Code First.


W tym miejscu, zwykle nie ma tych samych danych w środowisku produkcyjnym, czy masz do rozwoju. Podczas wdrażania witryny po raz pierwszy jest często, aby wykluczyć większość lub wszystkie konta użytkowników, które tworzysz do testowania. W związku z tym pobranego projektu ma dwie bazy danych członkostwa: *aspnet ContosoUniversity.mdf* użytkownikom programowanie i *aspnet-ContosoUniversity-Prod.mdf* użytkownikom produkcji. W tym samouczku nazwy użytkowników są takie same, w obu bazach danych: *admin* i *nonadmin*. Użytkownicy mają hasło *devpwd* w projektowej bazie danych i *prodpwd* w produkcyjnej bazie danych.

Użytkownicy programowanie wdrożony do środowiska testowego i produkcyjnego użytkownikom tymczasową i produkcyjną. W tym celu zostaną utworzone dwa skrypty SQL, w tym samouczku, jeden dla rozwoju i jeden w środowisku produkcyjnym, a w kolejnych samouczkach skonfigurujesz proces publikowania ich uruchamiać.

> [!NOTE]
> Baza danych członkostwa przechowuje skrót hasła do kont. Aby można było wdrożyć kont z jednego komputera na inny, należy się upewnić procedury wyznaczania wartości skrótu nie Generowanie skrótów różnych na serwerze docelowym niż na komputerze źródłowym. Tej samej wartości skrótu zostanie wygenerowany używania dostawców uniwersalnych ASP.NET, pod warunkiem, nie zmieniaj domyślny algorytm. Domyślny algorytm jest HMACSHA256 i jest określony w **weryfikacji** atrybutu  **[machineKey](https://msdn.microsoft.com/en-us/library/system.web.configuration.machinekeysection.aspx)**  elementu w pliku Web.config.


Skrypty wdrażania danych można utworzyć ręcznie, za pomocą programu SQL Server Management Studio (SSMS) lub przy użyciu narzędzia innej firmy. Ta pozostałej części tego samouczka zostanie pokazują, jak to zrobić w programie SSMS, ale jeśli nie chcesz zainstalować i używać narzędzia SSMS można uzyskać skrypty ukończone wersji projektu i przejdź do sekcji, w której są przechowywane w folderze rozwiązania.

Aby zainstalować narzędzia SSMS, zainstaluj go z [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/en-us/download/details.aspx?id=29062) , klikając [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) lub [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Po wybraniu niewłaściwy systemu nie będzie można go zainstalować i spróbować jeden z nich.

(Należy pamiętać, że jest to pobieranie 600 MB. Może potrwać długo, aby zainstalować i będzie wymagać ponownego uruchomienia komputera.)

Na pierwszej stronie Centrum instalacji programu SQL Server, kliknij przycisk **nowy serwer SQL instalacji autonomicznej lub Dodaj funkcje do istniejącej instalacji**i postępuj zgodnie z instrukcjami, akceptując ustawień domyślnych.

### <a name="create-the-development-database-script"></a>Utwórz skrypt programowanie bazy danych

1. Uruchom narzędzia SSMS.
2. W **Połącz z serwerem** okna dialogowego wprowadź *(localdb) \v11.0* jako **nazwy serwera**, pozostaw **uwierzytelniania** ustawioną **Uwierzytelniania systemu Windows**, a następnie kliknij przycisk **Connect**.

    ![SSMS połączyć się z serwerem](preparing-databases/_static/image10.png)
3. W **Eksplorator obiektów** okna, rozwiń węzeł **baz danych**, kliknij prawym przyciskiem myszy **aspnet ContosoUniversity**, kliknij przycisk **zadania**, a następnie kliknij przycisk **Generowania skryptów**.

    ![SSMS generowania skryptów](preparing-databases/_static/image11.png)
4. W **Generowanie i skrypty publikowania** okno dialogowe, kliknij przycisk **Ustaw opcje obsługi skryptów**.

    Można pominąć **wybierz obiekty** krok, ponieważ wartość domyślna to **skryptów całej bazy danych i wszystkie obiekty bazy danych** i chcesz.
5. Kliknij przycisk **zaawansowane**.

    ![Opcje obsługi skryptów programu SSMS](preparing-databases/_static/image12.png)
6. W **zaawansowane opcje obsługi skryptów** okno dialogowe, przewiń w dół do **typy danych do skryptu**i kliknij przycisk **tylko dane** opcji na liście rozwijanej.
7. Zmień **skryptu UŻYJ bazy danych** do **False**. UŻYJ instrukcji są nieprawidłowe dla bazy danych SQL Azure i nie są potrzebne do wdrożenia programu SQL Server Express w środowisku testowym.

    ![SSMS skryptu tylko dane, nie użycie instrukcji](preparing-databases/_static/image13.png)
8. Kliknij przycisk **OK**.
9. W **Generowanie i skrypty publikowania** okno dialogowe **nazwę pliku** pole określa, gdzie można utworzyć skrypt. Zmień ścieżkę do folderu rozwiązania (folder zawierający plik ContosoUniversity.sln) i nazwę pliku, aby *aspnet-data-dev.sql*.
10. Kliknij przycisk **dalej** można przejść do **Podsumowanie** , a następnie kliknij pozycję **dalej** ponownie, aby utworzyć skrypt.

    ![Skrypt SSMS utworzony](preparing-databases/_static/image14.png)
11. Kliknij przycisk **Zakończ**.

### <a name="create-the-production-database-script"></a>Tworzenie skryptu bazy danych w środowisku produkcyjnym

Ponieważ projekt nie zostało uruchomione z produkcyjną bazę danych, nie jest dołączona jeszcze z wystąpieniem LocalDB. W związku z tym należy najpierw dołączyć bazy danych.

1. W programie SSMS **Eksplorator obiektów**, kliknij prawym przyciskiem myszy **baz danych** i kliknij przycisk **Attach**.

    ![Dołącz SSMS](preparing-databases/_static/image15.png)
- W **dołączyć bazy danych** okno dialogowe, kliknij przycisk **Dodaj** , a następnie przejdź do *aspnet-ContosoUniversity-Prod.mdf* w pliku *aplikacji\_ Dane* folderu.

    ![Dodaj SSMS plików .mdf można dołączyć](preparing-databases/_static/image16.png)
- Kliknij przycisk **OK**.
- Postępuj zgodnie z tą samą procedurą, który został wcześniej użyty do utworzenia skryptu dla pliku produkcji. Nazwa pliku skryptu *aspnet-data-prod.sql*.

## <a name="summary"></a>Podsumowanie

Obie bazy danych są teraz gotowe do wdrożenia i mieć dwa skrypty wdrażania danych w folderze rozwiązania.

![Skrypty wdrażania danych](preparing-databases/_static/image17.png)

W samouczku następujące skonfigurować ustawienia projektu, które mają wpływ na wdrożenie, a następnie skonfigurować automatyczne *Web.config* pliku przekształcenia ustawień, które muszą być różne w wdrożonej aplikacji.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o NuGet, zobacz [Zarządzanie biblioteki projektu z NuGet](https://msdn.microsoft.com/en-us/magazine/hh547106.aspx) i [dokumentacji NuGet](http://docs.nuget.org/docs/start-here/overview). Jeśli nie chcesz używać NuGet, należy dowiedzieć się, jak analizować pakietu NuGet, aby ustalić, jakie operacje po jej zainstalowaniu. (Na przykład może skonfigurować *Web.config* przekształcenia, skonfigurować skrypty programu PowerShell do uruchamiania w czasie kompilacji itp.) Aby dowiedzieć się więcej na temat działania NuGet, zobacz [tworzenie i publikowanie pakietu](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) i [pliku konfiguracji i przekształcenia kod źródłowy](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Poprzednie](introduction.md)
[dalej](web-config-transformations.md)
