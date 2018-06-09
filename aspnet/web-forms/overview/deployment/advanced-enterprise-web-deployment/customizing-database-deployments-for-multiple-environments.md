---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Dostosowywanie wdrożenia bazy danych w wielu środowiskach | Dokumentacja firmy Microsoft
author: jrjlee
description: 'W tym temacie opisano, jak dostosować właściwości bazy danych do określonego celu środowisk jako część procesu wdrażania. Uwaga: Temacie założono th...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 06f22bc9a3068ee5621df62ee5ed1bea06d7e9e6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30881360"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Dostosowywanie wdrożenia bazy danych w wielu środowiskach
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak dostosować właściwości bazy danych do określonego celu środowisk jako część procesu wdrażania.
> 
> > [!NOTE]
> > Temat przyjęto założenie, że jest wdrażany projekt bazy danych programu Visual Studio 2010, używając MSBuild.exe i VSDBCMD.exe. Aby uzyskać więcej informacji na Dlaczego możesz wybrać tej metody, zobacz [Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) i [wdrażania projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Podczas wdrażania projektu bazy danych do wielu miejsc docelowych, często należy dostosować właściwości wdrożenia bazy danych dla każdego środowiska docelowego. Na przykład w środowisku testowym będzie zazwyczaj ponownego tworzenia bazy danych przy każdym wdrożeniu w środowisku tymczasowym czy produkcyjnym może być znacznie bardziej prawdopodobne zachować dane aktualizacje przyrostowe.
> 
> W projekcie bazy danych programu Visual Studio 2010 ustawienia wdrażania są zawarte w pliku konfiguracji (.sqldeployment) wdrożenia. W tym temacie opisano sposób tworzenia plików konfiguracji wdrożenia określonego środowiska i określić, który ma być używany jako parametr VSDBCMD.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

W tym temacie założono, że:

- Użyj podziału podejścia pliku projektu wdrażania rozwiązania, zgodnie z opisem w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Wywołanie VSDBCMD z pliku projektu, aby wdrożyć projekt bazy danych, zgodnie z opisem w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Można utworzyć systemu wdrożenia, obsługujący różne właściwości wdrożenia bazy danych między środowiskami docelowej, musisz:

- Utwórz plik konfiguracji (.sqldeployment) wdrożenia dla każdego środowiska docelowego.
- Utwórz polecenie VSDBCMD, które określa plik konfiguracji wdrożenia jako przełącznik wiersza polecenia.
- Parametryzacja polecenia VSDBCMD w pliku projektu Microsoft kompilacji Engine (MSBuild), dzięki czemu opcje VSDBCMD są odpowiednie do środowiska docelowego.

W tym temacie opisano sposób wykonywania każdego z tych procedur.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Tworzenie plików konfiguracji określonego środowiska wdrażania

Domyślnie projekt bazy danych zawiera plik konfiguracji wdrożenia jednego o nazwie *Database.sqldeployment*. Po otwarciu tego pliku w Visual Studio 2010, można wyświetlić opcje inne wdrożenie, które są dostępne:

- **Sortowanie porównania wdrożenia**. Dzięki temu można wybrać, czy do korzystania z sortowania bazy danych projektu ( *źródła* sortowania) lub z sortowaniem bazy danych na serwerze docelowym ( *docelowej* sortowania). W większości przypadków należy używać sortowania źródła podczas wdrażania na komputerze projektowym lub testowym. Podczas wdrażania w środowisku tymczasowym czy produkcyjnym zwykle należy pozostawić bez zmian, aby uniknąć problemów ze współdziałaniem docelowym sortowaniu.
- **Właściwości bazy danych wdrażania**. Dzięki temu można wybrać, czy zastosować właściwości bazy danych, zgodnie z definicją w *Database.sqlsettings* pliku. Podczas wdrażania bazy danych po raz pierwszy, należy wdrożyć właściwości bazy danych. Jeśli aktualizujesz istniejącą bazę danych właściwości już powinny zostać zastosowane, a nie należy wdrażać je ponownie.
- **Zawsze ponownie utworzyć bazę danych**. Pozwala to wybrać czy ponownie utworzyć docelowej bazy danych w każdym wdrażania lub zmiany przyrostowe do docelowej bazy danych na bieżąco z schemat. Jeśli ponownie utworzyć bazę danych, utracisz wszystkie dane w istniejącej bazy danych. Tak, można zwykle ustawić to **false** do wdrożeń na środowisku tymczasowym czy produkcyjnym.
- **Blokować przyrostowe wdrożenia, jeśli może wystąpić utrata danych**. Dzięki temu można wybrać, czy wdrożenie ma zostać zatrzymana, jeśli zmiany schematu bazy danych spowoduje utratę danych. Zwykle opcji **true** wdrożenia do środowiska produkcyjnego, aby zapewnić możliwość interweniować, a następnie włącz ochronę wszystkich ważnych danych. Jeśli ustawiono **bazy danych zawsze ponownie utworzyć** do **false**, to ustawienie nie odniesie żadnego skutku.
- **Wykonaj wdrożenie w trybie jednego użytkownika**. To nie jest zazwyczaj problem środowisk deweloperskich lub testowania. Jednak należy zwykle ustawić to **true** wdrożeń w środowiskach, w tymczasowym czy produkcyjnym. Zapobiega to wprowadzania zmian w bazie danych, gdy wdrożenie jest przetwarzane przez użytkowników.
- **Utwórz kopię zapasową bazy danych przed wdrożeniem**. Zwykle opcji **true** podczas wdrażania w środowisku produkcyjnym w celu ochrony przed utratą danych. Możesz ustawić ją na **true** podczas wdrażania w środowisku przemieszczania, jeżeli tymczasowa baza danych zawiera dużą ilość danych.
- **Generuj instrukcje UPUSZCZANIA dla obiektów, które znajdują się w docelowej bazie danych, ale nie znajdują się w bazie danych projektu**. W większości przypadków jest integralną i podstawową część przyrostowe zmiany bazy danych. Jeśli ustawiono **bazy danych zawsze ponownie utworzyć** do **false**, to ustawienie nie odniesie żadnego skutku.
- **Nie należy używać instrukcji instrukcja ALTER ASSEMBLY można zaktualizować typów CLR**. To ustawienie określa, jak zaktualizować typowych języka wspólnego (CLR) do nowszej wersji zestawu programu SQL Server. To powinien być ustawiony na **false** w większości przypadków.

W poniższej tabeli zamieszczono Ustawienia typowe wdrożenie w środowiskach różnych miejsc docelowych. Jednak ustawienia mogą być różne w zależności od wymagań dokładne.

|  | Deweloperów i testowanie | Przemieszczania/integracji | Produkcji |
| --- | --- | --- | --- |
| **Sortowanie porównania wdrożenia** | Źródło | docelowy | docelowy |
| **Wdrażanie właściwości bazy danych** | True | Tylko po raz pierwszy | Tylko po raz pierwszy |
| **Zawsze ponownie utworzyć bazę danych** | True | False | False |
| **Blokować przyrostowe wdrożenia, jeśli może wystąpić utrata danych** | False | Być może | True |
| **Uruchom skrypt wdrażania w trybie jednego użytkownika** | False | True | True |
| **Wykonaj kopię zapasową bazy danych przed wdrożeniem** | False | Być może | True |
| **Generuj instrukcje UPUSZCZANIA obiektów, które znajdują się w docelowej bazie danych, ale nie znajdują się w bazie danych projektu** | False | True | True |
| **Nie należy używać instrukcji instrukcja ALTER ASSEMBLY można zaktualizować typów CLR** | False | False | False |
  

> [!NOTE]
> Aby uzyskać więcej informacji o właściwościach wdrożenia bazy danych i zagadnienia dotyczące środowiska, zobacz [omówienie z ustawień bazy danych projektu](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [jak: Konfigurowanie właściwości szczegóły wdrożenia](https://msdn.microsoft.com/library/dd172125.aspx), [ Tworzenie i wdrażanie bazy danych do środowiska projektowego izolowanego](https://msdn.microsoft.com/library/dd193409.aspx), i [tworzenie i wdrażanie baz danych do środowiska produkcyjnego lub przemieszczania](https://msdn.microsoft.com/library/dd193413.aspx).


Aby obsłużyć wdrożenie projektu bazy danych do wielu miejsc docelowych, należy utworzyć plik konfiguracji wdrożenia dla każdego środowiska docelowego.

**Aby utworzyć plik konfiguracji środowiska**

1. W programie Visual Studio 2010 w **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy projekt bazy danych, a następnie kliknij przycisk **właściwości**.
2. Na stronie właściwości projektu bazy danych na **Wdróż** karcie **plik konfiguracji wdrożenia** wiersz, kliknij przycisk **nowy**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. W **nowy plik konfiguracji wdrożenia** okno dialogowe pola, nadaj plikowi nazwę opisową (na przykład **TestEnvironment.sqldeployment**), a następnie kliknij przycisk **zapisać**.
4. Na *[nazwapliku] *** .sqldeployment** strony, ustaw właściwości wdrożenia, aby można było zgodne z wymaganiami środowiska docelowego, a następnie zapisz plik.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Zwróć uwagę, że nowy plik został dodany do właściwości folderu projektu bazy danych.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Określanie w VSDBCMD plik konfiguracji wdrożenia

Gdy używasz konfiguracje rozwiązania (na przykład Debug i Release) w programie Visual Studio 2010, plik konfiguracji wdrożenia można skojarzyć z każdej konfiguracji. Podczas konstruowania określoną konfigurację procesu kompilacji generuje plik manifestu specyficzne dla konfiguracji wdrażania, który wskazuje plik konfiguracji wdrożenia specyficzne dla konfiguracji. Jednak jest jednym z głównych celów podejścia do wdrażania opisane w tych samouczkach do nadawania użytkownikom możliwość kontrolowania proces wdrożenia bez korzystania z programu Visual Studio 2010 i konfiguracje rozwiązania. W tej metody konfiguracji rozwiązania jest taki sam, niezależnie od wdrażania środowiska docelowego. Dostosować wdrożenia bazy danych w środowisku określonych docelowych, opcje wiersza polecenia VSDBCMD służy do określ plik konfiguracji wdrożenia.

Aby określić plik konfiguracji wdrożenia w sieci VSDBCMD, użyj **p:/DeploymentConfigurationFile** przełącznika i podaj pełną ścieżkę do pliku. Spowoduje to zastąpienie pliku konfiguracji wdrożenia, który identyfikuje manifest wdrażania. Na przykład można użyć tego polecenia VSDBCMD wdrażania **ContactManager** bazy danych do środowiska testowego:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Należy pamiętać, że proces kompilacji może zmienić nazwę pliku .sqldeployment podczas kopiowania pliku do katalogu wyjściowego.


Użycie zmiennych poleceń SQL w skryptach SQL przed wdrożeniem lub po wdrożeniu, można użyć podejście podobne do skojarzenia z wdrożeniem pliku .sqlcmdvars określonego środowiska. W takim przypadku należy użyć **p:/SqlCommandVariablesFile** przełącznik, aby zidentyfikować pliku .sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Uruchamiając polecenie VSDBCMD z pliku projektu MSBuild

Polecenie VSDBCMD w pliku projektu MSBuild można wywołać za pomocą **Exec** zadanie w ramach docelowy programu MSBuild. W najprostszej postaci będzie wyglądać następująco:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- W praktyce plików projektu ułatwia odczytu i ponownie użyć, należy utworzyć właściwości do przechowywania różnych parametrów wiersza polecenia. Dzięki temu można ułatwić użytkownikom, aby przekazać wartości właściwości w pliku projektu określonego środowiska lub zastąpić wartości domyślne w wierszu polecenia programu MSBuild. Jeśli używasz podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), możesz powinna być dzielnikiem liczby właściwości między dwoma plikami i instrukcje kompilacji, na których odpowiednio:
- Ustawienia charakterystyczne dla środowiska, takie jak nazwa pliku konfiguracji wdrożenia, ciąg połączenia bazy danych i nazwa docelowej bazy danych należy go w pliku projektu określonego środowiska.
- Docelowy programu MSBuild, który uruchamia polecenie VSDBCMD, wraz z dowolnego uniwersalnych właściwości, takie jak lokalizacji pliku wykonywalnego VSDBCMD, należy go w pliku projektu uniwersalnej.

Należy również upewnić się, skompiluj projekt bazy danych, przed wywołania VSDBCMD, dzięki czemu plik .deploymanifest został utworzony i gotowe do użycia. Widoczny jest pełny przykład tej metody w temacie [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), który przeprowadzi Cię przez kolejne pliki projektu [kontaktów Menedżerze przykładowe rozwiązanie](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano, jak dostosować właściwości bazy danych do innej docelowej środowisk podczas wdrażania projektów bazy danych przy użyciu programu MSBuild i VSDBCMD. Takie rozwiązanie jest przydatne, gdy należy wdrożyć projektów bazy danych jako część większe, rozwiązania skali przedsiębiorstwa. Te rozwiązania często są wdrażane na wielu miejsc docelowych, takich jak piaskownicy środowisk deweloperskich lub testowania, przemieszczania lub integracji i produkcji lub środowiskach produkcyjnych. Każdy z tych środowisk docelowy zwykle wymaga unikatowego zestawu właściwości wdrożenia bazy danych.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat wdrażania projektów bazy danych przy użyciu VSDBCMD.exe, zobacz [wdrażania projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać więcej informacji na temat używania niestandardowe pliki projektu MSBuild kontrolować proces wdrażania, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Te artykuły w witrynie MSDN zawierają bardziej ogólne wskazówki dotyczące wdrażania bazy danych:

- [Omówienie ustawienia projektu bazy danych](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Porady: Konfigurowanie właściwości szczegółów wdrożenia](https://msdn.microsoft.com/library/dd172125.aspx)
- [Tworzenie i wdrażanie baz danych do środowiska projektowego izolowanym](https://msdn.microsoft.com/library/dd193409.aspx)
- [Tworzenie i wdrażanie baz danych do środowiska produkcyjnego lub przemieszczania](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Poprzednie](performing-a-what-if-deployment.md)
> [dalej](deploying-database-role-memberships-to-test-environments.md)
