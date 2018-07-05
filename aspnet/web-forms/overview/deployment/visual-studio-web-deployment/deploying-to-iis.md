---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie w środowisku testowym | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 8c5034dd4948d96c5722e2dcc960cc0349241a1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365688"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie w środowisku testowym
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób wdrażania aplikacji sieci web platformy ASP.NET w usługach IIS na komputerze lokalnym.

Podczas opracowywania aplikacji zwykle testu przez uruchomienie jej w programie Visual Studio. Domyślnie projekty aplikacji sieci web w programie Visual Studio 2012 użyć usług IIS Express jako serwera wdrożeniowego sieci web. Usługi IIS Express zachowuje się bardziej jak usługi IIS niż serwera programu Visual Studio Development (znanego także jako Cassini), która domyślnie używa programu Visual Studio 2010. Ale ani serwera wdrożeniowego sieci web działa tak samo jak usługi IIS. W wyniku jest możliwe, że aplikacja będzie działać poprawnie po ją przetestować w programie Visual Studio, ale się nie powieść, gdy aplikacja jest wdrożona w usługach IIS.

Bardziej niezawodnie przetestować aplikację w następujący sposób:

1. Wdrażanie aplikacji w usługach IIS na komputerze deweloperskim, za pomocą tego samego procesu, którego będziesz później wdrożyć w środowisku produkcyjnym. Można skonfigurować programu Visual Studio do używania usług IIS, gdy uruchamiasz projekt sieci web, ale tych czynności nie będzie przetestować proces wdrażania. Ta metoda sprawdza proces wdrażania oprócz sprawdzania poprawności, aplikacja będzie działać poprawnie w środowisku usług IIS.
2. Wdrażanie aplikacji w środowisku testowym, który jest niemal identyczny jak w środowisku produkcyjnym. Ponieważ środowiska produkcyjnego potrzeby tych samouczków aplikacji sieci Web w usłudze Azure App Service, środowisko testowe idealnym rozwiązaniem jest dodatkowe utworzona aplikacja sieci web w usłudze Azure App Service. Należy użyć tej drugiej aplikacji sieci web tylko do celów testowych, ale będzie można skonfigurować taki sam sposób jak w aplikacji sieci web w środowisku produkcyjnym.

Opcja 2 jest najbardziej niezawodnym sposobem, aby przetestować, a jeśli to zrobisz, nie koniecznie trzeba opcja 1. Jednak jeśli wdrażasz do innych opcji dostawcy hostingu 2 może nie być możliwe, lub może być kosztowne, dzięki czemu tej serii samouczków opisano obie metody. Wskazówki dotyczące opcji 2 znajduje się w [wdrażanie w środowisku produkcyjnym](deploying-to-production.md) samouczka.

