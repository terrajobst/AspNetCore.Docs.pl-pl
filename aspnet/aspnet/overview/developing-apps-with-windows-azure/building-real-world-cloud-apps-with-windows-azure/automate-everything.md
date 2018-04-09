---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatyzowanie wszystko (Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 2e30ab7831a10f215a08f74e61adf2d147e76543
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatyzowanie wszystko (Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Aby obejrzeć wprowadzenie do e-book, zobacz [pierwszy rozdział](introduction.md).


Pierwsze trzy wzorce, które przyjrzymy się faktycznie żadnego projektu rozwoju oprogramowania, ale szczególnie projektów chmury. Ten wzorzec jest dotyczące automatyzowania zadań związanych z projektowaniem. Jest ważne tematu ponieważ procesów ręcznych jest powolne, podatnych; Automatyzacja jako wiele z nich jako możliwych pomaga skonfigurować szybkie, niezawodne i elastyczne przepływ pracy. Jest unikatowo ważne na potrzeby opracowywania chmury, ponieważ można łatwo automatyzować wiele zadań, które są trudne lub niemożliwe do automatyzacji w środowisku lokalnym. Na przykład można skonfigurować całej testu środowisk, w tym nowy serwer sieci web i zaplecza maszyn wirtualnych, baz danych, obiektu blob magazynu plików, kolejki itd.

## <a name="devops-workflow"></a>DevOps przepływu pracy

Coraz słyszysz termin "DevOps." Termin opracowany poza rozpoznawania, czy masz do integracji i zadań w celu opracowywania oprogramowania wydajnie. Rodzaj przepływu pracy, który chcesz włączyć jest jeden, w którym mogą tworzyć aplikacji, wdrożyć ją, Dowiedz się od użycia produkcji go, zmienić w odpowiedzi na zostały rozpoznane i powtórz cyklu szybkie i niezawodne.

Niektóre zespoły deweloperów chmury pomyślnie wdrożyć wiele razy dziennie do środowiska produkcyjnego. Zespół Azure używane do wdrażania poważnej aktualizowane co 2 – 3 miesiące, ale teraz wersje niewielkie aktualizacje co 2 do 3 dni i główna zwalnia co 2 do 3 tygodni. Wprowadzenie do tego okresach naprawdę pomaga reagować na opinie klientów.

Aby to zrobić, należy włączyć Cykl opracowywania i wdrażania, który jest powtarzalne, niezawodne i przewidywalne, a czas cyklu niski.

![DevOps przepływu pracy](automate-everything/_static/image1.png)

Innymi słowy musi być możliwie krótki czas między Jeśli masz pomysł dla funkcji, a podczas korzystania z niego i przekazanie swoich opinii klientów. Pierwsze trzy wzorce — zautomatyzować wszystko kontroli źródła i ciągłej integracji i dostarczania — są wszystkie o najlepszych rozwiązaniach, które firma Microsoft zaleca, aby włączyć tego rodzaju procesu.

## <a name="azure-management-scripts"></a>Skrypty zarządzania platformy Azure

W [wprowadzenie do Książka elektroniczna](introduction.md), widać konsoli sieci web portalu zarządzania Azure. Portal zarządzania umożliwia monitorowanie i zarządzanie nimi, wszystkie zasoby, które zostały wdrożone na platformie Azure. Jest to prosty sposób tworzenia i usuwania usług, takich jak aplikacje sieci web i maszyn wirtualnych, konfiguracji tych usług, monitorowania operacji usługi i tak dalej. Jest to doskonałe narzędzie, ale za jego pomocą jest proces ręczny. Jeśli zamierzasz opracowywania aplikacji produkcyjnych o dowolnej wielkości, a szczególnie w środowisku zespołu zalecamy, aby go za pośrednictwem portalu interfejsu użytkownika, aby dowiedzieć się i eksplorowanie usługi Azure, a następnie automatyzować procesy, które będzie można kilkukrotnie realizacji.

Prawie wszystko, co można zrobić ręcznie w portalu zarządzania lub z programu Visual Studio można zrobić również przez wywołanie interfejsu API zarządzania REST. Napisać skrypty przy użyciu [programu Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), lub można użyć framework typu open source, takie jak [Chef](http://www.opscode.com/chef/) lub [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Umożliwia także narzędzia wiersza polecenia Bash w środowisku Mac lub Linux. Platforma Azure ma skryptów interfejsów API dla tych różnych środowisk i ma [interfejsu API zarządzania .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) w przypadku, gdy chcesz napisać kod zamiast skryptu.

Dla aplikacji Usuń utworzyliśmy niektóre skrypty programu Windows PowerShell, które automatyzują procesy tworzenia środowiska testowego i wdrażania projektu do tego środowiska i firma Microsoft można przejrzeć części zawartości tych skryptów.

## <a name="environment-creation-script"></a>Środowisko tworzenia skryptów

Nosi nazwę pierwszego skryptu przyjrzymy *AzureWebsiteEnv.ps1 nowy*. Tworzy poprawka aplikacji na potrzeby testowania wdrożenia środowiska platformy Azure. Główne zadania, które ten skrypt wykonuje są następujące:

- Tworzenie aplikacji sieci web.
- Utwórz konto magazynu. (Wymagane dla obiektów blob i kolejek, jak można zauważyć w późniejszym rozdziałach.)
- Tworzenie serwera bazy danych SQL i dwie bazy danych: bazy danych aplikacji i bazy danych członkostwa.
- Ustawienia są przechowywane w Azure, z którymi aplikacja będzie używać dostęp do konta magazynu i bazy danych.
- Utwórz pliki ustawień, które będą używane do automatyzowania wdrażania.

### <a name="run-the-script"></a>Uruchom skrypt


> [!NOTE]
> Ta część rozdziale przedstawiono przykładowe skrypty i polecenia, które należy wprowadzić w celu uruchomienia ich. Ten pokaz i nie zapewnia wszystko, co należy wiedzieć, aby można było uruchomić skrypty. Aby uzyskać instrukcje krok po kroku how-do-it, zobacz [dodatku: Napraw go przykładowej aplikacji](the-fix-it-sample-application.md#deploybase).


Aby uruchomić skrypt programu PowerShell, która zarządza usług Azure należy zainstalować konsolę programu Azure PowerShell i skonfigurować go do pracy z subskrypcją platformy Azure. Po skonfigurowaniu, można uruchomić za pomocą polecenia, takie jak ta poprawka środowisko tworzenia skryptu:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` Parametr określa nazwę do użycia podczas tworzenia konta magazynu i bazy danych i `SqlDatabasePassword` parametr określa hasło dla konta administratora, które zostaną utworzone dla bazy danych SQL. Istnieją inne parametry, których można używać, aby przyjrzymy później.

![Okno programu PowerShell](automate-everything/_static/image2.png)

Po zakończeniu działania skryptu w portalu zarządzania można wyświetlić utworzone elementy. Znajdują się dwie bazy danych:

![Bazy danych](automate-everything/_static/image3.png)

Konto magazynu:

![Konto magazynu](automate-everything/_static/image4.png)

I aplikacji sieci web:

![Witryna sieci Web](automate-everything/_static/image5.png)

Na **Konfiguruj** kartę dla aplikacji sieci web widać, że posiada ustawienia konta magazynu i parametry połączenia bazy danych SQL skonfigurowanej na potrzeby poprawkę go aplikacji.

![appSettings i connectionStrings](automate-everything/_static/image6.png)

*Automatyzacji* folder teraz zawiera także * &lt;podaną&gt;.pubxml* pliku. Ten plik przechowuje ustawienia, które MSBuild będzie używany do wdrożenia aplikacji w środowisku platformy Azure, która została właśnie utworzona. Na przykład:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Jak widać, skrypt został utworzony w środowisku testowym pełną, a cały proces odbywa się w ciągu około 90 sekund.

Jeśli ktoś inny w zespół chce utworzyć środowisko testowe, mogą uruchamiać tylko skrypt. Nie tylko jest to szybki, ale również można mieć pewność, używają środowiska identyczne, którego używasz. Nie można jeszcze jako pewność tego wszyscy został czynności ręczne definiowanie przy użyciu interfejsu użytkownika portalu zarządzania.

### <a name="a-look-at-the-scripts"></a>Obejrzyj skryptów

Są faktycznie trzy skrypty, które wykonują tę pracę. Należy wywołać z poziomu wiersza polecenia i pozostałe dwa automatycznie używa robić niektórych zadań:

- *Nowy AzureWebSiteEnv.ps1* jest głównego skryptu.

    - *Nowy AzureStorage.ps1* tworzy konto magazynu.
    - *Nowy AzureSql.ps1* tworzy baz danych.

### <a name="parameters-in-the-main-script"></a>Parametry w skrypcie głównym

Głównego skryptu *AzureWebSiteEnv.ps1 nowy*, definiuje kilka parametrów:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Wymagane są dwa parametry:

- Nazwa aplikacji sieci web, która tworzy skrypt. (Ten służy także do adresu URL: `<name>.azurewebsites.net`.)
- Hasło dla nowego użytkownika administracyjnego serwera bazy danych, która tworzy skrypt.

Parametry opcjonalne pozwalają określić lokalizację centrum danych (wartość domyślna to "Zachodnie stany USA"), nazwa administratora serwera bazy danych (wartość domyślna to "dbuser") i reguły zapory dla serwera bazy danych.

### <a name="create-the-web-app"></a>Tworzenie aplikacji sieci web

Ten skrypt wykonuje najpierw jest tworzenie aplikacji sieci web przez wywołanie metody `New-AzureWebsite` polecenia cmdlet, przekazując do niej aplikacji sieci web nazwy i lokalizacji wartości parametrów:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Tworzenie konta magazynu

A następnie uruchamia skrypt głównego <em>AzureStorage.ps1 nowy</em> skryptu, określając "<em>&lt;podaną&gt;</em>magazynu" nazwy konta magazynu, i lokalizacji co Centrum tych samych danych Aplikacja sieci web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nowy AzureStorage.ps1* wywołania `New-AzureStorageAccount` , aby utworzyć konto magazynu, a zwraca konta nazwy i dostępu do wartości kluczy. Aby uzyskać dostęp do obiektów blob i kolejek w ramach konta magazynu, aplikacja musi mieć te wartości.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Może nie zawsze chcesz utworzyć nowe konto magazynu; Skrypt można zwiększyć przez dodanie parametru, który opcjonalnie kieruje go do użycia istniejącego konta magazynu.

### <a name="create-the-databases"></a>Tworzenie bazy danych

Głównego skryptu następnie uruchamia skrypt tworzenia bazy danych, *AzureSql.ps1 nowy*, po ustawienie domyślnej bazy danych i nazwy reguł zapory:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Skrypt tworzenia bazy danych pobiera adres IP komputera deweloperów i Ustawia regułę zapory, dzięki czemu maszyny deweloperów może nawiązać połączenie i zarządzanie serwerem. Skrypt tworzenia bazy danych jest następnie przechodzi przez kilka kroków do skonfigurowania bazy danych:

- Tworzy serwer przy użyciu `New-AzureSqlDatabaseServer` polecenia cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Tworzy reguł zapory umożliwiające włączenie maszyny deweloperów, aby zarządzać serwerem i umożliwić aplikacji sieci web do nawiązania połączenia. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Tworzy kontekst bazy danych, która zawiera nazwę serwera i poświadczenia, za pomocą `New-AzureSqlDatabaseServerContext` polecenia cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` jest to funkcja w skrypcie, który wywołuje `ConvertTo-SecureString` polecenia cmdlet do szyfrowania haseł i zwraca `PSCredential` obiekt, taki sam typ, który `Get-Credential` polecenie cmdlet zwraca.
- Tworzy bazę danych aplikacji i bazy danych członkostwa przy użyciu `New-AzureSqlDatabase` polecenia cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Wywołuje tocreates zdefiniowane lokalnie funkcji parametry połączenia dla każdej bazy danych. Aplikacja będzie używać tych parametrów połączenia dostępu do baz danych. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString jest funkcją zdefiniowaną w skrypcie, który tworzy parametry połączenia na podstawie dostarczonych wartości parametrów.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Zwraca tabelę wyznaczania wartości skrótu o nazwie serwera bazy danych i parametry połączenia.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Aplikacja napraw korzysta z odrębne Członkostwo i bazy danych aplikacji. Istnieje również możliwość umieszczanie danych zarówno członkostwa i aplikacji w jednej bazie danych.

### <a name="store-app-settings-and-connection-strings"></a>Ustawienia aplikacji sklepu i parametry połączenia

Platforma Azure ma funkcja, która umożliwia do przechowywania ustawień i parametry połączenia, które automatycznie zastąpić, co jest zwracana do aplikacji podczas próby odczytu `appSettings` lub `connectionStrings` kolekcji w pliku Web.config. Jest to alternatywa zastosowaniu [przekształcenia pliku Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) podczas wdrażania. Aby uzyskać więcej informacji, zobacz [przechowywać poufnych danych na platformie Azure](source-control.md#appsettings) dalszej części tego Książka elektroniczna.

Skrypt tworzenia środowiska są przechowywane w Azure wszystkie `appSettings` i `connectionStrings` wartości, które aplikacja potrzebuje dostępu do konta magazynu i bazy danych po uruchomieniu na platformie Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[Usługi New Relic](http://newrelic.com/) to struktura telemetrii, że przedstawiony w [monitorowanie i dane telemetryczne](monitoring-and-telemetry.md) działu. Skrypt tworzenia środowiska również ponowne uruchomienie aplikacji sieci web, aby upewnić się, że przejmuje ustawienia usługi New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Przygotowywanie do wdrożenia

Po zakończeniu procesu skryptu tworzenia środowiska wywołuje dwie funkcje do tworzenia plików, które będą używane przez skrypt wdrożenia.

Jedna z tych funkcji tworzy profil publikowania *(&lt;podaną&gt;.pubxml* pliku). Kod wywołuje interfejs API REST Azure można pobrać ustawień publikowania i zapisuje informacje w *.publishsettings* pliku. Następnie używa informacji z tego pliku wraz z plikiem szablonu (*pubxml.template*) do utworzenia *.pubxml* pliku, który zawiera profil publikowania. Ten dwuetapowy proces symuluje, co można zrobić w programie Visual Studio: Pobierz *.publishsettings* pliku i zaimportować, który można utworzyć profilu publikowania.

Innych funkcji używa innego pliku szablonu (witryny sieci Web environment.template) do utworzenia *environment.xml witryny sieci Web* pliku zawierającego ustawienia skrypt wdrożenia użyje wraz z *.pubxml*pliku.

### <a name="troubleshooting-and-error-handling"></a>Rozwiązywanie problemów i obsługa błędów

Skrypty są takie jak programy: może zakończyć się niepowodzeniem, a gdy tak robią chcesz wiedzieć, jak można o awarii i co spowodowało. Z tego powodu skryptu tworzenia środowiska zmienia wartość `VerbosePreference` zmienną z `SilentlyContinue` do `Continue` tak, aby wszystkie komunikaty pełne są wyświetlane. Ponadto zmienia wartość `ErrorActionPreference` zmienną z `Continue` do `Stop`, dzięki czemu skrypt zatrzymuje nawet wtedy, gdy wystąpią błędy niepowodujący:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Zanim wszystkie działa, skrypt przechowuje czas rozpoczęcia, dzięki czemu można obliczyć czas, po zakończeniu:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Po zakończeniu pracy, skrypt wyświetli Upłynęło czasu:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

I dla każdej operacji klucza skrypt zapisuje komunikaty pełne, na przykład:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Skrypt wdrożenia

Co *AzureWebsiteEnv.ps1 nowy* skryptów jest tworzenie środowiska *AzureWebsite.ps1 publikowania* skrypt wykonuje dla wdrożenia aplikacji.

Skrypt wdrożenia pobiera nazwę aplikacji sieci web z *environment.xml witryny sieci Web* plik utworzony przez skrypt tworzenia środowiska.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Pobiera hasło użytkownika wdrożenia z *.publishsettings* pliku:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Wykonuje on [MSBuild](http://msbuildbook.com/) polecenie, które tworzy i wdraża projektu:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

A jeśli został określony `Launch` parametru w wierszu polecenia, wywołuje `Show-AzureWebsite` polecenia cmdlet, aby otworzyć domyślnej przeglądarki na adres URL witryny sieci Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Możesz uruchomić skrypt wdrożenia za pomocą polecenia podobne do następującego:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

I po jej zakończeniu przeglądarce otworzy się z lokacją uruchomioną w chmurze w `<websitename>.azurewebsites.net` adresu URL.

![Poprawka aplikacji wdrożone w systemie Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Podsumowanie

Z tych skryptów można mieć pewność, że te same kroki będą zawsze wykonywane w tej samej kolejności, przy użyciu tych samych opcji. Pomaga to zapewnić, że każdy Deweloper zespołu nie pominięcia coś mess coś lub wdrożyć coś niestandardowe na jego własnej maszynie, która faktycznie nie będą działać tak samo w środowisku innego członka zespołu lub w środowisku produkcyjnym.

W podobny sposób można zautomatyzować najbardziej Azure funkcji zarządzania, które można wykonać w portalu zarządzania za pomocą interfejsu API REST, skryptów środowiska Windows PowerShell, interfejsu API języka .NET lub narzędzie Bash, które można uruchomić w systemie Linux lub Mac.

W [następnego rozdziału](source-control.md) firma Microsoft będzie przyjrzeć się kodu źródłowego i wyjaśnić, dlaczego warto uwzględnić skrypty w repozytorium kodu źródłowego.

## <a name="resources"></a>Zasoby

- [Instalacja i Konfiguracja środowiska Windows PowerShell dla usługi Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Wyjaśniono, jak zainstalować polecenia cmdlet programu Azure PowerShell i sposobu instalacji certyfikatu, że należy na komputerze, aby można było zarządzać platformy Azure o koncie. To jest doskonałym miejscem, aby rozpocząć pracę, ponieważ zawiera ona także linki do zasobów do uczenia PowerShell sam.
- [Centrum skryptów Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal WindowsAzure.com zasoby na potrzeby tworzenia skryptów, które zarządzają usług platformy Azure, wraz z łączami do pobierania pracy — samouczki, polecenia cmdlet odwołanie do dokumentacji i kod źródłowy i przykładowe skrypty
- [Autorzy skryptów weekendowe: Wprowadzenie do platformy Azure i programu PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). W blogu dedykowanego środowiska Windows PowerShell ten wpis zawiera doskonały wprowadzenie do przy użyciu programu PowerShell dla funkcji zarządzania platformy Azure.
- [Instalowanie i Konfigurowanie interfejsu wiersza polecenia platformy Azure i Platform](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Pierwsze kroki samouczka Azure framework skryptów, który działa na Mac i Linux, a także Windows systemów.
- [Narzędzia wiersza polecenia sekcji tego tematu Pobierz zestaw Azure SDK i narzędzia](https://azure.microsoft.com/downloads/). Strony portalu dokumentacją i pobrać pliki związane z narzędzi wiersza polecenia platformy Azure.
- [Automatyzowanie wszystko z biblioteki zarządzania Azure i .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman wprowadza zarządzanie .NET interfejsu API dla platformy Azure.
- [Za pomocą skryptów programu Windows PowerShell do opublikowania dla deweloperów i środowisk testowych](https://msdn.microsoft.com/library/azure/dn642480.aspx). Dokumentacji MSDN, który objaśnia, jak używać publikowania skrypty, które Visual Studio automatycznie generuje dla projektów sieci web.
- [Narzędzia programu PowerShell dla programu Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Rozszerzenia usługi Visual Studio, które dodaje obsługę języka dla środowiska Windows PowerShell w programie Visual Studio.

> [!div class="step-by-step"]
> [Poprzednie](introduction.md)
> [dalej](source-control.md)
