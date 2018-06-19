---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie do testu | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: dc11072e053cbddd089e5df4bcea6d2a7af864fc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890869"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie do testu
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek pokazuje, jak wdrożyć aplikację sieci web ASP.NET w usługach IIS na komputerze lokalnym.

Podczas opracowywania aplikacji zwykle testu przez uruchomienie jej w programie Visual Studio. Domyślnie projekty aplikacji sieci web w programie Visual Studio 2012 używają usług IIS Express jako serwera wdrożeniowego sieci web. Usługi IIS Express więcej działa jak usługi IIS niż serwera programu Visual Studio Programowanie (znanego także jako Cassini), która domyślnie używa programu Visual Studio 2010. Jednak ani serwera wdrożeniowego sieci web działa tak samo jak usług IIS. W związku z tym jest to możliwe, że aplikacja będzie działać poprawnie po przetestować go w programie Visual Studio, ale się nie powieść, gdy jest wdrożony w usługach IIS.

Bardziej niezawodnie przetestować aplikację w następujący sposób:

1. Wdrażanie aplikacji do usług IIS na komputerze deweloperskim przy użyciu tego samego procesu, który będzie potrzebny później wdrożyć w środowisku produkcyjnym. Można skonfigurować programu Visual Studio do używania serwera IIS, po uruchomieniu projektu sieci web, ale które nie może przetestować procesu wdrażania. Ta metoda sprawdza procesu wdrażania oprócz sprawdzania poprawności, która aplikacja jest uruchamiana poprawnie w środowisku usług IIS.
2. Wdrażanie aplikacji do środowiska testowego, który jest niemal identyczny jak w środowisku produkcyjnym. Ponieważ środowiska produkcyjnego te samouczki aplikacji sieci Web w usłudze Azure App Service, środowisko testowe idealne jest aplikacją dodatkowe sieci web utworzone w usłudze Azure App Service. Użyje tej drugiej aplikacji sieci web tylko do celów testowych, ale będzie można ustawić w taki sam sposób jak aplikacji sieci web w środowisku produkcyjnym.

Opcja 2 jest najbardziej niezawodnym sposobem sprawdzenia, a jeśli to zrobisz, który, nie zawsze trzeba opcja 1. Jednak jeśli wdrażasz opcja dostawcy hostingu innych firm 2 może nie być możliwe lub może być kosztowne, więc ta seria samouczek pokazuje obie metody. Wskazówki dotyczące opcja 2 znajduje się w [wdrażania w środowisku produkcyjnym](deploying-to-production.md) samouczka.

Aby uzyskać więcej informacji o korzystaniu z serwerów sieci web w programie Visual Studio, zobacz [serwerów sieci Web w programie Visual Studio dla projektów sieci Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).

## <a name="install-iis"></a>Zainstaluj usługi IIS

