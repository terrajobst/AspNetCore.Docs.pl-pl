---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie w środowisku testowym | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396340"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie w środowisku testowym
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub u dostawcy hostingu innych firm przy użyciu programu Visual Studio 2017. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku będziesz wdrożysz aplikację sieci web platformy ASP.NET do programu Internet Information Server (IIS) na komputerze lokalnym.

Zazwyczaj podczas opracowywania aplikacji, możesz go uruchomić i przetestować go w programie Visual Studio. Domyślnie projekty aplikacji sieci web w programie Visual Studio 2017 używają usług IIS Express jako serwera wdrożeniowego sieci web. Usługi IIS Express zachowuje się bardziej jak usługi IIS niż serwera programu Visual Studio Development (znanego także jako Cassini), która domyślnie używa programu Visual Studio 2017. Ale ani serwera wdrożeniowego sieci web działa tak samo jak usługi IIS. W związku z tym aplikację można uruchomić i poprawnie Testuj w programie Visual Studio, ale się nie powieść, gdy aplikacja jest wdrożona w usługach IIS.

Można wiarygodnie przetestować aplikację na dwa sposoby:

1. Wdrażanie aplikacji w usługach IIS na komputerze deweloperskim za pomocą tego samego procesu, którego będziesz później wdrożyć w środowisku produkcyjnym.

   Można skonfigurować w programie Visual Studio do używania usług IIS, gdy uruchamiasz projekt sieci web, ale które nie Testuj proces wdrażania. Ta metoda sprawdza proces wdrażania, a aplikacja działa poprawnie w środowisku usług IIS.

2. Wdróż aplikację w środowisku testowym, podobne do środowiska produkcyjnego. 
 
   Środowiska produkcyjnego potrzeby tych samouczków to funkcja Web Apps w usłudze Azure App Service. Środowisko testowe idealnym rozwiązaniem jest dodatkowe utworzona aplikacja sieci web w usłudze Azure. Chociaż należy go skonfigurować taki sam sposób jak produkcyjną aplikację internetową, będzie ona używana tylko do testowania.

Opcja 2 jest najbardziej niezawodnym sposobem, aby przetestować. Jeśli używasz opcji 2, nie jest konieczna opcja 1. Jednak jeśli wdrażasz do innych firm hostingu dostawcy, opcja 2 może nie być możliwe lub może być kosztowne, więc tej serii samouczków opisano obie metody. Wskazówki dotyczące opcji 2 znajduje się w [wdrażanie w środowisku produkcyjnym](deploying-to-production.md) samouczka.

