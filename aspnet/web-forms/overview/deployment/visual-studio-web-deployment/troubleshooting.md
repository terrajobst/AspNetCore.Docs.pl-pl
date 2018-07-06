---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Rozwiązywanie problemów z | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b2be5d2f5aa0b7628f7b9338c860593660ef5893
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830895"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Rozwiązywanie problemów
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


Ta strona zawiera opis niektórych typowych problemów, które mogą wystąpić podczas wdrażania aplikacji sieci web ASP.NET za pomocą programu Visual Studio. Dla każdego z nich znajdują się możliwe przyczyny i odpowiednie rozwiązania.

Scenariusze przedstawione się do platformy Azure i innych dostawców hostingu. Aby uzyskać więcej informacji na temat rozwiązywania problemów z aplikacjami sieci web w usłudze Azure App Service zobacz następujące zasoby:

- [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Monitoruj aplikacje sieci Web w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Ogłaszamy wydanie programu Windows Azure SDK w wersji 2.0 dla programu .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (ScottGu blog pokazuje, jak można pobrać dzienników diagnostycznych w programie Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Błąd serwera w aplikacji» /» — bieżące ustawienia błędów niestandardowych uniemożliwiają szczegóły błędu przeglądanie zdalnie

### <a name="scenario"></a>Scenariusz

Po wdrożeniu witryny z hostem zdalnym, otrzymasz komunikat o błędzie, który wymienia ustawienie customErrors w pliku Web.config, ale nie określa, rzeczywista Przyczyna błędu został:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program ASP.NET zawiera szczegółowe informacje o błędzie, tylko wtedy, gdy aplikacja sieci web jest uruchomiona na komputerze lokalnym. Zwykle nie chcesz wyświetlić szczegółowe informacje o błędzie, gdy aplikacja sieci web jest publicznie dostępny przez Internet, ponieważ przed hakerami może użyć tych informacji można znaleźć luki w zabezpieczeniach w aplikacji. Jednak w przypadku wdrażania witryny lub aktualizacji do lokacji, czasem coś pójdzie źle i zajdzie potrzeba przywrócenia rzeczywistym komunikacie o błędzie.

Aby włączyć tę aplikację wyświetlić szczegółowe komunikaty o błędach, gdy działa na hoście zdalnym, należy edytować plik Web.config, aby ustawić tryb customErrors off, ponownego wdrażania aplikacji i ponownie uruchomić aplikację:

1. Jeśli plik Web.config zawiera acustomErrors element w elemencie thesystem.web, należy zmienić atrybutu themode na "wyłączone". W przeciwnym razie Dodaj acustomErrors element w elemencie thesystem.web atrybutem themode ustawiony na "wyłączone", jak pokazano w poniższym przykładzie: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Wdróż aplikację.
3. Uruchom aplikację, a następnie powtórz, niezależnie od rodzaju miało to miejsce wcześniej powodujący błąd występuje. Możesz teraz zobaczyć, co to jest rzeczywistym komunikacie o błędzie.
4. Jeśli błąd został rozwiązany, przywrócić pierwotne ustawienia customErrors i ponownego wdrażania aplikacji.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Nie można utworzyć/w tle kopii "ContosoUniversity" ten plik już istnieje.

### <a name="scenario"></a>Scenariusz

Jeśli zostanie podjęta próba uruchomienia projektu w programie Visual Studio pojawi się Strona błędu z komunikatem, jak w poniższym przykładzie:

Błąd serwera w aplikacji» /». Nie można utworzyć/w tle kopii "ContosoUniversity" ten plik już istnieje.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Poczekaj chwilę i Odśwież przeglądarkę, lub lokacji należy ponownie skompilować i spróbuj ponownie uruchomić.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Odmowa dostępu na stronie sieci Web, używa programu SQL Server Compact

### <a name="scenario"></a>Scenariusz

Wdrażając lokacji korzystającej z programu SQL Server Compact i uruchomić strony w witrynie wdrożone, który uzyskuje dostęp do bazy danych, zobaczysz następujący komunikat o błędzie:

Odmowa dostępu. (Wyjątek od HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto usługi SIECIOWEJ na serwerze musi mieć możliwość odczytu usługa SQL Compact natywnych plików binarnych, które znajdują się w *bin\amd64* lub *bin\x86* folder, ale nie masz uprawnienia do odczytu tych folderów. Ustaw uprawnienia do odczytu dla usługi SIECIOWEJ *bin* folderu, upewniając się rozszerzyć uprawnień do jego podfolderach.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nie można odczytać pliku konfiguracji z powodu niewystarczających uprawnień

### <a name="scenario"></a>Scenariusz

Po kliknięciu programu Visual Studio publikowania przycisk, aby wdrożyć aplikację w usługach IIS na komputerze lokalnym, publikowanie kończy się niepowodzeniem i **dane wyjściowe** okno pokazuje komunikat o błędzie podobny do następującego:

Wystąpił błąd podczas odczytu pliku konfiguracji usług IIS "MACHINE/PRZEKIEROWYWANIE". Tożsamość, o których wykonanie tej operacji to... Błąd: Nie można odczytać pliku konfiguracji z powodu niewystarczających uprawnień.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Aby użyć jednym kliknięciem publikować w usługach IIS na komputerze lokalnym, musi być uruchomiony program Visual Studio z uprawnieniami administratora. Zamknij program Visual Studio, a następnie uruchom go ponownie z uprawnieniami administratora.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Nie można nawiązać połączenia z komputerem docelowym... Przy użyciu określonego procesu

### <a name="scenario"></a>Scenariusz

Po kliknięciu programu Visual Studio publikowania przycisk, aby wdrożyć aplikację, publikowanie kończy się niepowodzeniem i **dane wyjściowe** okno pokazuje komunikat o błędzie podobny do następującego:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer proxy jest przerywania komunikacji z serwerem docelowym. W Panelu sterowania Windows lub w programie Internet Explorer, wybierz **Opcje internetowe** i wybierz **połączeń** kartę. W **właściwości internetowe** okno dialogowe, kliknij przycisk **ustawienia sieci LAN**. W **ustawienia sieci lokalnej (LAN)** okno dialogowe wyczyść **Automatycznie wykryj ustawienia** pola wyboru. Następnie ponownie kliknij przycisk Publikuj.

Jeśli problem będzie się powtarzać, skontaktuj się z administratorem systemu, aby ustalić, co można zrobić za pomocą ustawień serwera proxy lub zapory. Problem występuje, ponieważ narzędzie Web Deploy używa niestandardowego portu dla wdrożenia usługi zarządzania siecią Web (8172); w przypadku innych połączeń narzędzia Web Deploy korzysta z portu 80. W przypadku wdrażania u dostawcy hostingu innych firm są zazwyczaj przy użyciu usługi zarządzania siecią Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Domyślna pula aplikacji 4.0 .NET nie istnieje.

### <a name="scenario"></a>Scenariusz

Gdy wdrażasz aplikację, która wymaga programu .NET Framework 4, zobaczysz następujący komunikat o błędzie:

Domyślna pula aplikacji programu .NET 4.0 nie istnieje lub nie można dodać aplikację. Sprawdź, czy program ASP.NET 4.0 jest zainstalowany na tym komputerze.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

ASP.NET 4 nie jest zainstalowany w usługach IIS. Jeśli serwer, z którym jest wdrażany z jest komputer deweloperski i ma na nim zainstalowany program Visual Studio 2010, platformy ASP.NET 4 jest zainstalowana na komputerze, ale może nie być zainstalowane w usługach IIS. Na serwerze, na którym jest wdrażany z Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zainstalowanie programu ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Konieczne może również ręcznie ustawić .NET Framework w wersji domyślnej puli aplikacji. Aby uzyskać więcej informacji zobacz Wdrażanie w usługach IIS jako środowisku testowym samouczek z tej serii.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format ciągu inicjującego nie jest zgodny ze specyfikacją, rozpoczynając od indeksu 0.

### <a name="scenario"></a>Scenariusz

Po wdrożeniu aplikacji za pomocą jednego kliknięcia publikowania, po uruchomieniu strony, który uzyskuje dostęp do bazy danych otrzymasz następujący komunikat o błędzie:

Format ciągu inicjującego nie jest zgodny ze specyfikacją, rozpoczynając od indeksu 0.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Otwórz *Web.config* plik w witrynie wdrożone i sprawdź, czy wartości ciągu połączenia zaczynają się od $(ReplacableToken\_, jak w poniższym przykładzie:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Jeśli parametry połączenia wyglądają następująco, Edytuj plik projektu i dodaj następującą właściwość do PropertyGroup — element, który jest w przypadku wszystkich konfiguracji kompilacji:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Następnie należy ponownie wdrożyć aplikację.

## <a name="http-500-internal-server-error"></a>HTTP 500 Wewnętrzny błąd serwera

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie, zobaczysz następujący komunikat o błędzie bez szczegółowe informacje wskazujące przyczynę błędu:

Błąd HTTP 500 — Wewnętrzny błąd serwera.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Istnieje wiele przyczyn błędów 500, ale jest jedną z możliwych przyczyn Jeśli postępujesz zgodnie z tymi samouczkami, umieść XML element w niewłaściwym miejscu w jednym z plików transformacji pliku Web.config. Na przykład, czy ten błąd Jeśli umieścisz transformacji, który wstawia &lt;lokalizacji&gt; pod &lt;system.web&gt; zamiast bezpośrednio pod &lt;konfiguracji&gt;. Aby sprawdzić, czy przekształcenia działają zgodnie z oczekiwaniami, można użyć funkcji Podgląd przekształcenia pliku Web.config. Rozwiązanie, jeśli okaże się przekształcenia zakodowaną niepoprawnie jest Popraw plik transformacji i ponowne wdrażanie. Jeśli błąd jest oczywiste, spróbuj zakomentowując przekształceń i ponownego wdrożenia, aby zobaczyć, który z nich jest przyczyną błędu 500.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 wewnętrzny błąd serwera

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie, zobaczysz następujący komunikat o błędzie:

Błąd HTTP 500.21 — wewnętrzny błąd serwera. Procedura obsługi "PageHandlerFactory-Integrated" ma zły modułu "ManagedPipelineHandler" na liście modułów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Witryna została wdrożona platformy ASP.NET 4, ale platformy ASP.NET 4 nie jest zarejestrowany w usługach IIS na serwerze obiektów docelowych. Na serwerze otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zarejestruj platformy ASP.NET 4, uruchamiając następujące polecenia:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Konieczne może również ręcznie ustawić .NET Framework w wersji domyślnej puli aplikacji. Aby uzyskać więcej informacji zobacz Wdrażanie w usługach IIS jako środowisku testowym samouczek z tej serii.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Logowanie nie powiodło się otwieranie Express bazy danych SQL Server w aplikacji\_danych

### <a name="scenario"></a>Scenariusz

Możesz zaktualizować *Web.config* pliku parametrów połączenia, aby wskazywać bazę danych programu SQL Server Express jako *.mdf* plików w Twojej *aplikacji\_danych* folder, a pierwsza czas uruchamiania aplikacji, który zostanie wyświetlony następujący komunikat o błędzie:

System.Data.SqlClient.SqlException: Nie można otworzyć bazy danych "DatabaseName" żądanego podczas logowania. Logowanie nie powiodło się.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Nazwa *.mdf* pliku nie może odpowiadać nazwie dowolnego programu SQL Server Express bazy danych, która nigdy nie istniało na komputerze, nawet, jeśli usunięto *.mdf* pliku wcześniej istniejącej bazy danych. Zmień nazwę *.mdf* pliku nazwę, która nie była nigdy używana jako nazwa bazy danych i zmiana *Web.config* pliku do użycia nowej nazwy. Alternatywnie, można użyć [programu SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) do usuwania istniejących programu SQL Server Express bazy danych.

## <a name="model-compatibility-cannot-be-checked"></a>Model zgodności nie można sprawdzić

### <a name="scenario"></a>Scenariusz

Możesz zaktualizować *Web.config* pliku parametrów połączenia, aby wskazywał nową bazę danych programu SQL Server Express i uruchomić aplikację po raz pierwszy, zostanie wyświetlony następujący komunikat o błędzie:

Nie można sprawdzić zgodności modelu, ponieważ baza danych nie zawiera metadanych modelu. Upewnij się, że dodano IncludeMetadataConvention konwencjami DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Jeśli nazwa bazy danych, które można umieścić w pliku Web.config nigdy nie było używane, zanim na komputerze bazy danych może już istnieć niektórych tabel w nim. Wybierz nową nazwę, który nie został użyty na Twoim komputerze przed i zmień *Web.config* pliku w celu wskazania, aby użyć tej nowej nazwy bazy danych. Alternatywnie, można użyć [programu SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) lub [programu SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) można usunąć istniejącej bazy danych.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Błąd SQL podczas próby tworzenia użytkowników lub ról skryptu

### <a name="scenario"></a>Scenariusz

W przypadku korzystania z bazy danych wdrożenia skonfigurowane na **Pakuj/Publikuj SQL** karcie skrypty SQL, które są uruchamiane podczas wdrażania obejmują polecenia Create User lub Utwórz rolę i wykonywania skryptu zakończy się niepowodzeniem podczas te polecenia są wykonywane. Możesz zobaczyć bardziej szczegółowe komunikaty, takie jak następujące:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Jeśli ten błąd występuje, gdy skonfigurowano wdrożenie bazy danych w **publikowanie w sieci Web** kreatora, a nie od **Pakuj/Publikuj SQL** pozycję Utwórz wątek na [konfiguracji i Wdrożenie](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum i rozwiązania zostaną dodane do tej strony, rozwiązywania problemów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto użytkownika, którego używasz do wykonywania wdrożenia nie ma uprawnień do tworzenia użytkowników lub ról. Na przykład przypisać bazy danych firmy hostingowej\_datareader, bazy danych\_datawriter i db\_ddladmin ról do konta użytkownika, który konfiguruje dla Ciebie. Te są wystarczające do tworzenia większość obiektów bazy danych, ale nie do tworzenia użytkowników lub ról. Jednym ze sposobów, aby uniknąć tego błędu jest wykluczenie z użytkownikami i rolami z wdrożenie bazy danych. Można to zrobić, edytując PreSource element dla bazy danych automatycznie wygenerowany skrypt, aby obejmowała następujące atrybuty:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Aby uzyskać informacje o sposobie edytowania PreSource elementu w pliku projektu, zobacz [porady: edytowanie ustawień wdrożenia w pliku projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Jeśli użytkownicy lub role w bazie danych programu rozwoju muszą znajdować się w docelowej bazie danych, skontaktuj się z dostawcą hostingu.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server błąd upływu limitu czasu podczas uruchamiania niestandardowych skryptów podczas wdrażania

### <a name="scenario"></a>Scenariusz

Określono niestandardowe skrypty SQL do uruchomienia podczas wdrożenia i uruchomienie narzędzia Web Deploy je, ich przekroczyło limit czasu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Uruchamianie wiele skryptów, które mają tryby innej transakcji może spowodować błędy przekroczenia limitu czasu. Domyślnie automatycznie generowanych skrypty uruchamiane w ramach transakcji, ale nie obsługują niestandardowych skryptów. Jeśli wybierzesz **pobierania danych i/lub schemat z istniejącej bazy danych** opcja **Pakuj/Publikuj SQL** kartę, jeśli dodasz niestandardowy skrypt SQL, musisz zmienić ustawienia transakcji na niektóre skrypty tak, aby wszystkie skrypty używają tych samych ustawień transakcji. Aby uzyskać więcej informacji, zobacz [jak: wdrożyć bazę danych z projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465343.aspx).

Jeśli skonfigurowano ustawienia transakcji, tak aby wszystkie są takie same, ale ten błąd się nadal pojawiać, możliwym obejściem jest na uruchamianie skryptów oddzielnie. W **skryptów bazy danych** siatki w **Pakuj/Publikuj** karta SQL, wyczyść **Include** pole wyboru dla skryptu, który powoduje przekroczenie limitu czasu, a następnie opublikowanie projektu. Następnie przejdź do **skryptów bazy danych** siatki, wybierz ten skrypt **Include** pole wyboru, a następnie wyczyść **Include** pola wyboru dla innych skryptów. Następnie opublikujesz projekt ponownie. Tym razem po opublikowaniu, tylko wybrane niestandardowy skrypt będzie uruchamiany.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Data Stream manifestu lokacji nie jest jeszcze dostępna

### <a name="scenario"></a>Scenariusz

Jeśli podczas instalowania pakietu przy użyciu *pliku deploy.cmd* plików z opcją t (test), zostanie wyświetlony następujący komunikat o błędzie:

Błąd: Strumień danych "sitemanifest/dbFullSql [@path="C:\TEMP\AdventureWorksGrant.sql']/sqlScript"nie jest jeszcze dostępna.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Komunikat o błędzie oznacza, że polecenie nie może wygenerować raport z testu. Jednak w przypadku użycia opcji (rzeczywista instalacja) y może być uruchamiane polecenie. Komunikat wskazuje tylko, że występuje problem z uruchamianiem polecenia w trybie testowym.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Ta aplikacja wymaga ManagedRuntimeVersion w wersji 4.0

### <a name="scenario"></a>Scenariusz

Podczas próby wdrożenia, zobaczysz następujący komunikat o błędzie:

Puli aplikacji, które próbujesz użyć ma właściwość 'managedRuntimeVersion' do 'v2.0'. Ta aplikacja wymaga "v4.0".

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

ASP.NET 4 nie jest zainstalowany w usługach IIS. Jeśli serwer, z którym jest wdrażany z jest komputer deweloperski i ma na nim zainstalowany program Visual Studio 2010, platformy ASP.NET 4 jest zainstalowana na komputerze, ale może nie być zainstalowane w usługach IIS. Na serwerze, na którym jest wdrażany z Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zainstalowanie programu ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nie można rzutować Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scenariusz

W przypadku wdrażania pakietu, zobaczysz następujący komunikat o błędzie:

Nie można rzutować obiektu typu "Microsoft.Web.Deployment.DeploymentProviderOptions" do "Microsoft.Web.Deployment.DeploymentProviderOptions".

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Próbujesz wdrożyć z Menedżera usług IIS przy użyciu Interfejsu sieci Web do 1.1 wdrażanie na serwer, na którym jest narzędziu Web Deploy 2.0 został zainstalowany. Jeśli używasz narzędzia administracyjnego IIS zdalnego wdrożyć przez zaimportowanie pakietów, sprawdź **dostępność nowych funkcji** okno dialogowe po nawiązaniu połączenia. (To okno dialogowe może być wyświetlane tylko raz po raz pierwszy nawiązywane jest połączenie. Aby wyczyścić połączenie i rozpocząć od nowa, Zamknij Menedżera usług IIS i uruchomić go ponownie, wprowadzając inetmgr/reset w wierszu polecenia.) Jeśli jedna z funkcji na liście jest **interfejs użytkownika sieci Web wdrażanie**, ma numer wersji jest niższy niż 8, serwer wdrażania może być wersji 1.1 i 2.0 zainstalowane narzędzia Web Deploy. Aby wdrożyć z klienta, który został zainstalowany w wersji 2.0, serwer musi mieć tylko narzędziu Web Deploy 2.0 zainstalowany. Należy skontaktować się z dostawcą hostingu, aby rozwiązać ten problem.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nie można załadować macierzystego składniki programu SQL Server Compact

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie, zobaczysz następujący komunikat o błędzie:

Nie można załadować macierzystego składniki odpowiadający dostawcy ADO.NET 8482 wersji programu SQL Server Compact. Zainstaluj poprawną wersję programu SQL Server Compact. Można znaleźć w artykule KB 974247, aby uzyskać więcej informacji.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wdrożone lokacji nie ma *amd64* i *x86* podfoldery z natywnych zestawów w nich w ramach aplikacji *bin* folderu. Na komputerze, na którym jest zainstalowany program SQL Server Compact, zestawów natywnych znajdują się w *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. To najlepszy sposób pozyskania poprawne pliki na prawidłowe foldery w projekcie programu Visual Studio można zainstalować pakietu NuGet SqlServerCompact. Instalacja pakietu dodaje skrypt po kompilacji, aby kopiowanie natywnych zestawów do *amd64* i *x86*. Aby te, które mają zostać wdrożone należy jednak ręcznie je uwzględnić w projekcie. Aby uzyskać więcej informacji, zobacz [wdrażania programu SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) samouczka.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Ścieżka jest nieprawidłowa" Błąd po wdrożeniu aplikacji Entity Framework Code First

### <a name="scenario"></a>Scenariusz

Wdrażanie aplikacji, która używa migracje Code First Framework jednostki i DBMS, takich jak SQL Server Compact, która przechowuje swojej bazy danych w pliku w aplikacji\_folderu danych. Masz migracje Code First skonfigurowany tak, aby utworzyć bazę danych po wdrożeniu pierwszego. Po uruchomieniu aplikacji, otrzymasz komunikat o błędzie, jak w poniższym przykładzie:

Ścieżka jest nieprawidłowa. Sprawdź katalog dla bazy danych. [Ścieżka = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Kod najpierw próbuje utworzyć bazy danych, ale aplikacja\_danych folder nie istnieje. Albo nie masz żadnych plików w *aplikacji\_danych* folderu, gdy wdrożono lub wybrano **wykluczanie aplikacji\_danych** na **pakowaniu/publikowaniu Web** karcie okna właściwości projektu. Proces wdrażania nie utworzy folder na serwerze, jeśli są połączone żadne pliki w folderze do skopiowania do serwera. Jeśli masz już bazę danych w witrynie, proces wdrażania spowoduje usunięcie plików i *aplikacji\_danych* sam w przypadku wybrania folder **Usuń dodatkowe pliki w lokalizacji docelowej** w profil publikowania. Aby rozwiązać ten problem, umieść plik symbolu zastępczego, takich jak plik z rozszerzeniem txt w *aplikacji\_danych* folderu, upewnij się, że nie masz **wykluczanie aplikacji\_danych** zaznaczone, a następnie ponownie wdrożyć.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nie można użyć obiektu COM, który został rozdzielony ze podstawowego RCW."

### <a name="scenario"></a>Scenariusz

Zostały pomyślnie za pomocą jednego kliknięcia publikowanie do wdrażania aplikacji i następnie uruchom ten błąd:

Niepowodzenie zadania wdrażania w sieci Web. (Nie można ukończyć żądania do agenta zdalnego adresu URL "<https://serverurl.com/msdeploy.axd?site=sitename>".)  
 Nie można ukończyć żądania do agenta zdalnego adresu URL "<https://url/msdeploy.axd?site=sitename>".  
Żądanie zostało przerwane: żądanie zostało anulowane.  
Nie można użyć obiektu COM, który został rozdzielony ze podstawowego RCW.

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

Domyślnie, Visual Studio, zestawy uprawnień w folderze głównym lokacji uprawnienia odczytu i zapisu w aplikacji\_folderu danych. Jeśli aplikacja wymaga dostęp do zapisu do podfolderu, jak pokazano ustawianie uprawnień folderów i wdrażane w tej serii samouczków środowiska produkcyjnego można ustawić uprawnienia dla tego folderu. Jeśli aplikacja wymaga uprawnienia do zapisu w folderze głównym lokacji, należy uniemożliwić ustawienie dostęp tylko do odczytu w folderze głównym, dodając **&lt;docelowy IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** plik profilu publikowania (mieć wpływ na jeden profil) lub pliku wpp.targets (mieć wpływ na wszystkie profile). Aby uzyskać informacje o tym, jak edytowanie tych plików, zobacz [porady: edytowanie ustawień wdrażania plików profilu (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Błąd konfiguracji — atrybut targetFramework odwołuje się do wersji, która jest nowsza niż zainstalowana wersja programu .NET Framework

### <a name="scenario"></a>Scenariusz

Pomyślnie opublikowano projektu sieci web, który jest przeznaczony dla platformy ASP.NET 4.5, ale po uruchomieniu aplikacji (przy użyciu trybu customErrors ustawiona na "wyłączone" w pliku Web.config pliku) zostanie wyświetlony następujący błąd:

TargetFramework atrybutu w &lt;kompilacji&gt; elementu w pliku Web.config służy tylko do docelowej wersji 4.0 i nowszy programu .NET Framework (na przykład "&lt;kompilacji targetFramework ="4.0"&gt;'). Atrybut "targetFramework" obecnie odwołuje się do wersji, która jest nowsza niż zainstalowana wersja programu .NET Framework. Określ prawidłową docelową wersję programu .NET Framework lub zainstaluj wymaganą wersję programu .NET Framework.

Błąd źródła strony błędu wyróżnione następujący wiersz z pliku Web.config jako przyczynę błędu:

&lt;Kompilacja targetFramework = "4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer nie obsługuje ASP.NET 4.5. Skontaktuj się z dostawcy hostingu, aby określić, kiedy i w przypadku pomocy technicznej dla platformy ASP.NET 4.5 można dodać. W przypadku uaktualniania serwera nie jest opcją, musisz wdrożyć projekt sieci web, który jest przeznaczony dla platformy ASP.NET 4 lub wersji wcześniejszych zamiast tego.

W przypadku wdrożenia programu ASP.NET 4 lub wcześniejszej projektu sieci web do tego samego miejsca docelowego, wybierz **Usuń dodatkowe pliki w lokalizacji docelowej** pole wyboru na **ustawienia** karcie **publikowanie w sieci Web**kreatora. Jeśli nie zaznaczysz **Usuń dodatkowe pliki w lokalizacji docelowej**, nadal będzie wyświetlona strona błędu konfiguracji.

Projekt **właściwości** systemu windows zawiera listy rozwijanej Target framework, ale nie może rozwiązać ten problem, wystarczy zmienić z **.NET Framework 4.5** do **.NET Framework 4**. Zmiana platformy docelowej wcześniejszą wersję framework projektu będą nadal mieć odwołania do zestawów w nowszej wersji framework i nie będzie działać. Należy ręcznie zmienić te odwołania lub Utwórz nowy projekt, który jest przeznaczony dla .NET Framework 4 lub wersji wcześniejszych. Aby uzyskać więcej informacji, zobacz [.NET Framework Ustawianie elementów docelowych dla witryn sieci Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Błędy w trybie średniego zaufania

### <a name="scenario"></a>Scenariusz

Po uruchomieniu aplikacji w środowisku produkcyjnym, pobiera błąd związany w trybie średniego zaufania.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wiele innych dostawców hostingu Uruchom witrynę sieci web w trybie średniego zaufania, co oznacza, że istnieją pewne rzeczy, które nie jest dozwolone w celu. Na przykład kod aplikacji nie może uzyskać dostępu do rejestru Windows i nie można czytać lub zapisywać pliki, które znajdują się poza hierarchii folderów aplikacji. Domyślnie aplikacja działa w *pełne zaufanie* na komputerze lokalnym, co oznacza, że aplikacja może być możliwość wykonywania czynności, które będą się kończyć niepowodzeniem podczas wdrażania do środowiska produkcyjnego.

Można skonfigurować aplikację do uruchamiania w trybie średniego zaufania w środowisku lokalnym, usługi IIS w celu rozwiązania zgłoszonego. Aby to zrobić, Otwórz aplikację *Web.config* pliku i Dodaj **zaufania** element **system.web** elementu, jak pokazano w poniższym przykładzie.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Aplikacja będzie teraz działać w trybie średniego zaufania w usługach IIS nawet na komputerze lokalnym.

Nie należy korzystać z tego, Jeżeli wdrażasz usługi Azure App Service, ponieważ platformy Azure nie jest wymagane w trybie średniego zaufania. Od momentu, w tym samouczku jest zapisywana w lutym 2012 r. za pomocą tej metody, aby uruchamiało aplikacji w trybie średniego zaufania spowoduje, że błąd na platformie Azure.

Jeśli używasz migracje Code First Framework jednostki, a następnie są wdrażane do dostawcy usług hosta, który uruchamia aplikację w trybie średniego zaufania, upewnij się, że w wersji 5.0 lub nowszy. Platformy Entity Framework w wersji 4.3 migracje wymaga pełnego zaufania, aby można było zaktualizować schemat bazy danych.

## <a name="http-40417-not-found-error"></a>Błąd — nie znaleziono HTTP 404.17

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej lokacji na komputerze deweloperskim w usługach IIS, zobaczysz następujący komunikat o błędzie, raportowanie, że serwer nie może przetworzyć Default.aspx:

Błąd HTTP 404.17 — nie znaleziono

Żądana zawartość wydaje się być skryptu i będzie nie być obsługiwana przez program obsługi plików statycznych.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Program ASP.NET 4.5 nie może być zainstalowane na komputerze. Zobacz kroki w wdrażanie w usługach IIS jako środowisku testowym samouczek z tej serii, która wyjaśnia, jak zainstalować program ASP.NET 4.5.

> [!div class="step-by-step"]
> [Poprzednie](deploying-extra-files.md)
