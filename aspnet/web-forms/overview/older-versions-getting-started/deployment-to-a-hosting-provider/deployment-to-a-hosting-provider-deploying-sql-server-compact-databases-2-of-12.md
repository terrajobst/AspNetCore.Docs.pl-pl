---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie serwera Compact baz danych — 2 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5296bc1ca3fd0b24123bd79a550a7e2cffc34a44
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie serwera Compact baz danych — 2 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak skonfigurować dwie bazy danych programu SQL Server Compact i aparatu bazy danych do wdrożenia.

Aby uzyskać dostęp do bazy danych aplikacja Contoso University wymaga następującego oprogramowania, które należy wdrożyć w aplikacji, ponieważ nie znajduje się w programie .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (aparat bazy danych).
- [Dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umożliwiające systemu członkostwa programu ASP.NET użyć programu SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First z migracji).

Struktura bazy danych, a niektóre (nie wszystkie) danych w dwóch aplikacji bazy danych musi być także wdrożony. Zazwyczaj podczas opracowywania aplikacji test dane wprowadzane do bazy danych, których nie chcesz, aby wdrożyć w witrynie na żywo. Jednak może również wprowadzić części danych produkcyjnych, które chcesz wdrożyć. W tym samouczku skonfigurujesz projektu Contoso University tak, aby wymagane oprogramowanie i prawidłowe dane są uwzględnione podczas wdrażania.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact i SQL Server Express

Przykładowa aplikacja korzysta z programu SQL Server Compact 4.0. Ten aparat bazy danych jest stosunkowo nowej opcji witryn sieci Web; wcześniejszych wersji programu SQL Server Compact nie działają w środowisku hosta sieci web. SQL Server Compact zapewnia kilka korzyści w porównaniu do częściej scenariusz programowania z użyciem programu SQL Server Express i wdrażających aplikacje w pełnej wersji programu SQL Server. W zależności od wybranego dostawcy hostingu programu SQL Server Compact może być tańsze do wdrożenia, ponieważ niektórzy dostawcy pobierają dodatkowy do obsługi pełnej bazy danych SQL Server. Brak bez dodatkowych opłat dla programu SQL Server Compact, ponieważ samego aparatu bazy danych można wdrożyć w ramach aplikacji sieci web.

