---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie usług IIS jako środowisko testowe - 5 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a7995844ee6ed19efa130c4f6c019214d6652ea7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie usług IIS jako środowisko testowe - 5 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak wdrożyć aplikację sieci web ASP.NET w usługach IIS na komputerze lokalnym.

Podczas opracowywania aplikacji zwykle testu przez uruchomienie jej w programie Visual Studio. Domyślnie oznacza to, że używasz serwera wdrożeniowego usługi Visual Studio (znanego także jako Cassini). Serwera wdrożeniowego programu Visual Studio ułatwia testowanie podczas tworzenia w programie Visual Studio, ale nie działa tak samo jak usług IIS. W związku z tym jest to możliwe, że aplikacja będzie działać prawidłowo po przetestować go w programie Visual Studio, ale się nie powieść po wdrożeniu w usługach IIS w środowisku macierzystym.

Bardziej niezawodnie przetestować aplikację w następujący sposób:

1. Użyj usługi IIS Express lub pełnego IIS zamiast serwera wdrożeniowego programu Visual Studio podczas testowania w programie Visual Studio podczas tworzenia. Ta metoda zazwyczaj emuluje dokładniej działanie witryny w środowisku usług IIS. Jednak ta metoda testowanie procesu wdrażania komputera lub nie zweryfikować wynik proces wdrażania będzie działać poprawnie.
2. Wdrażanie aplikacji do usług IIS na komputerze deweloperskim przy użyciu tego samego procesu, który będzie potrzebny później wdrożyć w środowisku produkcyjnym. Ta metoda sprawdza procesu wdrażania oprócz sprawdzania poprawności, która aplikacja jest uruchamiana poprawnie w środowisku usług IIS.
3. Wdrażanie aplikacji do środowiska testowego, który jest możliwie jak najbardziej zbliżone do środowiska produkcyjnego. Ponieważ środowiska produkcyjnego te samouczki jest dostawcą hostingu innych firm, środowisko testowe idealne byłoby drugiego konta z dostawcą hostingu. Należy użyć tego drugiego konta tylko do celów testowych, ale będzie można ustawić w taki sam sposób jak konto produkcji.

