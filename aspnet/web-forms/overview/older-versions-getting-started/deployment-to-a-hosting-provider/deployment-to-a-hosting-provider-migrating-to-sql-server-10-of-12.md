---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Migracja do programu SQL Server - 10, 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: b97834e3e287645151bf927996fde63d93ae8356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Migracja do programu SQL Server - 10, 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak przeprowadzić migrację z programu SQL Server Compact do programu SQL Server. Jedną z przyczyn czy może chcesz to zrobić ma korzystać z funkcji programu SQL Server, które program SQL Server Compact nie obsługuje, takich jak procedur składowanych, wyzwalaczy, widoków i replikacji. Aby uzyskać więcej informacji na temat różnic między programu SQL Server Compact i SQL Server, zobacz [wdrażanie programu SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) samouczka.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express lub pełnego serwera SQL do tworzenia aplikacji

Gdy decydujesz się na uaktualnienie do programu SQL Server, można użyć programu SQL Server lub SQL Server Express w swoich środowiskach programistycznych i testowych. Oprócz różnice w obsłudze narzędzia i funkcje aparatu bazy danych ma różnic w implementacji dostawcy programu SQL Server Compact i inne wersje programu SQL Server. Te różnice może spowodować ten sam kod można wygenerować różne wyniki. W związku z tym Jeśli zdecydujesz się zachować programu SQL Server Compact jako programowanie bazy danych, należy dokładnie przetestować witryny w programie SQL Server lub SQL Server Express w środowisku testowym przed każdym wdrożenia do produkcji.

