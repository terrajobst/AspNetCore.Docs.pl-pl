---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Rozwiązywanie problemów z | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 15bda09c59afaf9e5449c68c5206bb28de245541
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Rozwiązywanie problemów
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


Na tej stronie opisano niektóre typowe problemy, które mogą wystąpić podczas wdrażania aplikacji sieci web ASP.NET przy użyciu programu Visual Studio. Dla każdego z nich znajdują się możliwe przyczyny i rozwiązania odpowiednie.

Te scenariusze przedstawiono dotyczą zarówno Azure, jak i zewnętrznych dostawców hostingu. Aby uzyskać więcej informacji na temat rozwiązywania problemów z aplikacjami sieci web w usłudze Azure App Service zobacz następujące zasoby:

- [Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Monitorowanie aplikacji sieci Web w usłudze aplikacji Azure](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Informuje o wersji systemu Windows Azure SDK 2.0 dla programu .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (ScottGu blog pokazano, jak można pobrać dzienników diagnostycznych w programie Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Błąd serwera w "/" aplikacja — aktualne niestandardowe ustawienia błędów uniemożliwiają szczegóły błędu przeglądanie zdalnie

### <a name="scenario"></a>Scenariusz

Po wdrożeniu lokacji z hostem zdalnym, otrzymasz komunikat o błędzie, który wymienia ustawienie customErrors w pliku Web.config, ale nie określa został rzeczywista Przyczyna błędu:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program ASP.NET zawiera szczegółowe informacje o błędzie tylko wtedy, gdy aplikacja sieci web jest uruchomiona na komputerze lokalnym. Zazwyczaj nie chcesz wyświetlić szczegółowe informacje o błędzie, gdy aplikacji sieci web jest publicznie dostępna przez Internet, ponieważ hakerów może istnieć możliwość dzięki tym informacjom można znaleźć luk w zabezpieczeniach w aplikacji. Jednak w przypadku wdrażania z witryną lub aktualizacji do lokacji, czasami coś się problemów i konieczne jest wyświetlany komunikat błędu.

Aby włączyć tę aplikację wyświetlić szczegółowe komunikaty o błędach, gdy jest uruchamiany na zdalnym hoście, należy zmodyfikować plik Web.config ustawić tryb customErrors, należy ponownie wdrożyć aplikację i ponownie uruchomić aplikację:

1. Jeśli plik Web.config ma acustomErrors element w elemencie thesystem.web, należy zmienić themode atrybutu na "off". W przeciwnym razie Dodaj acustomErrors element w elemencie thesystem.web z atrybutem themode ustawionym na "wyłączone", jak pokazano w poniższym przykładzie: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Wdrażanie aplikacji.
3. Uruchom aplikację, a następnie powtórz niezależnie od została wcześniej powodujący błąd występuje. Teraz widoczny jest komunikat błędu.
4. Po rozwiązaniu błąd, przywrócić oryginalny ustawienie customErrors i ponownie wdrożyć aplikację.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Nie można utworzyć/tle kopii "ContosoUniversity" Jeśli ten plik już istnieje.

### <a name="scenario"></a>Scenariusz

Podczas próby uruchomienia projektu w programie Visual Studio można uzyskać stronę błędu z komunikatem, jak w następującym przykładzie:

Błąd serwera w "/" aplikacji. Nie można utworzyć/tle kopii "ContosoUniversity" Jeśli ten plik już istnieje.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Zaczekaj chwilę i odświeżyć przeglądarkę, lub ponownie skompilować witrynę i spróbuj ponownie uruchomić.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Na stronie sieci Web jest odmowa dostępu, że używa programu SQL Server Compact

### <a name="scenario"></a>Scenariusz

Wdrażając lokacji korzystającej z programu SQL Server Compact i uruchomić strony w witrynie wdrożone, który uzyskuje dostęp do bazy danych, zobaczysz komunikat o błędzie:

Odmowa dostępu. (Wyjątek od HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto Usługa sieciowa na serwerze musi mieć możliwość odczytu usługa SQL Compact natywne pliki binarne, które znajdują się w *bin\amd64* lub *bin\x86* folder, ale nie ma uprawnień odczytu dla folderów. Ustaw uprawnienia do odczytu dla usługi SIECIOWEJ *bin* folderu, upewniając się rozszerzyć uprawnienia do podfolderów.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nie można odczytać pliku konfiguracji z powodu niewystarczających uprawnień

### <a name="scenario"></a>Scenariusz

Po kliknięciu programu Visual Studio publikowania przycisk, aby wdrożyć aplikację do usług IIS na komputerze lokalnym, publikowanie kończy się niepowodzeniem i **dane wyjściowe** okno zawiera komunikat o błędzie podobny do poniższego:

Wystąpił błąd podczas odczytu pliku konfiguracji usług IIS "MACHINE/przekierowanie". Tożsamość tej operacji została... Błąd: Nie można odczytać pliku konfiguracji z powodu niewystarczających uprawnień.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Aby użyć jednego kliknięcia publikowanie w usługach IIS na komputerze lokalnym, działanie programu Visual Studio z uprawnieniami administratora. Zamknij program Visual Studio i uruchom go ponownie z uprawnieniami administratora.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Nie można nawiązać połączenia z komputerem docelowym... Przy użyciu określonego procesu

### <a name="scenario"></a>Scenariusz

Po kliknięciu programu Visual Studio publikowania przycisk, aby wdrożyć aplikację, publikowanie kończy się niepowodzeniem i **dane wyjściowe** okno zawiera komunikat o błędzie podobny do poniższego:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer proxy jest przerwanie komunikacji z serwerem docelowym. W Panelu sterowania systemu Windows lub w programie Internet Explorer, wybierz **Opcje internetowe** i wybierz **połączeń** kartę. W **właściwości internetowe** okno dialogowe, kliknij przycisk **ustawienia sieci LAN**. W **ustawienia sieci lokalnej (LAN)** okno dialogowe, usuń zaznaczenie **Automatycznie wykryj ustawienia** wyboru. Następnie ponownie kliknij przycisk Publikuj.

Jeśli problem będzie się powtarzać, skontaktuj się z administratorem systemu, aby ustalić, co można zrobić za pomocą ustawienia serwera proxy lub zapory. Problem wynika z faktu, narzędzie Web Deploy korzysta z portem niestandardowym dla wdrożenia usługi zarządzania siecią Web (8172); w przypadku innych połączeń narzędzia Web Deploy korzysta z portu 80. W przypadku wdrażania do dostawcy hostingu innych firm są zazwyczaj przy użyciu usługi zarządzania siecią Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Domyślna pula aplikacji 4.0 .NET nie istnieje.

### <a name="scenario"></a>Scenariusz

Podczas wdrażania aplikacji, która wymaga programu .NET Framework 4 pojawić się następujący komunikat o błędzie:

Domyślna pula aplikacji programu .NET 4.0 nie istnieje lub nie można dodać aplikacji. Sprawdź, czy program ASP.NET 4.0 jest zainstalowany na tym komputerze.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Nie zainstalowano programu ASP.NET 4 w usługach IIS. Jeśli serwer, z którym jest wdrażany z jest komputer deweloperski i jest na nim zainstalowany program Visual Studio 2010, platformy ASP.NET 4 jest zainstalowana na komputerze, ale może nie być zainstalowane w usługach IIS. Na serwerze, na którym jest wdrażany z Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i instalowanie platformy ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Również może być konieczne ręczne ustawienie programu .NET Framework w wersji domyślnej puli aplikacji. Aby uzyskać więcej informacji zobacz Wdrażanie usług IIS jako samouczek środowiska testowego w tej serii.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format ciągu inicjowania jest niezgodny ze specyfikacją rozpoczynającą się pod indeksem 0.

### <a name="scenario"></a>Scenariusz

Po wdrożeniu aplikacji przy użyciu jednego kliknięcia publikowania, po uruchomieniu strony, który uzyskuje dostęp do bazy danych otrzymasz następujący komunikat o błędzie:

Format ciągu inicjowania jest niezgodny ze specyfikacją rozpoczynającą się pod indeksem 0.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Otwórz *Web.config* plik w witrynie wdrożone i sprawdź, czy wartości parametrów połączeń rozpoczynać się od $(ReplacableToken\_, jak w poniższym przykładzie:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Jeśli parametry połączenia wyglądać w tym przykładzie, Edytuj plik projektu i dodaj następujące właściwości do elementu PropertyGroup przez wszystkie konfiguracje kompilacji:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Następnie należy ponownie wdrożyć aplikację.

## <a name="http-500-internal-server-error"></a>HTTP 500 Wewnętrzny błąd serwera

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie pojawić następujący komunikat o błędzie bez określone informacje wskazujące przyczynę błędu:

Błąd HTTP 500 — Wewnętrzny błąd serwera.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Istnieje wiele przyczyn błędów 500, ale jest jedną z możliwych przyczyn Jeśli wykonujesz te samouczki, możesz zaznaczyć XML element w niewłaściwym miejscu w jednym z plików transformacji pliku Web.config. Na przykład, czy ten błąd wystąpi Jeśli umieścisz przekształcania, która wstawia &lt;lokalizacji&gt; pod &lt;system.web&gt; zamiast bezpośrednio pod &lt;konfiguracji&gt;. Funkcja podglądu transformacji pliku Web.config służy do Sprawdź, czy przekształcenia działają zgodnie z oczekiwaniami. Rozwiązanie, jeśli okaże się, przekształcania, który został niepoprawnie zaprogramowany jest Popraw plik transformacji i wdrożenie. Jeśli błąd nie jest widoczne, spróbuj komentowania limit transformacji i ponownego wdrażania, aby zobaczyć, która z nich jest przyczyną komunikatu o błędzie 500.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 wewnętrzny błąd serwera

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie pojawić się następujący komunikat o błędzie:

Błąd HTTP 500.21 — wewnętrzny błąd serwera. Program obsługi "PageHandlerFactory-Integrated" ma niewłaściwy moduł "ManagedPipelineHandler" liście modułów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Witryny wdrożeniu programu ASP.NET 4, ale ASP.NET 4 nie jest zarejestrowany w usługach IIS na serwerze obiektów docelowych. Na serwerze otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zarejestruj programu ASP.NET 4, uruchamiając następujące polecenia:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Również może być konieczne ręczne ustawienie programu .NET Framework w wersji domyślnej puli aplikacji. Aby uzyskać więcej informacji zobacz Wdrażanie usług IIS jako samouczek środowiska testowego w tej serii.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Logowanie nie powiodło się otwieranie Express bazy danych serwera SQL w aplikacji\_danych

### <a name="scenario"></a>Scenariusz

Aktualne *Web.config* pliku parametry połączenia wskazują bazy danych programu SQL Server Express jako *.mdf* plików w sieci *aplikacji\_danych* folderu, a pierwsza czas, uruchom aplikację, która zostanie wyświetlony następujący komunikat o błędzie:

System.Data.SqlClient.SqlException: Nie można otworzyć bazy danych "DatabaseName" żądanego podczas logowania. Logowanie nie powiodło się.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Nazwa *.mdf* pliku nie może odpowiadać nazwie dowolnego programu SQL Server Express bazy danych, która ma kiedykolwiek znajdował się na komputerze, nawet po usunięciu *.mdf* pliku wcześniej istniejącej bazy danych. Zmień nazwę *.mdf* nigdy nie był używany jako nazwa bazy danych i Zmień nazwę pliku *Web.config* pliku do użycia nowej nazwy. Alternatywnie, można użyć [programu SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) do usuwania istniejących programu SQL Server Express w bazach danych.

## <a name="model-compatibility-cannot-be-checked"></a>Nie zgodność modelu można sprawdzić

### <a name="scenario"></a>Scenariusz

Aktualne *Web.config* pliku parametrów połączenia, aby wskazywał nową bazę danych programu SQL Server Express i uruchom aplikację po raz pierwszy, zostanie wyświetlony następujący komunikat o błędzie:

Nie można sprawdzić zgodności modelu, ponieważ baza danych nie zawiera metadanych modelu. Upewnij się, że dodano IncludeMetadataConvention konwencje DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Jeśli nazwa bazy danych, należy umieścić w pliku Web.config kiedykolwiek została użyta przed na komputerze, bazy danych może już istnieć niektóre tabele w nim. Wybierz nową nazwę, która nie została użyta na komputerze przed i zmień *Web.config* pliku do punktu, aby użyć tej nowej nazwy bazy danych. Alternatywnie, można użyć [programu SQL Server Express narzędzie](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) lub [programu SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) usunąć istniejącą bazę danych.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Błąd SQL podczas próby utworzenia użytkowników lub ról skryptu

### <a name="scenario"></a>Scenariusz

W przypadku korzystania z bazy danych wdrażania skonfigurowane na **Pakuj/Publikuj SQL** , skrypty SQL, które są uruchamiane podczas wdrażania obejmują polecenia Utwórz użytkownika lub Utwórz rolę i wykonanie skryptu kończy się niepowodzeniem podczas te polecenia są wykonywane. Może pojawić się bardziej szczegółowe komunikaty, takie jak następujące:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Jeśli ten błąd występuje, gdy skonfigurowano wdrażania bazy danych w **publikowanie w sieci Web** kreatora, a nie **Pakuj/Publikuj SQL** karcie, Utwórz wątek [konfiguracji i Wdrożenie](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum i rozwiązania zostaną dodane do tej strony rozwiązywania problemów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto użytkownika, którego używasz do wykonywania wdrożenia nie ma uprawnień do tworzenia użytkowników lub ról. Na przykład firma może przypisywać bazę danych\_datareader, bazy danych\_datawriter i db\_ddladmin ról z kontem użytkownika, który konfiguruje dla Ciebie. Te są wystarczające do tworzenia większości obiektów bazy danych, ale nie do tworzenia użytkowników lub ról. Jest jednym ze sposobów uniknąć tego błędu, wyłączając użytkowników i ról z bazy danych. Można to zrobić, edytując PreSource element do automatycznie generowanego skryptu bazy danych, co zawierają one następujące atrybuty:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Aby uzyskać informacje o sposobie edytowania PreSource elementu w pliku projektu, zobacz [porady: edytowanie ustawień wdrażania w pliku projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Jeśli użytkownicy lub role w bazie danych programowanie muszą być w docelowej bazie danych, skontaktuj się z dostawcą hostingu.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Błąd programu SQL Server limit czasu podczas uruchamiania niestandardowych skryptów podczas wdrażania

### <a name="scenario"></a>Scenariusz

Określono niestandardowe skrypty SQL, aby uruchomić podczas wdrażania i uruchomienie narzędzia Web Deploy je, ich limitu czasu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Uruchamianie wielu skryptów, które mają tryby innej transakcji może spowodować błędy przekroczenia limitu czasu. Domyślnie automatycznie wygenerowanych skryptów uruchomić w transakcji, ale skrypty niestandardowe nie. W przypadku wybrania **pobierania danych i/lub schemat z istniejącej bazy danych** opcja **Pakuj/Publikuj SQL** karcie i Dodawanie niestandardowego skryptu SQL, należy zmienić ustawienia transakcji na niektóre skrypty, aby wszystkie skrypty używać tych samych ustawień transakcji. Aby uzyskać więcej informacji, zobacz [porady: Wdrażanie bazy danych z projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465343.aspx).

Jeśli skonfigurowano ustawienia transakcji, tak aby wszystkie są takie same, ale nadal ten błąd, możliwym obejściem jest oddzielnie uruchamiania skryptów. W **skryptów bazy danych** siatki w **pakowania/publikowania** karcie SQL, wyczyść **Include** pole wyboru dla skryptu, który powoduje błąd upływu limitu czasu następnie publikowania projektu. Następnie przejdź do **skryptów bazy danych** siatki, wybierz ten skrypt **Include** pole wyboru, a następnie wyczyść **Include** pól wyboru dla innych skryptów. Następnie ponownie opublikować projekt. Teraz po opublikowaniu, uruchamia wybrane niestandardowego skryptu.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Strumień danych manifestu lokacji nie jest jeszcze dostępna

### <a name="scenario"></a>Scenariusz

Jeśli podczas instalowania pakietu przy użyciu *pliku deploy.cmd* plików z opcją t (test), zobacz następujący komunikat o błędzie:

Błąd: Strumień danych "sitemanifest/dbFullSql [@path="C:\TEMP\AdventureWorksGrant.sql']/sqlScript"nie jest jeszcze dostępna.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Komunikat o błędzie oznacza, że polecenie nie może utworzyć raport z testów. Jednak polecenie może działać, korzystając z opcji (rzeczywista instalacja) y. Komunikat wskazuje tylko, że występuje problem z uruchamianiem polecenia w trybie testowym.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Ta aplikacja wymaga ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Scenariusz

Podczas próby wdrożenia pojawić się następujący komunikat o błędzie:

Puli aplikacji, które próbujesz użyć ma jako wartość właściwości "managedRuntimeVersion" wartość "v2.0". Ta aplikacja wymaga "v4.0".

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Nie zainstalowano programu ASP.NET 4 w usługach IIS. Jeśli serwer, z którym jest wdrażany z jest komputer deweloperski i jest na nim zainstalowany program Visual Studio 2010, platformy ASP.NET 4 jest zainstalowana na komputerze, ale może nie być zainstalowane w usługach IIS. Na serwerze, na którym jest wdrażany z Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i instalowanie platformy ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nie można rzutować Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scenariusz

W przypadku wdrażania pakietu pojawić się następujący komunikat o błędzie:

Nie można rzutować obiektu typu "Microsoft.Web.Deployment.DeploymentProviderOptions" do "Microsoft.Web.Deployment.DeploymentProviderOptions".

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Próbujesz wdrożyć z Menedżera usług IIS za pomocą interfejsu sieci Web wdrażanie 1.1 użytkownika na serwerze sieci Web Deploy 2.0 został zainstalowany. Jeśli używasz narzędzia administracji zdalnej programu IIS, aby wdrożyć przez zaimportowanie pakietów, sprawdź **nowe funkcje dostępne** okno dialogowe podczas ustanawiania połączenia. (To okno dialogowe może być wyświetlane tylko raz po raz pierwszy nawiązywane jest połączenie. Aby wyczyścić połączenia i zacząć od nowa, Zamknij Menedżera usług IIS i uruchomić go ponownie, wprowadzając inetmgr/zresetować w wierszu polecenia.) Jeśli którejś z tych funkcji na liście jest **interfejsu użytkownika sieci Web wdrażanie**, ma numer wersji jest niższy niż 8, serwera jest wdrażany z może być zarówno 1.1, jak i 2.0 wersje zainstalowane narzędzia Web Deploy. Aby wdrożyć od klienta, który ma zainstalowaną 2.0, serwer musi mieć tylko Web Deploy 2.0 zainstalowany. Należy skontaktować się z dostawcą hostingu, aby rozwiązać ten problem.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nie można załadować macierzystego składniki programu SQL Server Compact

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej witrynie pojawić się następujący komunikat o błędzie:

Nie można załadować macierzystego składniki odpowiadający dostawcy ADO.NET 8482 wersji programu SQL Server Compact. Zainstaluj poprawną wersję programu SQL Server Compact. Można znaleźć w artykule KB 974247 więcej szczegółów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wdrożone lokacji nie ma *amd64* i *x86* podfoldery z natywnych zestawów w nich w aplikacji *bin* folderu. Na komputerze, na którym został zainstalowany program SQL Server Compact, natywnych zestawów znajdują się w *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. To najlepszy sposób pozyskania poprawne pliki w folderach poprawne z projektu programu Visual Studio można zainstalować pakietu NuGet SqlServerCompact. Instalacja pakietu dodaje skrypt mające miejsce po kompilacji, aby skopiować natywnych zestawów do *amd64* i *x86*. Aby je do wdrożenia należy jednak ręcznie je uwzględnić w projekcie. Aby uzyskać więcej informacji, zobacz [wdrażanie programu SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) samouczka.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Ścieżka jest nieprawidłowa" Błąd po wdrożeniu aplikacji Entity Framework Code First

### <a name="scenario"></a>Scenariusz

Wdrażanie aplikacji, która używa migracje Code First Framework jednostki i bazami danych, takich jak SQL Server Compact służący do przechowywania bazy danych w pliku w aplikacji\_folderem danych. Masz migracje Code First skonfigurowany tak, aby utworzyć bazę danych po pierwszym wdrożeniu. Po uruchomieniu aplikacji zostanie wyświetlony komunikat o błędzie, jak w następującym przykładzie:

Ścieżka jest nieprawidłowa. Sprawdź katalog bazy danych. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Kod najpierw próbuje utworzyć bazy danych, jednak aplikacja\_danych folder nie istnieje. Albo nie masz żadnych plików *aplikacji\_danych* folder podczas wdrażania lub wybrania **wykluczanie aplikacji\_danych** na **pakowaniu/publikowaniu Web** karcie okna właściwości projektu. Proces wdrażania nie Utwórz folder na serwerze, jeśli nie ma żadnych plików w folderze, który ma zostać skopiowany na serwerze. Jeśli masz już bazę danych w lokacji, proces wdrażania spowoduje usunięcie plików i *aplikacji\_danych* folderu w przypadku wybrania **Usuń dodatkowe pliki w miejscu docelowym** w profil publikowania. Aby rozwiązać ten problem, należy umieścić plik symbolu zastępczego takich jak plik txt w *aplikacji\_danych* folderu, upewnij się, że nie masz **wykluczanie aplikacji\_danych** zaznaczone i wdrożenie.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nie można użyć obiektu COM, który został oddzielony od swojej podstawowej otoki RCW."

### <a name="scenario"></a>Scenariusz

Zostały pomyślnie za pomocą jednego kliknięcia publikowanie do wdrożenia aplikacji i zacznij pobierania ten błąd:

Niepowodzenie zadania wdrażania w sieci Web. (Nie można ukończyć żądania do agenta zdalnego adresu URL "<https://serverurl.com/msdeploy.axd?site=sitename>".)  
 Nie można ukończyć żądania do agenta zdalnego adresu URL "<https://url/msdeploy.axd?site=sitename>".  
Żądanie zostało przerwane: żądanie zostało anulowane.  
Obiekt COM, który został oddzielony od swojej podstawowej otoki RCW, nie można użyć.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Zamknięcie i ponowne uruchomienie programu Visual Studio jest zwykle wszystkie punkty, które jest wymagane, aby rozwiązać ten problem.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Wdrożenie nie powiedzie się ponieważ użytkownika poświadczenia używane na potrzeby publikowania nie mają setACL urzędu

### <a name="scenario"></a>Scenariusz

Publikowanie kończy się niepowodzeniem z powodu błędu z informacją nie mają uprawnień do ustawić uprawnień do folderu (konto użytkownika, którego używasz nie mają uprawnień setACL).

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program Visual Studio zestawów uprawnień w folderze głównym lokacji Odczyt i zapis w aplikacji\_folderem danych. Jeśli wiesz, czy domyślne uprawnienia do folderów są prawidłowe i nie trzeba ustawić, dodając wyłączyć to zachowanie **&lt;docelowego IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** pliku profilu publikowania (na jednym profilu) lub pliku wpp.targets (mieć wpływ na wszystkie profile). Aby uzyskać informacje o sposobie edytowania tych plików, zobacz [porady: edytowanie ustawień wdrażania plików profilu (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Błędy odmowa dostępu, gdy aplikacja próbuje zapisać w folderze aplikacji

### <a name="scenario"></a>Scenariusz

Błędy aplikacji podczas próby tworzenia lub edytowania pliku w jednym z folderów aplikacji, ponieważ nie ma uprawnienia zapisu do tego folderu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program Visual Studio zestawów uprawnień w folderze głównym lokacji Odczyt i zapis w aplikacji\_folderem danych. Jeśli aplikacja wymaga dostępu do zapisu do podfolderu, należy ustawić uprawnienia do tego folderu, jak pokazano ustawiania uprawnień do folderu i wdrażanie w tej serii samouczków środowiska produkcyjnego. Jeśli aplikacja wymaga uprawnienia do zapisu w folderze głównym witryny, należy uniemożliwić ustawienie dostęp tylko do odczytu w folderze głównym, dodając **&lt;docelowego IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** pliku profilu publikowania (na jednym profilu) lub pliku wpp.targets (mieć wpływ na wszystkie profile). Aby uzyskać informacje o sposobie edytowania tych plików, zobacz [porady: edytowanie ustawień wdrażania plików profilu (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Błąd konfiguracji — atrybut targetFramework odwołuje się do wersji, która jest nowsza niż zainstalowana wersja programu .NET Framework

### <a name="scenario"></a>Scenariusz

Pomyślnie publikowania projektu sieci web, którego celem jest ASP.NET 4.5, ale po uruchomieniu aplikacji (z ustawioną wartość "off" w pliku Web.config pliku tryb customErrors) możesz uzyskać następujący błąd:

Atrybut targetFramework w &lt;kompilacji&gt; elementu w pliku Web.config jest używany tylko docelowego w wersji 4.0 lub nowszej programu .NET Framework (na przykład "&lt;kompilacji targetFramework ="4.0"&gt;'). Obecnie atrybut "targetFramework" odwołuje się do wersji, która jest nowsza niż zainstalowana wersja programu .NET Framework. Określ prawidłową wersję docelową programu .NET Framework lub zainstaluj wymaganą wersję systemu .NET Framework.

Pole źródła błędu strony błędu wyróżnione następujący wiersz z pliku Web.config jako przyczyna błędu:

&lt;Kompilacja targetFramework = "4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer nie obsługuje platformy ASP.NET 4.5. Skontaktuj się z pomocą dostawcy hostingu, aby określić, kiedy i czy można dodać obsługę dla platformy ASP.NET 4.5. Jeśli uaktualnienie serwera nie jest opcją, należy wdrożyć projekt sieci web, przeznaczonego dla programu ASP.NET 4 lub starszym zamiast tego.

Wdrożenie programu ASP.NET 4 lub starszej projektu sieci web do tego samego miejsca docelowego, wybierz **Usuń dodatkowe pliki w miejscu docelowym** pole wyboru na **ustawienia** karcie **publikowanie w sieci Web**kreatora. Jeśli nie zaznaczysz **Usuń dodatkowe pliki w miejscu docelowym**, możesz pobrać strony błędu konfiguracji.

Projekt **właściwości** systemu windows zawiera listy rozwijanej docelowej framework, ale nie może rozwiązać ten problem, tak zmieniając z **.NET Framework 4.5** do **.NET Framework 4**. Zmiana platformy docelowej na starszą wersję framework, projekt nadal będzie zawierał odwołania do zestawów w nowszej wersji framework i nie będzie działać. Należy ręcznie zmienić te odwołania lub Utwórz nowy projekt, przeznaczonego dla platformy .NET Framework 4 lub starszym. Aby uzyskać więcej informacji, zobacz [.NET Framework elementów docelowych dla witryn sieci Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Błędy w trybie średniego zaufania

### <a name="scenario"></a>Scenariusz

Po uruchomieniu aplikacji w środowisku produkcyjnym, pobiera komunikat o błędzie dotyczący do trybie średniego zaufania.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wielu dostawców hostingu innych firm Uruchom witrynę sieci web w trybie średniego zaufania, co oznacza, że są niektóre czynności, które nie może wykonać. Na przykład kod aplikacji nie może uzyskać dostępu do rejestru systemu Windows i nie może odczytać lub zapisywać pliki, które znajdują się poza hierarchię folderów aplikacji. Domyślnie aplikacja działa *pełne zaufanie* na komputerze lokalnym, co oznacza, że aplikacja będzie mógł wykonać czynności, które będą się kończyć niepowodzeniem podczas wdrażania w środowisku produkcyjnym.

Można skonfigurować aplikację do uruchamiania w trybie średniego zaufania w lokalnym środowisku usług IIS w celu rozwiązywania. Aby to zrobić, Otwórz aplikację *Web.config* pliku, a następnie dodaj **zaufania** element **system.web** element, jak pokazano w poniższym przykładzie.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Aplikacja będzie uruchamiana w trybie średniego zaufania w usługach IIS nawet na komputerze lokalnym.

Nie wykonuj tego Jeśli wdrażasz usłudze Azure App Service, ponieważ Azure nie wymaga trybie średniego zaufania. Z chwili utworzenia tego samouczka jest zapisywana w lutego 2012 aby aplikacji były uruchamiane w trybie średniego zaufania przy użyciu tej metody spowoduje błąd na platformie Azure.

Jeśli używasz migracje Code First Framework jednostki są wdrażanie u dostawcy hostingu działającą aplikację w trybie średniego zaufania, upewnij się, że w wersji 5.0 lub nowszej. W Entity Framework w wersji 4.3 migracje wymaga pełnego zaufania, aby można było zaktualizować schemat bazy danych.

## <a name="http-40417-not-found-error"></a>Błąd: nie odnaleziono HTTP 404.17

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej lokacji na komputerze deweloperskim w usługach IIS pojawić się następujący komunikat o błędzie raportowania, że serwer nie może przetworzyć Default.aspx:

Błąd HTTP 404.17 — nie znaleziono

Żądana zawartość najprawdopodobniej jest skryptem i nie zostanie obsłużona przez obsługę plików statycznych.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Program ASP.NET 4.5 nie może być zainstalowane na tym komputerze. Zobacz kroki opisane w sekcji Wdrażanie usług IIS jako samouczek środowiska testowego w tej serii, który objaśnia, jak zainstalować program ASP.NET 4.5.

> [!div class="step-by-step"]
> [Poprzednie](deploying-extra-files.md)
