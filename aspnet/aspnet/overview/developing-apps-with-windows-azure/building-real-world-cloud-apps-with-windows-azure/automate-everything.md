---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatyzowanie wszystkiego (Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839638"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatyzowanie wszystkiego (Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby zapoznać się z wprowadzeniem do e-book, zobacz [pierwszy rozdział](introduction.md).


Pierwsze trzy wzorce, które omówimy faktycznie Zastosuj do każdego projektu rozwoju oprogramowania, ale szczególnie do projektów w chmurze. Ten wzorzec jest temat automatyzowania zadań programistycznych. Jest ważne tematu, ponieważ ręczne procesy są powolne i podatne na błędy; Automatyzowanie, jak najwięcej z nich jako możliwe pomaga skonfigurować szybkie, niezawodne i elastyczne przepływ pracy. Jest jednoznacznie ważne w przypadku projektowania aplikacji w chmurze, ponieważ łatwo można zautomatyzować wiele zadań, które są trudne lub niemożliwe do automatyzacji w środowisku lokalnym. Na przykład, możesz skonfigurować całego testu środowisk, w tym dla nowego serwera sieci web i maszyny wirtualne zaplecza, baz danych, blob storage (magazyn plików), kolejki itd.

## <a name="devops-workflow"></a>Przepływ pracy DevOps

Coraz więcej słyszysz termin "DevOps." Termin opracowany poza rozpoznawania, trzeba zintegrować zadań tworzenia i operacji w celu wydajnego tworzenia oprogramowania. Rodzaj przepływu pracy, który chcesz włączyć jest jeden w którym mogą tworzyć aplikację, wdrożyć ją, uczenie się z produkcyjnym ją, odbywają się w odpowiedzi na co wykorzystasz zdobyte umiejętności i powtórz cykl szybko i niezawodnie.

Niektóre zespoły deweloperów w chmurze pomyślnie wdrożyć wiele razy dziennie do środowiska produkcyjnego. Zespół platformy Azure służące do wdrażania istotne aktualizacje co 2 – 3 miesięcy, ale teraz drobne aktualizacje wersje co 2 – 3 dni i główne wersje co 2 – 3 tygodnie. Wprowadzenie do wydawania oprogramowania w tej bardzo ułatwia reagować na opinie klientów.

Aby to zrobić, należy włączyć cykl tworzenia i wdrażania, który jest powtarzalne, niezawodne i przewidywalne, a czas cyklu niski.

![Przepływ pracy DevOps](automate-everything/_static/image1.png)

Innymi słowy musi być możliwie krótkie odstęp czasu między Jeśli masz pomysł na funkcję i podczas korzystania z niego i przekazywania opinii klientów. Pierwsze trzy wzorce — Automatyzowanie wszystkiego, kontroli źródła i ciągłej integracji i dostarczania — to wszystko na temat najlepszych rozwiązań, które firma Microsoft zaleca, aby włączyć ten rodzaj procesu.

## <a name="azure-management-scripts"></a>Skrypty do zarządzania platformy Azure

W [wprowadzenie do tę książkę elektroniczną](introduction.md), konsoli sieci web portalu zarządzania systemu Azure, jak działa. Portal zarządzania pozwala na monitorowanie i zarządzanie nimi, wszystkie zasoby, które zostały wdrożone na platformie Azure. Jest to prosty sposób tworzenia i usuwania usług, takich jak aplikacje sieci web i maszyny wirtualne, konfiguracji tych usług, monitorowanie operacji usługi i tak dalej. Jest doskonałym narzędziem, ale za jego pomocą jest procesem wykonywanym ręcznie. Jeśli zamierzasz tworzenia aplikacji produkcyjnych o dowolnym rozmiarze, a szczególnie w środowisku zespołowym zalecamy go za pośrednictwem portalu, interfejsu użytkownika, aby dowiedzieć się i Poznaj platformę Azure, a następnie automatyzować procesy, które kilkukrotnie czynności.

Prawie wszystkie czynności, które można wykonać ręcznie w portalu zarządzania lub z programu Visual Studio może również odbywać się przez wywołanie interfejsu API REST. Można napisać skryptów przy użyciu [programu Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), lub można użyć platforma typu open source, takich jak [Chef](http://www.opscode.com/chef/) lub [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Można również użyć narzędzia wiersza polecenia powłoki Bash w środowisku Mac lub Linux. Platforma Azure oferuje interfejsy API obsługi skryptów dla tych różnych środowisk i ma [interfejsu API .NET — zarządzanie](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) w przypadku, gdy chcesz napisać kod zamiast skryptów.

Dla aplikacji naprawić przygotowaliśmy kilka skryptów programu Windows PowerShell, które automatyzują procesy tworzenia środowiska testowego i wdrażania projektu w tym środowisku i omówimy niektóre zawartość tych skryptów.

## <a name="environment-creation-script"></a>Skrypt tworzenia środowiska

Pierwszy skrypt przyjrzymy nosi nazwę *New AzureWebsiteEnv.ps1*. Tworzy środowisko platformy Azure, że można wdrożyć poprawka aplikacji na potrzeby testowania. Poniżej przedstawiono główne zadania, które wykonuje ten skrypt:

- Tworzenie aplikacji sieci web.
- Utwórz konto magazynu. (Wymagane dla obiektów blob i kolejki, jak można zauważyć w rozdziałach nowsze.)
- Tworzenie serwera usługi SQL Database i dwie bazy danych: baza danych aplikacji i bazy danych członkostwa.
- Store ustawień na platformie Azure, która aplikacja będzie używać do dostępu do konta magazynu i baz danych.
- Tworzenie plików ustawień, które będą używane do automatycznego wdrożenia.

### <a name="run-the-script"></a>Uruchom skrypt


> [!NOTE]
> Ta część rozdziale pokazano przykłady skryptów i poleceń, które należy wprowadzić, aby można było uruchomić je. Ten pokaz i nie zapewnia wszystko, co musisz wiedzieć, aby można było uruchamiać skrypty. Aby uzyskać instrukcje krok po kroku jak-to-it, zobacz [dodatku: Napraw go przykładowej aplikacji](the-fix-it-sample-application.md#deploybase).


Do uruchomienia skryptu programu PowerShell, który zarządza usług platformy Azure musisz zainstalować konsolę programu Azure PowerShell i skonfigurować go do pracy z subskrypcją platformy Azure. Po skonfigurowaniu, napraw go środowisko tworzenia skryptu można uruchomić za pomocą polecenia podobny do poniższego:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` Parametr określa nazwę do użycia podczas tworzenia konta bazy danych i magazynu, a `SqlDatabasePassword` parametr określa hasło dla konta administratora, który zostanie utworzony dla usługi SQL Database. Istnieją inne parametry, których można użyć, omówimy później.

![Okno programu PowerShell](automate-everything/_static/image2.png)

Po zakończeniu działania skryptu w portalu zarządzania można wyświetlić utworzone elementy. Znajdują się dwie bazy danych:

![Bazy danych](automate-everything/_static/image3.png)

Konto magazynu:

![Konto magazynu](automate-everything/_static/image4.png)

I aplikacji sieci web:

![Witryny sieci Web](automate-everything/_static/image5.png)

Na **Konfiguruj** kartę w aplikacji sieci web widać, że ma ona ustawienia konta magazynu i parametry połączenia bazy danych SQL skonfigurowano już poprawki jego aplikacji.

![sekcji appSettings i connectionStrings](automate-everything/_static/image6.png)

*Automatyzacji* folder teraz zawiera także  *&lt;podaną nazwą&gt;.pubxml* pliku. Ten plik przechowuje ustawienia, które program MSBuild będzie używany do wdrażania aplikacji w środowisku platformy Azure, która właśnie została utworzona. Na przykład:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Jak widać, skrypt został utworzony w środowisku ukończenia testowej, a cały proces odbywa się w ciągu około 90 sekund.

Jeśli ktoś inny w zespole chce utworzyć środowisko testowe, można po prostu uruchomić skrypt. Nie tylko jest to szybkie, ale także mogą mieć pewność, że używana taka sama jak, którego używasz środowiska. Nie można jeszcze jako pewność to wszystkim był czynności ręczne definiowanie przy użyciu interfejsu użytkownika w portalu zarządzania.

### <a name="a-look-at-the-scripts"></a>Sprawdź skryptów

Istnieją faktycznie trzy skrypty, które wykonują tę pracę. Należy wywołać z wiersza polecenia, a pozostałe dwa automatycznie używa do wykonania niektórych zadań:

- *Nowe AzureWebSiteEnv.ps1* główny skrypt.

    - *Nowe AzureStorage.ps1* tworzy konto magazynu.
    - *Nowe AzureSql.ps1* tworzy bazy danych.

### <a name="parameters-in-the-main-script"></a>Parametry w skrypcie głównym

Skrypt głównego *New AzureWebSiteEnv.ps1*, definiuje kilka parametrów:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Wymagane są dwa parametry:

- Nazwa aplikacji sieci web, która tworzy skrypt. (Jest ona również używana dla adresu URL: `<name>.azurewebsites.net`.)
- Hasło dla nowego użytkownika administracyjnego serwera bazy danych, która tworzy skrypt.

Następujące parametry opcjonalne umożliwiają określenie lokalizacji centrum danych (wartość domyślna to "Zachodnie stany USA"), nazwę administratora serwera bazy danych (wartość domyślna to "dbuser") i reguły zapory dla serwera bazy danych.

### <a name="create-the-web-app"></a>Tworzenie aplikacji sieci web

Pierwszą rzeczą, o której działanie skryptu jest, tworzenie aplikacji sieci web przez wywołanie metody `New-AzureWebsite` polecenia cmdlet, przekazując do niej aplikacji sieci web nazwę i lokalizację wartości parametrów:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Tworzenie konta magazynu

Uruchamia skrypt głównego <em>AzureStorage.ps1 nowy</em> skryptu, określając "<em>&lt;podaną nazwą&gt;</em>magazynu" dla nazwy konta magazynu i tych samych danych Centrum lokalizacji Aplikacja sieci web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nowe AzureStorage.ps1* wywołania `New-AzureStorageAccount` polecenia cmdlet, aby utworzyć konto magazynu i zwraca konto nazwy i dostęp do wartości klucza. Wartości te będą potrzebne w celu uzyskania dostępu do obiektów blob i kolejek na koncie magazynu aplikacji.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Nie zawsze warto utworzyć nowe konto magazynu; Skrypt można zwiększyć przez dodanie parametru, który opcjonalnie kieruje go do użycia istniejącego konta magazynu.

### <a name="create-the-databases"></a>Tworzenie bazy danych

Skrypt głównego następnie uruchamia skrypt tworzenia bazy danych, *New AzureSql.ps1*, gdy ustawienie domyślnej bazy danych i nazwy reguł zapory:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Skrypt tworzenia bazy danych pobiera adres IP komputerze deweloperskim i Ustawia regułę zapory, aby nawiązać połączenie i zarządzać serwerem komputerze deweloperskim. Skrypt tworzenia bazy danych, która jest następnie przechodzi przez kilka kroków do skonfigurowania bazy danych:

- Tworzy serwer przy użyciu `New-AzureSqlDatabaseServer` polecenia cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Tworzy reguły zapory, aby włączyć komputera deweloperskiego do zarządzania serwerem i włączyć w aplikacji sieci web z nim połączyć. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Tworzy kontekst bazy danych, która zawiera nazwę serwera i poświadczenia, za pomocą `New-AzureSqlDatabaseServerContext` polecenia cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` jest to funkcja w skrypcie, który wywołuje `ConvertTo-SecureString` polecenia cmdlet do szyfrowania haseł i zwraca `PSCredential` obiektu tego samego typu, który `Get-Credential` polecenie cmdlet zwraca.
- Tworzy bazę danych aplikacji i bazie danych członkostwa przy użyciu `New-AzureSqlDatabase` polecenia cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Wywołuje tocreates lokalnie zdefiniowana funkcja parametry połączenia dla każdej bazy danych. Aplikacja użyje tych parametrów połączenia do bazy danych programu access. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString jest funkcją zdefiniowaną w skrypcie, który tworzy ciąg połączenia na podstawie dostarczonych wartości parametrów.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Zwraca tabelę mieszania przy użyciu nazwy serwera bazy danych i parametry połączenia.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Aplikacja naprawić używa odrębne Członkostwo i baz danych aplikacji. Istnieje również możliwość zarówno członkostwa, jak i aplikacji dane są umieszczane w jednej bazie danych.

### <a name="store-app-settings-and-connection-strings"></a>Ustawienia app Store i parametry połączenia

Platforma Azure ma funkcję, która umożliwia przechowywanie ustawienia i parametry połączenia, które automatycznie zastąpić, co jest zwracana do aplikacji podczas próby odczytu `appSettings` lub `connectionStrings` kolekcji w pliku Web.config. Jest to alternatywa do stosowania [transformacje pliku Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) podczas wdrażania. Aby uzyskać więcej informacji, zobacz [Store poufnych danych na platformie Azure](source-control.md#appsettings) dalej w tej książce elektronicznej.

Skrypt tworzenia środowiska są przechowywane w Azure wszystkie `appSettings` i `connectionStrings` wartości, których aplikacja potrzebuje dostępu do konta magazynu i baz danych uruchamianych na platformie Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[Narzędzie New Relic](http://newrelic.com/) to struktura danych telemetrycznych, która pokażemy w [monitorowanie i dane telemetryczne](monitoring-and-telemetry.md) rozdziale. Skrypt tworzenia środowiska powoduje także ponowne uruchomienie aplikacji sieci web, aby upewnić się, że przejmuje ustawienia usługi New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Przygotowywanie do wdrożenia

Po zakończeniu procesu skryptu tworzenia środowiska wywołuje dwie funkcje do tworzenia plików, które będą używane przez skrypt wdrożenia.

Jeden z tych funkcji tworzy profil publikowania *(&lt;podaną nazwą&gt;.pubxml* pliku). Kod wywołuje interfejs API REST platformy Azure można pobrać ustawień publikowania i zapisuje informacje w *.publishsettings* pliku. Następnie wykorzystuje dane z tego pliku, wraz z plikiem szablonu (*pubxml.template*) do utworzenia *.pubxml* pliku, który zawiera profil publikowania. Ten dwuetapowy proces symuluje, co możesz zrobić w programie Visual Studio: Pobierz *.publishsettings* pliku i zaimportować, aby utworzyć profil publikowania.

Inne funkcje używa innego pliku szablonu (witryny sieci Web environment.template) do utworzenia *environment.xml witryny sieci Web* pliku zawierającego ustawienia skrypt wdrożenia użyje wraz z *.pubxml*pliku.

### <a name="troubleshooting-and-error-handling"></a>Rozwiązywanie problemów i obsługa błędów

Skrypty są podobne do programów: może zakończyć się niepowodzeniem, a w przypadku chcesz wiedzieć, jak można o awarii i co było przyczyną. Z tego powodu skryptu tworzenia środowiska zmienia wartość `VerbosePreference` zmiennych z `SilentlyContinue` do `Continue` tak, aby wszystkie komunikaty pełne są wyświetlane. Zmienia także wartość `ErrorActionPreference` zmiennych z `Continue` do `Stop`, dzięki czemu skrypt zatrzymuje, nawet wtedy, gdy napotka błędy niepowodujące:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Przed dowolnego działa, skrypt przechowuje czas rozpoczęcia, dzięki czemu można obliczyć czas, który upłynął, gdy wszystko będzie gotowe:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Po zakończeniu pracy skryptu przedstawia czas, który upłynął:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

I dla każdej operacji klucza skrypt zapisuje komunikaty pełne, na przykład:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Skrypt wdrożenia

Co *New AzureWebsiteEnv.ps1* skrypt wykonuje tworzenia środowiska *AzureWebsite.ps1 Publikuj* skrypt wykonuje wdrożenia aplikacji.

Skrypt wdrażania pobiera nazwę aplikacji internetowej z *environment.xml witryny sieci Web* plik utworzony przez skrypt tworzenia środowiska.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Pobiera hasło użytkownika wdrożenia za pomocą *.publishsettings* pliku:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Wykonuje [MSBuild](http://msbuildbook.com/) polecenia, który kompiluje i wdraża projekt:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

A jeśli został określony `Launch` parametr w wierszu polecenia, wywoływanych przez nią `Show-AzureWebsite` polecenia cmdlet, aby otworzyć domyślnej przeglądarki na adres URL witryny sieci Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Skrypt wdrożenia można uruchomić za pomocą polecenia podobny do poniższego:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

A gdy wszystko będzie gotowe, przeglądarki otwiera się z lokacją uruchomioną w chmurze w `<websitename>.azurewebsites.net` adresu URL.

![Napraw aplikacja wdrożona na platformie Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Podsumowanie

Za pomocą tych skryptów można mieć pewność, że te same kroki zawsze będą wykonywane w tej samej kolejności, przy użyciu tych samych opcji. Pozwala to zagwarantować, że każdy Deweloper w zespole nie przeoczyć coś awarię coś lub wdrożenie niestandardowe na własnej maszynie, która faktycznie nie będzie działać tak samo w środowisku innego członka zespołu lub w środowisku produkcyjnym.

W podobny sposób można zautomatyzować najbardziej Azure funkcji zarządzania, które można wykonać w portalu zarządzania za pomocą interfejsu API REST, skryptów programu Windows PowerShell, interfejsu API języka .NET lub narzędzia powłoki Bash, którą można uruchamiać w systemie Linux lub Mac.

W [następny rozdział](source-control.md) utworzymy Spójrz na kod źródłowy i wyjaśnić, dlaczego warto uwzględnić skrypty w repozytorium kodu źródłowego.

## <a name="resources"></a>Resources

- [Instalowanie i konfigurowanie programu Windows PowerShell dla platformy Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Wyjaśnia, jak zainstalować polecenia cmdlet programu Azure PowerShell oraz instrukcje dotyczące instalowania certyfikatu, należy na komputerze, aby można było zarządzać subskrypcji platformy Azure uwzględnione. Jest to doskonałe miejsce, aby rozpocząć pracę, ponieważ zawiera ona także linki do zasobów na potrzeby poznawania programu PowerShell, sam.
- [Centrum skryptów Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal witrynie WindowsAzure.com zasoby używane do tworzenia skryptów, które zarządzają usługami platformy Azure, wraz z łączami do pobierania pracy — samouczki, polecenia cmdlet odwołanie dokumentację i kod źródłowy i przykładowe skrypty
- [Autorzy skryptów weekendu: Wprowadzenie do platformy Azure i programu PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). W blogu dedykowany do programu Windows PowerShell ten wpis zawiera znakomite wprowadzenie do funkcji zarządzania platformy Azure przy użyciu programu PowerShell.
- [Instalowanie i Konfigurowanie interfejsu wiersza polecenia platformy Azure dla wielu Platform](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Wprowadzenie do samouczka platforma skryptów platformy Azure, która działa na Mac i Linux, a także Windows, systemów.
- [Narzędzia wiersza polecenia części tematu Pobierz zestawy Azure SDK i narzędzia](https://azure.microsoft.com/downloads/). Strony portalu na potrzeby dokumentacji i pobrać pliki związane z narzędzi wiersza polecenia platformy Azure.
- [Automatyzowanie wszystkiego przy użyciu bibliotek zarządzania platformy Azure i platformy .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman wprowadzono interfejs API zarządzania platformy .NET na platformie Azure.
- [Publikowanie dla deweloperów i środowisk testowych za pomocą skryptów programu Windows PowerShell](https://msdn.microsoft.com/library/azure/dn642480.aspx). Dokumentacja MSDN, który objaśnia, jak używać publikowanie skryptów, które program Visual Studio automatycznie generuje dla projektów sieci web.
- [Narzędzia programu PowerShell dla programu Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Rozszerzenia usługi Visual Studio, który dodaje obsługę języka dla programu Windows PowerShell w programie Visual Studio.

> [!div class="step-by-step"]
> [Poprzednie](introduction.md)
> [dalej](source-control.md)
