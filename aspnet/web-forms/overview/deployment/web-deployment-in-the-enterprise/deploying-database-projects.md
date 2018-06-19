---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Wdrażanie projektów bazy danych | Dokumentacja firmy Microsoft
author: jrjlee
description: 'Uwaga: W wielu scenariuszach dla przedsiębiorstwa wdrożenia, potrzebna jest możliwość do publikowania aktualizacji przyrostowych wdrożonej bazy danych. Alternatywą jest odtworzenie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: a0b3871ea098b549271bce2b9d5f0c24f9ca8a9c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882504"
---
<a name="deploying-database-projects"></a>Wdrażanie projektów bazy danych
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> W wielu scenariuszach dla przedsiębiorstwa wdrożenia potrzebna jest możliwość do publikowania aktualizacji przyrostowych wdrożonej bazy danych. Alternatywą jest ponownie utworzyć bazę danych przy każdym wdrożeniu, co oznacza, że zostaną utracone dane w istniejącej bazie danych. Podczas pracy z programu Visual Studio 2010, przy użyciu VSDBCMD jest zalecane podejście do przyrostowe publikowanie bazy danych. Jednak na następną wersję programu Visual Studio i Web potok publikowania (WPP) zawiera narzędzia, który obsługuje przyrostowe publikowanie bezpośrednio.


Po otwarciu menedżera kontaktu przykładowe rozwiązanie w Visual Studio 2010, zobaczysz, że projekt bazy danych zawiera właściwości folderu, który zawiera cztery pliki.

![](deploying-database-projects/_static/image1.png)

Wraz z pliku projektu (*ContactManager.Database.dbproj* w tym przypadku), te pliki kontrolować różne aspekty procesem kompilacji i wdrażania:

- *Database.sqlcmdvars* plik zawiera wartości wszelkich zmiennych SQLCMD można używać do wdrażania projektu. Każdej konfiguracji rozwiązania (na przykład debug i release) można określić plik różnych .sqlcmdvars.
- *Database.sqldeployment* plik zawiera ustawienia charakterystyczne dla wdrożenia, takie jak czy używać sortowania zdefiniowane w projekcie lub sortowanie na serwerze docelowym, czy ponownie utworzyć docelowej bazy danych co czasu lub po prostu zmodyfikować istniejącą bazę danych, aby przełączyć go do daty i tak dalej. Konfiguracja każdego rozwiązania można określić plik różnych .sqldeployment.
- *Database.sqlpermissions* plik jest dokument XML, który służy do definiowania uprawnień, które chcesz dodać do docelowej bazy danych. Wszystkie konfiguracje rozwiązania udostępnianie tego samego pliku .sqlpermissions.
- *Database.sqlsettings* plik Określa właściwości poziom bazy danych do użycia podczas tworzenia bazy danych, takich jak sortowania, zachowanie operatory porównania i tak dalej. Wszystkie konfiguracje rozwiązania udostępnianie tego samego pliku .sqlsettings.

Warto biorąc chwilę, aby otworzyć tych plików w programie Visual Studio i zapoznać się z zawartością.

Podczas kompilowania projektu bazy danych procesu kompilacji tworzy dwa pliki:

- A *schematu bazy danych* (plik .dbschema). Opisuje schemat bazy danych, która ma zostać utworzony w formacie XML.
- A *manifest rozmieszczenia* (plik .deploymanifest). Zawiera wszystkie informacje wymagane do tworzenia i wdrażania bazy danych. Odwołuje się plik .dbschema oraz inne zasoby, takie jak instrukcje dotyczące wdrażania (plik .sqldeployment) i skrypty SQL przed wdrożeniem lub po wdrożeniu.

Oznacza to relacji między tymi zasobami:

![](deploying-database-projects/_static/image2.png)

Jak widać, plik .sqlsettings i plik .sqlpermissions są dane wejściowe, aby proces kompilacji. Wraz z pliku projektu bazy danych pliki te są używane do tworzenia pliku schematu bazy danych. Plik .sqldeployment i plik .sqlcmdvars przechodzą przez proces kompilacji bez zmian. Manifest rozmieszczenia wskazuje lokalizację schemat bazy danych, plików .sqldeployment pliku .sqlcmdvars i skrypty SQL przed wdrożeniem lub po wdrożeniu.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Dlaczego warto używać VSDBCMD wdrożyć projekt bazy danych?

