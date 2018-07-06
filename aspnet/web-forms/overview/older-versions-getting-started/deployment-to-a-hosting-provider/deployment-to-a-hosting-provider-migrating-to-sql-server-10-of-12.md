---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Migracja do programu SQL Server — 10 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc0bca18d2f6e4cdbb527ab75952e9a50eb49b20
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827365"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Migracja do programu SQL Server — 10 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku dowiesz się, jak przeprowadzić migrację z programu SQL Server Compact do programu SQL Server. Jest jednym z powodów warto to zrobić z zalet funkcji programu SQL Server, SQL Server Compact nie obsługiwanych, takie jak procedur składowanych, wyzwalaczy, widoków ani replikacji. Aby uzyskać więcej informacji na temat różnic między programu SQL Server Compact i SQL Server, zobacz [wdrażania programu SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) samouczka.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express w stosunku do pełnej wersji SQL Server do tworzenia

Po podjęciu uaktualnienia do programu SQL Server, można użyć programu SQL Server lub SQL Server Express w środowiskach tworzenia i testowania. Oprócz różnice w obsłudze narzędzia i funkcje aparatu bazy danych ma różnic w implementacji dostawcy programu SQL Server Compact i innych wersjach programu SQL Server. Te różnice można spowodować, że ten sam kod można wygenerować różne wyniki. W związku z tym Jeśli zdecydujesz się zachować programu SQL Server Compact jako rozwoju bazy danych, należy dokładnie przetestować witryny programu SQL Server lub SQL Server Express w środowisku testowym przed przystąpieniem do każdego wdrożenia do środowiska produkcyjnego.