Aby uzyskać więcej informacji o korzystaniu z serwerów sieci web w programie Visual Studio, zobacz [serwerów sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Przypomnienie: Jeśli pojawi się komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, należy koniecznie sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Pobieranie uniwersytecie Contoso projekt startowy

Pobierz i zainstaluj rozwiązania starter Contoso University programu Visual Studio i projektu. To rozwiązanie zawiera samouczek ukończone. 

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Instalowanie usług IIS

Do wdrożenia usług IIS na komputerze deweloperskim, upewnij się, że są zainstalowane usługi IIS i narzędzia Web Deploy. Domyślnie program Visual Studio instaluje narzędzie Web Deploy, ale usługi IIS nie jest zawarty w domyślnej konfiguracji systemu Windows 10, Windows 8 lub Windows 7. Jeśli usługi IIS zostały już zainstalowane, a domyślna pula aplikacji jest już ustawione w .NET 4, przejdź do [następnej sekcji](#sqlexpress).

1. Zaleca się używanie [Instalatora platformy sieci Web (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) do zainstalowania usług IIS i narzędzia Web Deploy. WPI instaluje zalecaną konfigurację usług IIS, który zawiera wymagania wstępne dotyczące usług IIS i narzędzia Web Deploy, jeśli to konieczne.

   Jeśli po zainstalowaniu usług IIS, narzędzia Web Deploy lub dowolnej ich wymaganych składników, WPI instaluje tylko co to jest Brak.

   * Aby zainstalować usługi IIS i narzędzia Web Deploy, należy użyć Instalatora platformy sieci Web:
    
     ![Instalowanie usług IIS przy użyciu usługi](deploying-to-iis/_static/image30.png)

     ![Zainstaluj narzędzie Web Deploy, za pomocą usługi](deploying-to-iis/_static/image31.png)

     Zostaną wyświetlone komunikaty wskazujące, czy zostaną zainstalowane usługi IIS 7. Łącze działa w przypadku usług IIS 8 w systemie Windows 8. Jednak dla systemu Windows 8 i nowszych przejść przez następujące kroki, aby upewnić się, że platforma ASP.NET 4.7 jest zainstalowany:

   * Otwórz **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz lub wyłącz funkcje** .

   * Rozwiń **Internetowe usługi informacyjne**, **usługi World Wide Web**, i **funkcje tworzenia aplikacji**.
   
   * Upewnij się, że **platforma ASP.NET 4.7** jest zaznaczone.

     ![Select ASP.NET 4.7](deploying-to-iis/_static/image1a.png)

   * Upewnij się, że **usługi World Wide Web** i **Konsola zarządzania usługami IIS** jest zaznaczone. Spowoduje to zainstalowanie usług IIS i Menedżera usług IIS.
    
     ![Wybierz opcję usługi WWW](deploying-to-iis/_static/image24.png)    
  
   * Kliknij przycisk **OK**. Komunikaty okna dialogowego wskazujący, że instalacja ma miejsce są wyświetlane.

Po zainstalowaniu usług IIS, należy uruchomić **Menedżera usług IIS** aby upewnić się, że .NET Framework w wersji 4 są przypisane do domyślnej puli aplikacji.

1. Naciśnij klawisze WINDOWS + R, aby otworzyć **Uruchom** okno dialogowe.

   (W systemie Windows 8 lub nowszym, wprowadź "Uruchom" w **Start** strony. Windows 7, wybierz **Uruchom** z **Start** menu. Jeśli **Uruchom** nie znajduje się w **Start** menu, kliknij prawym przyciskiem myszy pasek zadań, wybrać **właściwości**, wybierz opcję **Start Menu** , a następnie wybierz **Dostosuj**i wybierz **Uruchom polecenie**.)

2. Wprowadź "inetmgr" i wybierz **OK**.

3. W **połączeń** okienku rozwiń węzeł serwera i wybierz **pul aplikacji**. W **pul aplikacji** okienko Jeśli **DefaultAppPool** jest przypisany do programu .NET framework w wersji 4 tak jak na poniższej ilustracji, przejdź do następnej sekcji.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Jeśli zostanie wyświetlony tylko dwie pule aplikacji i są ustawione na .NET Framework 2.0, programu ASP.NET 4 należy zainstalować w usługach IIS.

   Dla systemu Windows 8 lub nowszym, zobacz instrukcje w poprzedniej sekcji, upewniając się, że platforma ASP.NET 4.7 jest zainstalowane, lub zobacz [sposobu instalacji ASP.NET 4.5 w systemie Windows 8 i Windows Server 2012](https://support.microsoft.com/kb/2736284). Windows 7, Otwórz okno wiersza polecenia, klikając prawym przyciskiem myszy **polecenia** w Windows **Start** menu i wybierając polecenie **Uruchom jako Administrator**. Uruchom [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) do zainstalowania programu ASP.NET 4 w usługach IIS przy użyciu następujących poleceń. (W systemach 32-bitowych, zamiast "Framework64" z "Framework").

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   To polecenie umożliwia utworzenie nowej aplikacji pul dla programu .NET Framework 4, ale domyślnej puli aplikacji będzie zachowana wartość 2.0. Wdrożysz aplikację, czy cele .NET 4 do puli aplikacji, więc Zmień pulę aplikacji .NET 4.

5. Jeśli został zamknięty **Menedżera usług IIS**, uruchom ją ponownie, rozwiń węzeł serwera, a następnie wybierz **pul aplikacji**.

6. W **pul aplikacji** okienku wybierz **DefaultAppPool**. W **akcje** okienku wybierz **podstawowych ustawień**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. W **edytowanie puli aplikacji** okno dialogowe, zmiana **wersja środowiska .NET CLR** do **4.0.30319 .NET CLR**. Kliknij przycisk **OK**.

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Teraz możesz przystąpić do publikowania aplikacji sieci web usług IIS. Najpierw jednak tworzyć baz danych na potrzeby testowania.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Zainstaluj program SQL Server Express

LocalDB nie jest przeznaczona do pracy w usługach IIS, więc nie można zainstalować programu SQL Server Express środowiska testowego. Jeśli używasz programu Visual Studio 2010 programu SQL Server Express jest już zainstalowana domyślnie. Jeśli używasz programu Visual Studio 2012 lub nowszym należy zainstalować program SQL Server Express.

Aby zainstalować program SQL Server Express, Pobierz i zainstaluj go z [Centrum pobierania: Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Na pierwszej stronie Centrum instalacji programu SQL Server, wybierz **nowy serwer SQL instalacji autonomicznej lub Dodaj funkcje do istniejącej instalacji** i postępuj zgodnie z instrukcjami przyjmuje opcje domyślne. W Kreatorze instalacji Zaakceptuj ustawienia domyślne. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [Instalowanie programu SQL Server za pomocą Kreatora instalacji (Instalatora)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Tworzenie programu SQL Server Express bazy danych dla środowiska testowego

Aplikacja Contoso University ma dwie bazy danych: 

1. Bazy danych członkostwa 
2. Baza danych aplikacji 

Można wdrożyć tych baz danych, dwie osobne bazy danych lub pojedynczą bazę danych. Łącząc je sprawia, że sprzężeń danych między nimi łatwiejsze. 

Jeśli wdrażasz do dostawcy hostingu innych firm, planu hostingu może również umożliwiają powód do ich łączenia. Na przykład dostawca może pobierać tylko dla wielu baz danych lub może nawet zezwala na więcej niż jednej bazy danych.

W tym samouczku wdrożysz dwie bazy danych w środowisku testowym i jedna baza danych w środowiskach przejściowych i produkcyjnych.

Z **widoku** menu w programie Visual Studio, wybierz **Eksploratora serwera** (**Eksplorator bazy danych** w Visual Web Developer). Kliknij prawym przyciskiem myszy **połączeń danych** i wybierz **Tworzenie nowej bazy danych SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

W **Tworzenie nowej bazy danych SQL Server** okna dialogowego wprowadź ". \SQLExpress" w **nazwy serwera** pole i "aspnet-ContosoUniversity" w **nazwę nowej bazy danych** pole. Kliknij przycisk **OK**.

![Utwórz aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Wykonaj tę samą procedurę, aby utworzyć nową bazę danych programu SQL Server Express School o nazwie `ContosoUniversity`.

**Eksplorator serwera** zawiera dwa nowe bazy danych.

![Nowe bazy danych w Eksploratorze serwera](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Utwórz skrypt grant dla nowych baz danych

Po uruchomieniu aplikacji w usługach IIS na komputerze deweloperskim, aplikacja używa domyślnej puli aplikacji poświadczenia dostępu do bazy danych. Jednak domyślnie pula aplikacji nie ma uprawnień do otwierania bazy danych. Oznacza to, że trzeba uruchomić skrypt, aby przyznać to uprawnienie. W tej sekcji możesz utworzyć ten skrypt i uruchomić go później, aby upewnić się, że aplikacja będzie mógł otworzyć baz danych po uruchomieniu w usługach IIS.

W edytorze tekstów Skopiuj poniższe polecenia SQL do nowego pliku i zapisz go jako *Grant.sql*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

W programie Visual Studio Otwórz rozwiązanie University firmy Contoso. Kliknij prawym przyciskiem myszy rozwiązanie (nie jednego z projektów), a następnie wybierz pozycję **Dodaj**. Wybierz **istniejący element**, przejdź do *Grant.sql*, a następnie otwórz go.

> [!NOTE]
> Ten skrypt została zaprojektowana do pracy z programem SQL Server Express 2012 lub nowszym i przy użyciu ustawień usług IIS w systemie Windows 10, Windows 8 lub Windows 7, ponieważ są one określone w tym samouczku. Jeśli używasz innej wersji programu SQL Server lub Windows, czy należy zainstalować program IIS na komputerze inaczej, zmiany w tym skrypcie może być wymagane. Aby uzyskać więcej informacji na temat skrypty programu SQL Server, zobacz [programu SQL Server — książki Online](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Uwaga dotycząca zabezpieczeń** ten skrypt umożliwia `db_owner` uprawnień do użytkownika, który uzyskuje dostęp do bazy danych w czasie wykonywania, który jest uzyskasz w środowisku produkcyjnym. W niektórych scenariuszach możesz chcieć określić użytkownika, która ma schemat pełnej bazy danych, zaktualizować uprawnienia tylko do wdrożenia i określ czas wykonywania inny użytkownik z uprawnieniami tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz [przeglądanie automatyczne zmiany w pliku Web.config na potrzeby migracji Code First](#reviewingmigrations) później w tym samouczku.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uruchom skrypt przydział w bazie danych aplikacji

Można skonfigurować profil publikowania, aby uruchomić skrypt przydział w bazie danych członkostwa podczas wdrażania, ponieważ korzysta z tego wdrożenia bazy danych [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) dostawcy. Nie można uruchamiać skrypty w trakcie wdrażania migracje Code First jest sposób wdrażania aplikacji bazy danych. Oznacza to, że trzeba ręcznie uruchom skrypt przed wdrożeniem w bazie danych aplikacji.

1. W programie Visual Studio, otwórz *Grant.sql* pliku, który został utworzony wcześniej.

2. Wybierz przycisk **Połącz**. 

    ![Przycisk Połącz](deploying-to-iis/_static/image11.png)

3. W **Połącz z serwerem** okna dialogowego wprowadź *. \SQLExpress* jako **nazwy serwera**. Wybierz przycisk **Połącz**.

4. Na liście rozwijanej bazy danych wybierz **ContosoUniversity**. Wybierz pozycję **Wykonaj**. 

   ![](deploying-to-iis/_static/image12.png)

Tożsamość puli aplikacji domyślnej teraz ma wystarczające uprawnienia w bazie danych aplikacji migracje Code First do tworzenia tabel bazy danych, gdy aplikacja zostanie uruchomiona.

## <a name="publish-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka sposobów, które można wdrożyć w usługach IIS przy użyciu programu Visual Studio i narzędzia Web Deploy:

* Za pomocą programu Visual Studio publikowanie jednym kliknięciem.
* Publikowanie z poziomu wiersza polecenia.
* Utwórz pakiet wdrożeniowy i zainstaluj ją za pomocą Menedżera usług IIS. Pakiet zawiera plik zip ze wszystkimi plikami i metadane wymagane do przeprowadzenia instalacji lokacji w usługach IIS.
* Utwórz pakiet wdrożeniowy i zainstaluj go przy użyciu wiersza polecenia.

Proces, który zostały kolejno przerobione w poprzednich samouczkach, aby skonfigurować program Visual Studio do automatyzowania zadań wdrażania, który ma zastosowanie do wszystkich tych metod. W tych samouczkach użyjemy dwóch pierwszych metod. Aby dowiedzieć się, jak za pomocą pakietów wdrażania, zobacz [wdrażanie aplikacji sieci web przez tworzenie i instalowanie pakietu wdrożeniowego sieci web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) mapy zawartości wdrażania sieci Web dla programu Visual Studio i platformy ASP.NET.

Przed opublikowaniem, upewnij się, że używasz programu Visual Studio w trybie administratora. Jeśli nie widzisz **(Administrator)** na pasku tytułu, zamknij program Visual Studio. W systemie Windows 8 (lub nowszym) **Start** strony lub Windows 7 **Start** menu, kliknij prawym przyciskiem myszy ikonę programu Visual Studio i wybierz **Uruchom jako Administrator**. Tryb administratora jest tylko wymagane do publikowania, gdy podczas publikowania w usługach IIS na komputerze lokalnym.

### <a name="create-the-publish-profile"></a>Tworzenie profilu publikowania

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu (nie **ContosoUniversity.DAL** projektu). Wybierz **publikowania**. **Publikuj** zostanie wyświetlona strona.

2. Wybierz **nowy profil**. **Wybierz lokalizację docelową publikowania** pojawi się okno dialogowe.

3. Wybierz **usług IIS, FTP, etc**. Wybierz **Utwórz profil**. **Publikuj** pojawi się Kreator.

   ![Publikowanie w sieci Web Kreatora profilu karty](deploying-to-iis/_static/image26.png)

4. Z **metody publikowania** menu rozwijanego wybierz opcję **narzędzia Web Deploy**.

5. Aby uzyskać **serwera**, wprowadź *localhost*.

6. Aby uzyskać **Nazwa lokacji**, wprowadź *domyślnej witryny sieci Web/ContosoUniversity*.

7. Aby uzyskać **docelowy adres URL**, wprowadź *http://localhost/ContosoUniversity*.

   **Docelowy adres URL** ustawienie nie jest wymagane. Po zakończeniu wdrażania aplikacji programu Visual Studio automatycznie otwiera domyślnej przeglądarki do tego adresu URL. Jeśli nie chcesz, aby otwierać automatycznie po wdrożeniu przeglądarka, pozostaw to pole puste.

8. Wybierz **Waliduj połączenie** można sprawdzić, czy ustawienia są poprawne i czy możesz nawiązać połączenie usług IIS na komputerze lokalnym.

   Zielony znacznik wyboru sprawdza, czy połączenie zostanie nawiązane.

   ![Opublikuj na karcie Połączenie Kreatora sieci Web](deploying-to-iis/_static/image27.png)

9. Wybierz **dalej** celu przechodzenia do **ustawienia** kartę.

10. **Konfiguracji** pole listy rozwijanej określa konfigurację kompilacji, aby wdrożyć. Pozostaw ona ustawiona na wartość domyślną **wersji**. Nie będzie wdrażanie kompilacji do debugowania w ramach tego samouczka.

11. Rozwiń **opcje publikowania plików**. Wybierz **wykluczanie plików z aplikacji\_folderu danych**.

    W środowisku testowym, aplikacja uzyskuje dostęp do bazy danych, które utworzono w lokalnym wystąpieniu programu SQL Server Express nie plików MDF w *aplikacji\_danych* folderu.

12. Pozostaw **Precompile podczas publikowania** i **Usuń dodatkowe pliki w lokalizacji docelowej** żadnych pól wyboru.

    ![Opcji publikowania pliku, na karcie Ustawienia](deploying-to-iis/_static/image15a.png)

    Prekompilowanie to opcja, która jest przydatna szczególnie w przypadku dużych witryn. Umożliwia skrócenie czasu uruchamiania po raz pierwszy po opublikowaniu lokacji w żądaniu strony.

    Nie będzie konieczne usunięcie dodatkowych plików, ponieważ jest to pierwszy wdrożenia, a nie będą istnieć wszystkie pliki w folderze docelowym jeszcze.

    > [!NOTE] 
    > Jeśli wybierzesz **Usuń dodatkowe pliki w lokalizacji docelowej** na kolejne wdrożenie do tej samej lokacji, należy upewnić się, czy funkcja w wersji zapoznawczej umożliwiającego wyświetlanie wcześniej pliki, które zostaną usunięte przed wdrożeniem. Oczekiwane zachowanie jest, że narzędzie Web Deploy spowoduje usunięcie plików na serwerze docelowym, które zostały usunięte w projekcie. Jednak struktura cały folder, w obszarze foldery źródłowe i docelowe są porównywane; a w niektórych scenariuszach wdrażania sieci Web może usunąć pliki, których nie chcesz, aby usunąć.
    > 
    > Na przykład jeśli masz aplikację sieci web w podfolderze na serwerze podczas wdrażania projektu do folderu głównego, podfolderów zostaną usunięte. Może mieć jednego projektu do lokacji głównej w domenie contoso.com i innego projektu dla bloga w contoso.com/blog. Aplikacja blog znajduje się w podfolderze. Jeśli wybierzesz **Usuń dodatkowe pliki w lokalizacji docelowej** po wdrożeniu lokacji głównej aplikacji blogu zostaną usunięte.
    > 
    > Inny przykład, aplikacja\_folderu danych, które mogą zostać usunięte nieoczekiwanie. Niektórych baz danych, takich jak SQL Server Compact przechowywania plików bazy danych w aplikacji\_folderu danych. Po początkowym wdrożeniu nie chcesz zachować kopiowanie plików bazy danych w przypadku kolejnych wdrożeń, musisz więc wybrać **wykluczanie aplikacji\_danych** na karcie pakowanie/publikowanie w sieci Web. Po wykonaniu, że jeśli masz **Usuń dodatkowe pliki w lokalizacji docelowej** wybrane pliki bazy danych i aplikacji\_sam folder dane zostaną usunięte podczas następnego publikowania.

### <a name="configure-deployment-for-the-membership-database"></a>Konfiguruj wdrożenie dla bazy danych członkostwa

Poniższa procedura dotyczy **DefaultConnection** bazy danych w oknie dialogowym **baz danych** sekcji.

1. W **parametry połączenia zdalnego** wprowadź następujący ciąg połączenia wskazujący nowego programu SQL Server Express bazie danych członkostwa.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Proces wdrażania umieszcza te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

    Możesz też pobrać parametry połączenia z **Eksploratora serwera**. W **Eksploratora serwera**, rozwiń węzeł **połączeń danych** i wybierz  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Baza danych, następnie z **właściwości** okna kopii **parametry połączenia** wartość. Czy parametry połączenia będzie mieć jedno ustawienie dodatkowe, które można usunąć: `Pooling=False`.

2. Wybierz **Aktualizuj bazę danych**.

   Powoduje to, że schemat bazy danych, które zostały utworzone w docelowej bazie danych podczas wdrażania. W następnych krokach możesz określić dodatkowe skrypty, które są potrzebne do uruchomienia: jeden do udzielania dostępu do bazy danych do domyślnej puli aplikacji i jeden, aby wdrożyć dane.

3. Wybierz **Konfiguruj aktualizacje bazy danych**.
 
4. W **Konfiguruj aktualizacje bazy danych** okno dialogowe, wybierz opcję **Dodaj skrypt SQL**. Przejdź do *Grant.sql* skrypt, który został wcześniej zapisany w folderze rozwiązania.

5. Powtórz te czynności, aby dodać *aspnet-data-dev.sql* skryptu.

   ![Konfiguruj aktualizacje bazy danych dla bazy danych członkostwa](deploying-to-iis/_static/image16.png)

6. Wybierz **Zamknij**.

### <a name="configure-deployment-for-the-application-database"></a>Konfigurowanie wdrożenia dla aplikacji bazy danych

Gdy program Visual Studio wykryje Entity Framework `DbContext` klasy, tworzy wpis w **baz danych** sekcji, która ma **wykonaj migracje Code First** pole wyboru, a nie  **Aktualizuj bazę danych** pole wyboru. W tym samouczku użyjesz tego pola wyboru do określenia wdrożenia migracje Code First.

W niektórych scenariuszach może być używany `DbContext` bazy danych, ale chcesz, aby użyć dostawcy dbDacFx zamiast migracji do wdrożenia bazy danych. W takim przypadku zobacz [jak wdrożyć Code First bazy danych bez migracje?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) w wdrażania sieci Web platformy ASP.NET, często zadawanych Pytaniach, w witrynie MSDN.

Poniższa procedura dotyczy **SchoolContext** bazy danych w oknie dialogowym **baz danych** sekcji.

1. W **parametry połączenia zdalnego** wprowadź następujące parametry połączenia, który wskazuje na nowego programu SQL Server Express bazy danych aplikacji.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Proces wdrażania umieszcza te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

   Możesz też pobrać parametry połączenia bazy danych aplikacji z **Eksploratora serwera** tak samo jak masz parametry połączenia bazy danych członkostwa.

2. Wybierz **wykonaj migracje Code First (uruchomieniu aplikacji)**.

   Ta opcja powoduje, że proces wdrażania i konfigurowania wdrożonym pliku Web.config w celu określenia `MigrateDatabaseToLatestVersion` inicjatora. Gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu tego inicjatora automatycznie aktualizuje bazę danych do najnowszej wersji.

### <a name="configure-publish-profile-transforms"></a>Skonfigurować przekształcenia profilu publikowania

1. Wybierz **Zamknij**. Wybierz **tak** po zapyta, czy chcesz zapisać zmiany.

2. W **Eksploratora rozwiązań**, rozwiń węzeł **właściwości**, rozwiń węzeł **PublishProfiles**.

3. Kliknij prawym przyciskiem myszy *CustomProfile.pubxml* i zmień jego nazwę *Test.pubxml*.

4. Kliknij prawym przyciskiem myszy *Test.pubxml*. Wybierz **Dodaj przekształcenie konfiguracji**.

   ![Dodaj przekształcenie konfiguracji menu](deploying-to-iis/_static/image17.png)

   Program Visual Studio tworzy *Web.Test.config* pliku transformacji i otwiera go.

5. W *Web.Test.config* pliku przekształcenia, Wstaw następujący kod bezpośrednio po otwarciu tag konfiguracji.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Gdy używasz testu profil publikowania, przekształcenia ustawia wskaźnik środowiska, aby "Test". W witrynie wdrożonej zobaczysz "(Test)" po nagłówku H1 "Uniwersytet firmy Contoso".

6. Zapisz i zamknij plik.

7. Kliknij prawym przyciskiem myszy *Web.Test.config* plik i wybierz **Przekształcanie wersji zapoznawczej** aby upewnić się, że przekształcenie zakodowane produkuje oczekiwane zmiany.

    **(Wersja zapoznawcza) w pliku Web.config** okno pokazuje wynik zastosowania zarówno *Web.Release.config* przekształca i *Web.Test.config* przekształcenia.

### <a name="preview-the-deployment-updates"></a>Wyświetl podgląd aktualizacji wdrażania

1. Otwórz **publikowanie w sieci Web** kreatora ponownie (kliknij prawym przyciskiem myszy projekt ContosoUniversity, wybierz **publikowania**, następnie **Podgląd**).

2. W **Podgląd** okno dialogowe, wybierz opcję **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane. 

   ![Podgląd publikacji](deploying-to-iis/_static/image29.png)

   Możesz również wybrać **bazy danych w wersji zapoznawczej** linku umożliwia wyświetlenie skryptów, które będą uruchamiane w bazie danych członkostwa. (Nie skrypty są uruchamiane dla wdrożenia migracje Code First, więc nie ma nic do bazy danych aplikacji w wersji zapoznawczej.)

3. Wybierz **publikowania**.

   Jeśli program Visual Studio nie jest w trybie administratora, możesz otrzymać komunikat o błędzie uprawnień. W takim przypadku zamknięcie programu Visual Studio, otwórz go w trybie administratora i spróbuj opublikować ponownie.

   Jeśli program Visual Studio jest w trybie administratora **dane wyjściowe** pomyślne raporty okna tworzysz i publikujesz.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Po wprowadzeniu adresu URL w **docelowy adres URL** pole na profil publikowania **połączenia** , przeglądarka automatycznie zostanie otwarta karta do strony głównej University Contoso uruchomionych w usługach IIS na komputerze.

## <a name="test-in-the-test-environment"></a>Testowanie w środowisku testowym

Należy zauważyć, że wskaźnik środowisko zawiera "(Test)" zamiast "(Dev)", który wskazuje, że *Web.config* transformacji dla wskaźnika środowisko zakończyło się pomyślnie.

Uruchom **Instruktorzy** stronę, aby sprawdzić, czy Code First zasilany w bazie danych przez instruktorów. Po wybraniu tej strony, może to potrwać kilka minut, aby załadować, ponieważ Code First tworzy bazę danych, a następnie uruchamia `Seed` metody. (Go nie wykonać, jeśli zostały na stronie głównej, ponieważ aplikacja nie próbuje jeszcze dostępu do bazy danych).

Wybierz **studentów** kartę, aby sprawdzić, czy wdrożonej bazy danych jest nie studentów.

Wybierz **dodać uczniów** z **studentów** menu. Dodać Studenta, a następnie Wyświetl nowego studenta w **studentów** strony. Sprawdza, czy można pomyślnie zapisać do bazy danych.

Z **kursów** menu, wybierz opcję **aktualizacji środki na korzystanie z**. **Aktualizacji środki na korzystanie z** strony musi mieć uprawnienia administratora, więc **logowanie** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, utworzony wcześniej ("admin" i "devpwd"). **Aktualizacji środki na korzystanie z** zostanie wyświetlona strona. Sprawdza, czy konto administratora, który został utworzony w poprzednim samouczku poprawnie została wdrożona do środowiska testowego.

Upewnij się, że *ELMAH* folder istnieje w *c:\inetpub\wwwroot\ContosoUniversity* folderu z plikiem w symbol zastępczy w nim.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przejrzyj automatycznej zmiany pliku Web.config dla migracje Code First

Otwórz *Web.config* plik w aplikacji wdrożonej w *C:\inetpub\wwwroot\ContosoUniversity* i zobaczyć, gdzie w procesie wdrażania skonfigurowane migracje Code First, aby automatycznie aktualizacji bazy danych do najnowszej wersji.

![](deploying-to-iis/_static/image21.png)

Proces wdrażania jest tworzona nowe parametry połączenia dla migracje Code First do użycia wyłącznie w celu zaktualizowania schematu bazy danych:

![Parametry połączenia Database_Publish](deploying-to-iis/_static/image22.png)

Te dodatkowe parametry można określić jedno konto użytkownika dla aktualizacje schematu bazy danych i inne konto użytkownika uzyskać dostęp do danych aplikacji. Na przykład można przypisać **db\_właściciela** roli migracje Code First i **db\_datareader** z **db\_datawriter**role w aplikacji. Jest to typowy wzorzec ochronę w głębi, który uniemożliwia potencjalnie złośliwego kodu w aplikacji zmianę schematu bazy danych. (Na przykład może się to zdarzyć w pomyślnym ataku polegającego na iniekcji SQL.) Te samouczki, nie używaj tego wzorca. Implementacja tego wzorca, w tym scenariuszu, wykonaj następujące czynności:

1. W **publikowanie w sieci Web** kreatora w obszarze **ustawienia** wprowadź parametry połączenia, które określa użytkownika z uprawnieniami do aktualizacji schematu pełnej bazy danych. Wyczyść **Użyj tych parametrów połączenia w czasie wykonywania** pole wyboru. W wdrożonym pliku Web.config, staje się on `DatabasePublish` parametry połączenia.

2. Utwórz przekształcenia pliku Web.config dla parametrów połączenia, który ma aplikacji do użycia w czasie wykonywania.

## <a name="summary"></a>Podsumowanie

Udało Ci wdrożyć aplikację w usłudze IIS na komputerze deweloperskim i przetestowano ją tam.

![Strona główna w teście](deploying-to-iis/_static/image23.png)

Sprawdza, czy Proces wdrażania skopiować zawartość aplikacji do odpowiedniej lokalizacji (z wyjątkiem plików, które nie miały wdrażania) i również narzędzia Web Deploy IIS poprawnie skonfigurowane podczas wdrażania. W następnym samouczku uruchomisz jeden więcej test, który umożliwia znalezienie zadanie wdrożenia, które jeszcze nie zostało wykonane: Ustawianie uprawnień do folderów na *wiąz ah* folderu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacji na temat uruchamiania usług IIS lub IIS Express w programie Visual Studio zobacz następujące zasoby:

- [Omówienie usługi IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.net w witrynie.
- [Wprowadzenie do usług IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) w blogu Scotta Guthrie.
- [Serwery w sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Podstawowe różnice między usług IIS i programem ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie programu ASP.NET.

Aby uzyskać informacje o problemach, które mogą wystąpić, gdy aplikacja działa w trybie średniego zaufania, zobacz [hostingu aplikacji ASP.NET w relacji zaufania średni](http://www.4guysfromrolla.com/articles/100307-1.aspx) na czterech Guys z Rolla witryny.

> [!div class="step-by-step"]
> [Poprzednie](project-properties.md)
> [dalej](setting-folder-permissions.md)
