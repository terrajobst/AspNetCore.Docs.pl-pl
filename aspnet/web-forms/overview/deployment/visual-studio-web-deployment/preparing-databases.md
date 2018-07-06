---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Przygotowanie do wdrożenia bazy danych | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: a9ddeda3bfe4315c835cd447f6178669797dceb2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803194"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Przygotowanie do wdrożenia bazy danych
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak można pobrać projektu gotowe do wdrożenia bazy danych. Struktura bazy danych, a niektóre (nie wszystkie) danych w dwóch aplikacji bazy danych musi zostać wdrożony na testów, przemieszczania i środowisk produkcyjnych.

Zazwyczaj podczas opracowywania aplikacji, możesz wprowadzić dane testowe do bazy danych, których nie chcesz, aby wdrożyć w witrynie na żywo. Jednak może również być części danych produkcyjnych, które chcesz wdrożyć. W tym samouczku możesz skonfigurować projekt Contoso University i przygotować skrypty SQL poprawnych danych jest uwzględniony, podczas wdrażania.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Przykładowa aplikacja korzysta z programu SQL Server Express LocalDB. SQL Server Express to bezpłatna wersja programu SQL Server. Często są one wykorzystywane podczas projektowania, ponieważ jest on oparty na tego samego aparatu bazy danych jako pełne wersje programu SQL Server. Możesz przeprowadzić test za pomocą programu SQL Server Express i mieć pewność, że aplikacja będzie działa tak samo w środowisku produkcyjnym, z pewnymi wyjątkami dla funkcji, które być różne w różnych wersjach programu SQL Server.

LocalDB to specjalnego trybu wykonania programu SQL Server Express, która umożliwia pracę z bazami danych jako *.mdf* plików. Zazwyczaj pliki bazy danych LocalDB są przechowywane w *aplikacji\_danych* folderu projektu sieci web. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia także pracować z *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzała; dlatego LocalDB jest zalecane w przypadku pracy z *.mdf* plików.

Zazwyczaj programu SQL Server Express nie jest używana dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web, ponieważ nie jest przeznaczony do pracy z usługami IIS.

W programie Visual Studio 2012 LocalDB jest instalowany domyślnie z programem Visual Studio. W Visual Studio 2010 i starszych, SQL Server Express (bez LocalDB) jest instalowany domyślnie z programem Visual Studio; oznacza to dlaczego zainstalowany jako jeden z warunków wstępnych w [pierwszym samouczku tej serii](introduction.md).