W przeciwieństwie do programu SQL Server Compact SQL Server Express jest zasadniczo tego samego aparatu bazy danych i używa tego samego dostawcy .NET jako pełnej wersji programu SQL Server. Podczas testowania za pomocą programu SQL Server Express, można mieć pewność, te same wyniki pobierania, jak będzie z programem SQL Server. Za pomocą większość tych samych narzędzi bazy danych programu SQL Server Express używanej z programem SQL Server (Trwa istotne wyjątek [programu SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), i obsługuje inne funkcje programu SQL Server, takich jak procedury składowane, widoki, wyzwalacze, i replikacji. (Zazwyczaj musisz jednak użyć pełnej wersji programu SQL Server w produkcyjnej witrynie internetowej. Program SQL Server Express można uruchomić w środowisku współdzielonym hostingu, ale nie był projektowany do tego, a wielu dostawców hostingu nie obsługują.)

Jeśli używasz programu Visual Studio 2012, zazwyczaj wybierasz programu SQL Server Express LocalDB środowiska deweloperskiego ponieważ jest to, co jest instalowany domyślnie w programie Visual Studio. Jednak LocalDB nie działa w usługach IIS, dzięki czemu do danego środowiska testowego należy użyć programu SQL Server lub SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Łączenie baz danych i zapewnienie ich w oddzielnych

Aplikacja Contoso University ma dwie bazy danych programu SQL Server Compact: bazy danych członkostwa (*aspnet.sdf*) i bazy danych aplikacji (*School.sdf*). Podczas migracji, można przeprowadzić migrację tych baz danych, dwie osobne bazy danych lub pojedynczą bazę danych. Możesz chcieć połączyć je w celu ułatwienia sprzężenia bazy danych między bazy danych aplikacji i bazy danych członkostwa. Plan hostingu może również podać przyczynę do ich łączenia. Na przykład dostawcy hostingu mogą pobierać tylko dla wielu baz danych lub może nawet zezwala na więcej niż jednej bazy danych. To w przypadku konta, który jest używany na potrzeby tego samouczka, który umożliwia tylko jednej bazy danych SQL Server hostingu Lite Cytanium.

W tym samouczku będziesz migracja dwóch baz danych w ten sposób:

- Przeprowadź migrację do dwóch LocalDB baz danych w środowisku programistycznym.
- Przeprowadź migrację do dwóch programu SQL Server Express bazy danych w środowisku testowym.
- Przeprowadź migrację do jednego połączonego pełnej bazy danych SQL Server w środowisku produkcyjnym.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalowanie programu SQL Server Express

SQL Server Express jest automatycznie instalowany domyślnie w programie Visual Studio 2010, ale domyślnie nie jest zainstalowany za pomocą programu Visual Studio 2012. Aby zainstalować program SQL Server 2012 Express, kliknij poniższe łącze

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Wybierz *ENU/x64/SQLEXPR\_x64\_ENU.exe* lub *ENU/x86/SQLEXPR\_x86\_ENU.exe*i zaakceptuj wartość domyślną w Kreatorze instalacji Ustawienia. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [Instalowanie programu SQL Server 2012 za pomocą Kreatora instalacji (Instalatora)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Tworzenie bazy danych programu SQL Server Express dla środowiska testowego

Następnym krokiem jest do tworzenia członkostwa ASP.NET i bazy danych School.

Z **widoku** menu wybierz opcję **Eksploratora serwera** (**Eksplorator bazy danych** w Visual Web Developer), a następnie kliknij prawym przyciskiem myszy **połączeń danych**i wybierz **Tworzenie nowej bazy danych SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

W **Tworzenie nowej bazy danych SQL Server** okna dialogowego wprowadź ". \SQLExpress" w **nazwy serwera** pole i "aspnet-Test" w **nazwę nowej bazy danych** polu, a następnie kliknij przycisk **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Postępuj zgodnie z tą samą procedurą, aby utworzyć nową bazę danych programu SQL Server Express School o nazwie "School-Test".

(Dołączasz "Test" do tych nazw baz danych, ponieważ później utworzymy dodatkowe wystąpienia każdej bazy danych w środowisku deweloperskim i muszą mieć możliwość rozróżnienia dwóch zestawów baz danych.)

**Eksplorator serwera** pojawi się na dwóch nowych baz danych.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Tworzenie skryptu Grant dla nowych baz danych

Po uruchomieniu aplikacji w usługach IIS na komputerze deweloperskim w aplikacji uzyskuje dostęp do bazy danych przy użyciu poświadczeń domyślnej puli aplikacji. Jednak domyślnie tożsamość puli aplikacji nie ma uprawnień do otwierania bazy danych. Dlatego należy uruchomić skrypt, aby przyznać to uprawnienie. W tej sekcji utworzysz skrypt, uruchomisz nowszej, aby upewnić się, że aplikacja będzie mógł otworzyć baz danych po uruchomieniu w usługach IIS.

W rozwiązaniu *SolutionFiles* folder, który został utworzony w [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka, Utwórz nowy plik SQL o nazwie *Grant.sql*. Skopiuj poniższe polecenia SQL do pliku, a następnie zapisz i zamknij plik:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Ten skrypt jest przeznaczony do pracy z programu SQL Server 2008 i ustawienia programu IIS w Windows 7, ponieważ są one określone w tym samouczku. Jeśli używasz innej wersji programu SQL Server lub Windows, czy należy zainstalować program IIS na komputerze inaczej, zmiany w tym skrypcie może być wymagane. Aby uzyskać więcej informacji na temat skrypty programu SQL Server, zobacz [programu SQL Server — książki Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** ten skrypt umożliwia db\_uprawnienia właściciela do użytkownika, który uzyskuje dostęp do bazy danych w czasie wykonywania, który jest uzyskasz w środowisku produkcyjnym. W niektórych scenariuszach możesz chcieć określić użytkownika, która ma schemat pełnej bazy danych, zaktualizować uprawnienia tylko do wdrożenia i określ czas wykonywania inny użytkownik z uprawnieniami tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz **przeglądanie automatyczne zmiany w pliku Web.config na potrzeby migracji Code First** w [wdrażanie w usługach IIS jako środowisku testowym](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurowanie wdrażania bazy danych dla środowiska testowego

Następnie należy skonfigurować program Visual Studio, dzięki czemu będzie on wykonywać następujące zadania dla każdej bazy danych:

- Generuj skrypt SQL, który tworzy struktury źródłowej bazy danych (tabele, kolumny, ograniczenia, itp.) w docelowej bazie danych.
- Generuj skrypt SQL, który wstawia dane źródłowej bazy danych w tabelach w docelowej bazie danych.
- Uruchom wygenerowanych skryptów oraz skrypt Grant, utworzone w docelowej bazie danych.

Otwórz **właściwości projektu** okna, a następnie wybierz pozycję **Pakuj/Publikuj SQL** kartę.

Upewnij się, że **aktywny (wersja)** lub **wersji** wybrano **konfiguracji** listy rozwijanej.

Kliknij przycisk **Włącz tę stronę**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**Pakuj/Publikuj SQL** karta zwykle została wyłączona, ponieważ określa on metodę wdrożenie starszego typu. W większości przypadków należy skonfigurować wdrożenie bazy danych w **publikowanie w sieci Web** kreatora. Migrowanie z programu SQL Server Compact do programu SQL Server lub SQL Server Express jest szczególnym przypadkiem, dla którego ta metoda jest dobrym wyborem.

Kliknij przycisk **Import z pliku Web.config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio szuka parametry połączenia w *Web.config* plik, znajduje się jeden dla bazy danych członkostwa i jeden dla bazy danych School i dodaje wiersz odpowiadający każdego ciągu połączenia w **wpisy w bazie danych**  tabeli. Parametry połączenia, znajdzie się dla istniejących baz danych programu SQL Server Compact i następnym krokiem będzie można skonfigurować sposób i miejsce do wdrożenia tych baz danych.

Wprowadź ustawienia wdrażania bazy danych **szczegóły wpisu w bazie danych** poniżej **wpisy w bazie danych** tabeli. Wprowadź ustawienia wymienione w **szczegóły wpisu w bazie danych** sekcji odnoszą się do zależności wiersz w **wpisy w bazie danych** tabeli jest zaznaczone, jak pokazano na poniższej ilustracji.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych członkostwa

Wybierz **wdrożenia DefaultConnection** wiersz w **wpisy w bazie danych** tabeli w celu skonfigurowania ustawień, które są stosowane do bazy danych członkostwa.

W **parametry połączenia dla docelowej bazy danych**, wprowadź ciąg połączenia wskazujący nowego programu SQL Server Express bazie danych członkostwa. Możesz uzyskać parametry połączenia w **Eksploratora serwera**. W **Eksploratora serwera**, rozwiń węzeł **połączeń danych** i wybierz **aspnetTest** bazy danych, następnie z **właściwości** okno kopiowania **Parametry połączenia** wartość.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Tych samych parametrach połączenia jest przedstawiony tutaj:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Skopiuj i Wklej parametry połączenia do **parametry połączenia dla docelowej bazy danych** w **Pakuj/Publikuj SQL** kartę.

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest zaznaczone. Jest to, co powoduje, że skrypty SQL, aby automatycznie generowane i uruchamiane w docelowej bazie danych.

**Parametry połączenia dla źródłowej bazy danych** wartości są wyodrębniane z *Web.config* plików i punkty w bazie danych programu SQL Server Compact rozwoju. Jest to źródłowej bazy danych, która będzie służyć do generowania skryptów, które będą działać dalej w docelowej bazie danych. Ponieważ jednak planujesz Wdrażanie wersji produkcyjnych bazy danych, należy zmienić "aspnet Dev.sdf" na "aspnet Prod.sdf".

Zmiana **opcje obsługi skryptów bazy danych** z **tylko schemat** do **schemat i dane**na to, że chcesz skopiować dane (konta użytkowników i ról) oraz struktury bazy danych.

Aby skonfigurować wdrożenie na uruchamianie skryptów grant utworzone wcześniej, należy dodać je do **skryptów bazy danych** sekcji. Kliknij przycisk **Dodaj skrypt**, a następnie w **Dodaj skrypty SQL** okno dialogowe, przejdź do folderu przechowywania skryptów przydział (jest to folder, który zawiera plik rozwiązania). Wybierz plik o nazwie *Grant.sql*i kliknij przycisk **Otwórz**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Ustawienia dla **wdrożenia DefaultConnection** wiersz w **wpisy w bazie danych** teraz wyglądać podobnie do poniższej ilustracji:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych School

Następnie wybierz pozycję **wdrożenia SchoolContext** wiersz w **wpisy w bazie danych** tabeli w celu skonfigurowania ustawień wdrożenia dla bazy danych School.

Można użyć tej samej metody, które było wcześniej używane do pobieranie parametrów połączenia dla nowej bazy danych programu SQL Server Express. Skopiuj te parametry połączenia do **parametry połączenia dla docelowej bazy danych** w **Pakuj/Publikuj SQL** kartę.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest zaznaczone.

**Parametry połączenia dla źródłowej bazy danych** wartości są wyodrębniane z *Web.config* plików i punkty w bazie danych programu SQL Server Compact rozwoju. Zmień "School Dev.sdf" na "School-Prod.sdf" do wdrożenia produkcyjnego wersję bazy danych. (Nigdy nie utworzony plik Prod.sdf służbowe w aplikacji\_folderu danych, dzięki czemu będzie skopiować ten plik ze środowiska testowego do aplikacji\_folderu danych w folderze projektu ContosoUniversity później.)

Zmiana **opcje obsługi skryptów bazy danych** do **schemat i dane**.

Użytkownik chce również uruchomić skrypt, aby udzielić odczytu i zapisu dla tej bazy danych do odpowiedniej tożsamości puli aplikacji, więc Dodaj *Grant.sql* plik skryptu, tak jak dla bazy danych członkostwa.

Gdy wszystko będzie gotowe, ustawienia **wdrożenia SchoolContext** wiersz w **wpisy w bazie danych** wyglądać podobnie do poniższej ilustracji:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Czy zapisać zmiany **Pakuj/Publikuj SQL** kartę.

Kopiuj *School-Prod.sdf* plik z *c:\inetpub\wwwroot\ContosoUniversity\App\_danych* folder do *aplikacji\_danych* folderu w Projekt ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Określanie trybu transakcyjne dla skryptu przydział

Proces wdrażania generuje skrypty, które wdrożyć schemat bazy danych i danych. Domyślnie te skrypty są uruchamiane w ramach transakcji. Niestandardowych skryptów (takich jak skrypty przydział) domyślnie nie należy uruchamiać w ramach transakcji. Jeśli proces wdrażania napisana tryby transakcji, możesz otrzymać błąd upływu limitu czasu uruchamiania skryptów podczas wdrażania. W tej sekcji możesz edytować plik projektu, aby skonfigurować niestandardowe skrypty do uruchomienia w ramach transakcji.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, a następnie wybierz **Zwolnij projekt**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Następnie ponownie kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Edytor programu Visual Studio pokazuje zawartość XML w pliku projektu. Należy zauważyć, że istnieje kilka `PropertyGroup` elementów. (Na ilustracji zawartość `PropertyGroup` elementy zostały pominięte.)

![Okno edytora pliku projektu](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Pierwszy z nich, które nie ma `Condition` atrybut, za ustawienia, które są stosowane niezależnie od tego, konfiguracja kompilacji. Jeden `PropertyGroup` element ma zastosowanie tylko do konfiguracji kompilacji debugowania (Uwaga `Condition` atrybut), jednego ma zastosowanie tylko do konfiguracji kompilacji wydania i jeden ma zastosowanie tylko do testu konfiguracji kompilacji. W ramach `PropertyGroup` elementu dla konfiguracji kompilacji wydania, zobaczysz `PublishDatabaseSettings` element, który zawiera ustawienia wprowadzone na **Pakuj/Publikuj SQL** kartę. Brak `Object` element, który odnosi się do wszystkich skryptów grant określone (Zwróć uwagę dwa wystąpienia elementu "Grant.sql"). Domyślnie `Transacted` atrybutu `Source` elementu dla każdego skryptu grant `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Zmień wartość właściwości `Transacted` atrybutu `Source` elementu `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Zapisz i zamknij plik projektu i kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Załaduj ponownie projekt**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Definiowanie transformacje pliku Web.Config dla parametrów połączenia

Połączenie ciągów dla nowego programu SQL Express baz danych, jak wprowadzona **Pakuj/Publikuj SQL** karty są używane przez narzędzie Web Deploy, tylko w przypadku aktualizowania docelowej bazy danych podczas wdrażania. Nadal jest konieczne konfigurowanie *Web.config* przekształcenia tak, aby połączenie ciągów w wdrożonych *Web.config* pliku wskaż nowych baz danych programu SQL Server Express. (Jeśli używasz **Pakuj/Publikuj SQL** karcie Parametry połączenia nie można skonfigurować w profilu publikowania.)

Otwórz *Web.Test.config* i Zastąp `connectionStrings` element z `connectionStrings` elementu w poniższym przykładzie. (Upewnij się, że kopiujesz tylko element connectionStrings, a nie otaczającego kod, który znajduje się tutaj do zapewniania kontekstu).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Ten kod powoduje `connectionString` i `providerName` atrybuty każdego `add` element ma zostać zastąpione w wdrożonych *Web.config* pliku. Te parametry połączenia nie są identyczne z tymi, które zostały wprowadzone w **Pakuj/Publikuj SQL** kartę. Ustawienie "MultipleActiveResultSets = True", ponieważ jest to wymagane dla programu Entity Framework i dostawców uniwersalnych została dodana do nich.

## <a name="installing-sql-server-compact"></a>Instalowanie programu SQL Server Compact

Pakiet SqlServerCompact NuGet zawiera bazy danych programu SQL Server Compact zestawów aparatu dla aplikacji Contoso University. Ale teraz nie jest aplikacja, ale narzędzie Web Deploy, który musi mieć możliwość odczytu baz danych programu SQL Server Compact, aby można było utworzyć skrypty do uruchomienia w bazach danych programu SQL Server. Aby włączyć narzędzie Web Deploy do odczytu baz danych programu SQL Server Compact, zainstalować SQL Server Compact na komputerze deweloperskim, korzystając z następującego linku: [programu Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Wdrażanie do środowiska testowego

Aby można było opublikować w środowisku testowym, należy utworzyć profil publikowania, który jest skonfigurowany do używania **Pakuj/Publikuj SQL** kartę publikowania zamiast ustawień bazy danych profilu publikowania bazy danych.

Najpierw usuń istniejący profil testu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **Publikuj**.

Wybierz **profilu** kartę.

Kliknij przycisk **Zarządzanie profilami**.

Wybierz **testu**, kliknij przycisk **Usuń**, a następnie kliknij przycisk **Zamknij**.

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.

Następnie utwórz nowy profil testu i umożliwia publikowanie projektu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **Publikuj**.

Wybierz **profilu** kartę.

Wybierz **&lt;nowy... &gt;** z listy rozwijanej listy, a następnie wprowadź "Test" jako nazwy profilu.

W **adres URL usługi** wprowadź *localhost*.

W **witryny/aplikacji** wprowadź *domyślnej witryny sieci Web/ContosoUniversity*.

W **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity/`.

Kliknij przycisk **Dalej**.

**Ustawienia** kartę ostrzega, że **Pakuj/Publikuj SQL** karta została skonfigurowana i daje możliwość je zastąpić, klikając przycisk Włącz nowe ulepszenia dotyczące publikowania bazy danych. W przypadku tego wdrożenia nie chcesz zastąpić **Pakuj/Publikuj SQL** karcie Ustawienia, więc po prostu kliknij **dalej**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Komunikat na **(wersja zapoznawcza)** karcie wskazuje, że **nie wybrano baz danych do publikowania**, ale oznacza to jedynie, że publikowanie bazy danych nie jest skonfigurowany w profilu publikowania.

Kliknij przycisk **publikowania**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio wdroży aplikację i otwiera przeglądarkę na stronę główną witryny w środowisku testowym. Uruchom na stronie instruktorów, aby zobaczyć, wyświetla te same dane, które były wyświetlane wcześniej. Uruchom **dodać uczniów** się dodać nowego Studenta, a następnie Wyświetl nowego studenta w **studentów** strony. Sprawdza, czy można zaktualizować bazy danych. Wybierz **aktualizacji środki na korzystanie z** strony (należy się zalogować), aby sprawdzić, czy została wdrożona w bazie danych członkostwa i masz do niego dostęp.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Tworzenie bazy danych programu SQL Server w środowisku produkcyjnym

Teraz, gdy wdrożono do środowiska testowego, możesz skonfigurować wdrożenie w środowisku produkcyjnym. Możesz rozpocząć, tak jak dla środowiska testowego, tworząc bazę danych do wdrożenia. Tak jak pamiętasz od omówienia Cytanium Lite planu hostingu zezwala tylko pojedynczej bazy danych programu SQL Server, więc zostanie skonfigurowane tylko jedną bazę danych, nie dwa. Wszystkie tabele i dane z członkostwa i bazy danych School programu SQL Server Compact będzie można wdrożyć w jednej bazie danych SQL Server w środowisku produkcyjnym.

Przejdź do panelu sterowania Cytanium w [ http://panel.cytanium.com ](http://panel.cytanium.com). Umieść wskaźnik myszy nad **baz danych** a następnie kliknij przycisk **programu SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

W **programu SQL Server 2008** kliknij **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nazwa bazy danych "Służbowego", a następnie kliknij przycisk **Zapisz**. (Strona automatycznie dodaje prefiksu "contosou", więc obowiązywać nazwą będzie "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

W tej samej stronie, kliknij polecenie **Create User**. Na serwerach Cytanium firmy, a nie korzysta ze zintegrowanych zabezpieczeń Windows, jak i umożliwienie tożsamość puli aplikacji Otwórz swoją bazę danych utworzymy użytkownika, który ma uprawnienia do otwierania bazy danych. Należy dodać poświadczenia użytkownika do parametrów połączenia, które bardziej szczegółowo w produkcji *Web.config* pliku. W tym kroku utworzysz tych poświadczeń.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Wypełnij wymagane pola w **właściwości użytkownika SQL** strony:

- Wprowadź "ContosoUniversityUser" jako nazwę.
- Wprowadź hasło.
- Wybierz **contosouSchool** jako domyślna baza danych.
- Wybierz **contosouSchool** pole wyboru.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurowanie wdrażania bazy danych w środowisku produkcyjnym

Teraz możesz przystąpić do konfigurowania ustawień wdrażania bazy danych w **Pakuj/Publikuj SQL** karcie, tak jak wcześniej dla środowiska testowego.

Otwórz **właściwości projektu** wybierz **Pakuj/Publikuj SQL** kartę i upewnij się, że **aktywny (wersja)** lub **wersji** jest wybrane w **konfiguracji** listy rozwijanej.

Podczas konfigurowania ustawień wdrożenia dla każdej bazy danych jest główną różnicą między co zrobić w przypadku środowisk produkcyjnych i testowych, w jaki sposób będziesz konfigurować parametry połączenia. Dla środowiska testowego wprowadzić parametry połączenia bazy danych w różnych miejsc docelowych, ale w środowisku produkcyjnym ciąg połączenia docelowy jest taki sam dla obu baz danych. Jest to spowodowane obu bazach danych jest wdrażany z jedną bazę danych w środowisku produkcyjnym.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych członkostwa

Aby skonfigurować ustawienia, które są stosowane do bazy danych członkostwa, wybierz **wdrożenia DefaultConnection** wiersz w **wpisy w bazie danych** tabeli.

W **parametry połączenia dla docelowej bazy danych**, wprowadź parametry połączenia, które wskazują na nowo utworzoną bazę danych programu SQL Server produkcji nowe. Parametry połączenia można uzyskać z powitalnej wiadomości e-mail. Odpowiednie części wiadomości e-mail zawiera następujące przykładowe parametry połączenia:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Po zamianie trzech zmiennych ciągu połączenia, które są potrzebne będzie wyglądać następująco:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Skopiuj i Wklej parametry połączenia do **parametry połączenia dla docelowej bazy danych** w **Pakuj/Publikuj SQL** kartę.

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest nadal zaznaczone oraz że **opcje obsługi skryptów bazy danych** jest nadal **schemat i dane**.

W **skryptów bazy danych** polu, wyczyść pole wyboru obok skryptu Grant.sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych School

Następnie wybierz pozycję **wdrożenia SchoolContext** wiersz w **wpisy w bazie danych** tabeli w celu skonfigurowania ustawień bazy danych School.

Skopiuj te same parametry połączenia do **parametry połączenia dla docelowej bazy danych** skopiowany do tego pola dla bazy danych członkostwa.

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest nadal zaznaczone oraz że **opcje obsługi skryptów bazy danych** jest nadal **schemat i dane**.

W **skryptów bazy danych** polu, wyczyść pole wyboru obok skryptu Grant.sql.

Czy zapisać zmiany **Pakuj/Publikuj SQL** kartę.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Konfigurowanie pliku Web.Config przekształcenia na parametry połączenia do produkcyjnych bazach danych

Następnie należy skonfigurować *Web.config* przekształcenia tak, aby połączenie ciągów w wdrożonych *Web.config* pliku, aby wskazywał nową bazę danych w środowisku produkcyjnym. Parametry połączenia, który został wprowadzony na **Pakuj/Publikuj SQL** kartę dla narzędzia Web Deploy do użycia jest taka sama jak aplikacja musi używać, z wyjątkiem dodawania `MultipleResultSets` opcji.

Otwórz *Web.Production.config* i Zastąp `connectionStrings` element z `connectionStrings` element, który wygląda podobnie jak w poniższym przykładzie. (Tylko kopiowanie `connectionStrings` elementu, nie otaczającego tagi, które są dostarczane do wyświetlenia w kontekście.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Czasami napotkać informację, że informuje zawsze Szyfruj parametry połączenia w *Web.config* pliku. Może to być przydatne, jeśli były wdrażane do serwerów w sieci własnej firmy. W przypadku wdrażania współużytkowanym środowisku hostingu, jednak masz zaufanie, rozwiązania w zakresie zabezpieczeń dostawcy hostingu i nie jest konieczne lub praktycznego do szyfrowania parametrów połączenia.

## <a name="deploying-to-the-production-environment"></a>Wdrażanie w środowisku produkcyjnym

Teraz możesz przystąpić do wdrażania w środowisku produkcyjnym. Narzędzie Web Deploy odczyta baz danych programu SQL Server Compact do projektu *aplikacji\_danych* folder i ponowne utworzenie wszystkich swoich tabel i danych w produkcyjnej bazy danych programu SQL Server. Aby można było opublikować za pomocą **pakowaniu/publikowaniu Web** karcie Ustawienia, należy utworzyć nowy profil publikowania w środowisku produkcyjnym.

Najpierw usuń istniejący profil produkcji, tak jak wcześniej profilu testowego.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **Publikuj**.

Wybierz **profilu** kartę.

Kliknij przycisk **Zarządzanie profilami**.

Wybierz **produkcji**, kliknij przycisk **Usuń**, a następnie kliknij przycisk **Zamknij**.

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.

Następnie utwórz nowy profil produkcji i umożliwia publikowanie projektu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **Publikuj**.

Wybierz **profilu** kartę.

Kliknij przycisk **importu**, a następnie wybierz plik .publishsettings, który został wcześniej pobrany.

Na **połączenia** kartę, zmień **docelowy adres URL** poprawny adres URL tymczasowy, który w tym przykładzie jest http://contosouniversity.com.vserver01.cytanium.com.

Zmień nazwę tego profilu do środowiska produkcyjnego. (Wybierz **profilu** kartę, a następnie kliknij przycisk **spravovat profily** Aby to zrobić).

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać zmiany.

W rzeczywistej aplikacji, w którym baza danych była aktualizowana w środowisku produkcyjnym należy dwa dodatkowe kroki teraz przed opublikowaniem:

1. Przekaż *aplikacji\_offline.htm*, jak pokazano na [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka.
2. Użyj **Menedżera plików** funkcji Panelu sterowania Cytanium, aby skopiować *aspnet Prod.sdf* i *School-Prod.sdf* plików z lokacji produkcyjnej do *Aplikacji\_danych* folderu projektu ContosoUniversity. Daje to gwarancję, że dane, które wdrażasz do nowej bazy danych programu SQL Server zawiera najnowsze aktualizacje wprowadzone w produkcyjnej witrynie internetowej.

W **Web publikacja w pojedynczym kliknięciem** narzędzi, upewnij się, że **produkcji** profilu jest zaznaczona, a następnie kliknij przycisk **Publikuj**.

Jeśli przekazano <em>aplikacji\_offline.htm</em> przed opublikowaniem, trzeba użyć <strong>Menedżera plików</strong> narzędzie w Panelu sterowania Cytanium, aby usunąć <em>aplikacji\_w trybie offline.</em> htm przed testowaniem. Można również jednocześnie usunąć <em>.sdf</em> plików ze <em>aplikacji\_danych</em> folderu.

Możesz teraz Otwórz przeglądarkę i przejdź do adresu URL witryny w sieci publicznej, aby przetestować aplikację w taki sam sposób, które były wykonane po wdrożeniu do środowiska testowego.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Przełączanie do programu SQL Server Express LocalDB w trakcie opracowywania

Jak wyjaśniono w przeglądzie, zazwyczaj najlepiej jest używać tego samego aparatu bazy danych w rozwoju, którego używasz w testowym i produkcyjnym. (Należy pamiętać, że przy użyciu programu SQL Server Express w rozwoju zaletą jest to czy baza danych będzie działać tak samo w środowiskach programowania, testowania i produkcji). W tej sekcji należy skonfigurować projekt ContosoUniversity można użyć programu SQL Server Express LocalDB, po uruchomieniu aplikacji w programie Visual Studio.

Najprostszym sposobem przeprowadzania migracji jest umożliwienie Code First i system członkostwa tworzyć zarówno nowe rozwoju bazy danych dla Ciebie. Aby przeprowadzić migrację za pomocą tej metody wymaga wykonania trzech kroków:

1. Zmień parametry połączenia, aby określić nowe bazy danych programu SQL Express LocalDB.
2. Uruchom narzędzie Administratorskie witryny sieci Web, aby utworzyć użytkownika administrator. Spowoduje to utworzenie bazy danych członkostwa.
3. Tworzenie i Inicjowanie bazy danych aplikacji, należy użyć polecenia update-database migracje Code First.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualizowanie parametrów połączenia w pliku Web.config

Otwórz *Web.config* i Zastęp `connectionStrings` element następującym kodem:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Tworzenie bazy danych członkostwa

W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity, a następnie kliknij **Konfiguracja ASP.NET** w **projektu** menu.

Wybierz kartę Zabezpieczenia.

Kliknij przycisk **tworzenia lub Zarządzaj rolami**, a następnie utwórz **administratora** roli.

Wróć do karty Zabezpieczenia.

Kliknij przycisk **Utwórz użytkownika**, a następnie wybierz pozycję **administratora** pole wyboru, a następnie utworzyć użytkownika o nazwie administratora.

Zamknij **narzędzie Administratorskie witryny sieci Web**.

### <a name="creating-the-school-database"></a>Tworzenie bazy danych School

Otwórz okno konsoli Menedżera pakietów.

W **projekt domyślny** listę rozwijaną, wybierz projekt ContosoUniversity.DAL na liście.

Wprowadź następujące polecenie:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migracje Code First dotyczą migracji początkowego, który tworzy bazę danych, a następnie stosuje migracji AddBirthDate, a następnie działa Seed — metoda

Uruchamianie witryny, naciskając klawisz F5 kontroli. Tak jak w przypadku środowisk testowych i produkcyjnych, uruchom **dodać uczniów** się dodać nowego Studenta, a następnie Wyświetl nowego studenta w **studentów** strony. Sprawdza, czy bazy danych School zostało utworzone i zainicjowane, a po ich przeczytaniu, a dostęp do zapisu do niego.

Wybierz **aktualizacji środki na korzystanie z** strony, a następnie zaloguj się, aby sprawdzić, czy bazy danych członkostwa został wdrożony i czy masz do niego dostęp. Nie w przypadku migrowania kont użytkowników, tworzenie konta administratora, a następnie wybierz pozycję **aktualizacji środki na korzystanie z** strony, aby sprawdzić, czy działa.

## <a name="cleaning-up-sql-server-compact-files"></a>Czyszczenie plików programu SQL Server Compact

Nie są już potrzebne pliki i pakiety NuGet, które zostały dodane w celu obsługi programu SQL Server Compact. Jeśli chcesz (ten krok nie jest wymagane), możesz wyczyścić niepotrzebnych plików i odwołań.

W **Eksploratora rozwiązań**, Usuń *.sdf* plików ze *aplikacji\_danych* folder i *amd64* i *x86* folderów z *bin* folderu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie (nie jednego z projektów), a następnie kliknij przycisk **Zarządzaj pakietami NuGet dla rozwiązania**.

W okienku po lewej stronie **Zarządzaj pakietami NuGet** okno dialogowe, wybierz opcję **zainstalowanych pakietów**.

Wybierz **EntityFramework.SqlServerCompact** pakietu, a następnie kliknij przycisk **Zarządzaj**.

W **Wybierz projekty** okno dialogowe, oba projekty są zaznaczone. Aby odinstalować pakiet w obu projektach, wyczyść pola wyboru, a następnie kliknij przycisk **OK**.

W oknie dialogowym z pytaniem, jeśli chcesz również odinstalować zależne pakiety, kliknij przycisk nie. Jeden z nich jest pakietu programu Entity Framework, który trzeba będzie przechowywać.

Wykonaj tę samą procedurę, aby odinstalować **SqlServerCompact** pakietu. (Pakiety muszą zostać odinstalowane w podanej kolejności, ponieważ **EntityFramework.SqlServerCompact** pakiet jest zależny od **SqlServerCompact** pakietu.)

Możesz teraz pomyślnie przeprowadzono migrację do programu SQL Server Express i pełnej wersji programu SQL Server. W następnym samouczku wprowadzisz inną zmianę w bazie danych, a opisano sposób wdrażania zmian w bazie danych, używając testowych i produkcyjnych baz danych programu SQL Server Express i pełnej wersji programu SQL Server.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
