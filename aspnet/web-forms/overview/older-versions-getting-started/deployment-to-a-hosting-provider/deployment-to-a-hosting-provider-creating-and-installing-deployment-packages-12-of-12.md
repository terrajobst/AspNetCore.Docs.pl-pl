---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Rozwiązywanie problemów z (12, 12) | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2923501289f31243a7341848ed3f7c2142c98e75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755916"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Rozwiązywanie problemów z (12, 12)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć do Windows Azure Web Sites, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Ta strona zawiera opis niektórych typowych problemów, które mogą wystąpić podczas wdrażania aplikacji sieci web ASP.NET za pomocą programu Visual Studio. Dla każdego z nich znajdują się możliwe przyczyny i odpowiednie rozwiązania.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Błąd serwera w aplikacji» /» — bieżące ustawienia błędów niestandardowych uniemożliwiają szczegóły błędu przeglądanie zdalnie

### <a name="scenario"></a>Scenariusz

Po wdrożeniu witryny z hostem zdalnym, otrzymasz komunikat o błędzie, który wymienia ustawienie customErrors w pliku Web.config, ale nie określa, rzeczywista Przyczyna błędu został:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program ASP.NET zawiera szczegółowe informacje o błędzie, tylko wtedy, gdy aplikacja sieci web jest uruchomiona na komputerze lokalnym. Zwykle nie chcesz wyświetlić szczegółowe informacje o błędzie, gdy aplikacja sieci web jest publicznie dostępny przez Internet, ponieważ przed hakerami może użyć tych informacji można znaleźć luki w zabezpieczeniach w aplikacji. Jednak w przypadku wdrażania witryny lub aktualizacji do lokacji, czasem coś pójdzie źle i zajdzie potrzeba przywrócenia rzeczywistym komunikacie o błędzie.

Aby włączyć tę aplikację wyświetlić szczegółowe komunikaty o błędach, gdy działa na hoście zdalnym, należy edytować plik Web.config można ustawić `customErrors` trybu wyłączone, ponownego wdrażania aplikacji i ponownie uruchomić aplikację:

1. Jeśli plik Web.config ma `customErrors` element `system.web` elementu, zmiana `mode` atrybutu na "wyłączone". W przeciwnym razie Dodaj `customErrors` element `system.web` element z `mode` atrybutu na "wyłączone", jak pokazano w poniższym przykładzie:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Wdróż aplikację.
3. Uruchom aplikację, a następnie powtórz, niezależnie od rodzaju miało to miejsce wcześniej powodujący błąd występuje. Możesz teraz zobaczyć, co to jest rzeczywistym komunikacie o błędzie.
4. Ten błąd został rozwiązany, przywrócić oryginalny `customErrors` ustawienie i ponownego wdrażania aplikacji.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Odmowa dostępu na stronie sieci Web, używa programu SQL Server Compact

### <a name="scenario"></a>Scenariusz

Wdrażając lokacji korzystającej z programu SQL Server Compact i uruchomić strony w witrynie wdrożone, który uzyskuje dostęp do bazy danych, zobaczysz następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto usługi SIECIOWEJ na serwerze musi mieć możliwość odczytu usługa SQL Compact natywnych plików binarnych, które znajdują się w *bin\amd64* lub *bin\x86* folder, ale nie masz uprawnienia do odczytu tych folderów. Ustaw uprawnienia do odczytu dla usługi SIECIOWEJ *bin* folderu, upewniając się rozszerzyć uprawnień do jego podfolderach.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nie można odczytać pliku konfiguracji z powodu niewystarczających uprawnień

### <a name="scenario"></a>Scenariusz

Po kliknięciu programu Visual Studio publikowania przycisk, aby wdrożyć aplikację w usługach IIS na komputerze lokalnym, publikowanie kończy się niepowodzeniem i **dane wyjściowe** okno pokazuje komunikat o błędzie podobny do następującego:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Aby użyć jednym kliknięciem publikować w usługach IIS na komputerze lokalnym, musi być uruchomiony program Visual Studio z uprawnieniami administratora. Zamknij program Visual Studio, a następnie uruchom go ponownie z uprawnieniami administratora.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Nie można nawiązać połączenia z komputerem docelowym... Przy użyciu określonego procesu