Jednak należy wziąć pod uwagę swoje ograniczenia. SQL Server Compact nie obsługuje procedur składowanych, wyzwalaczy, widoków i replikacji. (Aby uzyskać pełną listę funkcji programu SQL Server, które nie są obsługiwane przez program SQL Server Compact, zobacz [różnice między programu SQL Server Compact i SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Ponadto niektóre narzędzia, które służy do modyfikowania schematów i danych w programie SQL Server Express i baz danych programu SQL Server nie współpracujesz z programu SQL Server Compact. Na przykład nie można używać programu SQL Server Management Studio lub SQL Server Data Tools w programie Visual Studio baz danych programu SQL Server Compact. Masz inne opcje do pracy z bazy danych programu SQL Server Compact:

- W programie Visual Studio, który udostępnia funkcje manipulowania ograniczone baz danych programu SQL Server Compact, można użyć Eksploratora serwera.
- Można użyć funkcji manipulowania bazy danych z [WebMatrix](https://www.microsoft.com/web/webmatrix/), który ma więcej funkcji niż Eksploratora serwera.
- Możesz użyć stosunkowo oferujący wszystkie funkcje innych firm lub Otwieranie źródła narzędzi, takich jak [programu SQL Server Compact przybornika](https://github.com/ErikEJ/SqlCeToolbox) i [SQL Compact danych i schemat skryptu narzędzia](https://github.com/ErikEJ/SqlCeToolbox).
- Możesz wpisać i uruchom własnych skryptów (języka definicji danych) DDL do modyfikowania schematu bazy danych.

Można uruchomić z programu SQL Server Compact, a następnie Uaktualnij później rozwijać potrzeb. W kolejnych samouczkach z tej serii opisano sposób migracji z programu SQL Server Compact do programu SQL Server Express i programu SQL Server. Jednak jeśli w przypadku tworzenia nowej aplikacji i oczekiwać w najbliższej przyszłości wymagają programu SQL Server, jest prawdopodobnie najlepiej zacząć z programu SQL Server lub SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurowanie aparatu Compact bazy danych programu SQL Server na potrzeby wdrożenia

Oprogramowanie wymagane do dostępu do danych w aplikacji Contoso University został dodany przez zainstalowanie następujących pakietów NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (dostawców uniwersalnych ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Punktu linki do bieżącej wersji te pakiety, które może być nowsza niż zainstalowana w projekcie starter pobranego w tym samouczku. Do wdrożenia u dostawcy hostingu upewnij się, że używasz programu Entity Framework w wersji 5.0 lub nowszej. Starsze niż migracje Code First wymagają pełnego zaufania, a w wielu dostawców hostingu aplikacja jest uruchamiana w trybie średniego zaufania. Aby uzyskać więcej informacji o trybie średniego zaufania, zobacz [wdrażania usług IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczka.

Instalacja pakietu NuGet zazwyczaj odpowiada on za wszystko, co jest potrzebne w celu wdrożenia oprogramowania z aplikacją. W niektórych przypadkach ten proces obejmuje zadania, takie jak zmiana pliku Web.config i dodawania skryptów programu PowerShell, które są uruchamiane przy każdym Skompiluj rozwiązanie. **Jeśli chcesz dodać obsługę dowolnej z tych funkcji (takie jak SQL Server Compact i Entity Framework) bez przy użyciu narzędzia NuGet, upewnij się, że wiesz, jak instalacja pakietu NuGet działa tak, aby w tej samej pracy można wykonać ręcznie.**

Istnieje jeden wyjątek gdzie NuGet nie Zwróć uwagę na wszystkich elementów, które należy wykonać w celu zapewnienia pomyślnego wdrożenia. Pakiet SqlServerCompact NuGet dodaje skryptu postkompilacyjnego do projektu, który kopiuje natywnych zestawów do *x86* i *amd64* podfoldery w projekcie *bin* folder, ale skrypt zawiera te foldery w projekcie. W związku z tym narzędzia Web Deploy nie skopiuje je do docelowej witryny sieci web, chyba że ręcznie je uwzględnić w projekcie. (To zachowanie wyniki z domyślnej konfiguracji wdrożenia; jest inną opcją, którego będziesz korzystać w tych samouczkach, aby zmienić to ustawienie, która kontroluje to zachowanie. To ustawienie można zmienić **tylko pliki potrzebne do uruchomienia aplikacji** w obszarze **elementy do wdrożenia** na **pakowaniu/publikowaniu Web** karcie **projektu Właściwości** okna. Zmiana tego ustawienia jest zwykle niezalecane, ponieważ może to skutkować wdrożenie wielu większej liczby plików do środowiska produkcyjnego, nie są potrzebne istnieje. Aby uzyskać więcej informacji na temat rozwiązań alternatywnych, zobacz [Konfigurowanie właściwości projektu](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) samouczek.)

Skompiluj projekt, a następnie w **Eksploratora rozwiązań** kliknij **Pokaż wszystkie pliki** Jeśli nie ma jeszcze zrobione. Może być również konieczne kliknij **Odśwież**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Rozwiń węzeł **bin** folder, aby wyświetlić **amd64** i **x86** folderów, a następnie wybierz tych folderów, kliknij prawym przyciskiem myszy i wybierz **Include w projekcie**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Ikony folderu zmieniają się że folder został dołączony do projektu.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurowanie migracje Code First dla wdrożenia aplikacji bazy danych

Podczas wdrażania bazy danych aplikacji, zwykle nie można po prostu wdrożyć programowanie bazy danych za pomocą wszystkich danych znajdujących się w niej do środowiska produkcyjnego, ponieważ większość danych, prawdopodobnie jest tylko do celów testowych. Na przykład nazwy uczniów w testowej bazy danych są fikcyjne. Z drugiej strony, często nie można wdrożyć tylko struktury bazy danych nie zawiera żadnych danych w niej w ogóle. Niektóre dane w bazie danych test może być prawdziwe dane, a musi być dostępne, gdy użytkownicy rozpocząć korzystanie z aplikacji. Na przykład baza danych może być tabeli, która zawiera nieprawidłowy klasy wartości lub nazwy działów prawdziwe.

Aby symulować ten typowy scenariusz, należy skonfigurować metodę kod pierwszego migracje inicjatora wstawiany do bazy danych, które mają być dostępne w środowisku produkcyjnym. Ta metoda inicjatora nie wstawić danych testowych, ponieważ będzie uruchamiany w środowisku produkcyjnym po Code First utworzy bazę danych w środowisku produkcyjnym.

We wcześniejszych wersjach programu Code First przed migracje został zwolniony, było typowe dla metod inicjatora do wstawiania danych testowych również, ponieważ przy każdej zmianie modelu podczas tworzenia bazy danych musiała zostać całkowicie usunięty i utworzony ponownie od początku. Z migracje Code First dane testowe jest zachowywana po wprowadzeniu zmian w bazie danych, włącznie z danymi testu w metodzie inicjatora nie jest konieczne. Pobrany projekt używa metody migracji wstępnej, w tym wszystkie dane w metodzie inicjatora klasy inicjatora. W tym samouczku możesz wyłączyć klasa inicjatora i włączyć migracje. Następnie będzie aktualizowana Seed — metoda w klasie konfiguracji migracji, dzięki czemu wstawia tylko dane, które mają być wstawiane w środowisku produkcyjnym.

Na poniższym diagramie przedstawiono schemat bazy danych aplikacji:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Te samouczki będzie założono, że `Student` i `Enrollment` tabel powinien być pusty, jeśli najpierw zostanie wdrożona lokacja. Inne tabele zawierają dane, które ma zostać załadowane aplikacji systemowi na żywo.

Ponieważ będziesz używać migracje Code First, nie można już używać **DropCreateDatabaseIfModelChanges** inicjatora Code First. Kod dla tego inicjatora znajduje się w pliku SchoolInitializer.cs w projekcie ContosoUniversity.DAL. Ustawienia w **appSettings** tego inicjatora do uruchomienia, gdy aplikacja próbuje dostęp do bazy danych po raz pierwszy powoduje, że element pliku Web.config:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Otwórz plik Web.config i Usuń element, który określa klasę inicjatora Code First z elementu appSettings. AppSettings element teraz wygląda następująco:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Innym sposobem określania klasy inicjatora jest to zrobić przez wywołanie metody `Database.SetInitializer` w `Application_Start` metody w *Global.asax* pliku. Jeśli włączasz migracje w projekcie, który używa tej metody można określić inicjatora, usuń go.


Należy również włączyć migracje Code First.

Pierwszym krokiem jest, aby upewnić się, że projekt ContosoUniversity jest ustawiony jako projekt startowy. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz **Ustaw jako projekt startowy**. Migracje Code First będzie wyszukiwał projekt startowy można znaleźć ciąg połączenia bazy danych.

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** , a następnie **Konsola Menedżera pakietów**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

W górnej części **Konsola Menedżera pakietów** oknie Wybierz ContosoUniversity.DAL jako domyślny projekt, a następnie at `PM>` wierszu wprowadź "enable-migrations".

![Włącz migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

To polecenie tworzy *Configuration.cs* plik w nowym *migracje* folderu w projekcie ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Wybrano projekt DAL, ponieważ polecenie "enable-migrations" musi zostać wykonana w projekcie, który zawiera klasę kontekstu Code First. W przypadku tej klasy w projektu biblioteki klas, migracje Code First szuka parametry połączenia bazy danych w projekcie uruchamiania dla rozwiązania. W rozwiązaniu ContosoUniversity projektu sieci web został ustawiony jako projekt startowy. (Jeśli chcesz wyznaczyć projektu, który ma parametry połączenia jako projekt startowy w programie Visual Studio, można określić projekt startowy w polecenia programu PowerShell. Aby wyświetlić składnię polecenia na potrzeby polecenia enable-migrations, można wprowadzić polecenie "get-help enable-migrations".)

Otwórz plik Configuration.cs i Zastąp komentarzy w `Seed` metodę z następującym kodem:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Odwołania do `List` mieć czerwone dowolnym kształcie linie w nich, ponieważ nie masz `using` jeszcze instrukcji dla jego przestrzeni nazw. Kliknij prawym przyciskiem myszy jednego wystąpienia `List` i kliknij przycisk **rozwiązać**, a następnie kliknij przycisk **przy użyciu system.Collections.Generic —**.

![Rozwiązania z wykorzystaniem instrukcji](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Wybranie tej opcji menu dodaje następujący kod do `using` instrukcje górnej części pliku.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Dodawanie kodu do `Seed` metoda jest wiele sposobów wstawiany stałych z danymi w bazie danych. Alternatywą jest Dodaj kod, aby `Up` i `Down` metody każdej klasy migracji. `Up` i `Down` metody zawiera kod, który implementuje zmian w bazie danych. Zobaczysz przykłady je w [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) samouczka.
> 
> Można również napisać kod, który wykonuje instrukcje SQL przy użyciu `Sql` metody. Na przykład dodawania kolumny budżetu do tabeli działu, aby zainicjować wszystkich budżetów działów do $1000,00 w ramach migracji należy dodać wierszy folllowing kodu `Up` metodę dla tej migracji:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> W tym przykładzie pokazano dla tego samouczka używa `AddOrUpdate` metody w `Seed` metodzie migracje Code First `Configuration` klasy. Kod wywoła migracje First `Seed` metody po każdym migracji, a ta metoda aktualizacji wierszy, które już zostały wstawione lub wstawia je, jeśli nie istnieje jeszcze. `AddOrUpdate` — Metoda nie może być najlepszym rozwiązaniem dla danego scenariusza. Aby uzyskać więcej informacji, zobacz [zajmie się za pomocą metody AddOrUpdate EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.


Naciśnij klawisze CTRL-SHIFT-B, aby skompilować projekt.

Następnym krokiem jest utworzenie `DbMigration` klasy początkowej migracji. Chcesz tej migracji, aby utworzyć nową bazę danych, więc musisz usunąć bazę danych, która już istnieje. Bazy danych programu SQL Server Compact są zawarte w *sdf* plików *aplikacji\_danych* folderu. W **Eksploratora rozwiązań**, rozwiń węzeł *aplikacji\_danych* projektu ContosoUniversity zobacz dwie bazy danych programu SQL Server Compact, w którym są reprezentowane przez *sdf*plików.

Kliknij prawym przyciskiem myszy *School.sdf* plik i kliknij przycisk **usunąć**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

W **Konsola Menedżera pakietów** okna, wprowadź polecenie "Dodaj migracji początkowego" Tworzenie początkowej migracji i nadaj jej nazwę na "Początkowego".

![Dodaj migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migracje Code First tworzy innego pliku klasy w *migracje* folderu, a ta klasa zawiera kod, który tworzy schemat bazy danych.

W **Konsola Menedżera pakietów**, wprowadź polecenie "update-database" Aby utworzyć bazę danych i uruchomić **inicjatora** metody.

![database_command aktualizacji](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Jeśli zostanie wyświetlony komunikat o błędzie wskazujący tabela już istnieje i nie można utworzyć, prawdopodobnie uruchomienia aplikacji, po usunięciu bazy danych i należy wykonać `update-database`. In that Case, Usuń *School.sdf* ponownie, a następnie spróbuj ponownie `update-database` polecenia.)

Uruchom aplikację. Teraz strony studentów jest pusty, ale strona instruktorów zawiera instruktorów. Jest to, co zostanie wyświetlony w środowisku produkcyjnym po wdrożeniu aplikacji.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projekt jest gotowy do wdrożenia *służbowe* bazy danych.

## <a name="creating-a-membership-database-for-deployment"></a>Trwa tworzenie bazy danych członkostwa dla wdrożenia

Aplikacja Contoso University używa uwierzytelniania formularzy i systemu członkostwa programu ASP.NET do uwierzytelniania i autoryzacji użytkowników. Jeden z jego stron jest dostępny tylko dla administratorów. Aby wyświetlić tę stronę, uruchom aplikację, a następnie wybierz **środków aktualizacji** w menu wysuwane pod **kursów**. Wyświetla aplikacji **dziennika w** strony, ponieważ tylko administratorzy są autoryzowane do używania **środków aktualizacji** strony.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Zaloguj się jako "admin" przy użyciu hasła "Adresy$ w0rd" (Zwróć uwagę, liczby zero zamiast litery "o" w "w0rd"). Po zalogowaniu, **środków aktualizacji** zostanie wyświetlona strona.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Podczas wdrażania witryny po raz pierwszy jest często, aby wykluczyć większość lub wszystkie konta użytkowników, które tworzysz do testowania. W takim przypadku przedstawiono wdrażania konta administratora i żadnych kont użytkowników. Zamiast ręcznego usuwania testowe konta, utworzysz nową bazę danych członkostwa, który ma tylko jeden administrator konta użytkownika, które należy w środowisku produkcyjnym.

> [!NOTE]
> Baza danych członkostwa przechowuje skrót hasła do kont. Aby można było wdrożyć kont z jednego komputera na inny, należy się upewnić procedury wyznaczania wartości skrótu nie Generowanie skrótów różnych na serwerze docelowym niż na komputerze źródłowym. Tej samej wartości skrótu zostanie wygenerowany używania dostawców uniwersalnych ASP.NET, pod warunkiem, nie zmieniaj domyślny algorytm. Domyślny algorytm jest HMACSHA256 i jest określony w **weryfikacji** atrybutu  **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)**  elementu w pliku Web.config.


Bazy danych członkostwa nie jest obsługiwana przez migracje Code First, i nie ma żadnego inicjatora automatyczne określającej wartość początkową bazy danych z kontami testu (ponieważ jest służbowe bazy danych). W związku z tym aby zachować dane testowe dostępnych będzie wykonanie kopii bazy danych testu przed utworzeniem nowego.

W **Eksploratora rozwiązań**, Zmień nazwę *aspnet.sdf* w pliku *aplikacji\_danych* folder *aspnet Dev.sdf*. (Nie utworzyć kopię, po prostu zmienić jego nazwę, utworzysz nową bazę danych za chwilę.)

W **Eksploratora rozwiązań**, upewnij się, że wybrano projektu sieci web (ContosoUniversity, nie ContosoUniversity.DAL). Następnie w **projektu** menu, wybierz opcję **konfiguracja platformy ASP.NET** do uruchomienia **narzędzie do administrowania witryną sieci Web**(WAT).

Wybierz **zabezpieczeń** kartę.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Kliknij przycisk **Create lub Zarządzanie rolami** i Dodaj **administratora** roli.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Przejdź z powrotem do **zabezpieczeń** , kliknij pozycję **Tworzenie użytkownika**, i Dodaj użytkownika "Administrator" jako administrator. Przed kliknięciem przycisku **Tworzenie użytkownika** znajdującego się na **Tworzenie użytkownika** upewnij się, że wybrano **administratora** pole wyboru. Hasło używane w tym samouczku jest "$w0rd adresy dostawcy", a następnie można użyć dowolnego adresu e-mail.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Zamknij przeglądarkę. W **Eksploratora rozwiązań**, kliknij przycisk Odśwież, aby zobaczyć nowe *aspnet.sdf* pliku.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Kliknij prawym przyciskiem myszy **aspnet.sdf** i wybierz **Include w projekcie**.

## <a name="distinguishing-development-from-production-databases"></a>Programowanie odróżniający z baz danych w środowisku produkcyjnym

W tej sekcji, aby Dev.sdf szkoły są wersje programowanie i aspnet Dev.sdf i wersji produkcyjnej są Prod.sdf służbowe i aspnet Prod.sdf będzie Zmień nazwę bazy danych. Nie jest to konieczne, ale to tak może pomóc zapobiec pobierania mylić wersji testowych i produkcyjnych baz danych.

W **Eksploratora rozwiązań**, kliknij przycisk **Odśwież** i rozwiń listę aplikacji\_folderu danych do bazy danych służbowych, utworzony wcześniej w temacie; kliknij go prawym przyciskiem myszy i wybierz **Include w projekcie** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Zmień nazwę *aspnet.sdf* do *aspnet Prod.sdf*.

Zmień nazwę *School.sdf* do *służbowe Dev.sdf*.

Po uruchomieniu aplikacji w programie Visual Studio nie chcesz używać *-produkcyjną* wersje plików bazy danych, którego chcesz użyć *- deweloperów* wersji. W związku z tym należy zmienić parametry połączenia w pliku Web.config, tak aby wskazywały do *- deweloperów* wersji bazy danych. (Nie utworzono pliku Prod.sdf służbowych, ale jest OK, ponieważ Code First spowoduje utworzenie bazy danych w produkcji pierwszym uruchomieniu aplikacji istnieje).

Otwórz plik Web.config, a następnie zlokalizuj parametry połączenia:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Zmień "aspnet.sdf" na "aspnet Dev.sdf" i zmień "School.sdf" na "Służbowe Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Aparat bazy danych programu SQL Server Compact i obie bazy danych jest teraz gotowa do wdrożenia. W samouczku następujące skonfigurować automatyczne *Web.config* pliku przekształcenia ustawień, które muszą być różne w środowisku projektowania, testów i produkcji. (Należą ustawienia, które należy zmienić parametry połączenia, ale należy skonfigurować je później podczas tworzenia profilu publikowania).

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o NuGet, zobacz [Zarządzanie biblioteki projektu z NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) i [dokumentacji NuGet](http://docs.nuget.org/docs/start-here/overview). Jeśli nie chcesz używać NuGet, należy dowiedzieć się, jak analizować pakietu NuGet, aby ustalić, jakie operacje po jej zainstalowaniu. (Na przykład może skonfigurować *Web.config* przekształcenia, skonfigurować skrypty programu PowerShell do uruchamiania w czasie kompilacji itp.) Aby dowiedzieć się więcej na temat działania NuGet, zobacz szczególnie [tworzenie i publikowanie pakietu](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) i [pliku konfiguracji i przekształcenia kod źródłowy](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-introduction-1-of-12.md)
[dalej](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