W przeciwieństwie do programu SQL Server Compact programu SQL Server Express jest zasadniczo tego samego aparatu bazy danych i używa tego samego dostawcy .NET jako pełnego serwera SQL. Podczas testowania z programu SQL Server Express, można mieć pewność pobierania takie same wyniki jak będzie z programem SQL Server. Większość tych samych narzędzi bazy danych można używać z programu SQL Server Express używanego z programem SQL Server (oprócz trwa [programu SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), i obsługuje inne funkcje programu SQL Server, takich jak widoki procedur składowanych, wyzwalaczy, i replikacji. (Zazwyczaj należy jednak używać pełnej wersji programu SQL Server w środowisku produkcyjnym witryny sieci Web. Program SQL Server Express można uruchomić w środowisku macierzystym udostępnionego, ale nie został zaprojektowany do tego, a wielu dostawców hostingu nie obsługują.)

Jeśli używasz programu Visual Studio 2012, zwykle wybierz programu SQL Server Express LocalDB dla środowiska deweloperskiego ponieważ jest to, co jest instalowany domyślnie z programem Visual Studio. Jednak LocalDB nie działa w usługach IIS, dlatego dla danego środowiska testowego należy użyć programu SQL Server lub SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Połączenie bazy danych i przechowywanie ich oddzielne

Aplikacja Contoso University ma dwie bazy danych programu SQL Server Compact: bazy danych członkostwa (*aspnet.sdf*) i bazy danych aplikacji (*School.sdf*). Podczas migracji, należy przeprowadzić migrację tych baz danych do dwóch oddzielnych baz danych lub do pojedynczej bazy danych. Możesz połączyć je w celu umożliwienia sprzężenia bazy danych między bazy danych aplikacji i członkostwa w bazie danych. Plan hostingu może też podać przyczynę łączyć je. Na przykład dostawca hostingu może pobierać jednego dla wielu baz danych lub może nie pozwalać nawet więcej niż jednej bazy danych. To w przypadku konta używanego do celów tego samouczka, dzięki czemu tylko jednej bazy danych SQL Server obsługującego Lite Cytanium.

W tym samouczku będziesz migracji dwóch baz danych w ten sposób:

- Przeprowadź migrację do dwóch LocalDB baz danych w środowisku programistycznym.
- Przeprowadź migrację do dwóch programu SQL Server Express baz danych w środowisku testowym.
- Przeprowadź migrację do jednego połączonego pełnej bazy danych SQL Server w środowisku produkcyjnym.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalowanie programu SQL Server Express

SQL Server Express jest automatycznie instalowany domyślnie z programu Visual Studio 2010, ale domyślnie nie jest zainstalowana z programu Visual Studio 2012. Aby zainstalować program SQL Server 2012 Express, kliknij poniższe łącze.

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Wybierz *SQLEXPR-plk/x64\_x64\_ENU.exe* lub *SQLEXPR-plk/x86\_x86\_ENU.exe*i w Kreatorze instalacji zaakceptuj wartość domyślną Ustawienia. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [Instalowanie programu SQL Server 2012 z poziomu Kreatora instalacji (Instalator)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Tworzenie bazy danych programu SQL Server Express dla środowiska testowego

Następnym krokiem jest tworzenie członkostwa ASP.NET i szkoły baz danych.

Z **widoku** menu wybierz opcję **Eksploratora serwera** (**Eksploratora bazy danych** w Visual Web Developer), a następnie kliknij prawym przyciskiem myszy **połączenia danych**i wybierz **Tworzenie nowej bazy danych SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

W **Tworzenie nowej bazy danych SQL Server** okna dialogowego wprowadź ". \SQLExpress" w **nazwy serwera** pole i "aspnet-Test" w **nowej nazwy bazy danych** , a następnie kliknij przycisk **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Postępuj zgodnie z tą samą procedurą, aby utworzyć nową bazę danych programu SQL Server Express służbowe o nazwie "Służbowe-Test".

(Są dołączane "Test" te nazwy bazy danych, ponieważ później utworzysz dodatkowe wystąpienia każdej bazy danych dla środowiska programowania i muszą mieć możliwość rozróżnienia dwa zestawy baz danych.)

**W Eksploratorze serwera** zawiera teraz dwa nowe bazy danych.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Tworzenie skryptu Grant dla nowych baz danych

Po uruchomieniu aplikacji w usługach IIS na komputerze dewelopera aplikacji dostęp do bazy danych przy użyciu poświadczeń domyślnej puli aplikacji. Jednak domyślnie tożsamość puli aplikacji nie ma uprawnień do otwierania bazy danych. Dlatego należy uruchomić skrypt, aby przyznać uprawnienie. W tej sekcji utworzysz skryptu, że uruchomisz później, aby upewnić się, że aplikacji można otworzyć bazy danych po uruchomieniu w usługach IIS.

W rozwiązaniu *SolutionFiles* folder utworzony w [wdrażania w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczek, Utwórz nowy plik SQL o nazwie *Grant.sql*. Skopiuj następujące polecenia SQL do pliku, a następnie zapisz i zamknij plik:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Ten skrypt jest przeznaczona do pracy z programu SQL Server 2008 i ustawienia programu IIS w systemie Windows 7, są one określone w tym samouczku. Jeśli używasz innej wersji programu SQL Server lub systemu Windows lub skonfigurowanie usług IIS na komputerze inaczej, zmiany w tym skrypcie może być wymagane. Aby uzyskać więcej informacji na temat skryptów programu SQL Server, zobacz [programu SQL Server — książki Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** ten skrypt podaje db\_uprawnień właściciela na użytkownika, który uzyskuje dostęp do bazy danych w czasie wykonywania, czyli, należy w środowisku produkcyjnym. W niektórych scenariuszach można określić użytkownika, który ma schematu pełnej bazy danych zaktualizować uprawnienia tylko do wdrożenia i określ czas uruchomienia innego użytkownika z uprawnieniami tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz **przeglądania automatyczne zmian pliku Web.config dla migracje Code First** w [wdrażania usług IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurowanie wdrażania bazy danych dla środowiska testowego

Następnie należy skonfigurować program Visual Studio, dzięki czemu będzie wykonywać następujące zadania dla każdej bazy danych:

- Generuj skrypt SQL, który tworzy struktury źródłowej bazy danych (tabel, kolumn, ograniczenia, itp.) w docelowej bazie danych.
- Generuj skrypt SQL, która wstawia dane źródłowej bazy danych w tabelach w docelowej bazie danych.
- Uruchom wygenerowanych skryptów i skrypt Grant utworzone w docelowej bazie danych.

Otwórz **właściwości projektu** i zaznacz **Pakuj/Publikuj SQL** kartę.

Upewnij się, że **aktywny (wersja)** lub **wersji** wybrano **konfiguracji** listy rozwijanej.

Kliknij przycisk **włączyć tę stronę**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**Pakuj/Publikuj SQL** kartę zwykle jest wyłączona, ponieważ określa on metody wdrażania starszej wersji. W przypadku większości scenariuszy należy skonfigurować wdrożenie bazy danych w **publikowanie w sieci Web** kreatora. Migrowanie z programu SQL Server Compact do programu SQL Server lub SQL Server Express jest szczególnych przypadkach, dla którego ta metoda jest dobrym rozwiązaniem.

Kliknij przycisk **Import z pliku Web.config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio przeszuka parametry połączenia w *Web.config* plików, znajduje się jeden dla bazy danych członkostwa i drugi dla bazy danych służbowych i dodaje odpowiadający każdej parametry połączenia w **wpisów bazy danych.**  tabeli. Parametry połączenia znajdzie są dla istniejących baz danych programu SQL Server Compact i z następnym krokiem jest skonfigurowanie, jak i gdzie do wdrożenia tych baz danych.

Wprowadź ustawienia wdrażania bazy danych w **Szczegóły zapisu bazy danych** poniżej **wpisy w bazie danych** tabeli. Ustawienia w **Szczegóły zapisu bazy danych** sekcji odnoszą się do zależności wierszu w **wpisy w bazie danych** zaznaczonych tabel, jak pokazano na poniższej ilustracji.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurowanie ustawień wdrażania bazy danych członkostwa

Wybierz **wdrożenia połączenia DefaultConnection** wierszu w **wpisy w bazie danych** tabeli w celu skonfigurowania ustawień, które są stosowane do bazy danych członkostwa.

W **parametry połączenia dla docelowej bazy danych**, wprowadź parametry połączenia, które wskazuje nowego programu SQL Server Express bazie danych członkostwa. Można pobrać parametry połączenia z **Eksploratora serwera**. W **Eksploratora serwera**, rozwiń węzeł **połączenia danych** i wybierz **aspnetTest** bazy danych, następnie z **właściwości** okno kopiowania **Ciąg połączenia** wartość.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Te same parametry połączenia jest przedstawiony poniżej:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Skopiuj i Wklej parametry połączenia do **parametry połączenia dla docelowej bazy danych** w **Pakuj/Publikuj SQL** kartę.

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest zaznaczone. Jest to, co powoduje, że skrypty SQL automatycznie generowane i uruchamiane w docelowej bazie danych.

**Ciąg połączenia dla źródłowej bazy danych** wartość jest wyodrębniana z *Web.config* pliku i wskazuje na projektowej bazie danych programu SQL Server Compact. Jest to źródłowej bazy danych, która będzie służyć do generowania skryptów, które będą uruchamiane później w docelowej bazie danych. Ponieważ chcesz wdrożyć wersji produkcyjnej bazy danych, należy zmienić "aspnet Dev.sdf" na "aspnet Prod.sdf".

Zmień **bazy danych opcje obsługi skryptów** z **tylko schematu** do **schemat i dane**, ponieważ chcesz skopiować dane (kont użytkowników i ról) oraz struktury bazy danych.

Aby skonfigurować wdrożenie na uruchamianie skryptów grant, które zostały utworzone wcześniej, należy dodać je do **skryptów bazy danych** sekcji. Kliknij przycisk **Dodaj skrypt**i w **dodać skryptów SQL** okno dialogowe pola, przejdź do folderu przechowywania skryptów grant (jest to folder, który zawiera plik rozwiązania). Wybierz plik o nazwie *Grant.sql*i kliknij przycisk **Otwórz**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Ustawienia dla **wdrożenia połączenia DefaultConnection** wierszu w **wpisy w bazie danych** wyglądać jak na poniższej ilustracji:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurowanie ustawień wdrażania bazy danych służbowych

Następnie wybierz pozycję **wdrożenia SchoolContext** wierszu w **wpisy w bazie danych** tabeli w celu skonfigurowania ustawienia wdrażania bazy danych służbowych.

Można użyć tej samej metody użytej wcześniej można pobrać ciągu połączenia dla nowej bazy danych programu SQL Server Express. Skopiuj parametry połączenia do **parametry połączenia dla docelowej bazy danych** w **Pakuj/Publikuj SQL** kartę.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest zaznaczone.

**Ciąg połączenia dla źródłowej bazy danych** wartość jest wyodrębniana z *Web.config* pliku i wskazuje na projektowej bazie danych programu SQL Server Compact. Zmień "Służbowe Dev.sdf" na "Służbowe — Prod.sdf", aby wdrożyć wersji produkcyjnej bazy danych. (Nigdy nie utworzony plik Prod.sdf służbowe w aplikacji\_folderu danych, więc będzie skopiować ten plik ze środowiska testowego do aplikacji\_folderu danych w folderze projektu ContosoUniversity później.)

Zmień **bazy danych opcje obsługi skryptów** do **schemat i dane**.

Należy uruchomić skrypt w celu przyznania odczytu i zapisu dla tej bazy danych do odpowiedniej tożsamości puli aplikacji, a więc Dodaj *Grant.sql* plik skryptu, jak bazy danych członkostwa.

Gdy wszystko będzie gotowe, ustawienia **wdrożenia SchoolContext** wierszu w **wpisy w bazie danych** wyglądać jak na poniższej ilustracji:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Zapisać zmiany w **Pakuj/Publikuj SQL** kartę.

Kopiuj *służbowe-Prod.sdf* plik z *c:\inetpub\wwwroot\ContosoUniversity\App\_danych* folder do *aplikacji\_danych* folderu w Projekt ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Określanie trybu transakcyjne dla skryptu Grant

Proces wdrażania generuje skryptów wdrażających schematu bazy danych i danych. Domyślnie te skrypty są uruchamiane w transakcji. Jednakże skrypty niestandardowe (takie jak skrypty grant), domyślnie nie należy uruchamiać w transakcji. Jeśli proces wdrażania łączy tryby transakcji, można otrzymać błąd upływu limitu czasu podczas uruchamiania skryptów podczas wdrażania. W tej sekcji Edytuj plik projektu w celu skonfigurowania niestandardowych skryptów do uruchomienia w ramach transakcji.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projekt i wybierz **Zwolnij projekt**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Następnie ponownie kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Edytor programu Visual Studio zawiera zawartość XML w pliku projektu. Zwróć uwagę, że istnieje kilka `PropertyGroup` elementów. (W obrazie zawartość `PropertyGroup` elementy zostały pominięte.)

![Okno edytora pliku projektu](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Pierwsza z nich, które nie ma `Condition` atrybutu, jest dla ustawień, które są stosowane niezależnie od tego, konfiguracja kompilacji. Jeden `PropertyGroup` element ma zastosowanie tylko do konfiguracji kompilacji debugowania (Uwaga `Condition` atrybut), co dotyczy tylko wersji konfiguracji kompilacji i jeden ma zastosowanie tylko do testu konfiguracji kompilacji. W ramach `PropertyGroup` element konfiguracji kompilacji wydania, zobaczysz `PublishDatabaseSettings` element, który zawiera ustawienia wprowadzone **Pakuj/Publikuj SQL** kartę. Brak `Object` element, który odpowiada wszystkie skrypty grant określone (powiadomienie dwa wystąpienia elementu "Grant.sql"). Domyślnie `Transacted` atrybutu `Source` jest element dla każdego skryptu grant `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Zmień wartość `Transacted` atrybutu `Source` elementu `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Zapisz i zamknij plik projektu i kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Załaduj ponownie projekt**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Trwa konfigurowanie przekształcenia pliku Web.Config dla parametrów połączenia

Ciągi połączenia dla nowego programu SQL Express baz danych tego wprowadzono **Pakuj/Publikuj SQL** kartę są używane przez narzędzie Web Deploy tylko w przypadku aktualizowania docelowej bazy danych podczas wdrażania. Nadal jest konieczne konfigurowanie *Web.config* przekształcenia tak, aby parametry połączenia w wdrożone *Web.config* pliku wskaż nowych baz danych programu SQL Server Express. (Jeśli używasz **Pakuj/Publikuj SQL** kartę, parametry połączenia nie można skonfigurować w profilu publikowania.)

Otwórz *Web.Test.config* i Zastąp `connectionStrings` element z `connectionStrings` elementu w poniższym przykładzie. (Upewnij się, że tylko skopiuj element connectionStrings nie otaczającego kod, który znajduje się tutaj do zapewnienia kontekstu.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Powoduje, że ten kod `connectionString` i `providerName` atrybuty każdego `add` elementu można zamienić w wdrożone *Web.config* pliku. Te parametry połączenia nie są takie same jak te zdefiniowane w **Pakuj/Publikuj SQL** kartę. Ustawienie "MultipleActiveResultSets = True", ponieważ są wymagane dla programu Entity Framework i dostawców uniwersalnych został dodany do nich.

## <a name="installing-sql-server-compact"></a>Instalowanie programu SQL Server Compact

Pakiet SqlServerCompact NuGet zawiera bazy danych programu SQL Server Compact zestawy aparatu dla aplikacji Contoso University. Ale obecnie nie jest aplikacja, ale narzędzie Web Deploy, który musi mieć możliwość odczytu bazy danych programu SQL Server Compact, aby można było utworzyć na uruchamianie skryptów w baz danych programu SQL Server. Aby włączyć narzędzia Web Deploy do odczytu bazy danych programu SQL Server Compact, zainstaluj program SQL Server Compact na komputerze dewelopera przy użyciu następującego łącza: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Wdrażanie do środowiska testowego

Aby publikować w środowisku testowym, należy utworzyć profil publikowania, który jest skonfigurowany do używania **Pakuj/Publikuj SQL** kartę dla bazy danych publikacji zamiast ustawień publikowania bazy danych profilu.

Najpierw usuń istniejący profil testu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **publikowania**.

Wybierz **profilu** kartę.

Kliknij przycisk **zarządzania profilami**.

Wybierz **testu**, kliknij przycisk **Usuń**, a następnie kliknij przycisk **Zamknij**.

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.

Następnie utwórz nowy profil testu i użyć go do publikowania projektu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **publikowania**.

Wybierz **profilu** kartę.

Wybierz  **&lt;nowych... &gt;**  z listy rozwijanej liście, a następnie wprowadź "Test" jako nazwy profilu.

W **adres URL usługi** wprowadź *localhost*.

W **witryny i aplikacji** wprowadź *domyślna witryna sieci Web/ContosoUniversity*.

W **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity/`.

Kliknij przycisk **Dalej**.

**Ustawienia** kartę ostrzega, że **Pakuj/Publikuj SQL** skonfigurowano kartę i daje możliwość zastąpienia ich klikając włączyć nowe ulepszenia publikowania bazy danych. Dla tego wdrożenia nie chcesz zastąpić **Pakuj/Publikuj SQL** karcie Ustawienia, więc wystarczy kliknąć **dalej**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Komunikat na **Podgląd** kartę wskazuje, że **nie wybrano baz danych do publikowania**, ale oznacza to tylko, że publikowanie bazy danych nie jest skonfigurowane w profilu publikowania.

Kliknij przycisk **publikowania**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio wdraża aplikację i otwiera przeglądarkę do strony głównej witryny w środowisku testowym. Uruchom stronę instruktorów, aby zobaczyć, wyświetla te same dane wyświetlane wcześniej. Uruchom **dodać studentów** strony, Dodaj nowe studentów, a następnie wyświetlić nowy uczniów w **studentów** strony. Pozwoli to sprawdzić, czy można zaktualizować bazy danych. Wybierz **środków aktualizacji** strony (należy się zalogować) aby sprawdzić, czy została wdrożona w bazie danych członkostwa i czy masz do niego dostęp.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Tworzenie bazy danych programu SQL Server w środowisku produkcyjnym

Teraz, gdy została wdrożona do środowiska testowego, możesz przystąpić do konfigurowania wdrożenia do produkcji. Możesz rozpocząć tak samo jak dla środowiska testowego, tworząc bazę danych do wdrożenia. Tak jak pamiętasz z przeglądu, Cytanium Lite plan hostingu umożliwia tylko pojedynczej bazy danych programu SQL Server, zostanie zainstalowana tylko jedną bazę danych, tj. Wszystkie tabele i dane z członkostwa i szkoły programu SQL Server Compact baz danych będzie można wdrożyć do jednej bazy danych SQL Server w środowisku produkcyjnym.

Przejdź do panelu sterowania Cytanium w [http://panel.cytanium.com](http://panel.cytanium.com). Umieść wskaźnik myszy nad **baz danych** , a następnie kliknij przycisk **programu SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

W **programu SQL Server 2008** kliknij przycisk **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nazwa bazy danych "Służbowe", a następnie kliknij przycisk **zapisać**. (Strona automatycznie dodaje prefiksu "contosou", więc efektywny nazwa będzie "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Na tej samej stronie, kliknij przycisk **Tworzenie użytkownika**. Na Cytanium dla serwerów, a nie korzysta ze zintegrowanych zabezpieczeń systemu Windows i pozwolić tożsamość puli aplikacji, otwórz bazę danych utworzysz użytkownika, który ma uprawnienia do otwierania bazy danych. Poświadczenia użytkownika zostanie dodana do parametrów połączenia, które go do produkcji *Web.config* pliku. W tym kroku utworzysz tych poświadczeń.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Wypełnij wymagane pola w **właściwości użytkownika SQL** strony:

- Wprowadź "ContosoUniversityUser" jako nazwy.
- Wprowadź hasło.
- Wybierz **contosouSchool** jako domyślnej bazy danych.
- Wybierz **contosouSchool** pole wyboru.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurowanie wdrażania bazy danych w środowisku produkcyjnym

Teraz możesz przystąpić do konfigurowania ustawień wdrażania bazy danych w **Pakuj/Publikuj SQL** karcie, tak jak wcześniej dla środowiska testowego.

Otwórz **właściwości projektu** wybierz **Pakuj/Publikuj SQL** karcie i upewnij się, że **aktywny (wersja)** lub **wersji** jest wybrany w **konfiguracji** listy rozwijanej.

Podczas konfigurowania ustawień wdrażania dla każdej bazy danych, klucza różnica między co zrobić w środowiskach produkcyjnych i testowych jest w sposób konfigurowania parametrów połączeń. Dla środowiska testowego wprowadzone parametry połączenia bazy danych w innej lokalizacji docelowej, ale w środowisku produkcyjnym ciąg połączenia docelowego będzie taka sama dla obu baz danych. Jest to spowodowane obie bazy danych jest wdrażany z jednej bazy danych w środowisku produkcyjnym.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurowanie ustawień wdrażania bazy danych członkostwa

Aby skonfigurować ustawienia stosowane do bazy danych członkostwa, wybierz **wdrożenia połączenia DefaultConnection** wierszu w **wpisy w bazie danych** tabeli.

W **parametry połączenia dla docelowej bazy danych**, wprowadź parametry połączenia, które wskazuje nową bazę danych programu SQL Server produkcji, nowo utworzony. Parametry połączenia można uzyskać z powitalną. Odpowiedniej części wiadomości e-mail zawiera następujące przykładowe parametry połączenia:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Po Zastąp trzy zmienne, ciąg połączenia, które są potrzebne wygląda w tym przykładzie:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Skopiuj i Wklej parametry połączenia do **parametry połączenia dla docelowej bazy danych** w **Pakuj/Publikuj SQL** kartę.

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest nadal wybrany, a **bazy danych opcje obsługi skryptów** jest nadal **schemat i dane**.

W **skryptów bazy danych** polu, wyczyść pole wyboru obok Grant.sql skryptu.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurowanie ustawień wdrażania bazy danych służbowych

Następnie wybierz pozycję **wdrożenia SchoolContext** wierszu w **wpisy w bazie danych** tabeli w celu skonfigurowania ustawień bazy danych służbowych.

Skopiuj te same parametry połączenia do **parametry połączenia dla docelowej bazy danych** skopiowaną do pola dla bazy danych członkostwa.

Upewnij się, że **pobierania danych i/lub schemat z istniejącej bazy danych** jest nadal wybrany, a **bazy danych opcje obsługi skryptów** jest nadal **schemat i dane**.

W **skryptów bazy danych** polu, wyczyść pole wyboru obok Grant.sql skryptu.

Zapisać zmiany w **Pakuj/Publikuj SQL** kartę.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Konfigurowanie pliku Web.Config przekształcenia na parametry połączenia do produkcyjnych baz danych

Następnie należy skonfigurować *Web.config* przekształcenia tak, aby parametry połączenia w wdrożone *Web.config* pliku, aby wskazywał nową bazę danych w środowisku produkcyjnym. Parametry połączenia, które zostały wprowadzone **Pakuj/Publikuj SQL** karta dla narzędzia Web Deploy umożliwia jest taka sama, jak aplikacja powinna być używana, z wyjątkiem dodawania `MultipleResultSets` opcji.

Otwórz *Web.Production.config* i Zastąp `connectionStrings` element z `connectionStrings` element, który wygląda jak w następującym przykładzie. (Tylko skopiować `connectionStrings` element, nie otaczającego tagi, które są dostarczane do wyświetlenia w kontekście.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Czasami Zobacz informację, że informuje zawsze szyfrowania parametrów połączenia w *Web.config* pliku. Może to być odpowiednie, jeśli jest wdrażana na serwery w sieci własnej firmy. W przypadku wdrażania udostępnionym środowisku macierzystym, jednak jest ufające rozwiązania w zakresie zabezpieczeń dostawcy hostingu i nie jest konieczne lub praktyczne do szyfrowania parametrów połączenia.

## <a name="deploying-to-the-production-environment"></a>Wdrażanie w środowisku produkcyjnym

Teraz możesz przystąpić do wdrażania w środowisku produkcyjnym. Narzędzie Web Deploy odczyta baz danych programu SQL Server Compact do projektu *aplikacji\_danych* folderu i ponownie utwórz wszystkie tabele i danych w produkcyjnej bazy danych programu SQL Server. Aby opublikować za pomocą **pakowaniu/publikowaniu Web** karcie Ustawienia, należy utworzyć nowy profil publikowania do produkcji.

Najpierw usuń istniejący profil produkcji, tak jak profil testu wcześniej.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **publikowania**.

Wybierz **profilu** kartę.

Kliknij przycisk **zarządzania profilami**.

Wybierz **produkcji**, kliknij przycisk **Usuń**, a następnie kliknij przycisk **Zamknij**.

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.

Następnie utwórz nowy profil produkcji i użyć go do publikowania projektu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij przycisk **publikowania**.

Wybierz **profilu** kartę.

Kliknij przycisk **importu**i wybierz plik .publishsettings, który został wcześniej pobrany.

Na **połączenia** Zmień **docelowy adres URL** poprawny adres URL tymczasowy, który w tym przykładzie jest http://contosouniversity.com.vserver01.cytanium.com.

Zmień nazwę profilu w środowisku produkcyjnym. (Wybierz **profilu** i kliknij polecenie **zarządzania profilami** w tym celu).

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać zmiany.

W rzeczywistej aplikacji, w którym aktualizacji bazy danych w środowisku produkcyjnym należy dwa dodatkowe kroki teraz przed opublikowaniem:

1. Przekaż *aplikacji\_offline.htm*, jak pokazano w [wdrażania w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka.
2. Użyj **Menedżera plików** funkcji Panelu sterowania Cytanium, aby skopiować *aspnet Prod.sdf* i *służbowe-Prod.sdf* plików z lokacji produkcyjnej do *Aplikacji\_danych* folderu projektu ContosoUniversity. Daje to pewność, że dane, które wdrażasz do nowej bazy danych programu SQL Server zawiera najnowsze aktualizacje wprowadzone przez witryny sieci Web produkcji.

W **jeden kliknij publikowania w sieci Web** narzędzi, upewnij się, że **produkcji** profilu jest zaznaczona, a następnie kliknij przycisk **publikowania**.

Jeśli został przekazany *aplikacji\_offline.htm* przed opublikowaniem, należy użyć **Menedżera plików** narzędzia w Panelu sterowania Cytanium, aby usunąć *aplikacji\_w trybie offline.* htm, aby można było przetestować. Można również w tym samym czasie usunąć *sdf* plików ze *aplikacji\_danych* folderu.

Możesz teraz Otwórz przeglądarkę i przejdź do adresu URL witryny sieci publicznych, aby przetestować aplikację w taki sam sposób jak w przypadku po wdrożeniu do środowiska testowego.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Przełączanie do programu SQL Server Express LocalDB w rozwoju

Jak wyjaśniono w przeglądzie, jest zalecane używanie tego samego aparatu bazy danych do rozwoju używanego w testowych i produkcyjnych. (Pamiętaj, że zaletą używania programu SQL Server Express do rozwoju jest czy bazy danych będzie działać w identyczny w środowiska programowania, testowego i produkcyjnego). W tej sekcji należy skonfigurować ContosoUniversity projektu do użycia programu SQL Server Express LocalDB, po uruchomieniu aplikacji w programie Visual Studio.

Najprostszym sposobem przeprowadzania migracji jest umożliwienie Code First i system członkostwa utworzyć zarówno nowych baz danych programowanie dla Ciebie. Aby przeprowadzić migrację za pomocą tej metody wymaga trzy kroki:

1. Zmień parametry połączenia, aby określić nowe bazy danych SQL Express LocalDB.
2. Uruchom narzędzie do administrowania witryną sieci Web do utworzenia użytkownika administrator. Spowoduje to utworzenie bazy danych członkostwa.
3. Aby utworzyć i inicjatora bazy danych aplikacji, należy użyć polecenia update-database migracje Code First.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualizowanie parametrów połączenia w pliku Web.config

Otwórz *Web.config* plików i Zastąp `connectionStrings` element z następującym kodem:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Tworzenie bazy danych członkostwa

W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity, a następnie kliknij przycisk **konfiguracja platformy ASP.NET** w **projektu** menu.

Wybierz kartę Zabezpieczenia.

Kliknij przycisk **Create lub Zarządzanie rolami**, a następnie utwórz **administratora** roli.

Wróć do karty zabezpieczeń.

Kliknij przycisk **tworzenia użytkownika**, a następnie wybierz **administratora** pole wyboru i Utwórz użytkownika o nazwie administratora.

Zamknij **narzędzie do administrowania witryną sieci Web**.

### <a name="creating-the-school-database"></a>Tworzenie bazy danych służbowych

Otwórz okno konsoli Menedżera pakietów.

W **domyślny projekt** listy rozwijanej wybierz ContosoUniversity.DAL projektu.

Wprowadź następujące polecenie:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migracje Code First dotyczy migracji początkowego, które utworzy bazę danych, a następnie stosuje AddBirthDate migracji, a następnie uruchomieniu metoda inicjatora.

Uruchom witrynę, naciskając klawisz F5 kontroli. Tak samo jak w przypadku środowisk testowych i produkcyjnych, uruchom **dodać studentów** strony, Dodaj nowe studentów, a następnie wyświetlić nowy uczniów w **studentów** strony. Pozwoli to sprawdzić, czy szkoły bazy danych została utworzona, a zainicjowany i że zostały przeczytane i zapisywać do niego dostęp.

Wybierz **środków aktualizacji** strony i zaloguj się, aby sprawdzić, czy wdrożono bazie danych członkostwa i czy masz do niego dostęp. Nie w przypadku migrowania kont użytkowników, Utwórz konto administratora, a następnie wybierz **środków aktualizacji** stronę, aby sprawdzić, czy działa.

## <a name="cleaning-up-sql-server-compact-files"></a>Czyszczenie plików programu SQL Server Compact

Nie są już potrzebne pliki i pakiety NuGet, które zostały dołączone do obsługi programu SQL Server Compact. Jeśli chcesz (ten krok nie jest wymagana), możesz wyczyścić niepotrzebnych plików i odwołania.

W **Eksploratora rozwiązań**, Usuń *sdf* plików ze *aplikacji\_danych* folderu i *amd64* i *x86* foldery z *bin* folderu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie (nie jeden z projektów), a następnie kliknij przycisk **Zarządzaj pakietami NuGet dla rozwiązania**.

W lewym okienku **Zarządzaj pakietami NuGet** okno dialogowe, wybierz opcję **zainstalowane pakiety**.

Wybierz **EntityFramework.SqlServerCompact** pakiet, a następnie kliknij przycisk **Zarządzaj**.

W **Wybierz projekty** okno dialogowe, oba projekty są wybrane. Aby odinstalować pakiet w obu tych projektów, wyczyść pola wyboru, a następnie kliknij przycisk **OK**.

W oknie dialogowym z pytaniem, czy chcesz również odinstalować pakietów zależnych, kliknij przycisk nie. Pakiet programu Entity Framework, który ma być jednym z nich jest.

Postępuj zgodnie z tą samą procedurą, aby odinstalować **SqlServerCompact** pakietu. (Pakiety musi zostać odinstalowane w tej kolejności, ponieważ **EntityFramework.SqlServerCompact** pakiet zależy od **SqlServerCompact** pakietu.)

Teraz pomyślnie zostały zmigrowane do programu SQL Server Express i pełnego serwera SQL. W następnej samouczek inną zmianę w bazie danych, na które należy podjąć, zostanie wyświetlone sposobu wdrażania zmian w bazie danych testowych i produkcyjnych baz danych użycia programu SQL Server Express i pełnego serwera SQL.

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[dalej](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
