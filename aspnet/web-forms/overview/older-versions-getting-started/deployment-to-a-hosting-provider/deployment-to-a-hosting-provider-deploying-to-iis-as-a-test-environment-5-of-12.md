---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie w usługach IIS jako środowisku testowym - 5 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 70bc1b8e3c5d01470c2cebfbabd0c119e8f6d360
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394719"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie w usługach IIS jako środowisku testowym - 5 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób wdrażania aplikacji sieci web platformy ASP.NET w usługach IIS na komputerze lokalnym.

Podczas opracowywania aplikacji zwykle testu przez uruchomienie jej w programie Visual Studio. Domyślnie oznacza to, że używasz serwera wdrożeniowego usługi Visual Studio (znanego także jako Cassini). Ułatwia on serwera wdrożeniowego programu Visual Studio, który umożliwia testowanie podczas tworzenia w programie Visual Studio, ale nie działa tak samo jak usługi IIS. W rezultacie jest to możliwe, że aplikacja będzie działać poprawnie po ją przetestować w programie Visual Studio, ale się nie powieść, gdy aplikacja jest wdrożona w środowisku hostingu usług IIS.

Bardziej niezawodnie przetestować aplikację w następujący sposób:

1. Użyj usług IIS Express lub pełnej IIS zamiast serwera wdrożeniowego programu Visual Studio, podczas testowania w programie Visual Studio podczas programowania. Ta metoda jest ogólnie emuluje dokładniej działanie witryny w usługach IIS. Ta metoda nie przetestować proces wdrażania lub, sprawdź poprawność wynik proces wdrażania będzie działać poprawnie.
2. Wdrażanie aplikacji w usługach IIS na komputerze deweloperskim, za pomocą tego samego procesu, którego będziesz później wdrożyć w środowisku produkcyjnym. Ta metoda sprawdza proces wdrażania oprócz sprawdzania poprawności, aplikacja będzie działać poprawnie w środowisku usług IIS.
3. Wdrażanie aplikacji w środowisku testowym, który jest możliwie jak najbardziej zbliżone do środowiska produkcyjnego. Ponieważ środowiska produkcyjnego potrzeby tych samouczków dostawcy hostingu innych firm, środowiska testowego idealne będzie drugiego konta za pomocą dostawcy hostingu. Należy użyć tego drugiego konta tylko do celów testowych, ale będzie można skonfigurować taki sam sposób jak konta w środowisku produkcyjnym.