Ten samouczek przedstawia kroki dla opcji 2. Wskazówki dotyczące opcja 3 znajduje się na końcu [wdrażania w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczek, i na końcu tego samouczka istnieją linki do zasobów dla opcji 1.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurowanie aplikacji do uruchamiania w trybie średniego zaufania

Przed zainstalowaniem usług IIS i wdrażania jej, będzie zmienić ustawienie pliku Web.config aby uruchamiało lokacji bardziej tak jak będzie ono w typowych współużytkowanego środowiska hostingu.

Dostawcy usług hosta są zazwyczaj uruchamiane witryny sieci web *trybie średniego zaufania*, co oznacza, że istnieje kilka kwestii, które nie może wykonywać. Na przykład kod aplikacji nie może uzyskać dostępu do rejestru systemu Windows i nie może odczytać lub zapisywać pliki, które znajdują się poza hierarchię folderów aplikacji. Domyślnie aplikacja działa *wysokiego zaufania* na komputerze lokalnym, co oznacza, że aplikacja będzie mógł wykonać czynności, które będą się kończyć niepowodzeniem podczas wdrażania w środowisku produkcyjnym. W związku z tym aby utworzyć środowisko testowe, które więcej dokładnie odzwierciedlał środowiska produkcyjnego, należy skonfigurować aplikację do uruchamiania w trybie średniego zaufania.

W pliku Web.config aplikacji, należy dodać **zaufania** element **system.web** element, jak pokazano w poniższym przykładzie.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Aplikacja będzie uruchamiana w trybie średniego zaufania w usługach IIS nawet na komputerze lokalnym. To ustawienie umożliwia catch możliwie jak najszybciej wielokrotnych przez kod aplikacji coś zrobić nie powiedzie się w środowisku produkcyjnym.

> [!NOTE]
> Jeśli używasz migracje Code First Framework jednostki, upewnij się, że w wersji 5.0 lub nowszej. W Entity Framework w wersji 4.3 migracje wymaga pełnego zaufania, aby można było zaktualizować schemat bazy danych.


## <a name="installing-iis-and-web-deploy"></a>Instalowanie usług IIS, jak i sieci Web wdrażania

Aby wdrożyć usług IIS na komputerze deweloperskim, musi mieć usług IIS i zainstalowane narzędzie Web Deploy. Nie są one wyświetlane w domyślnej konfiguracji systemu Windows 7. Jeśli zainstalowano już usług IIS i Web Deploy, przejdź do następnej sekcji.

Przy użyciu [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx) jest preferowany sposób instalowania usług IIS i narzędzia Web Deploy, ponieważ Instalator platformy sieci Web instaluje zalecanej konfiguracji dla usług IIS i automatycznie instaluje wymagania wstępne dotyczące usług IIS i sieci Web Wdrażanie w razie potrzeby.

Aby uruchomić Instalatora platformy sieci Web do zainstalowania usług IIS i Web Deploy, użyj następującego łącza. Jeśli już zostały zainstalowane usługi IIS, narzędzia Web Deploy lub dowolną ich wymagane składniki, Instalator platformy sieci Web instaluje tylko co to jest Brak.

- [Instalacja usług IIS i Web Deploy, przy użyciu WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Ustawianie domyślnej puli aplikacji .NET 4

Po zainstalowaniu usług IIS, należy uruchomić **Menedżera usług IIS** aby upewnić się, że .NET Framework w wersji 4 jest przypisane do domyślnej puli aplikacji.

Z systemu Windows **Start** menu, wybierz opcję **Uruchom**wprowadź "inetmgr", a następnie kliknij przycisk **OK**. (Jeśli **Uruchom** polecenia nie znajduje się w sieci **Start** menu, można nacisnąć klawisz systemu Windows i R, aby go otworzyć. Lub kliknij prawym przyciskiem myszy pasek zadań, kliknij przycisk **właściwości**, wybierz pozycję **Start Menu** , kliknij pozycję **Dostosuj**i wybierz **Uruchom polecenie**.)

W **połączeń** okienku rozwiń węzeł serwera i wybierz **pul aplikacji**. W **pul aplikacji** okienka, jeśli **DefaultAppPool** jest przypisany do programu .NET framework w wersji 4 jak na poniższej ilustracji, przejdź do następnej sekcji.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Jeśli zostanie wyświetlony tylko dwa pul aplikacji i z nich są ustawione na .NET Framework 2.0, należy zainstalować ASP.NET 4 w usługach IIS:

- Otwórz okno wiersza polecenia, klikając prawym przyciskiem myszy **wiersza polecenia** w oknach **Start** menu i wybierając **Uruchom jako Administrator**. Następnie uruchom [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) do zainstalowania programu ASP.NET 4 w usługach IIS, za pomocą następujących poleceń. (W systemach 64-bitowych, zastąp "Framework" z "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    To polecenie umożliwia utworzenie nowej puli aplikacji dla programu .NET Framework 4, ale domyślna pula aplikacji będzie nadal ustawiony 2.0. Można będzie wdrażania aplikacji którego element docelowy .NET 4 do tej puli aplikacji, więc musisz zmienić pulę aplikacji .NET 4.

Jeśli został zamknięty **Menedżera usług IIS**, uruchom go ponownie, rozwiń węzeł serwera, a następnie kliknij przycisk **pul aplikacji** do wyświetlenia **pul aplikacji** okienko ponownie.

W **pul aplikacji** okienku, kliknij przycisk **domyślna pula aplikacji**, a następnie w **akcje** okienku kliknij pozycję **podstawowe ustawienia**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

W **edytowanie puli aplikacji** okno dialogowe, zmień **.NET Framework w wersji** do **4.0.30319 .NET Framework** i kliknij przycisk **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Teraz można przystąpić do publikowania w usługach IIS.

## <a name="publishing-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka metod, które można wdrożyć przy użyciu programu Visual Studio 2010 i narzędzie Web Deploy:

- Za pomocą programu Visual Studio publikowania jednym kliknięciem.
- Utwórz *pakietu wdrożeniowego* i zainstalować go za pomocą interfejsu użytkownika Menedżera usług IIS. Pakiet wdrożeniowy, który składa się z *zip* plik, który zawiera wszystkie pliki i metadane wymagane do zainstalowania lokacji w usługach IIS.
- Utwórz pakiet wdrożeniowy i zainstalować go za pomocą wiersza polecenia.

Proces, który prowadzący w poprzednim samouczkach do instalacji programu Visual Studio do automatyzowania zadań wdrażania stosuje się do wszystkich tych trzech metod. W tych samouczkach użyjesz pierwsza z tych metod. Informacji o użyciu pakiety wdrażania znajduje się w temacie [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521.aspx).

Przed opublikowaniem, upewnij się, że używasz programu Visual Studio w trybie administratora. (W systemie Windows 7 **Start** menu, kliknij prawym przyciskiem myszy ikonę dla używanej wersji programu Visual Studio używasz i wybierz **Uruchom jako Administrator**.) Tryb administratora jest wymagana do publikowania, tylko jeśli jest publikowany w usługach IIS na komputerze lokalnym.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity (nie projekt ContosoUniversity.DAL) i wybierz **publikowania**.

**Publikowanie w sieci Web** zostanie wyświetlony Kreator.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Na liście rozwijanej wybierz  **&lt;nowy... &gt;**.

W **nowy profil** okno dialogowe, wprowadź "Test", a następnie kliknij przycisk **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Ta nazwa jest taka sama jak środkowej węzeł Web.Test.config transformacji pliku, który został utworzony wcześniej. To zgodności jest, co powoduje przekształcenia Web.Test.config ma być stosowany podczas publikowania za pomocą tego profilu.

Kreator automatycznie przechodzi do **połączenia** kartę.

W **adres URL usługi** wprowadź *localhost*.

W **witryny i aplikacji** wprowadź *domyślna witryna sieci Web/ContosoUniversity*.

W **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity`.

**Docelowy adres URL** ustawienie nie jest wymagane. Po zakończeniu pracy programu Visual Studio, wdrażanie aplikacji, automatycznie uruchomi domyślną przeglądarkę do tego adresu URL. Jeśli nie chcesz otwierać automatycznie po wdrożeniu przeglądarka, pozostaw to pole puste.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Kliknij przycisk **sprawdzania poprawności połączenia** do Sprawdź, czy ustawienia są poprawne i czy możesz nawiązać połączenie usług IIS na komputerze lokalnym.

Zielony znacznik wyboru sprawdza, czy połączenie zostanie nawiązane.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Kliknij przycisk **dalej** aby przejść do **ustawienia** kartę.

**Konfiguracji** pola rozwijanego Określa konfigurację kompilacji do wdrożenia. Wartość domyślna to zlecenia, które chcesz.

Pozostaw **Usuń dodatkowe pliki w miejscu docelowym** pole wyboru jest wyczyszczone. Ponieważ jest to pierwszy wdrożenia, nie będzie żadnych plików w folderze docelowym jeszcze.

W **baz danych** wprowadź następującą wartość w polu Parametry połączenia dla **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Proces wdrażania będzie umieścić te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

Również w obszarze **SchoolContext**, wybierz pozycję **zastosować migracje Code First**. Ta opcja powoduje, że proces wdrażania do skonfigurowania wdrożonej plik Web.config, aby określić `MigrateDatabaseToLatestVersion` inicjatora. Gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu tego inicjatora automatycznie aktualizuje bazę danych do najnowszej wersji.

W polu Parametry połączenia dla **połączenia DefaultConnection**, wprowadź następujące wartości:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Pozostaw **aktualizacji bazy danych** wyczyszczone. Bazy danych członkostwa będą wdrażane przez skopiowanie pliku .sdf w aplikacji\_danych, a nie mają procesu wdrażania, aby nic robić z tej bazy danych.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Kliknij przycisk **dalej** aby przejść do **Podgląd** kartę.

W **Podgląd** , kliknij pozycję **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Kliknij przycisk **publikowania**.

Jeśli program Visual Studio nie jest w trybie administratora, można otrzymać komunikat o błędzie, który wskazuje błąd uprawnień. W takim przypadku zamknij program Visual Studio, otwórz go w trybie administratora i spróbuj ponownie opublikować.

Jeśli program Visual Studio jest w trybie administratora, **dane wyjściowe** pomyślne raporty okna Tworzenie i publikowanie.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Przeglądarka automatycznie otwiera do strony głównej University Contoso systemem w usługach IIS na komputerze lokalnym.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testowanie w środowisku testowym

Zwróć uwagę, że wskaźnik środowisko zawiera "(Test)" zamiast "(deweloperów)", który wskazuje, że *Web.config* przekształcania wskaźnika środowisko zakończyło się pomyślnie.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Uruchom **studentów** stronę, aby sprawdzić, czy nie studentów wdrożonej bazy danych. Po wybraniu tej strony może potrwać kilka minut, aby załadować, ponieważ Code First utworzy bazę danych, a następnie uruchamia `Seed` metody. (Go nie zrobić po były na stronie głównej, ponieważ aplikacja nie spróbuj jeszcze dostępu do bazy danych.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Uruchom **instruktorów** stronę, aby sprawdzić Code First rozpoczęta w bazie danych instruktora:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Wybierz **dodać studentów** z **studentów** menu Dodaj Studenta, a następnie wyświetlić nowy uczniów w **studentów** stronę, aby sprawdzić, czy możesz pomyślnie zapisywać do bazy danych :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Z **kursów** menu, wybierz opcję **środków aktualizacji**. **Środków aktualizacji** strona musi mieć uprawnienia administratora, więc **dziennika w** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, utworzony wcześniej ("Administrator" i "Dostawcy$ w0rd"). **Środków aktualizacji** zostanie wyświetlona strona, która sprawdza, czy konto administratora, utworzony w poprzednim samouczek poprawnie została wdrożona do środowiska testowego.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Sprawdź, czy *Elmah* istnieje folder z plikiem w symbolu zastępczego w nim.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przeglądanie zmian automatyczne pliku Web.config dla migracje Code First

Otwórz *Web.config* plik w aplikacji wdrożonych na *C:\inetpub\wwwroot\ContosoUniversity* , aby zobaczyć, których proces wdrażania skonfigurowany migracje Code First, aby automatycznie Aktualizowanie bazy danych do najnowszej wersji.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Proces wdrażania również utworzyć nowe parametry połączenia dla migracje Code First użyć wyłącznie w celu zaktualizowania schematu bazy danych:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Te dodatkowe parametry można określić jedno konto użytkownika do aktualizacji schematu bazy danych, a innego konta użytkownika dla dostępu do danych aplikacji. Na przykład można przypisać bazę danych\_właściciela roli migracje Code First i db\_datareader i db\_datawriter role w aplikacji. Jest to wspólnego wzorca obrony zabezpieczeń, która blokuje potencjalnie złośliwego kodu w aplikacji zmianę schematu bazy danych. (Na przykład może się to zdarzyć w udanego ataku iniekcji kodu SQL.) Ten wzorzec nie jest używany przez te samouczki. Nie ma zastosowania do programu SQL Server Compact, a nie ma zastosowania w przypadku migracji do programu SQL Server w późniejszym samouczku z tej serii. Witryna Cytanium zawiera tylko jedno konto użytkownika do uzyskiwania dostępu do bazy danych SQL Server, które tworzysz na Cytanium. Jeśli jesteś w stanie do wykonania tego wzorca w danym scenariuszu, możesz to zrobić, wykonując następujące czynności:

1. W **ustawienia** karcie **publikowanie w sieci Web** kreatora, wprowadź ciąg połączenia, który określa użytkownika z uprawnienia do aktualizacji schematu pełnej bazy danych, a następnie wyczyść **Użyj tych parametrów połączenia w czasie wykonywania** pole wyboru. W wdrożonym pliku Web.config, staje się on `DatabasePublish` parametry połączenia.
2. Utwórz transformacji pliku Web.config dla parametrów połączenia, który chcesz aplikacji do użycia w czasie wykonywania.

Teraz wdrożeniu aplikacji usług IIS na komputerze deweloperskim i przetestować go brak. Sprawdza to proces wdrażania skopiować zawartość aplikacji do odpowiedniej lokalizacji (z wyjątkiem plików, które nie mają zostać wdrożone) i również narzędzia Web Deploy IIS poprawnie skonfigurowany podczas wdrażania. W następnym samouczku należy uruchomić jeden test więcej, wyszukującą jeszcze nie zostało zrobione zadania wdrażania: Ustawianie uprawnień folderów na *Elmah* folderu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacje dotyczące uruchamiania usług IIS lub usług IIS Express programu Visual Studio zobacz następujące zasoby:

- [Omówienie usługi IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) w witrynie IIS.net.
- [Wprowadzenie do usług IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) na blogu Scott Guthrie.
- [Porady: Określanie serwera sieci Web dla projektów sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Podstawowe różnice między usług IIS i ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie platformy ASP.NET.
- [Test z platformy ASP.NET MVC lub w aplikacji formularzy sieci Web w usługach IIS 7 w ciągu 30 sekund](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) na blogu Ricka Andersona. Ten wpis zawiera przykłady Dlaczego testowanie za pomocą serwera programu Visual Studio Programowanie (Cassini) nie jest tak niezawodna jak testowania w usługach IIS Express i dlaczego testowania w usługach IIS Express nie jest tak niezawodna jak testowania w programie IIS.

Aby uzyskać informacje na temat problemów, jakie mogą wystąpić, gdy aplikacja działa w trybie średniego zaufania, zobacz [Hosting aplikacji ASP.NET w relacji zaufania średni](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 Guys z Rolla lokacji.

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[dalej](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