Aby uzyskać więcej informacji o korzystaniu z serwerów sieci web w programie Visual Studio, zobacz [serwerów sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="install-iis"></a>Instalowanie usług IIS

Aby wdrożyć w usługach IIS na komputerze deweloperskim, konieczne jest posiadanie usług IIS i zainstalowane narzędzie Web Deploy. Narzędzie Web Deploy jest instalowany domyślnie z programem Visual Studio, ale usługi IIS nie znajduje się w domyślnej konfiguracji systemu Windows 8 lub Windows 7. Jeśli zainstalowano już program IIS i domyślnej puli aplikacji jest już ustawione w .NET 4, przejdź do [następnej sekcji](#sqlexpress).

1. Za pomocą [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx) jest preferowanym sposobem instalowania programu IIS i narzędzia Web Deploy, ponieważ Instalator platformy sieci Web instaluje to zalecana konfiguracja usług IIS i automatycznie instaluje wymagania wstępne dotyczące sieci Web i usług IIS Wdrażanie, jeśli to konieczne.

    Aby uruchomić Instalatora platformy sieci Web, aby zainstalować usługi IIS i narzędzia Web Deploy, użyj następującego linku. Jeśli została już zainstalowana IIS, narzędzie Web Deploy lub dowolnej ich wymagane składniki, Instalator platformy sieci Web instaluje tylko co to jest Brak.

   - [Instalowanie usług IIS i narzędzia Web Deploy, za pomocą Instalatora WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Zostaną wyświetlone komunikaty wskazujące, czy zostaną zainstalowane usługi IIS 7. Działa link dla usług IIS 8 w systemie Windows 8, ale dla systemu Windows 8 upewnij się, że ASP.NET 4.5 jest zainstalowany, wykonując następujące czynności:

   - Otwórz **Panelu sterowania**, **programy i funkcje**, **Windows Włącz lub wyłącz funkcje**.
   - Rozwiń **Internetowe usługi informacyjne**, **usługi World Wide Web**, i **funkcje tworzenia aplikacji**.
   - Upewnij się, że **ASP.NET 4.5** jest zaznaczone.

      ![Wybierz pozycję ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Po zainstalowaniu usług IIS, należy uruchomić **Menedżera usług IIS** aby upewnić się, że .NET Framework w wersji 4 są przypisane do domyślnej puli aplikacji.

1. Naciśnij klawisze WINDOWS + R, aby otworzyć **Uruchom** okno dialogowe.

    (Lub w systemie Windows 8 wprowadź "Uruchom" **Start** strony lub Windows 7 wybierz **Uruchom** z **Start** menu. Jeśli **Uruchom** nie znajduje się w **Start** menu, kliknij prawym przyciskiem myszy pasek zadań, kliknij przycisk **właściwości**, wybierz opcję **Start Menu** kliknij **Dostosuj**i wybierz **Uruchom polecenie**.)
2. Wprowadź "inetmgr", a następnie kliknij przycisk **OK**.
3. W **połączeń** okienku rozwiń węzeł serwera i wybierz **pul aplikacji**. W **pul aplikacji** okienko, jeśli **DefaultAppPool** jest przypisany do programu .NET framework w wersji 4 tak jak na poniższej ilustracji, przejdź do następnej sekcji.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Jeśli zostanie wyświetlony tylko dwie pule aplikacji i z nich są ustawione na .NET Framework 2.0, musisz zainstalować platformy ASP.NET 4 w usługach IIS.

    Dla systemu Windows 8, zobacz instrukcje w poprzedniej sekcji, upewniając się, że program ASP.NET 4.5 jest zainstalowane, lub zobacz [w tym artykule KB](https://support.microsoft.com/kb/2736284). Windows 7, Otwórz okno wiersza polecenia, klikając prawym przyciskiem myszy **polecenia** w Windows **Start** menu i wybierając polecenie **Uruchom jako Administrator**. Następnie uruchom [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) do zainstalowania programu ASP.NET 4 w usługach IIS, używając następujących poleceń. (W systemach 32-bitowych, zamiast "Framework64" z "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    To polecenie umożliwia utworzenie nowej puli aplikacji dla programu .NET Framework 4, ale domyślnej puli aplikacji będzie nadal ustawiony w wersji 2.0. Możesz będzie można wdrażanie aplikacji przeznaczonego .NET 4 dla tej puli aplikacji, więc trzeba będzie zmienić pulę aplikacji .NET 4.
5. Jeśli został zamknięty **Menedżera usług IIS**, uruchom ją ponownie, rozwiń węzeł serwera, a następnie kliknij przycisk **pul aplikacji** do wyświetlenia **pul aplikacji** okienko ponownie.
6. W **pul aplikacji** okienku kliknij **DefaultAppPool**, a następnie w polu **akcje** okienku kliknij pozycję **podstawowych ustawień**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. W **edytowanie puli aplikacji** okno dialogowe, zmiana **.NET Framework w wersji** do **4.0.30319 .NET Framework** i kliknij przycisk **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

Usługi IIS jest teraz gotowy do publikowania aplikacji sieci web do niego, ale przed możesz to zrobić, należy utworzyć bazy danych, które będą używane w środowisku testowym.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Zainstaluj program SQL Server Express

LocalDB nie jest przeznaczony do pracy w usługach IIS, dlatego dla danego środowiska testowego należy zainstalować SQL Server Express. Jeśli używasz programu Visual Studio 2010 programu SQL Server Express jest już zainstalowana domyślnie. Jeśli używasz programu Visual Studio 2012, należy go zainstalować.

Aby zainstalować program SQL Server Express, zainstaluj go z [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) , klikając [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) lub [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Jeśli wybierzesz niewłaściwy systemu zakończy się niepowodzeniem do zainstalowania i wypróbować jeden z nich.

Na pierwszej stronie Centrum instalacji programu SQL Server, kliknij przycisk **nowy serwer SQL instalacji autonomicznej lub Dodaj funkcje do istniejącej instalacji**i postępuj zgodnie z instrukcjami, przyjmuje opcje domyślne. W Kreatorze instalacji Zaakceptuj ustawienia domyślne. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [Instalowanie programu SQL Server 2012 za pomocą Kreatora instalacji (Instalatora)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Tworzenie programu SQL Server Express bazy danych dla środowiska testowego

Aplikacja Contoso University ma dwie bazy danych: bazy danych członkostwa i bazy danych aplikacji. Można wdrożyć tych baz danych, dwie osobne bazy danych lub pojedynczą bazę danych. Możesz chcieć połączyć je w celu ułatwienia sprzężenia bazy danych między bazy danych aplikacji i bazy danych członkostwa. Jeśli wdrażasz do dostawcy hostingu innych firm, plan hostingu może również podać przyczynę do ich łączenia. Na przykład dostawcy hostingu mogą pobierać tylko dla wielu baz danych lub może nawet zezwala na więcej niż jednej bazy danych.

W tym samouczku wdrożysz dwie bazy danych w środowisku testowym i jedna baza danych w środowiskach przejściowych i produkcyjnych.

Z **widoku** menu w programie Visual Studio wybierz **Eksploratora serwera** (**Eksplorator bazy danych** w Visual Web Developer), a następnie kliknij prawym przyciskiem myszy **połączeń danych**  i wybierz **Tworzenie nowej bazy danych SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

W **Tworzenie nowej bazy danych SQL Server** okna dialogowego wprowadź ". \SQLExpress" w **nazwy serwera** pole i "aspnet-ContosoUniversity" w **nazwę nowej bazy danych** polu następnie Kliknij przycisk **OK**.

![Utwórz aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Postępuj zgodnie z tą samą procedurą, aby utworzyć nową bazę danych programu SQL Server Express School o nazwie "ContosoUniversity".

**Eksplorator serwera** pojawi się na dwóch nowych baz danych.

![Nowe bazy danych w Eksploratorze serwera](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Utwórz skrypt grant dla nowych baz danych

Po uruchomieniu aplikacji w usługach IIS na komputerze deweloperskim w aplikacji uzyskuje dostęp do bazy danych przy użyciu poświadczeń domyślnej puli aplikacji. Jednak domyślnie tożsamość puli aplikacji nie ma uprawnień do otwierania bazy danych. Dlatego należy uruchomić skrypt, aby przyznać to uprawnienie. W tej sekcji utworzysz skrypt, uruchomisz nowszej, aby upewnić się, że aplikacja będzie mógł otworzyć baz danych po uruchomieniu w usługach IIS.

Kliknij prawym przyciskiem myszy rozwiązanie (nie jednego z projektów), a następnie kliknij przycisk **Dodaj nowy element**, a następnie utwórz nowy **plik SQL** o nazwie *Grant.sql*. Skopiuj poniższe polecenia SQL do pliku, a następnie zapisz i zamknij plik:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Ten skrypt jest przeznaczony do pracy z programem SQL Server Express 2012 i ustawienia programu IIS w systemie Windows 8 lub Windows 7, ponieważ są one określone w tym samouczku. Jeśli używasz innej wersji programu SQL Server lub Windows, czy należy zainstalować program IIS na komputerze inaczej, zmiany w tym skrypcie może być wymagane. Aby uzyskać więcej informacji na temat skrypty programu SQL Server, zobacz [programu SQL Server — książki Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** ten skrypt umożliwia db\_uprawnienia właściciela do użytkownika, który uzyskuje dostęp do bazy danych w czasie wykonywania, który jest uzyskasz w środowisku produkcyjnym. W niektórych scenariuszach możesz chcieć określić użytkownika, która ma schemat pełnej bazy danych, zaktualizować uprawnienia tylko do wdrożenia i określ czas wykonywania inny użytkownik z uprawnieniami tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz [przeglądanie automatyczne zmiany w pliku Web.config na potrzeby migracji Code First](#reviewingmigrations) później w tym samouczku.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uruchom skrypt przydział w bazie danych aplikacji

Można skonfigurować profilu publikowania do uruchomienia skryptu przydział w członkowskiej bazie danych podczas wdrażania, ponieważ dostawcy dbDacFx korzysta z tego wdrożenia bazy danych. Nie można uruchamiać skrypty w trakcie wdrażania migracje Code First jest sposób wdrażania aplikacji bazy danych. W związku z tym należy ręcznie uruchomić skrypt przed wdrożeniem w bazie danych aplikacji.

1. W programie Visual Studio, otwórz *Grant.sql* pliku, który został utworzony wcześniej.
2. Kliknij przycisk **Połącz**. 

    ![Przycisk Połącz](deploying-to-iis/_static/image11.png)
3. W **Połącz z serwerem** okna dialogowego wprowadź *. \SQLExpress* jako **nazwy serwera**, a następnie kliknij przycisk **Connect**.
4. Na liście rozwijanej bazy danych wybierz **ContosoUniversity**, a następnie kliknij przycisk **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

Tożsamość puli aplikacji domyślnej teraz ma wystarczające uprawnienia w bazie danych aplikacji migracje Code First do tworzenia tabel bazy danych, gdy aplikacja zostanie uruchomiona.

## <a name="publish-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka sposobów, które można wdrożyć w usługach IIS przy użyciu programu Visual Studio i narzędzia Web Deploy:

- Za pomocą programu Visual Studio publikowanie jednym kliknięciem.
- Publikowanie z poziomu wiersza polecenia.
- Tworzenie *pakietu wdrożeniowego* i zainstaluj go przy użyciu interfejsu użytkownika Menedżera usług IIS. Pakiet wdrożeniowy, który składa się z *zip* pliku, który zawiera wszystkie pliki i metadane wymagane do przeprowadzenia instalacji lokacji w usługach IIS.
- Utwórz pakiet wdrożeniowy i zainstaluj go przy użyciu wiersza polecenia.

Proces, który zostały kolejno przerobione w poprzednich samouczkach, aby skonfigurować program Visual Studio do automatyzowania zadań wdrażania, który ma zastosowanie do wszystkich tych metod. W tych samouczkach użyjemy pierwsze dwa z tych metod. Aby dowiedzieć się, jak za pomocą pakietów wdrażania, zobacz [wdrażanie aplikacji sieci web przez tworzenie i instalowanie pakietu wdrożeniowego sieci web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) mapy zawartości wdrażania sieci Web dla programu Visual Studio i platformy ASP.NET.

Przed opublikowaniem, upewnij się, że używasz programu Visual Studio w trybie administratora. Jeśli nie widzisz **(Administrator)** na pasku tytułu, zamknij program Visual Studio. W systemie Windows 8 **Start** strony lub Windows 7 **Start** menu, kliknij prawym przyciskiem myszy ikonę dla wersji programu Visual Studio używasz i wybierz **Uruchom jako Administrator**. Tryb administratora jest wymagana do publikowania, tylko gdy publikujesz do usług IIS na komputerze lokalnym.

### <a name="create-the-publish-profile"></a>Tworzenie profilu publikowania

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity (a nie projekt ContosoUniversity.DAL) i wybierz **Publikuj**.

    **Publikowanie w sieci Web** pojawi się Kreator.

    ![Publikowanie w sieci Web Kreatora profilu karty](deploying-to-iis/_static/image13.png)
2. Z listy rozwijanej wybierz  **&lt;nowy... &gt;**. (Z zainstalowaną najnowszą aktualizacją programu Visual Studio ma listy rozwijanej, a przycisk aby kliknij, aby utworzyć nowy profil od podstaw **niestandardowe**.)
3. W **nowy profil** okno dialogowe, wprowadź "Test", a następnie kliknij przycisk **OK**.

    Kreator automatycznie prowadzi do **połączenia** kartę.
4. W **adres URL usługi** wprowadź *localhost*.
5. W **witryny/aplikacji** wprowadź *domyślnej witryny sieci Web/ContosoUniversity*
6. W **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity`

    **Docelowy adres URL** ustawienie nie jest wymagane. Po zakończeniu wdrażania aplikacji programu Visual Studio automatycznie otwiera domyślnej przeglądarki do tego adresu URL. Jeśli nie chcesz, aby otwierać automatycznie po wdrożeniu przeglądarka, pozostaw to pole puste.
7. Kliknij przycisk **Waliduj połączenie** można sprawdzić, czy ustawienia są poprawne i czy możesz nawiązać połączenie usług IIS na komputerze lokalnym.

    Zielony znacznik wyboru sprawdza, czy połączenie zostanie nawiązane.

    ![Opublikuj na karcie Połączenie Kreatora sieci Web](deploying-to-iis/_static/image14.png)
8. Kliknij przycisk **dalej** celu przechodzenia do **ustawienia** kartę.
9. **Konfiguracji** pole listy rozwijanej określa konfigurację kompilacji, aby wdrożyć. Pozostaw ustawioną wartość domyślną wersji. Nie będzie wdrażanie kompilacji do debugowania w ramach tego samouczka.
10. Rozwiń **opcji publikowania pliku**, a następnie wybierz pozycję **wykluczanie plików z aplikacji\_folderu danych**.

    W środowisku testowym, aplikacja będzie uzyskiwać dostęp do baz danych, które zostały utworzone w lokalnym wystąpieniu programu SQL Server Express, pliki nie .mdf *aplikacji\_danych* folderu.
11. Pozostaw **Precompile podczas publikowania** i **Usuń dodatkowe pliki w lokalizacji docelowej** żadnych pól wyboru.

    ![Opcji publikowania pliku, na karcie Ustawienia](deploying-to-iis/_static/image15.png)

    Prekompilowanie to opcja, która jest przydatne głównie dla bardzo dużych witryn. można go ograniczyć czas uruchamiania strony po raz pierwszy po opublikowaniu lokacji w żądaniu strony.

    Nie będzie konieczne usunięcie dodatkowych plików, ponieważ jest to pierwszy wdrożenia, a nie będą istnieć wszystkie pliki w folderze docelowym jeszcze.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Jeśli wybierzesz **usunięcie dodatkowych plików** na kolejne wdrożenie do tej samej lokacji, należy upewnić się, czy funkcja w wersji zapoznawczej umożliwiającego wyświetlanie wcześniej pliki, które zostaną usunięte przed wdrożeniem. Oczekiwane zachowanie jest, że narzędzie Web Deploy spowoduje usunięcie plików na serwerze docelowym, które zostały usunięte w projekcie. Jednak struktura cały folder, w obszarze foldery źródłowe i docelowe są porównywane, a w niektórych scenariuszach wdrażania sieci Web może usunąć pliki, których nie chcesz, aby usunąć.
    > 
    > Na przykład jeśli masz aplikację sieci web w podfolderze na serwerze podczas wdrażania projektu do folderu głównego, podfolderu zostaną usunięte. Może mieć jednego projektu do lokacji głównej w domenie contoso.com i innego projektu dla bloga w contoso.com/blog. Aplikacja blog znajduje się w podfolderze. Jeśli wybierzesz opcję Usuń dodatkowe pliki w lokalizacji docelowej po wdrożeniu lokacji głównej, aplikacji blogu zostaną usunięte.
    > 
    > Inny przykład, aplikacja\_folderu danych, które mogą zostać usunięte nieoczekiwanie. Niektórych baz danych, takich jak SQL Server Compact przechowywania plików bazy danych w aplikacji\_folderu danych. Po początkowym wdrożeniu nie chcesz zachować kopiowanie plików bazy danych w przypadku kolejnych wdrożeń, musisz więc wybrać wykluczanie aplikacji\_danych na karcie pakowanie/publikowanie w sieci Web. Po wykonaniu tej, w przypadku Usuń dodatkowe pliki w wybrane miejsce docelowe, pliki bazy danych i aplikacji\_sam folder dane zostaną usunięte podczas następnego publikowania.

### <a name="configure-deployment-for-the-membership-database"></a>Konfiguruj wdrożenie dla bazy danych członkostwa

Poniższa procedura dotyczy **DefaultConnection** bazy danych w **baz danych** części okna dialogowego.

1. W **parametry połączenia zdalnego** wprowadź następujący ciąg połączenia wskazujący nowego programu SQL Server Express bazie danych członkostwa.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Proces wdrażania będzie umieścić te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

    Możesz też pobrać parametry połączenia z **Eksploratora serwera**. W **Eksploratora serwera**, rozwiń węzeł **połączeń danych** i wybierz  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Baza danych, następnie z **właściwości** okna kopii **parametry połączenia** wartość. Czy parametry połączenia będzie mieć jedno ustawienie dodatkowe, które można usunąć: `Pooling=False`.
2. Wybierz **Aktualizuj bazę danych**.

    To spowoduje, że schemat bazy danych, które zostały utworzone w docelowej bazie danych podczas wdrażania. W poniższych krokach możesz określić dodatkowe skrypty, które są potrzebne do uruchomienia: jeden do udzielania dostępu do bazy danych do domyślnej puli aplikacji i jeden, aby wdrożyć dane.
3. Kliknij przycisk **Konfiguruj aktualizacje bazy danych**.
4. W **Konfiguruj aktualizacje bazy danych** okno dialogowe, kliknij przycisk **Dodaj skrypt SQL** , a następnie przejdź do *Grant.sql* skrypt, który został wcześniej zapisany w folderze rozwiązania.
5. Powtórz te czynności, aby dodać *aspnet-data-dev.sql* skryptu.

    ![Konfiguruj aktualizacje bazy danych dla bazy danych członkostwa](deploying-to-iis/_static/image16.png)
6. Kliknij przycisk **Zamknij**.

### <a name="configure-deployment-for-the-application-database"></a>Konfigurowanie wdrożenia dla aplikacji bazy danych

Gdy program Visual Studio wykryje Entity Framework `DbContext` klasy, tworzy wpis w **baz danych** sekcji, która ma **wykonaj migracje Code First** pole wyboru, a nie  **Aktualizuj bazę danych** pole wyboru. W tym samouczku użyjesz tego pola wyboru do określenia wdrożenia migracje Code First.

W niektórych scenariuszach może być używany `DbContext` bazy danych, ale chcesz, aby użyć dostawcy dbDacFx zamiast migracji do wdrożenia bazy danych. W takim przypadku zobacz [jak wdrożyć Code First bazy danych bez migracje?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) w wdrażania sieci Web platformy ASP.NET, często zadawanych Pytaniach, w witrynie MSDN.

Poniższa procedura dotyczy **SchoolContext** bazy danych w **baz danych** części okna dialogowego.

1. W **parametry połączenia zdalnego** wprowadź następujące parametry połączenia, który wskazuje na nowego programu SQL Server Express bazy danych aplikacji.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Proces wdrażania będzie umieścić te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

    Możesz też pobrać parametry połączenia bazy danych aplikacji z **Eksploratora serwera** parametry połączenia bazy danych w taki sam sposób, masz członkostwa.
2. Wybierz **wykonaj migracje Code First (uruchomieniu aplikacji)**.

    Ta opcja powoduje, że proces wdrażania i konfigurowania wdrożonym pliku Web.config w celu określenia `MigrateDatabaseToLatestVersion` inicjatora. Gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu tego inicjatora automatycznie aktualizuje bazę danych do najnowszej wersji.

### <a name="configure-publish-profile-transforms"></a>Skonfigurować przekształcenia profilu publikowania

1. Kliknij przycisk **Zamknij**, a następnie kliknij przycisk **tak** po zapyta, czy chcesz zapisać zmiany.
2. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości**, rozwiń węzeł **PublishProfiles**.
3. Kliknij przycisk Rright *Test.pubxml,* a następnie kliknij przycisk **Dodaj przekształcenia konfiguracji**.

    ![Dodaj przekształcenie konfiguracji menu](deploying-to-iis/_static/image17.png)

    Program Visual Studio tworzy *Web.Test.config* pliku transformacji i otwiera go.
4. W *Web.Test.config* pliku przekształcenia, Wstaw następujący kod bezpośrednio po otwarciu tag konfiguracji.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Gdy używasz testu profil publikowania, przekształcenia ustawia wskaźnik środowiska, aby "Test". W witrynie wdrożonej zobaczysz "(Test)" po nagłówku H1 "Uniwersytet firmy Contoso".
5. Zapisz i zamknij plik.
6. Kliknij prawym przyciskiem myszy *Web.Test.config* plik i kliknij przycisk **Przekształcanie wersji zapoznawczej** aby upewnić się, że przekształcenie zakodowane produkuje oczekiwane zmiany.

    **(Wersja zapoznawcza) w pliku Web.config** okno pokazuje wynik zastosowania zarówno *Web.Release.config* przekształca i *Web.Test.config* przekształcenia.

### <a name="preview-the-deployment-updates"></a>Wyświetl podgląd aktualizacji wdrażania

1. Otwórz **publikowanie w sieci Web** kreatora ponownie (kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij przycisk **Publikuj**).
2. W **Podgląd** kartę, upewnij się, że **testu** profil jest nadal zaznaczone, a następnie kliknij **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane.

    ![Przycisk podglądu](deploying-to-iis/_static/image18.png)

    ![Podgląd publikacji](deploying-to-iis/_static/image19.png)

    Możesz również kliknąć **bazy danych w wersji zapoznawczej** linku umożliwia wyświetlenie skryptów, które będą uruchamiane w bazie danych członkostwa. (Nie skrypty są uruchamiane dla wdrożenia migracje Code First, więc nie ma nic do bazy danych aplikacji w wersji zapoznawczej.)
3. Kliknij przycisk **publikowania**.

    Jeśli program Visual Studio nie jest w trybie administratora, możesz otrzymać komunikat o błędzie wskazujący błąd uprawnień. W takim przypadku zamknięcie programu Visual Studio, otwórz go w trybie administratora i spróbuj opublikować ponownie.

    Jeśli program Visual Studio jest w trybie administratora **dane wyjściowe** pomyślne raporty okna tworzysz i publikujesz.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Po wprowadzeniu adresu URL w **docelowy adres URL** pole na profil publikowania **połączenia** , przeglądarka automatycznie zostanie otwarta karta na stronę główną University firmy Contoso, na uruchomione w usługach IIS na komputerze lokalnym.

## <a name="test-in-the-test-environment"></a>Testowanie w środowisku testowym

Należy zauważyć, że wskaźnik środowisko zawiera "(Test)" zamiast "(Dev)", który wskazuje, że *Web.config* transformacji dla wskaźnika środowisko zakończyło się pomyślnie.

Uruchom **Instruktorzy** stronę, aby sprawdzić, czy Code First zasilany w bazie danych przez instruktorów. Po wybraniu tej strony, może to potrwać kilka minut, aby załadować, ponieważ Code First tworzy bazę danych, a następnie uruchamia `Seed` metody. (Go nie wykonać, jeśli zostały na stronie głównej, ponieważ aplikacja nie próbuje jeszcze dostępu do bazy danych).

Kliknij przycisk **studentów** kartę, aby sprawdzić, czy wdrożonej bazy danych jest nie studentów.

Wybierz **dodać uczniów** z **studentów** menu Dodaj uczniem, a następnie wyświetlić nowego studenta w **studentów** stronę, aby sprawdzić, czy można pomyślnie zapisać do bazy danych .

Z **kursów** menu, wybierz opcję **aktualizacji środki na korzystanie z**. **Aktualizacji środki na korzystanie z** strony musi mieć uprawnienia administratora, więc **logowanie** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, utworzony wcześniej ("admin" i "devpwd"). **Aktualizacji środki na korzystanie z** zostanie wyświetlona strona, która sprawdza, czy konto administratora, który został utworzony w poprzednim samouczku poprawnie została wdrożona do środowiska testowego.

Upewnij się, że *Elmah* folder istnieje w *c:\inetpub\wwwroot\ContosoUniversity* folderu z plikiem w symbol zastępczy w nim.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przejrzyj automatycznej zmiany pliku Web.config dla migracje Code First

Otwórz *Web.config* plik w aplikacji wdrożonej w *C:\inetpub\wwwroot\ContosoUniversity* i zobaczyć, gdzie w procesie wdrażania skonfigurowane migracje Code First, aby automatycznie aktualizacji bazy danych do najnowszej wersji.

![](deploying-to-iis/_static/image21.png)

Proces wdrażania jest tworzona nowe parametry połączenia dla migracje Code First do użycia wyłącznie w celu zaktualizowania schematu bazy danych:

![Parametry połączenia Database_Publish](deploying-to-iis/_static/image22.png)

Te dodatkowe parametry można określić jedno konto użytkownika dla aktualizacje schematu bazy danych i inne konto użytkownika uzyskać dostęp do danych aplikacji. Na przykład można przypisać **db\_właściciela** roli migracje Code First, i **db\_datareader** i **db\_datawriter**role w aplikacji. Jest to typowy wzorzec ochronę w głębi, który uniemożliwia potencjalnie złośliwego kodu w aplikacji zmianę schematu bazy danych. (Na przykład może się to zdarzyć w pomyślnym ataku polegającego na iniekcji SQL.) Ten wzorzec nie jest używany przez te samouczki. Implementacja tego wzorca, w tym scenariuszu, należy to zrobił, wykonując następujące czynności:

1. W **ustawienia** karcie **publikowanie w sieci Web** kreatora wprowadź parametry połączenia, który określa użytkownika z uprawnieniami do aktualizacji schematu pełnej bazy danych, a następnie wyczyść **Użyj tych parametrów połączenia w czasie wykonywania** pole wyboru. W wdrożonym pliku Web.config, staje się on `DatabasePublish` parametry połączenia.
2. Utwórz przekształcenia pliku Web.config dla parametrów połączenia, który ma aplikacji do użycia w czasie wykonywania.

## <a name="summary"></a>Podsumowanie

Masz teraz wdrożonych aplikacji usług IIS na komputerze deweloperskim i przetestowano ją tam.

![Strona główna w teście](deploying-to-iis/_static/image23.png)

Sprawdza to proces wdrażania skopiować zawartość aplikacji do odpowiedniej lokalizacji (z wyjątkiem plików, które nie miały wdrażania) i również narzędzia Web Deploy IIS poprawnie skonfigurowany podczas wdrażania. W następnym samouczku uruchomisz jeden więcej test, który umożliwia znalezienie zadanie wdrożenia, które jeszcze nie zostało wykonane: Ustawianie uprawnień do folderów na *Elmah* folderu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacji na temat uruchamiania usług IIS lub IIS Express w programie Visual Studio zobacz następujące zasoby:

- [Omówienie usługi IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.net w witrynie.
- [Wprowadzenie do usług IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) w blogu Scotta Guthrie.
- [Serwery w sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Podstawowe różnice między usług IIS i programem ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie programu ASP.NET.

Aby uzyskać informacje o problemach, które mogą wystąpić, gdy aplikacja działa w trybie średniego zaufania, zobacz [hostingu aplikacji ASP.NET w relacji zaufania średni](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 Guys z Rolla witryny.

> [!div class="step-by-step"]
> [Poprzednie](project-properties.md)
> [dalej](setting-folder-permissions.md)