Istnieją różne różne podejścia do wdrażania projektów bazy danych. Jednak nie wszystkie z nich są odpowiednie do wdrażania projektu bazy danych na serwerach zdalnych w środowisku przedsiębiorstwa. Co należy rozważyć z wdrożenia projektu bazy danych. W scenariuszach wdrażania w przedsiębiorstwie prawdopodobnie chcesz:

- Możliwość wdrażania projektu bazy danych z lokalizacji zdalnej.
- Możliwość zapewnienia aktualizacji przyrostowych istniejącą bazę danych.
- Zdolność do uwzględnienia przed wdrożeniem lub po wdrożeniu skrypty.
- Możliwość dostosowania wdrożenia w wielu środowiskach, w miejsce docelowe.
- Możliwość wdrażania projektu bazy danych jako część większego, zwykle uwzględnione w skryptach, wdrożenie rozwiązania pojedynczy krok.

Istnieją trzy główne metody można użyć do wdrożenia projektu bazy danych:

- Funkcja wdrożenia z typem projektu bazy danych w Visual Studio 2010. Podczas tworzenia i wdrażania bazy danych projektu w programie Visual Studio 2010, proces wdrażania używa manifest wdrażania Generowanie pliku wdrożenia na podstawie SQL specyficzne dla konfiguracji kompilacji. Spowoduje to utworzenie bazy danych, jeśli plik już nie istnieje lub wprowadź niezbędne zmiany w bazie danych, jeśli już istnieje. SQLCMD.exe umożliwia uruchamianie tego pliku na serwer docelowy, lub możesz ustawić Visual Studio, aby utworzyć i uruchomić plik. Wadą tego podejścia jest ma tylko ograniczoną kontrolę nad ustawienia wdrażania. Może być konieczne często również zmodyfikować plik wdrożenia SQL, aby podać wartości zmiennej określonego środowiska. Takie podejście z komputera można używać tylko z programu Visual Studio 2010, a deweloper musi wiedzieć i Podaj parametry połączenia i poświadczenia do wszystkich środowisk docelowego.
- Narzędzie Internet Information Services (IIS) Web Deployment (Web Deploy) służy do [wdrożyć bazę danych w ramach projektu aplikacji sieci web](https://msdn.microsoft.com/library/dd465343.aspx). Jednak ta metoda jest znacznie bardziej skomplikowane, aby wdrożyć projekt bazy danych, a nie po prostu zreplikować istniejącej lokalnej bazy danych na serwerze docelowym. Można skonfigurować narzędzie Web Deploy, aby uruchomić skrypt wdrożenia programu SQL, który generuje projekt bazy danych, ale w tym celu, należy utworzyć niestandardowy plik elementów docelowych WPP dla projektu aplikacji sieci web. Spowoduje to dodanie znacznej ilości złożoności procesu wdrażania. Ponadto narzędzia Web Deploy nie obsługuje bezpośrednio aktualizacje przyrostowe istniejących baz danych. Aby uzyskać więcej informacji dotyczących tej metody, zobacz [rozszerzanie potoku publikowania w sieci Web, do projektu bazy danych pakietu wdrożonym pliku SQL](https://go.microsoft.com/?linkid=9805121).
- Narzędzie VSDBCMD wdrażania bazy danych, za pomocą schematu bazy danych lub manifest wdrażania. VSDBCMD.exe można wywołać z obiektu docelowego MSBuild, co umożliwia publikowanie baz danych w ramach procesu wdrażania większych i obsługę skryptów. Można zastąpić zmiennych w pliku .sqlcmdvars i wiele innych właściwości bazy danych z polecenia VSDBCMD, co pozwala na dostosowywanie wdrożenia w różnych środowiskach bez tworzenia wielu konfiguracji kompilacji. VSDBCMD udostępnia funkcję zróżnicowanie, co oznacza, że szablon wprowadzi tylko niezbędne zmiany, aby były wyrównane z schemat bazy danych w docelowej bazie danych. VSDBCMD oferuje szeroką gamę opcji wiersza polecenia, które zapewniają precyzyjną kontrolę nad procesem wdrażania.

Z tego przeglądu możesz sprawdzić, czy przy użyciu programu MSBuild przy użyciu VSDBCMD jest najbardziej odpowiednie do scenariusza wdrażania typowego przedsiębiorstwa podejście:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Obsługuje wdrażanie zdalnego? | Tak | Tak | Tak |
| Obsługuje aktualizacji przyrostowych? | Tak | Nie | Tak |
| Obsługuje skrypty przed/po-deployment? | Tak | Tak | Tak |
| Obsługuje środowisko wdrażania? | Ograniczone | Ograniczone | Tak |
| Obsługuje wdrożenia inicjowane przez skrypty? | Ograniczone | Tak | Tak |

W pozostałej części w tym temacie opisuje VSDBCMD przy użyciu programu MSBuild, aby wdrożyć projektów bazy danych.

## <a name="understanding-the-deployment-process"></a>Opis procesu wdrażania

Narzędzie VSDBCMD pozwala wdrożyć bazę danych przy użyciu schematu bazy danych (plik .dbschema) lub manifest rozmieszczenia (plik .deploymanifest). W praktyce prawie zawsze użyjesz manifest wdrażania, ponieważ manifest wdrażania pozwala podać wartości domyślnej dla różnych właściwości wdrożenia i Znajdź wszystkie przed wdrożeniem lub po wdrożeniu skrypty SQL, który chcesz uruchomić. Na przykład to polecenie VSDBCMD służy do wdrażania **ContactManager** bazy danych na serwerze bazy danych w środowisku testowym:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


W takim przypadku:

- **/A** (lub **należy**) przełącznik określa, co ma VSDBCMD zrobić. Możesz ustawić **importu** lub **Wdróż**. **Importu** opcja jest używana do generowania pliku .dbschema z istniejącej bazy danych i **Wdróż** opcja jest używana do wdrażania pliku .dbschema do docelowej bazy danych.
- **/Manifest** (lub **/ManifestFile**) przełącznik określa plik .deploymanifest, którą chcesz wdrożyć. Jeśli chcesz użyć pliku .dbschema zamiast tego należy użyć **/modelu** (lub **/ModelFile**) przełącznika.
- **/Cs** (lub **/ConnectionString**) przełącznik udostępnia parametry połączenia dla bazy danych serwera docelowego. Należy pamiętać, że to nazwa bazy danych nie zawiera&#x2014;VSDBCMD musi połączyć się z serwerem, można utworzyć bazy danych. nie trzeba nawiązać połączenia z bazą danych poszczególnych. Jeśli plik .deploymanifest zawiera parametry połączenia, można pominąć ten przełącznik. Jeśli mimo to należy użyć przełącznika, wartość przełącznika spowoduje zastąpienie wartości .deploymanifest.
- <strong>/P:TargetDatabase</strong> właściwość zawiera nazwę chcesz przypisać do docelowej bazy danych podczas ich tworzenia. Zastępuje to wartość <strong>TargetDatabase</strong> właściwość w pliku .deploymanifest. Można użyć <strong>/p:</strong> <em>[nazwa właściwości]</em>składni do konfigurowania różnych typów właściwości wdrożenia oraz zastąpić wszystkie zmienne SQLCMD zadeklarowany w pliku .sqlcmdvars.
- **/Dd+** (lub **/DeployToDatabase+**) przełącznika wskazuje, że chcesz utworzyć wdrożenie, a następnie wdrożyć ją w środowisku docelowym. Jeśli określisz **/dd-**, lub pominięta, VSDBCMD wygeneruje skryptu wdrażania, ale nie zostaną wdrożone w środowisku docelowym. Ten przełącznik jest często źródło pomyłek i jest co omówiono bardziej szczegółowo w następnej sekcji.
- **/Script** (lub **/DeploymentScriptFile**) przełącznik określa, gdzie chcesz wygenerować skrypt wdrożenia. Ta wartość nie wpływa na proces wdrażania.

Aby uzyskać więcej informacji o VSDBCMD, zobacz [dotyczące wiersza polecenia dla VSDBCMD. EXE (wdrożenia i importowania schematu)](https://msdn.microsoft.com/library/dd193283.aspx) i [porady: Przygotowanie bazy danych do wdrożenia z wiersza polecenia przy użyciu VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Na przykład stosowania VSDBCMD w pliku projektu MSBuild zobacz [opis procesu kompilacji](understanding-the-build-process.md). Przykłady sposobu konfigurowania ustawień wdrażania bazy danych w wielu środowiskach, zobacz [Dostosowywanie wdrożenia bazy danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Opis przełącznika DeployToDatabase

Zachowanie **/dd** lub **/DeployToDatabase** przełącznika zależy od tego, czy używasz VSDBCMD z pliku .dbschema lub .deploymanifest. Jeśli używasz pliku .dbschema zachowanie jest bardzo prosta:

- Jeśli określisz **/dd+** lub **/dd**, VSDBCMD wygenerowania skryptu wdrażania i wdrażania bazy danych.
- Jeśli określisz **/dd-** lub pominięta, VSDBCMD wygeneruje tylko skryptu wdrażania.

Jeśli używasz pliku .deploymanifest zachowanie jest znacznie bardziej skomplikowane. Wynika to z faktu plik .deploymanifest zawiera nazwę właściwości **DeployToDatabase** również określa, czy baza danych jest wdrożona.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Ta właściwość ma wartość zgodnie z właściwości projektu bazy danych. Jeśli ustawisz **wdrażanie akcji** do **utworzyć skrypt wdrożenia (SQL)**, wartość będzie **False**. Jeśli ustawisz **wdrażanie akcji** do **Tworzenie skryptu wdrażania (SQL) i wdrażanie do bazy danych**, wartość będzie **True**.

> [!NOTE]
> Te ustawienia są skojarzone z określonej kompilacji configuration i platform. Na przykład skonfigurować ustawienia dla **debugowania** konfiguracji, a następnie opublikować za pomocą **wersji** konfiguracji, ustawienia nie będą używane.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> W tym scenariuszu **wdrażanie akcji** powinny być zawsze ustawiony na **utworzyć skrypt wdrożenia (SQL)**, ponieważ nie ma programu Visual Studio 2010, aby wdrożyć bazę danych. Innymi słowy **DeployToDatabase** właściwość powinna zawsze być **False**.


Gdy **DeployToDatabase** określono właściwości **/dd** przełącznika tylko zastępować właściwość, jeśli wartość właściwości jest **false**:

- Jeśli **DeployToDatabase** właściwość jest **False**, i określeniu **/dd+** lub **/dd**, zastąpią VSDBCMD  **DeployToDatabase** właściwości i wdrażania bazy danych.
- Jeśli **DeployToDatabase** właściwość jest **False**, i określeniu **/dd-** lub pominięta, VSDBCMD nie wdroży bazy danych.
- Jeśli **DeployToDatabase** właściwość jest **True**, VSDBCMD zignoruje przełącznika i wdrażania bazy danych.
- Skrypt wdrożenia jest generowany w każdym przypadku, niezależnie od tego, czy wdrażana również bazy danych.

## <a name="conclusion"></a>Wniosek

W tym temacie podano Omówienie procesu tworzenia i wdrażania dla projektów bazy danych w Visual Studio 2010. Opisano również, jak używasz VSDBCMD.exe przy użyciu programu MSBuild do obsługi wdrożenia bazy danych w skali przedsiębiorstwa.

Aby uzyskać więcej informacji o jego działaniu w praktyce, zobacz [Dostosowywanie wdrożenia bazy danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać informacje na temat sposobu dostosowywania wdrożenia bazy danych, tworząc plik konfiguracji wdrożenia osobne dla każdego środowiska, zobacz [Dostosowywanie wdrożenia bazy danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Aby uzyskać wskazówki dotyczące sposobu konfigurowania członkostwo roli bazy danych, uruchamiając skrypt po wdrożeniu, zobacz [wdrażanie członkostwo roli bazy danych do środowisk testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Aby uzyskać wskazówki dotyczące zarządzania niektóre wyjątkowe wyzwanie członkostwo baz danych nałożyć, zobacz [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Te tematy w witrynie MSDN zawierają szerszych wskazówki i informacje dla projektów bazy danych programu Visual Studio i procesu wdrażania bazy danych:

- [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/library/ff678491.aspx)
- [Zarządzanie zmianą bazy danych](https://msdn.microsoft.com/library/aa833404.aspx)
- [Porady: Przygotowanie bazy danych do wdrożenia z wiersza polecenia przy użyciu VSDBCMD. WYWOŁANIE PLIKU EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Omówienie bazy danych kompilacji i wdrożenia](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-web-packages.md)
> [dalej](creating-and-running-a-deployment-command-file.md)