### <a name="scenario"></a>Scenariusz

Po kliknięciu programu Visual Studio publikowania przycisk, aby wdrożyć aplikację, publikowanie kończy się niepowodzeniem i **dane wyjściowe** okno pokazuje komunikat o błędzie podobny do następującego:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer proxy jest przerywania komunikacji z serwerem docelowym. W Panelu sterowania Windows lub w programie Internet Explorer, wybierz **Opcje internetowe** i wybierz **połączeń** kartę. W **właściwości internetowe** okno dialogowe, kliknij przycisk **ustawienia sieci LAN**. W **ustawienia sieci lokalnej (LAN)** okno dialogowe wyczyść **Automatycznie wykryj ustawienia** pola wyboru. Następnie ponownie kliknij przycisk Publikuj.

Jeśli problem będzie się powtarzać, skontaktuj się z administratorem systemu, aby ustalić, co można zrobić za pomocą ustawień serwera proxy lub zapory. Problem występuje, ponieważ narzędzie Web Deploy używa niestandardowego portu dla wdrożenia usługi zarządzania siecią Web (8172); w przypadku innych połączeń narzędzia Web Deploy korzysta z portu 80. W przypadku wdrażania u dostawcy hostingu innych firm są zazwyczaj przy użyciu usługi zarządzania siecią Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Domyślna pula aplikacji 4.0 .NET nie istnieje.

### <a name="scenario"></a>Scenariusz

Gdy wdrażasz aplikację, która wymaga programu .NET Framework 4, zobaczysz następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