Aby uzyskać więcej informacji o wersjach programu SQL Server, w tym LocalDB, zobacz następujące zasoby [Praca z bazami danych programu SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework i dostawców uniwersalnych

Aby uzyskać dostęp do bazy danych aplikacja Contoso University wymaga następującego oprogramowania, które muszą być wdrożone razem z aplikacją, ponieważ nie znajduje się w programie .NET Framework:

- [Dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umożliwia systemu członkostwa programu ASP.NET do korzystania z usługi Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Ponieważ to oprogramowanie jest uwzględniony w pakietach NuGet, projekt jest już skonfigurowane tak, aby wymaganych zestawów są wdrażane przy użyciu projektu. (Łącza punktu z bieżącą wersją, te pakiety, które może być nowsza niż zainstalowana w projekt startowy, który został pobrany w ramach tego samouczka).

Jeśli wdrażasz do dostawcy hostingu innych firm, a nie usługi Azure, należy upewnić się, że platformy Entity Framework w wersji 5.0 lub nowszy. Wcześniejszych wersjach migracje Code First wymaga pełnego zaufania, a większość dostawców usług hostingu spowoduje uruchomienie aplikacji w trybie średniego zaufania. Aby uzyskać więcej informacji o trybie średniego zaufania, zobacz [wdrażanie w usługach IIS jako środowisku testowym](deploying-to-iis.md) samouczka.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurowanie migracje Code First dla wdrażania bazy danych aplikacji

Baza danych aplikacji Contoso University odbywa się przez rozwiązanie Code First i wdrażana za pomocą migracje Code First. Aby uzyskać omówienie wdrożenia bazy danych przy użyciu migracje Code First, zobacz [pierwszym samouczku tej serii](introduction.md).

Podczas wdrażania bazy danych aplikacji, zazwyczaj nie po prostu są wdrażane rozwoju bazy danych przy użyciu wszystkich danych znajdujących się w niej do środowiska produkcyjnego, ponieważ większość danych, prawdopodobnie jest tylko do celów testowych. Na przykład nazwy dla uczniów w bazie danych testu są fikcyjne. Z drugiej strony, często nie można wdrożyć tylko struktury bazy danych bez danych w niej w ogóle. Niektóre dane w bazie danych testu może być prawdziwe dane i musi być dostępne, gdy użytkownicy rozpocząć korzystanie z aplikacji. Na przykład baza danych może być tabeli, która zawiera wartości nieprawidłowa klasa lub nazwy rzeczywistych działów.

Aby zasymulować ten częsty scenariusz, należy skonfigurować migracje Code First `Seed` metodę, która wstawia do bazy danych, które mają być dostępne w środowisku produkcyjnym. To `Seed` metoda nie należy wstawić dane testowe, ponieważ będzie ona uruchamiana w środowisku produkcyjnym, po pierwszym kod tworzy bazę danych w środowisku produkcyjnym.

We wcześniejszych wersjach programu Code First przed migracji został wydany, było często `Seed` metody, aby wstawić dane testowe Ponadto, ponieważ przy każdej zmianie modelu podczas tworzenia bazy danych musiały być całkowicie usunięta i utworzona ponownie od podstaw. Migracje Code First, test, dane są zachowywane po wprowadzeniu zmian w bazie danych, więc łącznie danych testowych w `Seed` metoda nie jest konieczne. Pobrany projekt używa metody, w tym wszystkie dane w `Seed` metody klasy inicjatora. W tym samouczku zostanie wyłączone tej klasy inicjatora i `enable Migrations. Then you'll update the `inicjatora "metody w konfiguracji migracje klasy, tak aby wstawia tylko dane, które mają zostać wstawione w środowisku produkcyjnym.

Na poniższym diagramie przedstawiono schemat bazy danych aplikacji:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Te samouczki będzie założył, że `Student` i `Enrollment` tabel powinien być pusty, gdy najpierw zostanie wdrożona lokacja. Inne tabele zawierają dane, który ma zostać wstępnie załadowane, gdy aplikacja przechodzi na żywo.

### <a name="disable-the-initializer"></a>Wyłącz inicjatora

Ponieważ będziesz używać migracje Code First, będą musieli używać `DropCreateDatabaseIfModelChanges` inicjatora Code First. Kod dla tego inicjatora znajduje się w *SchoolInitializer.cs* pliku w projekcie ContosoUniversity.DAL. Ustawienie w `appSettings` elementu *Web.config* tego inicjatora zawsze, gdy aplikacja próbuje dostęp do bazy danych po raz pierwszy powoduje, że plik:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Otwórz aplikację *Web.config* plików i usunąć lub skomentować `add` element, który określa Code First klasy inicjatora. `appSettings` Element wygląda teraz następująco:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Innym sposobem określenia klasy inicjatora to zrobić, wywołując `Database.SetInitializer` w `Application_Start` method in Class metoda *Global.asax* pliku. Jeśli włączasz migracji w projekcie, który używa tej metody w celu określenia inicjatora, Usuń ten wiersz kodu.


> [!NOTE]
> Jeśli używasz programu Visual Studio 2013, należy dodać następujące kroki między kroki 2 i 3: wprowadź () w konsoli zarządzania Pakietami "pakietu aktualizacji entityframework — wersja 6.1.1" Aby uzyskać bieżącą wersję programu EF. Następnie (b), skompiluj projekt, aby uzyskać listę błędów kompilacji i je rozwiązać. Usunąć za pomocą instrukcji dla przestrzeni nazw, który już nie istnieje, kliknij prawym przyciskiem myszy i kliknij przycisk Rozwiąż, aby dodać za pomocą instrukcji, w którym będą one potrzebne i zmień wystąpień System.Data.EntityState System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Włącz migracje Code First

1. Upewnij się, że projekt ContosoUniversity (nie ContosoUniversity.DAL) jest ustawiony jako projekt startowy. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz **Ustaw jako projekt startowy**. Migracje Code First będzie wyszukiwał projekt startowy, aby znaleźć parametry połączenia bazy danych.
2. Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** (lub **Menedżera pakietów NuGet**) i następnie **Konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. W górnej części **Konsola Menedżera pakietów** oknie Wybierz ContosoUniversity.DAL jako domyślny projekt, a następnie co `PM>` wierszu wprowadź "enable-migrations".

    ![polecenia enable-migrations](preparing-databases/_static/image4.png)

    (Jeśli zostanie wyświetlony błąd powiedzenie *enable-migrations* nie rozpoznano polecenia, wpisz polecenie *pakietu aktualizacji EntityFramework-ponownej instalacji* , a następnie spróbuj ponownie.)

    To polecenie umożliwia utworzenie *migracje* folderu w projekcie ContosoUniversity.DAL i umieszcza w tym folderze dwa pliki: *Configuration.cs* pliku, który służy do konfigurowania migracji i *InitialCreate.cs* pierwszej migracji, który tworzy bazę danych w pliku.

    ![Folder migracji](preparing-databases/_static/image5.png)

    Wybrany projekt DAL w **projekt domyślny** listy rozwijanej **Konsola Menedżera pakietów** ponieważ `enable-migrations` polecenie musi zostać wykonane w projekcie, który zawiera Code First klasy kontekstu. Ta klasa znajduje się w projekcie biblioteki klas, migracje Code First wyszukuje parametry połączenia bazy danych w projekt startowy rozwiązania. W tym rozwiązaniu ContosoUniversity projektu sieci web został ustawiony jako projekt startowy. Jeśli nie chcesz wyznaczyć projektu, który ma parametry połączenia jako projekt startowy w programie Visual Studio, można określić projekt startowy w polecenia programu PowerShell. Aby wyświetlić składnię polecenia, wpisz polecenie `get-help enable-migrations`.

    `enable-migrations` Polecenia automatycznie utworzył pierwszej migracji, ponieważ baza danych już istnieje. Alternatywą jest migracje utworzyć bazę danych. Aby to zrobić, należy użyć **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** można usunąć bazy danych ContosoUniversity przed włączeniem migracji. Po włączeniu migracji, należy ręcznie utworzyć pierwszej migracji, wprowadzając polecenie "Dodaj migracji InitialCreate". Następnie można utworzyć bazy danych, wprowadzając polecenie "update-database".

### <a name="set-up-the-seed-method"></a>Konfigurowanie Seed — metoda

W tym samouczku dodasz danych, dodając kod do `Seed` metody migracje Code First `Configuration` klasy. Kod wywołuje metodę migracji First `Seed` metody po każdej migracji.

Ponieważ `Seed` metoda jest uruchamiana po każdym migracji, danych już istnieje w tabelach po pierwszej migracji. Aby obsłużyć taką sytuację, użyjesz `AddOrUpdate` metodę aktualizującą wiersze, które już zostały wstawione lub wstawić je, jeśli ich jeszcze nie istnieją. `AddOrUpdate` Metoda może być najlepszym wyborem dla danego scenariusza. Aby uzyskać więcej informacji, zobacz [powinien zachować ostrożność przy użyciu metody AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

1. Otwórz *Configuration.cs* i Zastąp komentarze w `Seed` metoda następującym kodem:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Odwołania do `List` mają czerwoną linią falistą w ramach nich, ponieważ nie masz `using` instrukcji jeszcze swojej przestrzeni nazw. Kliknij prawym przyciskiem myszy jednego z wystąpień `List` i kliknij przycisk **rozwiązać**, a następnie kliknij przycisk **przy użyciu System.Collections.Generic**.

    ![Rozwiązanie przy użyciu instrukcji](preparing-databases/_static/image6.png)

    Ta opcja menu dodaje następujący kod do `using` instrukcji w górnej części pliku.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.

Projekt jest teraz gotowe do wdrożenia *ContosoUniversity* bazy danych. Po wdrożeniu aplikacji, podczas pierwszego uruchomienia go i przejdź do strony, który uzyskuje dostęp do bazy danych, Code First będzie utworzyć bazę danych i uruchom to `Seed` metody.

> [!NOTE]
> Dodawanie kodu do `Seed` metodą jest jeden z wielu sposobów wstawiany danych do bazy danych. Alternatywą jest Dodaj kod, aby `Up` i `Down` metody każda klasa migracji. `Up` i `Down` metody zawierają kod, który implementuje zmian w bazie danych. Zobaczysz je w przykłady [wdrażanie aktualizacji bazy danych](deploying-a-database-update.md) samouczka.
> 
> Można także napisać kod, który wykonuje instrukcje SQL przy użyciu `Sql` metody. Na przykład, jeśli były dodanie kolumny budżetu Tabela działu i chcesz zainicjować wszystkich budżetów działu do $1000,00 w ramach migracji, możesz dodać wiersz folllowing kodu do `Up` metody dla tej migracji:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Tworzenie skryptów do wdrożenia bazy danych członkostwa

Aplikacja Contoso University korzysta z uwierzytelniania systemu i formularze członkostwa ASP.NET do uwierzytelniania i autoryzacji użytkowników. **Aktualizacji środki na korzystanie z** strona jest dostępna tylko dla użytkowników, którzy znajdują się w roli administratora.

Uruchom aplikację, a następnie kliknij przycisk **kursów**, a następnie kliknij przycisk **aktualizacji środki na korzystanie z**.

![Kliknij przycisk Aktualizuj środki na korzystanie z](preparing-databases/_static/image7.png)

**Zaloguj** zostanie wyświetlona strona, ponieważ **aktualizacji środki na korzystanie z** strona wymaga uprawnień administracyjnych.

Wprowadź *administratora* jako nazwy użytkownika i *devpwd* jako hasło i kliknij przycisk **Zaloguj**.

![Zaloguj się na stronie](preparing-databases/_static/image8.png)

**Aktualizacji środki na korzystanie z** zostanie wyświetlona strona.

![Strona środki na korzystanie z aktualizacji](preparing-databases/_static/image9.png)

Informacje użytkownika i roli są w *aspnet ContosoUniversity* bazy danych, który jest określony przez **DefaultConnection** parametry połączenia w *Web.config* pliku.

Ta baza danych nie jest zarządzane przez Entity Framework Code First, więc nie możesz użyć migracje jej wdrożenie. Dostawcy dbDacFx zostanie użyty do wdrożenia schematu bazy danych. Ponadto należy skonfigurować profil publikowania do uruchomienia skryptu, który wstawia początkowe dane w tabelach bazy danych.

> [!NOTE]
> Nowego systemu członkostwa programu ASP.NET (teraz nazwę produktu ASP.NET Identity) została wprowadzona w programie Visual Studio 2013. Nowy system umożliwia utrzymanie zarówno aplikacji, jak i tabele członkostwa w tej samej bazy danych, a następnie można użyć migracje Code First można wdrożyć obydwa elementy. Przykładowa aplikacja używa starszego systemu członkostwa programu ASP.NET, której nie można wdrożyć przy użyciu migracje Code First. Procedury dotyczące wdrażania tej bazy danych członkostwa również zastosowanie do każdej innej sytuacji, w którym Twoja aplikacja potrzebuje do wdrożenia bazy danych programu SQL Server, który nie jest tworzony przez Entity Framework Code First.


W tym miejscu, zwykle nie chcesz tych samych danych w środowisku produkcyjnym, czy masz w trakcie opracowywania. Po raz pierwszy zostanie wdrożona lokacja, jest często większość lub wszystkie konta użytkowników, które tworzysz do testowania. W związku z tym, pobrany projekt zawiera dwie bazy danych członkostwa: *aspnet ContosoUniversity.mdf* użytkownikom rozwoju i *aspnet-ContosoUniversity-Prod.mdf* z użytkownikami w środowisku produkcyjnym. W tym samouczku nazwy użytkowników są takie same w obu bazach danych: *administratora* i *tekst nonadmin*. Zarówno użytkownicy mają hasło *devpwd* w bazie danych rozwoju i *prodpwd* w produkcyjnej bazie danych.

Wdrożysz użytkownikom tworzenia środowiska testowego i produkcyjnego użytkownikom środowisk przejściowych i produkcyjnych. Aby to zrobić utworzysz dwa skrypty SQL, w tym samouczku, jeden do tworzenia aplikacji, a drugi dla środowiska produkcyjnego, a w kolejnych samouczkach należy skonfigurować proces publikowania, aby uruchomić je.

> [!NOTE]
> Skrót hasła do kont są przechowywane w bazie danych członkostwa. Aby wdrożyć kont z jednego komputera, należy upewnić się, czy procedury wyznaczania wartości skrótu nie Generuj różne skróty na serwerze docelowym niż na ich na komputerze źródłowym. One wygeneruje tych samych skrótów korzystając z dostawców uniwersalnych ASP.NET, tak długo, jak nie zmieniaj domyślny algorytm. Domyślny algorytm jest HMACSHA256 i jest określony w **weryfikacji** atrybutu **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** elementu w pliku Web.config.


Skrypty wdrażania danych można utworzyć ręcznie, za pomocą programu SQL Server Management Studio (SSMS) lub za pomocą narzędzia innych firm. Ta pozostałej części tego samouczka opisano, jak to zrobić w programie SSMS, ale jeśli nie chcesz zainstalować i używać narzędzia SSMS można uzyskać skrypty z pełną wersję projektu i przejść do sekcji, w której są przechowywane w folderze rozwiązania.

Aby zainstalować narzędzie SSMS, zainstaluj go z [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) , klikając [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) lub [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Jeśli wybierzesz niewłaściwy systemu zakończy się niepowodzeniem do zainstalowania i wypróbować jeden z nich.

(Zwróć uwagę, że jest dostępny do pobrania 600 MB. Może potrwać dłuższy czas instalacji i konieczne będzie ponowne uruchomienie komputera.)

Na pierwszej stronie Centrum instalacji programu SQL Server, kliknij przycisk **nowy serwer SQL instalacji autonomicznej lub Dodaj funkcje do istniejącej instalacji**i postępuj zgodnie z instrukcjami, przyjmuje opcje domyślne.

### <a name="create-the-development-database-script"></a>Utworzyć skrypt tworzenia bazy danych

1. Uruchom program SSMS.
2. W **Połącz z serwerem** okna dialogowego wprowadź *(localdb) \v11.0* jako **nazwy serwera**, pozostaw **uwierzytelniania** równa **Uwierzytelniania Windows**, a następnie kliknij przycisk **Connect**.

    ![SSMS połączyć się z serwerem](preparing-databases/_static/image10.png)
3. W **Eksplorator obiektów** okna, rozwiń węzeł **baz danych**, kliknij prawym przyciskiem myszy **aspnet ContosoUniversity**, kliknij przycisk **zadania**, a następnie kliknij przycisk **Generowania skryptów**.

    ![SSMS generowania skryptów](preparing-databases/_static/image11.png)
4. W **Generuj oraz skryptów publikowania** okno dialogowe, kliknij przycisk **Ustaw opcje obsługi skryptów**.

    Możesz pominąć **wybierz obiekty** krok, ponieważ wartość domyślna to **skrypt całą bazę danych i wszystkie obiekty bazy danych** i to, co chcesz.
5. Kliknij przycisk **zaawansowane**.

    ![Opcje obsługi skryptów programu SSMS](preparing-databases/_static/image12.png)
6. W **zaawansowane opcje obsługi skryptów** okno dialogowe, przewiń w dół do **typy danych do skryptu**i kliknij przycisk **tylko dane** opcji na liście rozwijanej.
7. Zmiana **skryptu UŻYJ bazy danych** do **False**. UŻYJ instrukcji nie są prawidłowe dla usługi Azure SQL Database i nie są wymagane do wdrożenia programu SQL Server Express w środowisku testowym.

    ![SSMS danych tylko skrypt, nie użycie instrukcji](preparing-databases/_static/image13.png)
8. Kliknij przycisk **OK**.
9. W **Generuj oraz skryptów publikowania** okno dialogowe **nazwy pliku** pole określa, gdzie zostanie utworzony skrypt. Zmień ścieżkę, aby Twój folder rozwiązania (folder zawierający plik ContosoUniversity.sln) i nazwę pliku, aby *aspnet-data-dev.sql*.
10. Kliknij przycisk **dalej** można przejść do **Podsumowanie** kartę, a następnie kliknij przycisk **dalej** ponownie, aby utworzyć skrypt.

    ![Skrypt programu SSMS utworzony](preparing-databases/_static/image14.png)
11. Kliknij przycisk **Zakończ**.

### <a name="create-the-production-database-script"></a>Utworzyć skrypt bazy danych w środowisku produkcyjnym

Ponieważ projekt nie zostały uruchomione z produkcyjnej bazy danych, nie jest dołączona jeszcze do wystąpienia LocalDB. W związku z tym musisz najpierw dołączyć bazy danych.

1. W programie SSMS **Eksplorator obiektów**, kliknij prawym przyciskiem myszy **baz danych** i kliknij przycisk **Dołącz**.

    ![Dołączanie programu SSMS](preparing-databases/_static/image15.png)
2. W **Dołącz bazę danych** okno dialogowe, kliknij przycisk **Dodaj** , a następnie przejdź do *aspnet-ContosoUniversity-Prod.mdf* w pliku *aplikacji\_ Dane* folderu.

     ![Dodaj program SSMS pliku mdf, aby dołączyć](preparing-databases/_static/image16.png)
3. Kliknij przycisk **OK**.
4. Postępuj zgodnie z tej samej procedury, które było wcześniej używane do tworzenia skryptów dla pliku produkcji. Nazwa pliku skryptu *aspnet-data-prod.sql*.

## <a name="summary"></a>Podsumowanie

Obu bazach danych są teraz gotowe do wdrożenia, i masz dwa skrypty wdrażania danych w folderze rozwiązania.

![Skrypty wdrażania danych](preparing-databases/_static/image17.png)

W następującego samouczka skonfigurujesz ustawienia projektu, które mają wpływ na wdrożenie, a następnie skonfigurować automatyczne *Web.config* pliku przekształcenia ustawień, które muszą być inne w przypadku wdrożonej aplikacji.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat NuGet, zobacz [Zarządzanie biblioteki projektu z NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) i [dokumentacja programu NuGet](http://docs.nuget.org/docs/start-here/overview). Jeśli nie chcesz użyć NuGet, należy dowiedzieć się, jak analizować pakietu NuGet, aby ustalić, jakie operacje po jej zainstalowaniu. (Na przykład, może skonfigurować *Web.config* transformacji, skonfigurować skrypty programu PowerShell do uruchamiania w czasie kompilacji, itp.) Aby dowiedzieć się więcej na temat sposobu działania NuGet, zobacz [tworzenie i publikowanie pakietu](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) i [pliku konfiguracji i przekształceń kodu źródłowego](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Poprzednie](introduction.md)
> [dalej](web-config-transformations.md)