Aby wdrożyć usług IIS na komputerze deweloperskim, musi mieć usług IIS i zainstalowane narzędzie Web Deploy. Narzędzie Web Deploy jest instalowany domyślnie z programem Visual Studio, ale nie ma usługi IIS w domyślnej konfiguracji systemu Windows 8 lub Windows 7. Jeśli zainstalowano usług IIS i domyślnej puli aplikacji ustawiono już .NET 4, przejdź do [następnej sekcji](#sqlexpress).

1. Przy użyciu [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx) jest preferowany sposób instalowania usług IIS i narzędzia Web Deploy, ponieważ Instalator platformy sieci Web instaluje zalecanej konfiguracji dla usług IIS i automatycznie instaluje wymagania wstępne dotyczące usług IIS i sieci Web Wdrażanie w razie potrzeby.

    Aby uruchomić Instalatora platformy sieci Web do zainstalowania usług IIS i Web Deploy, użyj następującego łącza. Jeśli już zostały zainstalowane usługi IIS, narzędzia Web Deploy lub dowolną ich wymagane składniki, Instalator platformy sieci Web instaluje tylko co to jest Brak.

   - [Instalacja usług IIS i Web Deploy, przy użyciu WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Zostanie wyświetlone komunikaty wskazuje zainstalowanie usług IIS 7. Działania łącza dla usług IIS 8 w systemie Windows 8, ale dla systemu Windows 8, upewnij się, zainstalowanie ASP.NET 4.5, wykonując następujące czynności:

   - Otwórz **Panelu sterowania**, **programy i funkcje**, **Włącz lub wyłącz funkcje systemu Windows**.
   - Rozwiń węzeł **Internetowe usługi informacyjne**, **usługi sieci World Wide Web**, i **funkcje tworzenia aplikacji**.
   - Upewnij się, że **ASP.NET 4.5** jest zaznaczone.

      ![Wybierz funkcję ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Po zainstalowaniu usług IIS, należy uruchomić **Menedżera usług IIS** aby upewnić się, że .NET Framework w wersji 4 jest przypisane do domyślnej puli aplikacji.

1. Naciśnij klawisz systemu WINDOWS + R, aby otworzyć **Uruchom** okno dialogowe.

    (Lub w systemie Windows 8 wprowadź "Uruchom" **Start** strony lub w systemie Windows 7, wybierz **Uruchom** z **Start** menu. Jeśli **Uruchom** nie znajduje się w **Start** menu, kliknij prawym przyciskiem myszy pasek zadań, kliknij przycisk **właściwości**, wybierz pozycję **Start Menu** , kliknij pozycję **Dostosuj**i wybierz **Uruchom polecenie**.)
2. Wprowadź "inetmgr", a następnie kliknij przycisk **OK**.
3. W **połączeń** okienku rozwiń węzeł serwera i wybierz **pul aplikacji**. W **pul aplikacji** okienka, jeśli **DefaultAppPool** jest przypisany do programu .NET framework w wersji 4 jak na poniższej ilustracji, przejdź do następnej sekcji.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Jeśli zostanie wyświetlony tylko dwa pul aplikacji i z nich są ustawione na .NET Framework 2.0, należy zainstalować ASP.NET 4 w usługach IIS.

    Dla systemu Windows 8, należy zapoznać się z instrukcjami w poprzedniej sekcji, upewniając się, że program ASP.NET 4.5 jest zainstalowane, lub zobacz [w tym artykule KB](https://support.microsoft.com/kb/2736284). Dla systemu Windows 7, Otwórz okno wiersza polecenia, klikając prawym przyciskiem myszy **wiersza polecenia** w oknach **Start** menu i wybierając **Uruchom jako Administrator**. Następnie uruchom [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) do zainstalowania programu ASP.NET 4 w usługach IIS, za pomocą następujących poleceń. (W 32-bitowym, zastąp "Framework64" z "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    To polecenie umożliwia utworzenie nowej puli aplikacji dla programu .NET Framework 4, ale domyślna pula aplikacji będzie nadal ustawiony 2.0. Można będzie wdrażania aplikacji którego element docelowy .NET 4 do tej puli aplikacji, więc musisz zmienić pulę aplikacji .NET 4.
5. Jeśli został zamknięty **Menedżera usług IIS**, uruchom go ponownie, rozwiń węzeł serwera, a następnie kliknij przycisk **pul aplikacji** do wyświetlenia **pul aplikacji** okienko ponownie.
6. W **pul aplikacji** okienku, kliknij przycisk **domyślna pula aplikacji**, a następnie w **akcje** okienku kliknij pozycję **podstawowe ustawienia**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. W **edytowanie puli aplikacji** okno dialogowe, zmień **.NET Framework w wersji** do **4.0.30319 .NET Framework** i kliknij przycisk **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

Usługi IIS jest teraz gotowe do publikowania aplikacji sieci web do niego, ale przed możesz to zrobić, należy utworzyć baz danych, które będą używane w środowisku testowym.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Zainstaluj program SQL Server Express

LocalDB nie jest przeznaczony do pracy w usługach IIS, więc dla danego środowiska testowego należy zainstalować programu SQL Server Express. Jeśli używasz programu Visual Studio 2010 programu SQL Server Express jest już zainstalowana domyślnie. Jeśli używasz programu Visual Studio 2012, należy go zainstalować.

Aby zainstalować program SQL Server Express, zainstaluj ją z [Centrum pobierania: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) klikając [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) lub [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Po wybraniu niewłaściwy systemu nie będzie można go zainstalować i spróbować jeden z nich.

Na pierwszej stronie Centrum instalacji programu SQL Server, kliknij przycisk **nowy serwer SQL instalacji autonomicznej lub Dodaj funkcje do istniejącej instalacji**i postępuj zgodnie z instrukcjami, akceptując ustawień domyślnych. W Kreatorze instalacji Zaakceptuj ustawienia domyślne. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [Instalowanie programu SQL Server 2012 z poziomu Kreatora instalacji (Instalator)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Tworzenie programu SQL Server Express baz danych dla środowiska testowego

Aplikacja Contoso University ma dwie bazy danych: baza danych członkostwa i bazy danych aplikacji. Te bazy danych można wdrożyć do dwóch oddzielnych baz danych lub do pojedynczej bazy danych. Możesz połączyć je w celu umożliwienia sprzężenia bazy danych między bazy danych aplikacji i członkostwa w bazie danych. Jeśli wdrażasz do innego dostawcy hostingu, plan hostingu mogą również podać przyczynę łączyć je. Na przykład dostawca hostingu może pobierać jednego dla wielu baz danych lub może nie pozwalać nawet więcej niż jednej bazy danych.

W tym samouczku będziesz wdrożyć do dwóch baz danych w środowisku testowym i jedną bazę danych w środowisku przemieszczania i produkcji.

Z **widoku** menu w programie Visual Studio wybierz **Eksploratora serwera** (**Eksploratora bazy danych** w Visual Web Developer), a następnie kliknij prawym przyciskiem myszy **połączenia danych**  i wybierz **Tworzenie nowej bazy danych SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

W **Tworzenie nowej bazy danych SQL Server** okna dialogowego wprowadź ". \SQLExpress" w **nazwy serwera** pole i "aspnet-ContosoUniversity" w **nowej nazwy bazy danych** polu następnie Kliknij przycisk **OK**.

![Utwórz aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Postępuj zgodnie z tą samą procedurą, aby utworzyć nową bazę danych programu SQL Server Express służbowe o nazwie "ContosoUniversity".

**W Eksploratorze serwera** zawiera teraz dwa nowe bazy danych.

![Nowe bazy danych w Eksploratorze serwera](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Utwórz skrypt grant dla nowych baz danych

Po uruchomieniu aplikacji w usługach IIS na komputerze dewelopera aplikacji dostęp do bazy danych przy użyciu poświadczeń domyślnej puli aplikacji. Jednak domyślnie tożsamość puli aplikacji nie ma uprawnień do otwierania bazy danych. Dlatego należy uruchomić skrypt, aby przyznać uprawnienie. W tej sekcji utworzysz skryptu, że uruchomisz później, aby upewnić się, że aplikacji można otworzyć bazy danych po uruchomieniu w usługach IIS.

Kliknij prawym przyciskiem myszy rozwiązanie (nie jeden z projektów), a następnie kliknij przycisk **Dodaj nowy element**, a następnie utwórz nową **pliku SQL** o nazwie *Grant.sql*. Skopiuj następujące polecenia SQL do pliku, a następnie zapisz i zamknij plik:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Ten skrypt jest przeznaczona do pracy z programu SQL Server Express 2012 i ustawienia programu IIS w systemie Windows 8 lub Windows 7, są one określone w tym samouczku. Jeśli używasz innej wersji programu SQL Server lub systemu Windows lub skonfigurowanie usług IIS na komputerze inaczej, zmiany w tym skrypcie może być wymagane. Aby uzyskać więcej informacji na temat skryptów programu SQL Server, zobacz [programu SQL Server — książki Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** ten skrypt podaje db\_uprawnień właściciela na użytkownika, który uzyskuje dostęp do bazy danych w czasie wykonywania, czyli, należy w środowisku produkcyjnym. W niektórych scenariuszach można określić użytkownika, który ma schematu pełnej bazy danych zaktualizować uprawnienia tylko do wdrożenia i określ czas uruchomienia innego użytkownika z uprawnieniami tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz [przeglądania automatyczne zmian pliku Web.config dla migracje Code First](#reviewingmigrations) dalszej części tego samouczka.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uruchom skrypt grant w bazie danych aplikacji

Można skonfigurować profil publikowania, aby uruchomić skrypt grant w bazie danych członkostwa podczas wdrażania, ponieważ korzysta z tego wdrożenia bazy danych dostawcy dbDacFx narzędzia. Nie można uruchamiać skrypty, podczas wdrażania migracje Code First, czyli sposób wdrażana bazy danych aplikacji. W związku z tym należy ręcznie uruchomić skrypt przed wdrożeniem w bazie danych aplikacji.

1. W programie Visual Studio Otwórz *Grant.sql* pliku, który został utworzony wcześniej.
2. Kliknij przycisk **Połącz**. 

    ![Przycisk Połącz](deploying-to-iis/_static/image11.png)
3. W **Połącz z serwerem** okna dialogowego wprowadź *. \SQLExpress* jako **nazwy serwera**, a następnie kliknij przycisk **Connect**.
4. Wybierz z listy rozwijanej bazy danych **ContosoUniversity**, a następnie kliknij przycisk **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

Tożsamość puli aplikacji domyślne teraz ma wystarczające uprawnienia w bazie danych aplikacji migracje Code First do tworzenia tabel bazy danych, po uruchomieniu aplikacji.

## <a name="publish-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka sposobów, którą można wdrożyć do usług IIS przy użyciu programu Visual Studio i Web Deploy:

- Za pomocą programu Visual Studio publikowania jednym kliknięciem.
- Publikowanie z wiersza polecenia.
- Utwórz *pakietu wdrożeniowego* i zainstalować go za pomocą interfejsu użytkownika Menedżera usług IIS. Pakiet wdrożeniowy, który składa się z *zip* plik, który zawiera wszystkie pliki i metadane wymagane do zainstalowania lokacji w usługach IIS.
- Utwórz pakiet wdrożeniowy i zainstalować go za pomocą wiersza polecenia.

Proces, który prowadzący w poprzednim samouczkach do instalacji programu Visual Studio do automatyzowania zadań wdrażania stosuje się do wszystkich tych metod. W tych samouczkach użyjesz dwa pierwsze z tych metod. Informacji o użyciu pakiety wdrażania znajduje się w temacie [wdrażanie aplikacji sieci web, tworząc i instalowanie pakietu wdrożeniowego sieci web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) w Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio.

Przed opublikowaniem, upewnij się, że używasz programu Visual Studio w trybie administratora. Jeśli nie widzisz **(Administrator)** na pasku tytułu Zamknij program Visual Studio. W systemie Windows 8 **Start** strony lub Windows 7 **Start** menu, kliknij prawym przyciskiem myszy ikonę dla używanej wersji programu Visual Studio używasz i wybierz **Uruchom jako Administrator**. Tryb administratora jest wymagana do publikowania, tylko jeśli jest publikowany w usługach IIS na komputerze lokalnym.

### <a name="create-the-publish-profile"></a>Tworzenie profilu publikowania

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity (nie projekt ContosoUniversity.DAL) i wybierz **publikowania**.

    **Publikowanie w sieci Web** zostanie wyświetlony Kreator.

    ![Publikowanie karta Profil kreatora sieci Web](deploying-to-iis/_static/image13.png)
2. Na liście rozwijanej wybierz  **&lt;nowy... &gt;**. (Z zainstalowaną najnowszą aktualizacją Visual Studio nie ma listy rozwijanej, a przycisk, aby kliknij polecenie, aby utworzyć nowy profil od podstaw jest **niestandardowy**.)
3. W **nowy profil** okno dialogowe, wprowadź "Test", a następnie kliknij przycisk **OK**.

    Kreator automatycznie przechodzi do **połączenia** kartę.
4. W **adres URL usługi** wprowadź *localhost*.
5. W **witryny i aplikacji** wprowadź *domyślna witryna sieci Web/ContosoUniversity*
6. W **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity`

    **Docelowy adres URL** ustawienie nie jest wymagane. Po zakończeniu pracy programu Visual Studio, wdrażanie aplikacji, automatycznie uruchomi domyślną przeglądarkę do tego adresu URL. Jeśli nie chcesz otwierać automatycznie po wdrożeniu przeglądarka, pozostaw to pole puste.
7. Kliknij przycisk **sprawdzania poprawności połączenia** do Sprawdź, czy ustawienia są poprawne i czy możesz nawiązać połączenie usług IIS na komputerze lokalnym.

    Zielony znacznik wyboru sprawdza, czy połączenie zostanie nawiązane.

    ![Publikowanie na karcie Połączenie Kreatora sieci Web](deploying-to-iis/_static/image14.png)
8. Kliknij przycisk **dalej** aby przejść do **ustawienia** kartę.
9. **Konfiguracji** pola rozwijanego Określa konfigurację kompilacji do wdrożenia. Pozostaw ustawioną domyślną wartość wersji. Kompilacje debugowania nie wdrażania w tym samouczku.
10. Rozwiń węzeł **opcji publikowania pliku**, a następnie wybierz **Wyklucz pliki z aplikacji\_folderu danych**.

    W środowisku testowym aplikacji będą uzyskiwać dostęp do bazy danych, które zostały utworzone w lokalnym wystąpieniu programu SQL Server Express nie .mdf plików *aplikacji\_danych* folderu.
11. Pozostaw **Precompile podczas publikowania** i **Usuń dodatkowe pliki w miejscu docelowym** zaznaczaj pól wyboru.

    ![Opcji publikowania pliku na karcie Ustawienia](deploying-to-iis/_static/image15.png)

    Prekompilowanie to opcja, która jest przydatna szczególnie w przypadku bardzo dużych witryn. można go skrócić czas uruchamiania strony po raz pierwszy żądanej strony po opublikowaniu lokacji.

    Nie trzeba usunąć dodatkowe pliki, ponieważ jest to pierwszy wdrożenia i nie będzie żadnych plików w folderze docelowym jeszcze.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > W przypadku wybrania **Usuń dodatkowe pliki** kolejne wdrożenia do tej samej lokacji, upewnij się, że używana jest funkcja podglądu, dzięki czemu wcześniej zobaczyć pliki, które zostaną usunięte przed wdrożeniem. Oczekiwane zachowanie jest, że narzędzia Web Deploy spowoduje usunięcie plików na serwerze docelowym, które zostały usunięte w projekcie. Jednak struktury cały folder, w obszarze folder źródłowy i docelowy są porównywane, a w niektórych scenariuszach wdrażania w sieci Web może usunąć pliki, których nie chcesz, aby usunąć.
    > 
    > Na przykład jeśli masz aplikację sieci web w podfolderze na serwerze podczas wdrażania projektu do folderu głównego, podfolderu zostaną usunięte. Może istnieć jeden projekt dla lokacji głównej w contoso.com oraz innego projektu w blogu w contoso.com/blog. Blog aplikacji znajduje się w podfolderze. W przypadku wybrania Usuń dodatkowe pliki w miejscu docelowym podczas wdrażania lokacji głównej, blog aplikacji zostaną usunięte.
    > 
    > Innym przykładem aplikacji\_folderu danych może zostać usunięty nieoczekiwanie. Niektórych baz danych, takich jak SQL Server Compact przechowywane pliki bazy danych w aplikacji\_folderem danych. Po początkowym wdrożeniu nie chcesz przechowywać kopiowanie plików bazy danych w przypadku kolejnych wdrożeń, więc wybrać wykluczanie aplikacji\_danych na karcie pakowanie/publikowanie w sieci Web. Po można to zrobić, jeśli masz Usuń dodatkowe pliki w miejscu docelowym wybrana, pliki bazy danych i aplikacji\_sam folder dane zostaną usunięte przy następnym publikowania.

### <a name="configure-deployment-for-the-membership-database"></a>Konfigurowanie wdrożenia dla bazy danych członkostwa

Poniższa procedura dotyczy **połączenia DefaultConnection** bazy danych w **baz danych** części okna dialogowego.

1. W **ciąg połączenia zdalnego** wprowadź następujący ciąg połączenia, wskazujące nowego programu SQL Server Express bazie danych członkostwa.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Proces wdrażania będzie umieścić te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

    Można także uzyskać ciąg połączenia z **Eksploratora serwera**. W **Eksploratora serwera**, rozwiń węzeł **połączenia danych** i wybierz  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Baza danych, następnie z **właściwości** okno kopiowania **ciąg połączenia** wartość. Że parametry połączenia jednego ustawienia dodatkowe, które można usunąć: `Pooling=False`.
2. Wybierz **aktualizacji bazy danych**.

    To spowoduje, że schemat bazy danych zostały utworzone w docelowej bazie danych podczas wdrażania. W poniższych krokach Określ dodatkowe skrypty, które należy uruchomić: jedna, aby przyznać dostęp do domyślnej puli aplikacji i jeden do wdrażania danych bazy danych.
3. Kliknij przycisk **skonfigurować aktualizacje bazy danych**.
4. W **Konfigurowanie aktualizacji bazy danych** okno dialogowe, kliknij przycisk **Dodaj skrypt SQL** , a następnie przejdź do *Grant.sql* skrypt, który został wcześniej zapisany w folderze rozwiązania.
5. Powtórz proces dodawania *aspnet-data-dev.sql* skryptu.

    ![Konfigurowanie aktualizacji bazy danych dla bazy danych członkostwa](deploying-to-iis/_static/image16.png)
6. Kliknij przycisk **Zamknij**.

### <a name="configure-deployment-for-the-application-database"></a>Konfigurowanie wdrożenia dla aplikacji bazy danych

Jeśli program Visual Studio wykryje Entity Framework `DbContext` klasy, tworzy wpis w **baz danych** sekcja, która ma **wykonaj migracje Code First** pole wyboru, a nie  **Aktualizacja bazy danych** pole wyboru. W tym samouczku będziesz Użyj tego pola wyboru, aby określić wdrożenia migracje Code First.

W niektórych scenariuszach może być używany `DbContext` bazy danych, ale chcesz użyć dostawcy dbDacFx zamiast migracji w celu wdrażania bazy danych. W takim przypadku zobacz [sposób wdrażania Code First bazy danych bez migracje?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) w FAQ wdrożenia sieci Web ASP.NET w witrynie MSDN.

Poniższa procedura dotyczy **SchoolContext** bazy danych w **baz danych** części okna dialogowego.

1. W **ciąg połączenia zdalnego** wprowadź następujący ciąg połączenia, wskazujące nowego programu SQL Server Express bazy danych aplikacji.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Proces wdrażania będzie umieścić te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

    Można także uzyskać ciąg połączenia bazy danych aplikacji z **Eksploratora serwera** parametry połączenia bazy danych tak samo otrzymano członkostwa.
2. Wybierz **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**.

    Ta opcja powoduje, że proces wdrażania do skonfigurowania wdrożonej plik Web.config, aby określić `MigrateDatabaseToLatestVersion` inicjatora. Gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu tego inicjatora automatycznie aktualizuje bazę danych do najnowszej wersji.

### <a name="configure-publish-profile-transforms"></a>Skonfiguruj publikowanie transformacje profilu

1. Kliknij przycisk **Zamknij**, a następnie kliknij przycisk **tak** po zapyta, czy chcesz zapisać zmiany.
2. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości**, rozwiń węzeł **PublishProfiles**.
3. Kliknij przycisk Rright *Test.pubxml,* , a następnie kliknij przycisk **dodać przekształcenie konfiguracji**.

    ![Dodawanie menu Przekształcenie konfiguracji](deploying-to-iis/_static/image17.png)

    Program Visual Studio tworzy *Web.Test.config* pliku transformacji i otwarcie go.
4. W *Web.Test.config* pliku przekształcenia, wstaw poniższy kod bezpośrednio po otwarciu tagu konfiguracyjnym.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Gdy używasz testu profil publikowania, tej transformacji ustawia wskaźnik środowiska, aby "Test". W witrynie wdrożonej zobaczysz "(Test)" po nagłówku H1 "Contoso University".
5. Zapisz i zamknij plik.
6. Kliknij prawym przyciskiem myszy *Web.Test.config* plik i kliknij przycisk **przekształcenie Podgląd** aby upewnić się, że transformacji zostanie na stałe tworzy oczekiwanych zmian.

    **Podgląd pliku Web.config** okna przedstawia wynik zastosowania zarówno *Web.Release.config* przekształca i *Web.Test.config* przekształca.

### <a name="preview-the-deployment-updates"></a>Wyświetl podgląd aktualizacji wdrożenia

1. Otwórz **publikowanie w sieci Web** Kreator ponownie (kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij przycisk **publikowania**).
2. W **Podgląd** karcie, upewnij się, że **testu** profil jest nadal wybrany, a następnie kliknij przycisk **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane.

    ![Przycisk podglądu](deploying-to-iis/_static/image18.png)

    ![Wyświetlić podglądu publikowania](deploying-to-iis/_static/image19.png)

    Możesz również kliknąć **bazy danych w wersji zapoznawczej** łącze, aby wyświetlić skrypty, które będą uruchamiane w bazie danych członkostwa. (Nie skrypty są uruchamiane w przypadku wdrożenia migracje Code First, nie ma nic do podglądu dla bazy danych aplikacji.)
3. Kliknij przycisk **publikowania**.

    Jeśli program Visual Studio nie jest w trybie administratora, można otrzymać komunikat o błędzie, który wskazuje błąd uprawnień. W takim przypadku zamknij program Visual Studio, otwórz go w trybie administratora i spróbuj ponownie opublikować.

    Jeśli program Visual Studio jest w trybie administratora, **dane wyjściowe** pomyślne raporty okna Tworzenie i publikowanie.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Jeśli wprowadzony adres URL w **docelowy adres URL** pole na profil publikowania **połączenia** karcie przeglądarki automatycznie otwiera do strony głównej University Contoso systemem w usługach IIS na komputerze lokalnym.

## <a name="test-in-the-test-environment"></a>Testowanie w środowisku testowym

Zwróć uwagę, że wskaźnik środowisko zawiera "(Test)" zamiast "(deweloperów)", który wskazuje, że *Web.config* przekształcania wskaźnika środowisko zakończyło się pomyślnie.

Uruchom **instruktorów** stronę, aby sprawdzić Code First rozpoczęta w bazie danych instruktora. Po wybraniu tej strony może potrwać kilka minut, aby załadować, ponieważ Code First utworzy bazę danych, a następnie uruchamia `Seed` metody. (Go nie zrobić po były na stronie głównej, ponieważ aplikacja nie spróbuj jeszcze dostępu do bazy danych.)

Kliknij przycisk **studentów** kartę, aby sprawdzić, czy nie studentów wdrożonej bazy danych.

Wybierz **dodać studentów** z **studentów** menu Dodaj Studenta, a następnie wyświetlić nowy uczniów w **studentów** stronę, aby sprawdzić, czy możesz pomyślnie zapisywać do bazy danych .

Z **kursów** menu, wybierz opcję **środków aktualizacji**. **Środków aktualizacji** strona musi mieć uprawnienia administratora, więc **dziennika w** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, które zostało utworzone wcześniej ("Administrator" i "devpwd"). **Środków aktualizacji** zostanie wyświetlona strona, która sprawdza, czy konto administratora, utworzony w poprzednim samouczek poprawnie została wdrożona do środowiska testowego.

Sprawdź, czy *Elmah* folder istnieje w *c:\inetpub\wwwroot\ContosoUniversity* folder z plikiem w symbolu zastępczego programu.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przejrzyj automatycznej zmiany pliku Web.config dla migracje Code First

Otwórz *Web.config* plik w aplikacji wdrożonych na *C:\inetpub\wwwroot\ContosoUniversity* , aby zobaczyć, których proces wdrażania skonfigurowany migracje Code First, aby automatycznie Aktualizowanie bazy danych do najnowszej wersji.

![](deploying-to-iis/_static/image21.png)

Proces wdrażania również utworzyć nowe parametry połączenia dla migracje Code First użyć wyłącznie w celu zaktualizowania schematu bazy danych:

![Parametry połączenia Database_Publish](deploying-to-iis/_static/image22.png)

Te dodatkowe parametry można określić jedno konto użytkownika do aktualizacji schematu bazy danych, a innego konta użytkownika dla dostępu do danych aplikacji. Na przykład można przypisać **db\_właściciela** rolę migracje Code First, i **db\_datareader** i **db\_datawriter**role w aplikacji. Jest to wspólnego wzorca obrony zabezpieczeń, która blokuje potencjalnie złośliwego kodu w aplikacji zmianę schematu bazy danych. (Na przykład może się to zdarzyć w udanego ataku iniekcji kodu SQL.) Ten wzorzec nie jest używany przez te samouczki. Jeśli chcesz wdrożyć ten wzorzec w danym scenariuszu, możesz to zrobić, wykonując następujące czynności:

1. W **ustawienia** karcie **publikowanie w sieci Web** kreatora, wprowadź ciąg połączenia, który określa użytkownika z uprawnienia do aktualizacji schematu pełnej bazy danych, a następnie wyczyść **Użyj tych parametrów połączenia w czasie wykonywania** pole wyboru. W wdrożonym pliku Web.config, staje się on `DatabasePublish` parametry połączenia.
2. Utwórz transformacji pliku Web.config dla parametrów połączenia, który chcesz aplikacji do użycia w czasie wykonywania.

## <a name="summary"></a>Podsumowanie

Teraz wdrożeniu aplikacji usług IIS na komputerze deweloperskim i przetestować go brak.

![Strona główna w teście](deploying-to-iis/_static/image23.png)

Sprawdza to proces wdrażania skopiować zawartość aplikacji do odpowiedniej lokalizacji (z wyjątkiem plików, które nie mają zostać wdrożone) i również narzędzia Web Deploy IIS poprawnie skonfigurowany podczas wdrażania. W następnym samouczku należy uruchomić jeden test więcej, wyszukującą jeszcze nie zostało zrobione zadania wdrażania: Ustawianie uprawnień folderów na *Elmah* folderu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacje dotyczące uruchamiania usług IIS lub usług IIS Express programu Visual Studio zobacz następujące zasoby:

- [Omówienie usługi IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) w witrynie IIS.net.
- [Wprowadzenie do usług IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) na blogu Scott Guthrie.
- [Serwery w sieci Web w programie Visual Studio dla projektów sieci Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Podstawowe różnice między usług IIS i ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie platformy ASP.NET.

Aby uzyskać informacje na temat problemów, jakie mogą wystąpić, gdy aplikacja działa w trybie średniego zaufania, zobacz [Hosting aplikacji ASP.NET w relacji zaufania średni](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 Guys z Rolla lokacji.

> [!div class="step-by-step"]
> [Poprzednie](project-properties.md)
> [dalej](setting-folder-permissions.md)