ASP.NET 4 nie jest zainstalowany w usługach IIS. Jeśli serwer, z którym jest wdrażany z jest komputer deweloperski i ma na nim zainstalowany program Visual Studio 2010, platformy ASP.NET 4 jest zainstalowana na komputerze, ale może nie być zainstalowane w usługach IIS. Na serwerze, na którym jest wdrażany z Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zainstalowanie programu ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Konieczne może również ręcznie ustawić .NET Framework w wersji domyślnej puli aplikacji. Aby uzyskać więcej informacji, zobacz [wdrażanie w usługach IIS jako środowisku testowym](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczka.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format ciągu inicjującego nie jest zgodny ze specyfikacją, rozpoczynając od indeksu 0.

### <a name="scenario"></a>Scenariusz

Po wdrożeniu aplikacji za pomocą jednego kliknięcia publikowania, po uruchomieniu strony, który uzyskuje dostęp do bazy danych otrzymasz następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Otwórz *Web.config* plik w witrynie wdrożone i sprawdź, czy wartości ciągu połączenia zaczynają się od `$(ReplacableToken_`, jak w poniższym przykładzie:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Jeśli parametry połączenia wyglądają następująco, Edytuj plik projektu i dodaj następującą właściwość do `PropertyGroup` element, który jest dla wszystkich kompilacji konfiguracji:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Następnie należy ponownie wdrożyć aplikację.

## <a name="http-500-internal-server-error"></a>HTTP 500 Wewnętrzny błąd serwera

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie, zobaczysz następujący komunikat o błędzie bez szczegółowe informacje wskazujące przyczynę błędu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Istnieje wiele przyczyn błędów 500, ale jest jedną z możliwych przyczyn Jeśli postępujesz zgodnie z tymi samouczkami, umieść XML element w niewłaściwym miejscu w jednym z plików transformacji XML. Na przykład, czy ten błąd Jeśli umieścisz transformacji, który wstawia `<location>` pod `<system.web>` zamiast bezpośrednio pod `<configuration>`. Rozwiązanie jest w takim przypadku Popraw plik przekształcenia XML i ponowne wdrażanie.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 wewnętrzny błąd serwera

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie, zobaczysz następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Witryna została wdrożona platformy ASP.NET 4, ale platformy ASP.NET 4 nie jest zarejestrowany w usługach IIS na serwerze obiektów docelowych. Na serwerze otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zarejestruj platformy ASP.NET 4, uruchamiając następujące polecenia:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Konieczne może również ręcznie ustawić .NET Framework w wersji domyślnej puli aplikacji. Aby uzyskać więcej informacji, zobacz [wdrażanie w usługach IIS jako środowisku testowym](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) samouczka.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Logowanie nie powiodło się otwieranie Express bazy danych SQL Server w aplikacji\_danych

### <a name="scenario"></a>Scenariusz

Możesz zaktualizować *Web.config* pliku parametrów połączenia, aby wskazywać bazę danych programu SQL Server Express jako *.mdf* plików w Twojej *aplikacji\_danych* folder, a pierwsza czas uruchamiania aplikacji, który zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Nazwa *.mdf* pliku nie może odpowiadać nazwie dowolnego programu SQL Server Express bazy danych, która nigdy nie istniało na komputerze, nawet, jeśli usunięto *.mdf* pliku wcześniej istniejącej bazy danych. Zmień nazwę *.mdf* pliku nazwę, która nie była nigdy używana jako nazwa bazy danych i zmiana *Web.config* pliku do użycia nowej nazwy. Alternatywnie, można użyć [programu SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) do usuwania istniejących programu SQL Server Express bazy danych.

## <a name="model-compatibility-cannot-be-checked"></a>Model zgodności nie można sprawdzić

### <a name="scenario"></a>Scenariusz

Możesz zaktualizować *Web.config* pliku parametrów połączenia, aby wskazywał nową bazę danych programu SQL Server Express i uruchomić aplikację po raz pierwszy, zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Jeśli nazwa bazy danych, które można umieścić w pliku Web.config nigdy nie było używane, zanim na komputerze bazy danych może już istnieć niektórych tabel w nim. Wybierz nową nazwę, który nie został użyty na Twoim komputerze przed i zmień *Web.config* pliku w celu wskazania, aby użyć tej nowej nazwy bazy danych. Alternatywnie, można użyć [programu SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) lub [programu SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) można usunąć istniejącej bazy danych.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Błąd SQL podczas próby tworzenia użytkowników lub ról skryptu

### <a name="scenario"></a>Scenariusz

W przypadku korzystania z bazy danych wdrożenia skonfigurowane na **Pakuj/Publikuj SQL** karcie skrypty SQL, które są uruchamiane podczas wdrażania obejmują polecenia Create User lub Utwórz rolę i wykonywania skryptu zakończy się niepowodzeniem podczas te polecenia są wykonywane. Możesz zobaczyć bardziej szczegółowe komunikaty, takie jak następujące:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Jeśli ten błąd występuje, gdy skonfigurowano wdrożenie bazy danych w **publikowanie w sieci Web** kreatora, a nie od **Pakuj/Publikuj SQL** pozycję Utwórz wątek na [konfiguracji i Wdrożenie](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum i rozwiązania zostaną dodane do tej strony, rozwiązywania problemów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto użytkownika, którego używasz do wykonywania wdrożenia nie ma uprawnień do tworzenia użytkowników lub ról. Na przykład może przypisywać firmy hostingowej `db_datareader`, `db_datawriter`, i `db_ddladmin` ról do konta użytkownika, który konfiguruje dla Ciebie. Te są wystarczające do tworzenia większość obiektów bazy danych, ale nie do tworzenia użytkowników lub ról. Jednym ze sposobów, aby uniknąć tego błędu jest wykluczenie z użytkownikami i rolami z wdrożenie bazy danych. Można to zrobić, edytując `PreSource` elementu dla bazy danych użytkownika generowane automatycznie skrypt, aby obejmowała następujące atrybuty:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Aby uzyskać informacje o sposobie edytowania `PreSource` elementu w pliku projektu, zobacz [jak: edytowanie ustawień wdrożenia w pliku projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Jeśli użytkownicy lub role w bazie danych programu rozwoju muszą znajdować się w docelowej bazie danych, skontaktuj się z dostawcą hostingu.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server błąd upływu limitu czasu podczas uruchamiania niestandardowych skryptów podczas wdrażania

### <a name="scenario"></a>Scenariusz

Określono niestandardowe skrypty SQL do uruchomienia podczas wdrożenia i uruchomienie narzędzia Web Deploy je, ich przekroczyło limit czasu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Uruchamianie wiele skryptów, które mają tryby innej transakcji może spowodować błędy przekroczenia limitu czasu. Domyślnie automatycznie generowanych skrypty uruchamiane w ramach transakcji, ale nie obsługują niestandardowych skryptów. Jeśli wybierzesz **pobierania danych i/lub schemat z istniejącej bazy danych** opcja **Pakuj/Publikuj SQL** kartę, jeśli dodasz niestandardowy skrypt SQL, musisz zmienić ustawienia transakcji na niektóre skrypty tak, aby wszystkie skrypty używają tych samych ustawień transakcji. Aby uzyskać więcej informacji, zobacz [jak: wdrożyć bazę danych z projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465343.aspx).

Jeśli skonfigurowano ustawienia transakcji, tak aby wszystkie są takie same, ale ten błąd się nadal pojawiać, możliwym obejściem jest na uruchamianie skryptów oddzielnie. W **skryptów bazy danych** siatki w **Pakuj/Publikuj** karta SQL, wyczyść **Include** pole wyboru dla skryptu, który powoduje przekroczenie limitu czasu, a następnie opublikowanie projektu. Następnie przejdź do **skryptów bazy danych** siatki, wybierz ten skrypt **Include** pole wyboru, a następnie wyczyść **Include** pola wyboru dla innych skryptów. Następnie opublikujesz projekt ponownie. Tym razem po opublikowaniu, tylko wybrane niestandardowy skrypt będzie uruchamiany.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Data Stream manifestu lokacji nie jest jeszcze dostępna

### <a name="scenario"></a>Scenariusz

Jeśli podczas instalowania pakietu przy użyciu *pliku deploy.cmd* plik z `t` opcja (test), zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Komunikat o błędzie oznacza, że polecenie nie może wygenerować raport z testu. Jednak polecenie może działać, jeśli używasz `y` opcji (rzeczywista instalacja). Komunikat wskazuje tylko, że występuje problem z uruchamianiem polecenia w trybie testowym.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Ta aplikacja wymaga ManagedRuntimeVersion w wersji 4.0

### <a name="scenario"></a>Scenariusz

Podczas próby wdrożenia, zobaczysz następujący komunikat o błędzie:

 Błąd: Strumień danych "sitemanifest/dbFullSql [@path="C:\TEMP\AdventureWorksGrant.sql']/sqlScript"nie jest jeszcze dostępna. Puli aplikacji, które próbujesz użyć ma właściwość 'managedRuntimeVersion' do 'v2.0'. Ta aplikacja wymaga "v4.0". 

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

ASP.NET 4 nie jest zainstalowany w usługach IIS. Jeśli serwer, z którym jest wdrażany z jest komputer deweloperski i ma na nim zainstalowany program Visual Studio 2010, platformy ASP.NET 4 jest zainstalowana na komputerze, ale może nie być zainstalowane w usługach IIS. Na serwerze, na którym jest wdrażany z Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zainstalowanie programu ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nie można rzutować Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scenariusz

W przypadku wdrażania pakietu, zobaczysz następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Próbujesz wdrożyć z Menedżera usług IIS przy użyciu Interfejsu sieci Web do 1.1 wdrażanie na serwer, na którym jest narzędziu Web Deploy 2.0 został zainstalowany. Jeśli używasz narzędzia administracyjnego IIS zdalnego wdrożyć przez zaimportowanie pakietów, sprawdź **dostępność nowych funkcji** okno dialogowe po nawiązaniu połączenia. (To okno dialogowe może być wyświetlane tylko raz po raz pierwszy nawiązywane jest połączenie. Aby wyczyścić połączenie i rozpocząć od nowa, Zamknij Menedżera usług IIS i uruchomić go ponownie, wprowadzając `inetmgr /reset` w wierszu polecenia.) Jeśli jedna z funkcji na liście jest **interfejs użytkownika sieci Web wdrażanie**, ma numer wersji jest niższy niż 8, serwer wdrażania może być wersji 1.1 i 2.0 zainstalowane narzędzia Web Deploy. Aby wdrożyć z klienta, który został zainstalowany w wersji 2.0, serwer musi mieć tylko narzędziu Web Deploy 2.0 zainstalowany. Należy skontaktować się z dostawcą hostingu, aby rozwiązać ten problem.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nie można załadować macierzystego składniki programu SQL Server Compact

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie, zobaczysz następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wdrożone lokacji nie ma *amd64* i *x86* podfoldery z natywnych zestawów w nich w ramach aplikacji *bin* folderu. Na komputerze, na którym jest zainstalowany program SQL Server Compact, zestawów natywnych znajdują się w *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. To najlepszy sposób pozyskania poprawne pliki na prawidłowe foldery w projekcie programu Visual Studio można zainstalować pakietu NuGet SqlServerCompact. Instalacja pakietu dodaje skrypt po kompilacji, aby kopiowanie natywnych zestawów do *amd64* i *x86*. Aby te, które mają zostać wdrożone należy jednak ręcznie je uwzględnić w projekcie. Aby uzyskać więcej informacji, zobacz [wdrażania programu SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) samouczka.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Ścieżka jest nieprawidłowa" Błąd po wdrożeniu aplikacji Entity Framework Code First

### <a name="scenario"></a>Scenariusz

Wdrażanie aplikacji, która używa migracje Code First Framework jednostki i DBMS, takich jak SQL Server Compact, która przechowuje swojej bazy danych w pliku w aplikacji\_folderu danych. Masz migracje Code First skonfigurowany tak, aby utworzyć bazę danych po wdrożeniu pierwszego. Po uruchomieniu aplikacji, otrzymasz komunikat o błędzie, jak w poniższym przykładzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Kod najpierw próbuje utworzyć bazy danych, ale aplikacja\_danych folder nie istnieje. Albo nie masz żadnych plików w *aplikacji\_danych* folderu, gdy wdrożono lub wybrano **wykluczanie aplikacji\_danych** na **pakowaniu/publikowaniu Web** karcie **właściwości projektu** okna. Proces wdrażania nie utworzy folder na serwerze, jeśli są połączone żadne pliki w folderze do skopiowania do serwera. Jeśli masz już bazę danych w witrynie, proces wdrażania spowoduje usunięcie plików i *aplikacji\_danych* sam w przypadku wybrania folder **Usuń dodatkowe pliki w lokalizacji docelowej** w profil publikowania. Aby rozwiązać ten problem, umieść plik symbolu zastępczego, takich jak plik z rozszerzeniem txt w *aplikacji\_danych* folderu, upewnij się, że nie masz **wykluczanie aplikacji\_danych** zaznaczone, a następnie ponownie wdrożyć. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nie można użyć obiektu COM, który został rozdzielony ze podstawowego RCW."

### <a name="scenario"></a>Scenariusz

Zostały pomyślnie za pomocą jednego kliknięcia publikowanie do wdrażania aplikacji i następnie uruchom ten błąd:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Zamknięcie i ponowne uruchomienie programu Visual Studio jest zazwyczaj wszystko, co jest wymagane, aby rozwiązać ten problem.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Wdrożenia nie powiodło się ponieważ użytkownika poświadczenia używane do publikowania nie mają setACL urzędu

### <a name="scenario"></a>Scenariusz

Publikowania zakończy się niepowodzeniem z powodu błędu, który oznacza, że nie ma uprawnienia do ustawiania uprawnień do folderu (konto użytkownika, którego używasz nie ma setACL urzędu).

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie, Visual Studio, zestawy uprawnień w folderze głównym lokacji uprawnienia odczytu i zapisu w aplikacji\_folderu danych. Jeśli wiesz, że domyślne uprawnienia dla folderów witrynie są prawidłowe oraz że nie należy ustawić, możesz wyłączyć to zachowanie, dodając **&lt;docelowy IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** plik profilu publikowania (mieć wpływ na jeden profil) lub pliku wpp.targets (mieć wpływ na wszystkie profile). Aby uzyskać informacje o tym, jak edytowanie tych plików, zobacz [porady: edytowanie ustawień wdrażania plików profilu (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Błędy odmowy dostępu, gdy aplikacja próbuje zapisać do folderu aplikacji

### <a name="scenario"></a>Scenariusz

Błędy aplikacji podczas próby tworzenia lub edytowania pliku w jednym z folderów aplikacji, ponieważ nie ma uprawnienia zapisu dla tego folderu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie, Visual Studio, zestawy uprawnień w folderze głównym lokacji uprawnienia odczytu i zapisu w aplikacji\_folderu danych. Jeśli aplikacja wymaga dostępu do podfolderu zapisu, można ustawić uprawnienia dla tego folderu, jak pokazano w [ustawiania uprawnień do folderu](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) i [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczków. Jeśli aplikacja wymaga uprawnienia do zapisu w folderze głównym lokacji, należy uniemożliwić ustawienie dostęp tylko do odczytu w folderze głównym, dodając **&lt;docelowy IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** plik profilu publikowania (mieć wpływ na jeden profil) lub pliku wpp.targets (mieć wpływ na wszystkie profile). Aby uzyskać informacje o tym, jak edytowanie tych plików, zobacz [porady: edytowanie ustawień wdrażania plików profilu (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Błąd konfiguracji — atrybut targetFramework odwołuje się do wersji, która jest nowsza niż zainstalowana wersja programu .NET Framework

### <a name="scenario"></a>Scenariusz

Została pomyślnie opublikowana projektu sieci web, który jest przeznaczony dla platformy ASP.NET 4.5, ale po uruchomieniu aplikacji (przy użyciu `customErrors` tryb jest ustawiony na "wyłączone" w pliku Web.config) występuje następujący błąd:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Błąd źródła strony błędu wyróżnione następujący wiersz z pliku Web.config jako przyczynę błędu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer nie obsługuje ASP.NET 4.5. Skontaktuj się z dostawcy hostingu, aby określić, kiedy i w przypadku pomocy technicznej dla platformy ASP.NET 4.5 można dodać. W przypadku uaktualniania serwera nie jest opcją, musisz wdrożyć projekt sieci web, który jest przeznaczony dla platformy ASP.NET 4 lub wersji wcześniejszych zamiast tego. W przypadku wdrożenia programu ASP.NET 4 lub wcześniejszej projektu sieci web do tego samego miejsca docelowego, wybierz **Usuń dodatkowe pliki w lokalizacji docelowej** pole wyboru na **ustawienia** karcie **publikowanie w sieci Web**kreatora. Jeśli nie zaznaczysz **Usuń dodatkowe pliki w lokalizacji docelowej**, nadal będzie wyświetlona strona błędu konfiguracji.

Projekt **właściwości** systemu windows zawiera listy rozwijanej Target framework, ale nie może rozwiązać ten problem, wystarczy zmienić z **.NET Framework 4.5** do **.NET Framework 4**. Zmiana platformy docelowej wcześniejszą wersję framework projektu będą nadal mieć odwołania do zestawów w nowszej wersji framework i nie będzie działać. Należy ręcznie zmienić te odwołania lub Utwórz nowy projekt, który jest przeznaczony dla .NET Framework 4 lub wersji wcześniejszych. Aby uzyskać więcej informacji, zobacz [.NET Framework Ustawianie elementów docelowych dla witryn sieci Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
