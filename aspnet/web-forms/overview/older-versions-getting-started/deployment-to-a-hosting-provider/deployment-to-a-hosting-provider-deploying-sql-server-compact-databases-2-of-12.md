---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie Compact baz danych SQL Server - 2 z 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 378bcc038335ee852cd1a6c6e545eb72c6e0c78b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755591"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie Compact baz danych SQL Server - 2 z 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak skonfigurować dwie bazy danych programu SQL Server Compact i aparatu bazy danych dla wdrożenia.

Aby uzyskać dostęp do bazy danych aplikacja Contoso University wymaga następującego oprogramowania, które muszą być wdrożone razem z aplikacją, ponieważ nie znajduje się w programie .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (aparat bazy danych).
- [Dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (który Włącz systemu członkostwa programu ASP.NET do korzystania z programu SQL Server Compact)
- [Entity Framework w wersji 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(kod: najpierw w migracji).

Struktura bazy danych, a niektóre (nie wszystkie) danych w dwóch aplikacji bazy danych musi być także wdrożony. Zazwyczaj podczas opracowywania aplikacji, możesz wprowadzić dane testowe do bazy danych, których nie chcesz, aby wdrożyć w witrynie na żywo. Jednak może również wprowadzić części danych produkcyjnych, które chcesz wdrożyć. W tym samouczku skonfigurujesz projektu Contoso University tak, aby wymagane oprogramowanie i poprawne dane są uwzględniane podczas wdrażania.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact i SQL Server Express

Przykładowa aplikacja korzysta z programu SQL Server Compact 4.0. Ten aparat bazy danych jest stosunkowo nowa opcja dla witryn sieci Web; wcześniejszych wersjach programu SQL Server Compact nie działają w środowisku hostingu w sieci web. SQL Server Compact oferuje kilka zalet w porównaniu do bardziej typowy scenariusz tworzenia przy użyciu programu SQL Server Express i wdrażania do pełnej wersji programu SQL Server. W zależności od wybranego dostawcy hostingu programu SQL Server Compact może być tańsze do wdrożenia, ponieważ niektórzy dostawcy opłaty za dodatkowe do obsługi pełnego bazy danych SQL Server. Istnieje nie dodatkowych opłat w przypadku programu SQL Server Compact, ponieważ aparat bazy danych sam można wdrożyć jako część aplikacji sieci web.

Jednakże należy także pamiętać o jego pewne ograniczenia. SQL Server Compact nie obsługuje procedur składowanych, wyzwalaczy, widoków ani replikacji. (Aby uzyskać pełną listę funkcji programu SQL Server, które nie są obsługiwane przez program SQL Server Compact, zobacz [różnice między programu SQL Server Compact i SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Ponadto niektóre narzędzia, które służy do modyfikowania schematów i danych w programie SQL Server Express i baz danych programu SQL Server nie działają z programu SQL Server Compact. Na przykład nie można używać programu SQL Server Management Studio lub SQL Server Data Tools w programie Visual Studio baz danych programu SQL Server Compact. Masz inne opcje dotyczące pracy z bazami danych programu SQL Server Compact:

- Za pomocą Eksploratora serwera w programie Visual Studio, która oferuje funkcje manipulowania ograniczone bazy danych dla programu SQL Server Compact.
- Można użyć funkcji manipulowania bazy danych z [WebMatrix](https://www.microsoft.com/web/webmatrix/), która zawiera więcej funkcji niż Eksploratora serwera.
- Możesz użyć stosunkowo oferujący innych firm lub narzędzia open source, takich jak [programu SQL Server Compact Toolbox](https://github.com/ErikEJ/SqlCeToolbox) i [SQL Compact danych i schematu skryptu narzędzia](https://github.com/ErikEJ/SqlCeToolbox).
- Można napisać i exported własne (języka definicji danych) do modyfikowania schematu bazy danych.

Można uruchomić za pomocą programu SQL Server Compact i uaktualnisz później w miarę ewolucji potrzeb. W kolejnych samouczkach z tej serii dowiesz się, jak przeprowadzić migrację z programu SQL Server Compact, SQL Server Express i programu SQL Server. Jednak w przypadku tworzenia nowej aplikacji i będziesz już potrzebować w najbliższej przyszłości programu SQL Server, jest prawdopodobnie najlepiej rozpocząć od programu SQL Server lub SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurowanie aparatu programu SQL Server Compact bazy danych na potrzeby wdrażania

Oprogramowanie wymagane do dostępu do danych w aplikacji Contoso University został dodany, zainstaluj następujące pakiety NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (dostawców uniwersalnych ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Łącza punktu z bieżącą wersją, te pakiety, które może być nowsza niż zainstalowana w projekt startowy, który został pobrany w ramach tego samouczka. We wdrożeniach w dostawcy usług hostingowych upewnij się, że używasz programu Entity Framework w wersji 5.0 lub nowszy. Wcześniejszych wersjach migracje Code First wymaga pełnego zaufania, a u wielu dostawców hostingu aplikacja jest uruchamiana w trybie średniego zaufania. Aby uzyskać więcej informacji o trybie średniego zaufania, zobacz [wdrażanie w usługach IIS jako środowisku testowym](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczka.

Instalacja pakietu NuGet zazwyczaj zajmuje się wszystko, co jest potrzebne, aby można było wdrożyć to oprogramowanie z aplikacją. W niektórych przypadkach obejmuje to zadania takie jak zmiana w pliku Web.config i dodawanie skryptów programu PowerShell, które uruchamiana po każdym Skompiluj rozwiązanie. **Jeśli chcesz dodać obsługę dowolnej z tych funkcji (np. programu SQL Server Compact i Entity Framework) bez za pomocą narzędzia NuGet, upewnij się, wiedzieć, jak instalacji pakietu NuGet działa, dzięki czemu można zrobić to ręcznie.**

Istnieje jeden wyjątek gdzie NuGet nie przyjmuje opieki nad wszystko, czego należy wykonać w celu zapewnienia pomyślnego wdrożenia. Pakiet SqlServerCompact NuGet dodaje skrypt po kompilacji do projektu, który kopiuje natywnych zestawów, które mają *x86* i *amd64* podfoldery w projekcie *bin* folder, ale skrypt nie obejmuje te foldery w projekcie. W rezultacie narzędzia Web Deploy nie skopiuje je do docelowej witryny sieci web, chyba że ręcznie dołączysz je w projekcie. Takie zachowanie wynika z domyślnej konfiguracji wdrożenia, (inną opcję, której będziesz korzystać w tych samouczkach, jest, aby zmienić ustawienie kontrolujące to zachowanie. To ustawienie, które można zmienić **tylko pliki potrzebne do uruchomienia aplikacji** w obszarze **elementów, aby wdrożyć** na **pakowaniu/publikowaniu Web** karcie **projektu Właściwości** okna. Zmiana tego ustawienia jest zwykle niezalecane, ponieważ może spowodować we wdrożeniu z wieloma plikami więcej w środowisku produkcyjnym, nie są potrzebne istnieje. Aby uzyskać więcej informacji na temat rozwiązań alternatywnych, zobacz [Konfigurowanie właściwości projektu](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) samouczek.)

Skompiluj projekt, a następnie w polu **Eksploratora rozwiązań** kliknij **Pokaż wszystkie pliki** Jeśli nie zostały już wykonane. Może być również konieczne kliknij **Odśwież**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Rozwiń **bin** folderze, aby zobaczyć **amd64** i **x86** folderów, a następnie wybierz pozycję tych folderów, kliknij prawym przyciskiem myszy i wybierz **Include w projekcie**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Zmienić ikony folderów Aby pokazać, że folder został uwzględniony w projekcie.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurowanie migracje Code First dla wdrożenia bazy danych aplikacji

Podczas wdrażania bazy danych aplikacji, zazwyczaj nie po prostu są wdrażane rozwoju bazy danych przy użyciu wszystkich danych znajdujących się w niej do środowiska produkcyjnego, ponieważ większość danych, prawdopodobnie jest tylko do celów testowych. Na przykład nazwy dla uczniów w bazie danych testu są fikcyjne. Z drugiej strony, często nie można wdrożyć tylko struktury bazy danych bez danych w niej w ogóle. Niektóre dane w bazie danych testu może być prawdziwe dane i musi być dostępne, gdy użytkownicy rozpocząć korzystanie z aplikacji. Na przykład baza danych może być tabeli, która zawiera wartości nieprawidłowa klasa lub nazwy rzeczywistych działów.

Aby zasymulować ten częsty scenariusz, należy skonfigurować metodę pierwszy inicjatora migracji kodu, która wstawia do bazy danych, które mają być dostępne w środowisku produkcyjnym. Ta metoda inicjatora nie będzie wstawić dane testowe, ponieważ będzie ona uruchamiana w środowisku produkcyjnym, po pierwszym kod tworzy bazę danych w środowisku produkcyjnym.

We wcześniejszych wersjach programu Code First przed migracji został wydany, było typowe dla metod inicjatora wstawić dane testowe również, ponieważ przy każdej zmianie modelu podczas tworzenia bazy danych musiały być całkowicie usunięta i utworzona ponownie od podstaw. Za pomocą migracje Code First danych testowych są przechowywane po zmianie bazy danych, włącznie z danymi testu w metodzie inicjatora nie jest konieczne. Projekt, który został pobrany używa metody migracji wstępnej, w tym wszystkie dane w metodzie inicjatora klasy inicjatora. W tym samouczku możesz wyłączyć klasy inicjatora i włączanie funkcji migracje. Następnie zaktualizujesz Seed — metoda w klasie konfiguracji migracji, aby wstawia tylko te dane, które mają zostać wstawione w środowisku produkcyjnym.

Na poniższym diagramie przedstawiono schemat bazy danych aplikacji:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Te samouczki będzie założył, że `Student` i `Enrollment` tabel powinien być pusty, gdy najpierw zostanie wdrożona lokacja. Inne tabele zawierają dane, który ma zostać wstępnie załadowane, gdy aplikacja przechodzi na żywo.

Ponieważ będziesz używać migracje Code First, nie masz już używać **DropCreateDatabaseIfModelChanges** inicjatora Code First. Kod dla tego inicjatora znajduje się w pliku SchoolInitializer.cs w projekcie ContosoUniversity.DAL. Ustawienie w **appSettings** tego inicjatora zawsze, gdy aplikacja próbuje dostęp do bazy danych po raz pierwszy powoduje, że element pliku Web.config:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Otwórz plik Web.config i Usuń element, który określa Code First klasy inicjatora z elementu appSettings. AppSettings element wygląda teraz następująco:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Innym sposobem określenia klasy inicjatora to zrobić, wywołując `Database.SetInitializer` w `Application_Start` method in Class metoda *Global.asax* pliku. Jeśli włączasz migracji w projekcie, który używa tej metody w celu określenia inicjatora, Usuń ten wiersz kodu.


Następnie włącz migracje Code First.

Pierwszym krokiem jest, aby upewnić się, że projekt ContosoUniversity jest ustawiony jako projekt startowy. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz **Ustaw jako projekt startowy**. Migracje Code First będzie wyszukiwał projekt startowy, aby znaleźć parametry połączenia bazy danych.

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** i następnie **Konsola Menedżera pakietów**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

W górnej części **Konsola Menedżera pakietów** oknie Wybierz ContosoUniversity.DAL jako domyślny projekt, a następnie co `PM>` wierszu wprowadź "enable-migrations".

![Włącz migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

To polecenie umożliwia utworzenie *Configuration.cs* plik w nowym *migracje* folderu w projekcie ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Możesz wybrać projekt DAL, ponieważ polecenia "enable-migrations" musi zostać wykonana w projekcie, który zawiera Code First klasy kontekstu. Ta klasa znajduje się w projekcie biblioteki klas, migracje Code First wyszukuje parametry połączenia bazy danych w projekt startowy rozwiązania. W tym rozwiązaniu ContosoUniversity projektu sieci web został ustawiony jako projekt startowy. (Jeśli chcesz wyznaczyć projektu, który ma parametry połączenia jako projekt startowy w programie Visual Studio, można określić projekt startowy w polecenia programu PowerShell. Aby wyświetlić składnię polecenia dla polecenia enable-migrations, można wprowadzić polecenie "get-help enable-migrations".)

Otwórz plik Configuration.cs i Zastąp komentarze w `Seed` metoda następującym kodem:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Odwołania do `List` mają czerwoną linią falistą w ramach nich, ponieważ nie masz `using` instrukcji jeszcze swojej przestrzeni nazw. Kliknij prawym przyciskiem myszy jednego z wystąpień `List` i kliknij przycisk **rozwiązać**, a następnie kliknij przycisk **przy użyciu System.Collections.Generic**.

![Rozwiązanie przy użyciu instrukcji](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Ta opcja menu dodaje następujący kod do `using` instrukcji w górnej części pliku.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Dodawanie kodu do `Seed` metodą jest jeden z wielu sposobów wstawiany danych do bazy danych. Alternatywą jest Dodaj kod, aby `Up` i `Down` metody każda klasa migracji. `Up` i `Down` metody zawierają kod, który implementuje zmian w bazie danych. Zobaczysz je w przykłady [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) samouczka.
> 
> Można także napisać kod, który wykonuje instrukcje SQL przy użyciu `Sql` metody. Na przykład, jeśli były dodanie kolumny budżetu Tabela działu i chcesz zainicjować wszystkich budżetów działu do $1000,00 w ramach migracji, możesz dodać wiersz folllowing kodu do `Up` metody dla tej migracji:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> W tym przykładzie pokazano dla w tym samouczku `AddOrUpdate` method in Class metoda `Seed` metody migracje Code First `Configuration` klasy. Kod wywołuje metodę migracji First `Seed` metody po każdej migracji, a ta metoda aktualizuje wierszy, które już zostały wstawione lub wstawia je, jeśli ich jeszcze nie istnieją. `AddOrUpdate` Metoda może być najlepszym wyborem dla danego scenariusza. Aby uzyskać więcej informacji, zobacz [powinien zachować ostrożność przy użyciu metody AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.


Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.

Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji. Chcesz, aby tej migracji, aby utworzyć nową bazę danych, więc trzeba będzie usunąć bazę danych, która już istnieje. Bazy danych programu SQL Server Compact są zawarte w *.sdf* pliki *aplikacji\_danych* folderu. W **Eksploratora rozwiązań**, rozwiń węzeł *aplikacji\_danych* w projekcie ContosoUniversity zobacz dwie bazy danych programu SQL Server Compact, które są reprezentowane przez *.sdf*plików.

Kliknij prawym przyciskiem myszy *School.sdf* plik i kliknij przycisk **Usuń**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

W **Konsola Menedżera pakietów** okna, wprowadź polecenie "Dodaj migracji początkowego" do tworzenia początkowej migracji i nadaj mu nazwę "Początkowy".

![Dodaj migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migracje Code First tworzy inny plik klasy w *migracje* folder, a ta klasa zawiera kod, który tworzy schemat bazy danych.

W **Konsola Menedżera pakietów**, wprowadź polecenie "update-database" Aby utworzyć bazę danych i uruchomić **inicjatora** metody.

![Aktualizacja database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Jeśli zostanie wyświetlony komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych oraz zanim zostanie wykonany `update-database`. In that Case, Usuń *School.sdf* ponownie plik, a następnie spróbuj ponownie `update-database` polecenia.)

Uruchom aplikację. Teraz strona studentów jest pusta, ale strona Instruktorzy zawiera instruktorów. Jest to, co dostaniesz w środowisku produkcyjnym po wdrożeniu aplikacji.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projekt jest teraz gotowe do wdrożenia *School* bazy danych.

## <a name="creating-a-membership-database-for-deployment"></a>Tworzenie bazy danych członkostwa dla wdrożenia

Aplikacja Contoso University korzysta z uwierzytelniania systemu i formularze członkostwa ASP.NET do uwierzytelniania i autoryzacji użytkowników. Jednym z jego strony jest dostępny tylko dla administratorów. Aby wyświetlić tę stronę, uruchom aplikację, a następnie wybierz **aktualizacji środki na korzystanie z** w menu Okno wysuwane pod **kursów**. Wyświetla aplikacji **logowanie** strony, ponieważ tylko administratorzy są autoryzowane do używania **aktualizacji środki na korzystanie z** strony.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Zaloguj się jako "admin" przy użyciu hasła "Pas$ w0rd" (Zwróć uwagę, to cyfra zero zamiast litery "o" w "w0rd"). Po zalogowaniu, **aktualizacji środki na korzystanie z** zostanie wyświetlona strona.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Po raz pierwszy zostanie wdrożona lokacja, jest często większość lub wszystkie konta użytkowników, które tworzysz do testowania. W tym przypadku wdrożysz, konto administratora i żadnych kont użytkowników. Zamiast ręcznego usuwania konta testowania, utworzysz nową bazę danych członkostwa, który ma tylko jeden administrator konta użytkownika, które należy w środowisku produkcyjnym.

> [!NOTE]
> Skrót hasła do kont są przechowywane w bazie danych członkostwa. Aby wdrożyć kont z jednego komputera, należy upewnić się, czy procedury wyznaczania wartości skrótu nie Generuj różne skróty na serwerze docelowym niż na ich na komputerze źródłowym. One wygeneruje tych samych skrótów korzystając z dostawców uniwersalnych ASP.NET, tak długo, jak nie zmieniaj domyślny algorytm. Domyślny algorytm jest HMACSHA256 i jest określony w **weryfikacji** atrybutu **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** elementu w pliku Web.config.


Członkowskiej bazie danych nie jest obsługiwana przez migracje Code First i nie ma żadnego automatycznego inicjatora, który inicjowania inicjuje bazy danych za pomocą testu konta (ponieważ dla bazy danych School). W związku z tym aby zachować dane testowe dostępne wprowadzisz kopię bazy danych testów przed utworzeniem nowej.

W **Eksploratora rozwiązań**, Zmień nazwę *aspnet.sdf* w pliku *aplikacji\_danych* folder *aspnet Dev.sdf*. (Nie utworzyć kopię, po prostu zmień jej nazwę, utworzysz nową bazę danych w kilka minut.)

W **Eksploratora rozwiązań**, upewnij się, że wybrano projektu sieci web (ContosoUniversity, nie ContosoUniversity.DAL). Następnie w **projektu** menu, wybierz opcję **Konfiguracja ASP.NET** do uruchomienia **narzędzie Administratorskie witryny sieci Web**(WAT).

Wybierz **zabezpieczeń** kartę.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Kliknij przycisk **tworzenia lub Zarządzaj rolami** i Dodaj **administratora** roli.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Przejdź z powrotem do **zabezpieczeń** kliknij pozycję **Create User**, i Dodaj użytkownika "admin" jako administrator. Przed kliknięciem przycisku **Create User** znajdujący się na **Create User** strony, upewnij się, że wybrano **administratora** pole wyboru. Hasło używane w tym samouczku jest "Pas$ w0rd" i można wprowadzić dowolny adres e-mail.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Zamknij przeglądarkę. W **Eksploratora rozwiązań**, kliknij przycisk Odśwież, aby zobaczyć nowe *aspnet.sdf* pliku.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Kliknij prawym przyciskiem myszy **aspnet.sdf** i wybierz **Include w projekcie**.

## <a name="distinguishing-development-from-production-databases"></a>Wyróżniające się programowania w produkcyjnych bazach danych

W tej sekcji będzie zmienić nazwę bazy danych, tak aby Dev.sdf szkoły są wersje rozwoju i aspnet Dev.sdf i wersji produkcyjnych są Prod.sdf szkoły i aspnet Prod.sdf. Nie jest to konieczne, ale spowoduje to więc pomoże uniemożliwić pobieranie mylić test i produkcyjne wersje bazy danych.

W **Eksploratora rozwiązań**, kliknij przycisk **Odśwież** i rozwiń ją\_folder danych, aby wyświetlić utworzoną wcześniej bazę danych School; kliknij go prawym przyciskiem myszy i wybierz **Include w projekcie** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Zmień nazwę *aspnet.sdf* do *aspnet Prod.sdf*.

Zmień nazwę *School.sdf* do *School-Dev.sdf*.

Po uruchomieniu aplikacji w programie Visual Studio nie chcesz używać *-Prod* wersje dla plików bazy danych, do którego chcesz użyć *- Dev* wersji. W związku z tym należy zmienić parametry połączenia w pliku Web.config, tak, aby wskazywały do *- Dev* wersji bazy danych. (Nie utworzono pliku Prod.sdf szkoły, ale to normalne, ponieważ Code First spowoduje utworzenie bazy danych w produkcji pierwszym uruchomieniu aplikacji istnieje).

Otwórz plik Web.config, a następnie zlokalizuj parametry połączenia:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Zmień "aspnet.sdf" na "aspnet Dev.sdf" i "School Dev.sdf" Zmień "School.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Aparat bazy danych programu SQL Server Compact i obu bazach danych są teraz gotowe do wdrożenia. W następującego samouczka można skonfigurować automatyczne *Web.config* pliku przekształcenia ustawień, które muszą być różne w środowiskach programowania, testowania i produkcji. (Należą ustawienia, które należy zmienić parametry połączenia, ale należy skonfigurować te zmiany później podczas tworzenia profilu publikowania).

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat NuGet, zobacz [Zarządzanie biblioteki projektu z NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) i [dokumentacja programu NuGet](http://docs.nuget.org/docs/start-here/overview). Jeśli nie chcesz użyć NuGet, należy dowiedzieć się, jak analizować pakietu NuGet, aby ustalić, jakie operacje po jej zainstalowaniu. (Na przykład, może skonfigurować *Web.config* transformacji, skonfigurować skrypty programu PowerShell do uruchamiania w czasie kompilacji, itp.) Aby dowiedzieć się więcej na temat sposobu działania NuGet, zobacz szczególnie [tworzenie i publikowanie pakietu](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) i [pliku konfiguracji i przekształceń kodu źródłowego](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [dalej](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