Ten samouczek przedstawia kroki odpowiednie dla opcji 2. Wskazówki dotyczące opcji 3 znajduje się na końcu [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczek, a na końcu tego samouczka istnieją linki do zasobów, dla opcji 1.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurowanie aplikacji do wykonywania w trybie średniego zaufania

Przed zainstalowaniem usług IIS i wdrażania do niego, zmienisz ustawienia pliku Web.config w celu uruchamiało się w witrynie więcej takich, jak będą wykonywane następujące czynności w typowym środowisku współdzielonym hostingu.

Dostawcy hostingu zazwyczaj są uruchamiane witryny sieci web *trybie średniego zaufania*, co oznacza, że istnieje kilka kwestii, które nie może wykonywać. Na przykład kod aplikacji nie może uzyskać dostępu do rejestru Windows i nie można czytać lub zapisywać pliki, które znajdują się poza hierarchii folderów aplikacji. Domyślnie aplikacja działa w *wysokiego zaufania* na komputerze lokalnym, co oznacza, że aplikacja może być możliwość wykonywania czynności, które będą się kończyć niepowodzeniem podczas wdrażania do środowiska produkcyjnego. W związku z tym aby utworzyć środowisko testowe, które więcej dokładnie odzwierciedlają w środowisku produkcyjnym, należy skonfigurować aplikację do uruchamiania w trybie średniego zaufania.

W pliku Web.config aplikacji, Dodaj **zaufania** element **system.web** elementu, jak pokazano w poniższym przykładzie.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Aplikacja będzie teraz działać w trybie średniego zaufania w usługach IIS nawet na komputerze lokalnym. To ustawienie umożliwia catch możliwie jak najszybciej wszelkie mieszczące przez kod aplikacji coś zrobić, który może zakończyć się niepowodzeniem w środowisku produkcyjnym.

> [!NOTE]
> Jeśli używasz migracje Code First Framework jednostki, upewnij się, że w wersji 5.0 lub nowszy zainstalowany. Platformy Entity Framework w wersji 4.3 migracje wymaga pełnego zaufania, aby można było zaktualizować schemat bazy danych.


## <a name="installing-iis-and-web-deploy"></a>Instalowanie usług IIS i sieci Web wdrażanie

Aby wdrożyć w usługach IIS na komputerze deweloperskim, konieczne jest posiadanie usług IIS i zainstalowane narzędzie Web Deploy. Nie są one uwzględnione w domyślnej konfiguracji programu Windows 7. Jeśli już zainstalowano program IIS i narzędzia Web Deploy, przejdź do następnej sekcji.

Za pomocą [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx) jest preferowanym sposobem instalowania programu IIS i narzędzia Web Deploy, ponieważ Instalator platformy sieci Web instaluje to zalecana konfiguracja usług IIS i automatycznie instaluje wymagania wstępne dotyczące sieci Web i usług IIS Wdrażanie, jeśli to konieczne.

Aby uruchomić Instalatora platformy sieci Web, aby zainstalować usługi IIS i narzędzia Web Deploy, użyj następującego linku. Jeśli została już zainstalowana IIS, narzędzie Web Deploy lub dowolnej ich wymagane składniki, Instalator platformy sieci Web instaluje tylko co to jest Brak.

- [Instalowanie usług IIS i narzędzia Web Deploy, za pomocą Instalatora WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Ustawianie domyślnej puli aplikacji .NET 4

Po zainstalowaniu usług IIS, należy uruchomić **Menedżera usług IIS** aby upewnić się, że .NET Framework w wersji 4 są przypisane do domyślnej puli aplikacji.

Od Windows **Start** menu, wybierz opcję **Uruchom**wprowadź "inetmgr", a następnie kliknij przycisk **OK**. (Jeśli **Uruchom** polecenia nie znajduje się w Twojej **Start** menu, można nacisnąć klawisz Windows i języka R, aby go otworzyć. Lub kliknij prawym przyciskiem myszy pasek zadań, kliknij przycisk **właściwości**, wybierz opcję **Start Menu** kliknij pozycję **Dostosuj**i wybierz **Uruchom polecenie**.)

W **połączeń** okienku rozwiń węzeł serwera i wybierz **pul aplikacji**. W **pul aplikacji** okienko, jeśli **DefaultAppPool** jest przypisany do programu .NET framework w wersji 4 tak jak na poniższej ilustracji, przejdź do następnej sekcji.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Jeśli zostanie wyświetlony tylko dwie pule aplikacji i z nich są ustawione na .NET Framework 2.0, musisz zainstalować platformy ASP.NET 4 w usługach IIS:

- Otwórz okno wiersza polecenia, kliknij prawym przyciskiem myszy **polecenia** w Windows **Start** menu i wybierając polecenie **Uruchom jako Administrator**. Następnie uruchom [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) do zainstalowania programu ASP.NET 4 w usługach IIS, używając następujących poleceń. (W systemach 64-bitowych, zamiast "Framework" z "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    To polecenie umożliwia utworzenie nowej puli aplikacji dla programu .NET Framework 4, ale domyślnej puli aplikacji będzie nadal ustawiony w wersji 2.0. Możesz będzie można wdrażanie aplikacji przeznaczonego .NET 4 dla tej puli aplikacji, więc trzeba będzie zmienić pulę aplikacji .NET 4.

Jeśli został zamknięty **Menedżera usług IIS**, uruchom ją ponownie, rozwiń węzeł serwera, a następnie kliknij przycisk **pul aplikacji** do wyświetlenia **pul aplikacji** okienko ponownie.

W **pul aplikacji** okienku kliknij **DefaultAppPool**, a następnie w polu **akcje** okienku kliknij pozycję **podstawowych ustawień**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

W **edytowanie puli aplikacji** okno dialogowe, zmiana **.NET Framework w wersji** do **4.0.30319 .NET Framework** i kliknij przycisk **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Teraz można przystąpić do opublikowania w usługach IIS.

## <a name="publishing-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka sposobów, które można wdrożyć przy użyciu programu Visual Studio 2010 i narzędzia Web Deploy:

- Za pomocą programu Visual Studio publikowanie jednym kliknięciem.
- Tworzenie *pakietu wdrożeniowego* i zainstaluj go przy użyciu interfejsu użytkownika Menedżera usług IIS. Pakiet wdrożeniowy, który składa się z *zip* pliku, który zawiera wszystkie pliki i metadane wymagane do przeprowadzenia instalacji lokacji w usługach IIS.
- Utwórz pakiet wdrożeniowy i zainstaluj go przy użyciu wiersza polecenia.

Proces, który zostały kolejno przerobione w poprzednich samouczkach, aby skonfigurować program Visual Studio do automatyzowania zadań wdrażania, który ma zastosowanie do wszystkich tych trzech metod. W tych samouczkach użyjemy pierwsza z tych metod. Aby dowiedzieć się, jak za pomocą pakietów wdrażania, zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521.aspx).

Przed opublikowaniem, upewnij się, że używasz programu Visual Studio w trybie administratora. (W Windows 7 **Start** menu, kliknij prawym przyciskiem myszy ikonę dla wersji programu Visual Studio używasz i wybierz **Uruchom jako Administrator**.) Tryb administratora jest wymagana do publikowania, tylko gdy publikujesz do usług IIS na komputerze lokalnym.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt ContosoUniversity (a nie projekt ContosoUniversity.DAL) i wybierz **Publikuj**.

**Publikowanie w sieci Web** pojawi się Kreator.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Z listy rozwijanej wybierz  **&lt;nowy... &gt;**.

W **nowy profil** okno dialogowe, wprowadź "Test", a następnie kliknij przycisk **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Ta nazwa jest taka sama jak środkowej węzeł Web.Test.config Przekształcanie pliku, który został utworzony wcześniej. Ta zgodność jest o tym, co powoduje, że przekształcenia Web.Test.config mają być stosowane podczas publikowania za pomocą tego profilu.

Kreator automatycznie prowadzi do **połączenia** kartę.

W **adres URL usługi** wprowadź *localhost*.

W **witryny/aplikacji** wprowadź *domyślnej witryny sieci Web/ContosoUniversity*.

W **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity`.

**Docelowy adres URL** ustawienie nie jest wymagane. Po zakończeniu wdrażania aplikacji programu Visual Studio automatycznie otwiera domyślnej przeglądarki do tego adresu URL. Jeśli nie chcesz, aby otwierać automatycznie po wdrożeniu przeglądarka, pozostaw to pole puste.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Kliknij przycisk **Waliduj połączenie** można sprawdzić, czy ustawienia są poprawne i czy możesz nawiązać połączenie usług IIS na komputerze lokalnym.

Zielony znacznik wyboru sprawdza, czy połączenie zostanie nawiązane.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Kliknij przycisk **dalej** celu przechodzenia do **ustawienia** kartę.

**Konfiguracji** pole listy rozwijanej określa konfigurację kompilacji, aby wdrożyć. Wartość domyślna to wydanie, które chcesz.

Pozostaw **Usuń dodatkowe pliki w lokalizacji docelowej** pole wyboru jest wyczyszczone. Ponieważ jest to pierwszy wdrożenia, nie będą wszystkie pliki w folderze docelowym jeszcze.

W **baz danych** sekcji, wprowadź następującą wartość w polu ciągu połączenia dla **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Proces wdrażania będzie umieścić te parametry połączenia w wdrożonym pliku Web.config, ponieważ **Użyj tych parametrów połączenia w czasie wykonywania** jest zaznaczone.

Również w **SchoolContext**, wybierz opcję **zastosować migracje Code First**. Ta opcja powoduje, że proces wdrażania i konfigurowania wdrożonym pliku Web.config w celu określenia `MigrateDatabaseToLatestVersion` inicjatora. Gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu tego inicjatora automatycznie aktualizuje bazę danych do najnowszej wersji.

W polu ciągu połączenia dla **DefaultConnection**, wprowadź następującą wartość:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Pozostaw **Aktualizuj bazę danych** wyczyszczone. Bazy danych członkostwa będą wdrażane przez skopiowanie pliku .sdf w aplikacji\_danych, a nie chcesz, aby proces wdrażania czegoś jeszcze z tej bazy danych.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Kliknij przycisk **dalej** celu przechodzenia do **Podgląd** kartę.

W **Podgląd** kliknij pozycję **Uruchom Podgląd** umożliwia wyświetlenie listy plików, które zostaną skopiowane.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Kliknij przycisk **publikowania**.

Jeśli program Visual Studio nie jest w trybie administratora, możesz otrzymać komunikat o błędzie wskazujący błąd uprawnień. W takim przypadku zamknięcie programu Visual Studio, otwórz go w trybie administratora i spróbuj opublikować ponownie.

Jeśli program Visual Studio jest w trybie administratora **dane wyjściowe** pomyślne raporty okna tworzysz i publikujesz.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Przeglądarka automatycznie zostanie otwarty na stronie głównej University Contoso systemem w usługach IIS na komputerze lokalnym.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testowanie w środowisku testowym

Należy zauważyć, że wskaźnik środowisko zawiera "(Test)" zamiast "(Dev)", który wskazuje, że *Web.config* transformacji dla wskaźnika środowisko zakończyło się pomyślnie.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Uruchom **studentów** strony, aby sprawdzić, czy wdrożonej bazy danych jest nie studentów. Po wybraniu tej strony może potrwać kilka minut, aby załadować, ponieważ Code First tworzy bazę danych, a następnie uruchamia `Seed` metody. (Go nie wykonać, jeśli zostały na stronie głównej, ponieważ aplikacja nie próbuje jeszcze dostępu do bazy danych).

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Uruchom **Instruktorzy** stronę, aby sprawdzić, czy Code First zasilany w bazie danych przez instruktorów:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Wybierz **dodać uczniów** z **studentów** menu Dodaj uczniem, a następnie wyświetlić nowego studenta w **studentów** stronę, aby sprawdzić, czy można pomyślnie zapisać do bazy danych :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Z **kursów** menu, wybierz opcję **aktualizacji środki na korzystanie z**. **Aktualizacji środki na korzystanie z** strony musi mieć uprawnienia administratora, więc **logowanie** zostanie wyświetlona strona. Wprowadź poświadczenia konta administratora, utworzony wcześniej ("admin" i "Pas$ w0rd"). **Aktualizacji środki na korzystanie z** zostanie wyświetlona strona, która sprawdza, czy konto administratora, który został utworzony w poprzednim samouczku poprawnie została wdrożona do środowiska testowego.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Upewnij się, że *Elmah* istnieje folder z plikiem w symbol zastępczy w nim.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przeglądanie zmian automatyczne pliku Web.config na potrzeby migracji Code First

Otwórz *Web.config* plik w aplikacji wdrożonej w *C:\inetpub\wwwroot\ContosoUniversity* i zobaczyć, gdzie w procesie wdrażania skonfigurowane migracje Code First, aby automatycznie aktualizacji bazy danych do najnowszej wersji.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Proces wdrażania jest tworzona nowe parametry połączenia dla migracje Code First do użycia wyłącznie w celu zaktualizowania schematu bazy danych:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Te dodatkowe parametry można określić jedno konto użytkownika dla aktualizacje schematu bazy danych i inne konto użytkownika uzyskać dostęp do danych aplikacji. Na przykład można przypisać bazy danych\_roli właściciel do migracje Code First i db\_datareader i db\_datawriter role w aplikacji. Jest to typowy wzorzec ochronę w głębi, który uniemożliwia potencjalnie złośliwego kodu w aplikacji zmianę schematu bazy danych. (Na przykład może się to zdarzyć w pomyślnym ataku polegającego na iniekcji SQL.) Ten wzorzec nie jest używany przez te samouczki. Nie ma zastosowania do programu SQL Server Compact i nie ma zastosowania w przypadku migracji do programu SQL Server później w samouczku z tej serii. Witryna Cytanium zawiera tylko jedno konto użytkownika do uzyskiwania dostępu do bazy danych programu SQL Server, którą tworzysz na Cytanium. Jeśli jesteś w stanie implementacja tego wzorca, w tym scenariuszu, możesz zrobić to, wykonując następujące czynności:

1. W **ustawienia** karcie **publikowanie w sieci Web** kreatora wprowadź parametry połączenia, który określa użytkownika z uprawnieniami do aktualizacji schematu pełnej bazy danych, a następnie wyczyść **Użyj tych parametrów połączenia w czasie wykonywania** pole wyboru. W wdrożonym pliku Web.config, staje się on `DatabasePublish` parametry połączenia.
2. Utwórz przekształcenia pliku Web.config dla parametrów połączenia, który ma aplikacji do użycia w czasie wykonywania.

Masz teraz wdrożonych aplikacji usług IIS na komputerze deweloperskim i przetestowano ją tam. Sprawdza to proces wdrażania skopiować zawartość aplikacji do odpowiedniej lokalizacji (z wyjątkiem plików, które nie miały wdrażania) i również narzędzia Web Deploy IIS poprawnie skonfigurowany podczas wdrażania. W następnym samouczku uruchomisz jeden więcej test, który umożliwia znalezienie zadanie wdrożenia, które jeszcze nie zostało wykonane: Ustawianie uprawnień do folderów na *Elmah* folderu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacji na temat uruchamiania usług IIS lub IIS Express w programie Visual Studio zobacz następujące zasoby:

- [Omówienie usługi IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.net w witrynie.
- [Wprowadzenie do usług IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) w blogu Scotta Guthrie.
- [Porady: Określanie serwera sieci Web dla projektów sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Podstawowe różnice między usług IIS i programem ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie programu ASP.NET.
- [Testowanie usługi ASP.NET MVC ani aplikacji formularzy sieci Web w usługach IIS 7 w ciągu 30 sekund](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) na blogu Ricka Andersona. Ten wpis zawiera przykłady Dlaczego testowanie za pomocą serwera wdrożeniowego usługi Visual Studio (Cassini) nie jest tak niezawodna jak testowanie w usługach IIS Express i dlaczego testowania w usługach IIS Express nie jest tak niezawodna jak testowanie w usługach IIS.

Aby uzyskać informacje o problemach, które mogą wystąpić, gdy aplikacja działa w trybie średniego zaufania, zobacz [hostingu aplikacji ASP.NET w relacji zaufania średni](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 Guys z Rolla witryny.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [dalej](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
