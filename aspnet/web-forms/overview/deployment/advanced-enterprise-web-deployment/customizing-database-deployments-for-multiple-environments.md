---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Dostosowywanie wdrożeń bazy danych w wielu środowiskach | Dokumentacja firmy Microsoft
author: jrjlee
description: 'W tym temacie opisano, jak dostosować właściwości bazy danych w środowiskach docelowych określonych w ramach procesu wdrażania. Uwaga: Tematu założono th...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 3a368e5055f4921b68f5c5eb2739728a2f1fd4d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378076"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Dostosowywanie wdrożeń bazy danych dla wielu środowisk
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak dostosować właściwości bazy danych w środowiskach docelowych określonych w ramach procesu wdrażania.
> 
> > [!NOTE]
> > Temat zakłada wdrażany projekt bazy danych programu Visual Studio 2010 przy użyciu MSBuild.exe i VSDBCMD.exe. Aby uzyskać więcej informacji na temat Dlaczego możesz wybrać tej metody, zobacz [wdrażanie w Internecie w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) i [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Podczas wdrażania projektu bazy danych do wielu miejsc docelowych, często warto dostosować właściwości wdrożenia bazy danych dla każdego środowiska docelowego. Na przykład w środowiskach testowych będzie najczęściej ponownego tworzenia bazy danych przy każdym wdrożeniu w środowiskach przejściowych lub produkcyjnych może być znacznie częściej wprowadzić aktualizacje przyrostowe, aby zachować swoje dane.
> 
> W projekcie bazy danych programu Visual Studio 2010 ustawienia wdrożenia są zawarte w pliku konfiguracji (.sqldeployment) wdrożenia. W tym temacie pokazano sposób tworzenia plików konfiguracyjnych specyficznych dla środowiska wdrażania i określ ten, którego chcesz użyć jako parametru VSDBCMD.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

W tym temacie założono, że:

- Użyj podejście split projektu plików do wdrożenia rozwiązania, zgodnie z opisem w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Wywoływanie VSDBCMD z pliku projektu, aby wdrożyć projekt bazy danych, zgodnie z opisem w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Aby utworzyć system wdrożenia, który obsługuje różne właściwości wdrożenia bazy danych między środowiskami docelowej, musisz:

- Utwórz plik konfiguracji (.sqldeployment) wdrożenia dla każdego środowiska docelowego.
- Utwórz polecenie VSDBCMD, które określa plik konfiguracji wdrożenia jako przełącznik wiersza polecenia.
- Parametryzuj VSDBCMD polecenia w pliku projektu aparatu Microsoft Build Engine (MSBuild), dzięki czemu opcje VSDBCMD są odpowiednie dla środowiska docelowego.

W tym temacie pokazują sposób wykonywania każdego z tych procedur.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Tworzenie plików konfiguracyjnych specyficznych dla środowiska wdrażania

Projekt bazy danych zawiera domyślnie plik konfiguracji pojedynczego wdrożenia o nazwie *Database.sqldeployment*. Jeśli możesz otworzyć ten plik w programie Visual Studio 2010, zostanie wyświetlony opcje innego wdrożenia, które są dostępne dla użytkownika:

- **Sortowanie porównanie wdrożenia**. Dzięki temu można zdecydować, czy do korzystania z sortowania bazy danych projektu ( *źródła* sortowania) lub z sortowaniem bazy danych na serwerze docelowym ( *docelowej* sortowania). W większości przypadków będziesz chciał użyć sortowania źródła, podczas wdrażania do rozwoju lub środowiska testowego. Podczas wdrażania w środowisku tymczasowym lub produkcyjnym zwykle należy pozostawić bez zmian, aby uniknąć jakichkolwiek problemów współdziałanie docelowym sortowaniu.
- **Wdrażanie bazy danych właściwości**. Dzięki temu można zdecydować, czy zastosować właściwości bazy danych zgodnie z definicją w *Database.sqlsettings* pliku. Podczas wdrażania bazy danych po raz pierwszy, należy wdrożyć właściwości bazy danych. Jeśli aktualizujesz istniejącą bazę danych, właściwości powinien już znajdować się w miejscu, a nie należy wdrażać je ponownie.
- **Zawsze ponownie utworzyć bazę danych**. Dzięki temu, który określa, czy ponownie utworzyć docelowej bazy danych, za każdym razem, gdy wdrażanie lub zmiany przyrostowe do docelowej bazy danych na bieżąco ze schematu. Jeśli ponownie utworzyć bazę danych, utracisz wszystkie dane w istniejącej bazy danych. Dlatego należy zwykle ustawić to **false** dla wdrożeń w środowiskach przejściowych lub produkcyjnych.
- **Zablokować wdrożenie przyrostowe, gdy może wystąpić utrata danych**. Dzięki temu można wybrać, czy wdrożenie ma zostać zatrzymana, jeśli zmiana schematu bazy danych spowoduje utratę danych. Zazwyczaj Ustaw tę opcję na **true** wdrożenia do środowiska produkcyjnego, aby zapewnić możliwość interweniować i chronić wszystkie ważne dane. Jeśli ustawiono **zawsze ponownie utwórz bazę danych** do **false**, to ustawienie nie będzie miała zastosowania.
- **Wykonaj wdrożenie w trybie jednego użytkownika**. Nie stanowi to zwykle problemu w środowiskach deweloperskich lub testowania. Jednak należy zwykle ustawić to **true** dla wdrożeń w środowiskach przejściowych lub produkcyjnych. To uniemożliwia użytkownikom wprowadzanie zmian do bazy danych, gdy wdrożenie jest w toku.
- **Utwórz kopię zapasową bazy danych przed wdrożeniem**. Zazwyczaj Ustaw tę opcję na **true** podczas wdrażania w środowisku produkcyjnym w celu ochrony przed utratą danych. Możesz również ustawić ją na **true** podczas wdrażania w środowisku przejściowym, jeśli tymczasowej bazy danych zawiera dużą ilość danych.
- **Generuj instrukcje usuwania obiektów, które znajdują się w docelowej bazie danych, ale nie są w projekcie bazy danych**. W większości przypadków jest to całkowite i istotnych części wprowadzania przyrostowych zmian do bazy danych. Jeśli ustawiono **zawsze ponownie utwórz bazę danych** do **false**, to ustawienie nie będzie miała zastosowania.
- **Nie używaj instrukcji ALTER ASSEMBLY można zaktualizować typów CLR**. To ustawienie określa, jak zaktualizować typowych języka wspólnego (CLR) do nowszych wersji zestawów programu SQL Server. To powinien być ustawiony na **false** w większości scenariuszy.

W poniższej tabeli przedstawiono typowe wdrożenie ustawienia w środowiskach różnych miejsc docelowych. Jednak ustawienia mogą być różne w zależności od wymagań dokładne.

|  | Testowanie dla deweloperów | Etap przejściowy/integracji | Produkcji |
| --- | --- | --- | --- |
| **Sortowanie porównanie wdrożenia** | Źródło | Docelowy | Docelowy |
| **Wdrażanie właściwości bazy danych** | True | Tylko po raz pierwszy | Tylko po raz pierwszy |
| **Zawsze ponownie utworzyć bazę danych** | True | False | False |
| **Zablokować wdrożenie przyrostowe, gdy może wystąpić utrata danych** | False | Być może | True |
| **Wykonywanie skryptu wdrożenia w trybie jednego użytkownika** | False | True | True |
| **Tworzenie kopii zapasowej bazy danych przed przystąpieniem do wdrożenia** | False | Być może | True |
| **Generuj instrukcje usuwania obiektów, które znajdują się w docelowej bazie danych, ale nie są w projekcie bazy danych** | False | True | True |
| **Nie używaj instrukcji ALTER ASSEMBLY można zaktualizować typów CLR** | False | False | False |
  

> [!NOTE]
> Aby uzyskać więcej informacji na temat właściwości wdrożenia bazy danych i zagadnienia dotyczące środowiska, zobacz [przegląd z ustawienia projektu bazy danych](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [porady: Konfigurowanie właściwości szczegóły wdrożenia](https://msdn.microsoft.com/library/dd172125.aspx), [ Tworzenie i wdrażanie bazy danych do środowiska izolowanego rozwoju](https://msdn.microsoft.com/library/dd193409.aspx), i [tworzenia i wdrażania baz danych w tymczasowym lub produkcyjnym środowisku](https://msdn.microsoft.com/library/dd193413.aspx).


Aby zapewnić obsługę wdrażania projektu bazy danych do wielu miejsc docelowych, należy utworzyć plik konfiguracji wdrożenia dla każdego środowiska docelowego.

**Aby utworzyć plik konfiguracji specyficznych dla środowiska**

1. W programie Visual Studio 2010 w **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy projekt bazy danych, a następnie kliknij przycisk **właściwości**.
2. Na stronie właściwości projektu bazy danych na **Wdróż** na karcie **plik konfiguracji wdrożenia** wiersz, kliknij przycisk **New**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. W **nowy plik konfiguracji wdrożenia** okna dialogowego pole, nadaj plikowi nazwę opisową (na przykład **TestEnvironment.sqldeployment**), a następnie kliknij przycisk **Zapisz**.
4. Na *[nazwa_pliku] *** .sqldeployment** strony, ustaw właściwości wdrożenia w celu dopasowania do wymagań środowiska docelowego, a następnie zapisz plik.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Należy zauważyć, że nowy plik zostanie dodany do właściwości folderu w projekcie bazy danych.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Określanie pliku konfiguracji wdrożenia w VSDBCMD

Gdy używasz konfiguracje rozwiązania (na przykład Debug i Release) w programie Visual Studio 2010, można skojarzyć z plikiem konfiguracyjnym wdrożenia z konfiguracjami. Podczas kompilowania określoną konfigurację procesu kompilacji generuje specyficznych dla konfiguracji wdrażania pliku manifestu, który wskazuje plik konfiguracyjny specyficznych dla konfiguracji wdrażania. Jednak jest jednym z głównych celów podejścia do wdrażania opisanych w tych samouczkach aby zapewnić użytkownikom możliwość kontrolowania procesu wdrażania bez korzystania z programu Visual Studio 2010 i konfiguracje rozwiązania. W przypadku tej metody konfiguracji rozwiązania jest taki sam, niezależnie od docelowego środowiska wdrażania. Dostosować wdrożenia bazy danych do określonego miejsca docelowego środowiska, do VSDBCMD opcje wiersza polecenia służy do określania pliku konfiguracji wdrożenia.

Aby określić plik konfiguracji wdrożenia w Twojej VSDBCMD, użyj **p:/DeploymentConfigurationFile** Przełącz i podaj pełną ścieżkę do pliku. Spowoduje to zastąpienie pliku konfiguracji wdrożenia, który identyfikuje manifestu wdrażania. Na przykład można użyć tego polecenia VSDBCMD wdrażania **ContactManager** bazy danych do środowiska testowego:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Należy zauważyć, że proces kompilacji może Zmień nazwę pliku .sqldeployment podczas kopiowania pliku do katalogu wyjściowego.


Użycie zmiennych poleceń SQL w skrypty SQL przed wdrożeniem lub po wdrożeniu, można użyć podejście podobne do skojarzenia z plikiem .sqlcmdvars specyficznymi dla środowiska z wdrożeniem. W tym przypadku używasz **p:/SqlCommandVariablesFile** przełącznik, aby zidentyfikować .sqlcmdvars pliku.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Uruchamiając polecenie VSDBCMD z pliku projektu MSBuild

Możesz wywołać polecenie VSDBCMD w pliku projektu MSBuild przy użyciu **Exec** zadanie w ramach obiekt docelowy programu MSBuild. W najprostszej postaci wyglądała następująco:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- W praktyce aby ułatwić odczytywanie i ponownego użycia, plików projektu należy utworzyć właściwości do przechowywania różnych parametrów wiersza polecenia. Ułatwia użytkownikom podawanie wartości właściwości w pliku projektu specyficznymi dla środowiska lub zastąpić wartości domyślne w wierszu polecenia programu MSBuild. Jeśli używasz podziału podejście pliku projektu, opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), należy rozdzielać właściwości między dwoma plikami i instrukcje kompilacji, na których odpowiednio:
- Ustawienia specyficzne dla środowiska, takie jak nazwa pliku konfiguracji wdrożenia, parametry połączenia bazy danych i nazwa docelowej bazy danych powinny przejść w pliku projektu specyficznego dla danego środowiska.
- Docelowy programu MSBuild, który uruchamia polecenie VSDBCMD, wraz z dowolnego universal właściwości, takie jak lokalizacja pliku wykonywalnego VSDBCMD, należy go w pliku projektu uniwersalnej.

Należy również upewnić się, skompiluj projekt bazy danych, zanim wywołania VSDBCMD tak, aby plik .deploymanifest jest utworzone i gotowe do użycia. Możesz zobaczyć pełny przykład tej metody, w tym temacie [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), który przeprowadzi Cię przez pliki projektu [Contact Manager przykładowe rozwiązanie](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano, jak dostosować właściwości bazy danych do różnych miejsc docelowych środowisk podczas wdrażania projektów bazy danych przy użyciu programu MSBuild i VSDBCMD. To podejście jest przydatne, gdy zajdzie potrzeba wdrożenia projektów bazy danych jako część większe, rozwiązania w skali przedsiębiorstwa. Rozwiązania te często są wdrażane w wielu miejsc docelowych, takich jak piaskownicy środowiska deweloperskie lub testowe, przejściowe lub integracji i produkcji lub środowiskach produkcyjnych. Każda z tych środowisk docelowych zwykle wymaga unikatowego zestawu właściwości wdrożenia bazy danych.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat wdrażania projektów bazy danych przy użyciu VSDBCMD.exe, zobacz [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać więcej informacji na temat korzystania z niestandardowych plików projektu MSBuild do kontrolowania procesu wdrażania, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Te artykuły w witrynie MSDN zawierają bardziej ogólne wskazówki dotyczące wdrażania bazy danych:

- [Omówienie ustawienia projektu bazy danych](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Porady: Konfigurowanie właściwości szczegóły wdrożenia](https://msdn.microsoft.com/library/dd172125.aspx)
- [Twórz i wdrażaj bazy danych do środowiska izolowanego rozwoju](https://msdn.microsoft.com/library/dd193409.aspx)
- [Tworzenie i wdrażanie baz danych w tymczasowym lub produkcyjnym środowisku](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Poprzednie](performing-a-what-if-deployment.md)
> [dalej](deploying-database-role-memberships-to-test-environments.md)
