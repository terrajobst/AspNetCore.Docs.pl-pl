---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Wdrażanie projektów baz danych | Dokumentacja firmy Microsoft
author: jrjlee
description: 'Uwaga: W wiele scenariuszy wdrażania w przedsiębiorstwie, niezbędna jest możliwość publikowania aktualizacji przyrostowych wdrożonej bazy danych. Alternatywą jest ponownie utworzyć...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 43fa197a1d5a3cf521f4d2202754ff0d121cebe3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756037"
---
<a name="deploying-database-projects"></a>Wdrażanie projektów baz danych
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> W wielu scenariuszy wdrażania w przedsiębiorstwie niezbędna jest możliwość publikowania aktualizacji przyrostowych wdrożonej bazy danych. Alternatywą jest ponownie utworzyć bazę danych przy każdym wdrożeniu, co oznacza, że zostaną utracone dane w istniejącej bazie danych. Podczas pracy w programie Visual Studio 2010 przy użyciu VSDBCMD jest zalecane podejście do przyrostowe publikowanie bazy danych. Jednak w następnej wersji programu Visual Studio i Web potok publikowania (WPP) będzie zawierać narzędzia, które obsługuje przyrostowe publikowanie bezpośrednio.


Jeśli otworzysz przykładowe rozwiązanie Contact Manager w programie Visual Studio 2010, zobaczysz, że projekt bazy danych zawiera folder właściwości, który zawiera cztery pliki.

![](deploying-database-projects/_static/image1.png)

Wraz z pliku projektu (*ContactManager.Database.dbproj* w tym przypadku), te pliki kontrolować różne aspekty procesu kompilacji i wdrażania:

- *Database.sqlcmdvars* plik zawiera wartości wszelkich zmiennych SQLCMD, możesz użyć podczas wdrażania projektu. Każda konfiguracja rozwiązania (na przykład debug i release) można określić plik .sqlcmdvars różne.
- *Database.sqldeployment* plik zawiera ustawienia specyficzne dla danego wdrożenia, takie jak czy do korzystania z sortowania zdefiniowane w projekcie ani typu sortowania serwera docelowego, czy należy ponownie utworzyć docelowej bazy danych co czasu lub po prostu zmienić istniejącą bazę danych, aby przełączyć go do daty i tak dalej. Każda konfiguracja rozwiązania można określić plik .sqldeployment różne.
- *Database.sqlpermissions* plik jest dokument XML, który służy do definiowania wszelkich uprawnień, które chcesz dodać do docelowej bazy danych. Wszystkie konfiguracje rozwiązania współużytkować ten sam plik .sqlpermissions.
- *Database.sqlsettings* plik Określa właściwości poziomu bazy danych do użycia podczas tworzenia bazy danych, takich jak sortowaniem, aby użyć zachowanie operatorów porównania i tak dalej. Wszystkie konfiguracje rozwiązania współużytkować ten sam plik .sqlsettings.

Warto poświęcić trochę czasu na pliki te można otworzyć w programie Visual Studio i zapoznać się z zawartością.

Gdy tworzysz projekt bazy danych, proces kompilacji tworzy dwa pliki:

- A *schemat bazy danych* (plik .dbschema). Opisuje schemat bazy danych, który chcesz utworzyć w formacie XML.
- A *manifest wdrożenia* (plik .deploymanifest). Zawiera wszystkie informacje wymagane do tworzenia i wdrażania bazy danych. Odwołuje się plik .dbschema wraz z innych zasobów, takich jak instrukcje dotyczące wdrażania (plik .sqldeployment) i wszystkie skrypty SQL przed wdrożeniem lub po wdrożeniu.

To pokazuje relację między tymi zasobami:

![](deploying-database-projects/_static/image2.png)

Jak widać, plik .sqlsettings i plik .sqlpermissions są dane wejściowe do procesu kompilacji. Wraz z plikiem projektu bazy danych pliki te są używane do tworzenia pliku schematu bazy danych. Plik .sqldeployment i plik .sqlcmdvars przechodzić przez proces kompilacji, bez zmian. Manifest wdrażania wskazuje lokalizację schemat bazy danych, plików .sqldeployment, plik .sqlcmdvars i wszystkie skrypty SQL przed wdrożeniem lub po wdrożeniu.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Dlaczego warto używać VSDBCMD, aby wdrożyć projekt bazy danych?

Istnieją różne różne metody wdrażania projektów bazy danych. Jednak nie wszystkie z nich są odpowiednie do wdrażania projektu bazy danych na serwerach zdalnych w środowisku przedsiębiorstwa. Co należy rozważyć z wdrożenia projektu bazy danych. W scenariuszach wdrażania w przedsiębiorstwie prawdopodobnie chcesz:

- Możliwość wdrażania projektu bazy danych z lokalizacji zdalnej.
- Możliwość zapewnienia aktualizacje przyrostowe istniejącą bazę danych.
- Zdolność do uwzględnienia przed wdrożeniem lub po wdrożeniu skrypty.
- Możliwość Dostosowywanie wdrożenia w wielu środowiskach docelowego.
- Możliwość wdrażania projektu bazy danych jako część większego, zwykle uwzględnione w skryptach, wdrażania rozwiązania w jednym kroku.

Istnieją trzy główne metody można użyć do wdrożenia projektu bazy danych:

- Za pomocą funkcji wdrażania z tym typem projektu bazy danych w programie Visual Studio 2010. Podczas tworzenia i wdrażania projektu bazy danych w programie Visual Studio 2010, proces wdrażania używa pliku manifestu wdrożenia do generowania pliku wdrożenie oparte na SQL specyficzne dla konfiguracji kompilacji. Spowoduje to utworzenie bazy danych, jeśli już nie istnieje lub wprowadź niezbędne zmiany w bazie danych, jeśli już istnieje. Można użyć SQLCMD.exe, aby uruchomić ten plik na serwerze docelowym lub można ustawić Visual Studio, aby utworzyć i uruchomić plik. Wadą tego podejścia jest mają kontrolę nad ustawienia wdrażania tylko ograniczone. Często również może być konieczne zmodyfikowanie pliku wdrożenia SQL w celu udostępnienia wartości zmiennych specyficznymi dla środowiska. To podejście z komputera można używać tylko przy użyciu programu Visual Studio 2010, a deweloper musi znasz i Podaj parametry połączenia i poświadczenia dla wszystkich środowisk docelowego.
- Narzędzie wdrażania sieci Web usług Internet Information Services (IIS), (Web Deploy) służy do [wdrożyć bazę danych w ramach projektu aplikacji sieci web](https://msdn.microsoft.com/library/dd465343.aspx). Jednak to podejście jest o wiele bardziej skomplikowane, aby wdrożyć projekt bazy danych, a nie po prostu zreplikować istniejącej lokalnej bazy danych na serwerze docelowym. Można skonfigurować narzędzie Web Deploy do uruchomienia skryptu wdrożenia SQL, który generuje projekt bazy danych, ale aby to zrobić, należy utworzyć niestandardowy plik elementów docelowych WPP dla projektu aplikacji sieci web. Spowoduje to dodanie znacznej ilości złożoności procesu wdrażania. Ponadto narzędzie Web Deploy nie obsługuje bezpośrednio aktualizacje przyrostowe istniejących baz danych. Aby uzyskać więcej informacji na temat tego podejścia, zobacz [rozszerzanie potoku publikowania w sieci Web, do projektu bazy danych pakietu wdrożonego pliku SQL](https://go.microsoft.com/?linkid=9805121).
- Narzędzie VSDBCMD wdrożenie bazy danych, przy użyciu schematu bazy danych lub pliku manifestu wdrożenia. Możesz wywołać VSDBCMD.exe z obiektu docelowego programu MSBuild, które pozwala publikować bazy danych jako część procesu większych i inicjowanych przez skrypty wdrażania. Można zastąpić zmienne w pliku .sqlcmdvars i wiele innych właściwości bazy danych z poleceń VSDBCMD, co pozwala na dostosowywanie wdrożenia w różnych środowiskach bez konieczności tworzenia wielu konfiguracjach kompilacji. VSDBCMD oferuje funkcję różnice między, co oznacza, że jej spowoduje, że tylko niezbędne zmiany, aby wyrównać docelowej bazy danych przy użyciu schematu bazy danych. VSDBCMD oferuje szeroką gamę opcji wiersza polecenia, które zapewniają szczegółową kontrolę nad procesem wdrażania.

W tym omówieniu znajdują się za pomocą VSDBCMD za pomocą narzędzia MSBuild to podejście, najlepiej dopasowane do typowego przedsiębiorstwa scenariusza wdrażania:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Obsługuje zdalnego wdrażania? | Tak | Tak | Tak |
| Obsługiwane są aktualizacje przyrostowe? | Tak | Nie | Tak |
| Obsługuje skrypty przed/po-deployment? | Tak | Tak | Tak |
| Obsługuje wdrażanie wielośrodowiskowego? | Ograniczone | Ograniczone | Tak |
| Obsługuje inicjowanych przez skrypty wdrażania? | Ograniczone | Tak | Tak |

W pozostałej części tego tematu opisano użycie VSDBCMD za pomocą narzędzia MSBuild wdrażania projektów bazy danych.

## <a name="understanding-the-deployment-process"></a>Opis procesu wdrażania

Narzędzie VSDBCMD pozwala wdrożyć bazę danych przy użyciu schematu bazy danych (plik .dbschema) lub manifestu wdrażania (plik .deploymanifest). W praktyce prawie zawsze użyjesz manifest wdrożenia, ponieważ manifest wdrażania pozwala podać wartości domyślne dla różnych właściwości wdrożenia i zidentyfikować wszelkie przed wdrożeniem lub po wdrożeniu skrypty SQL, który chcesz uruchomić. Na przykład, to polecenie VSDBCMD służy do wdrażania **ContactManager** bazy danych do serwera bazy danych w środowisku testowym:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


W takim przypadku:

- **/A** (lub **Action**) przełącznik określa, co ma VSDBCMD celu. Ustaw tę opcję na **importu** lub **Wdróż**. **Importu** opcja jest używana do generowania pliku .dbschema z istniejącej bazy danych i **Wdróż** opcja jest używana do wdrożenia pliku .dbschema docelową bazę danych.
- **/Manifest** (lub **/ManifestFile**) przełącznik określa plik .deploymanifest, którą chcesz wdrożyć. Jeśli chcesz użyć pliku .dbschema, należy użyć **/modelu** (lub **/ModelFile**) przełącznika.
- **Skróconej/CS** (lub **/ConnectionString**) przełącznik udostępnia parametry połączenia dla bazy danych serwera docelowego. Należy pamiętać, że nie obejmuje to nazwa bazy danych&#x2014;VSDBCMD musi połączyć się z serwerem, aby utworzyć bazę danych; nie musi nawiązać połączenie z poszczególnych baz danych. Jeśli plik .deploymanifest zawiera parametry połączenia, można pominąć ten przełącznik. Jeśli mimo to użyć przełącznika przełącznik wartość zastępuje wartość .deploymanifest.
- <strong>/P:TargetDatabase</strong> właściwość zawiera nazwę chcesz przypisać do docelowej bazy danych podczas ich tworzenia. Zastępuje wartości <strong>TargetDatabase</strong> właściwości w pliku .deploymanifest. Możesz użyć <strong>/p:</strong> <em>[nazwa właściwości]</em>składni, aby ustawić szerokiej gamy właściwości wdrożenia i aby zastąpić wszelkie zmienne SQLCMD zadeklarowana w pliku .sqlcmdvars.
- **/Dd+** (lub **/DeployToDatabase+**) przełącznik wskazuje, że chcesz utworzyć wdrożenie, i wdrożyć ją do środowiska docelowego. Jeśli określisz **/dd-**, lub pominięta, VSDBCMD spowoduje to wygenerowanie skryptu wdrażania, ale nie spowoduje wdrożenia do środowiska docelowego. Ten przełącznik jest często źródła pomyłek i zostało wyjaśnione bardziej szczegółowo w następnej sekcji.
- **/Script** (lub **/DeploymentScriptFile**) przełącznik określa, gdzie można wygenerować skryptu wdrażania. Ta wartość nie wpływa na proces wdrażania.

Aby uzyskać więcej informacji na temat VSDBCMD, zobacz [VSDBCMD dokumentacja wiersza polecenia. Plik EXE (wdrożenia i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx) i [porady: przygotowywanie bazy danych do wdrożenia z poziomu wiersza polecenia przy użyciu VSDBCMD. Plik EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Przykład wykorzystania VSDBCMD z pliku projektu programu MSBuild, zobacz [objaśnienie procesu kompilacji](understanding-the-build-process.md). Aby zapoznać się z przykładami sposobu konfigurowania ustawień wdrażania bazy danych w wielu środowiskach, zobacz [Dostosowywanie wdrożeń bazy danych dla wielu środowisk](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Informacje o przełącznikiem DeployToDatabase

Zachowanie **/dd** lub **/DeployToDatabase** przełącznika zależy od tego, czy używasz VSDBCMD .dbschema .deploymanifest pliku lub. Jeśli używasz pliku .dbschema, zachowanie jest dość prosta:

- Jeśli określisz **/dd+** lub **/dd**, VSDBCMD wygeneruje skryptu wdrażania i wdrażanie bazy danych.
- Jeśli określisz **/dd-** lub pominięta, VSDBCMD spowoduje wygenerowanie skryptu wdrażania.

Jeśli używasz pliku .deploymanifest zachowanie jest o wiele bardziej skomplikowane. Jest to spowodowane plik .deploymanifest zawiera nazwę właściwości **DeployToDatabase** również określający, czy baza danych jest wdrożona.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Ta właściwość ma wartość zgodnie z właściwości projektu bazy danych. Jeśli ustawisz **wdrażanie akcji** do **utworzyć skrypt wdrażania (.sql)**, wartość będzie **False**. Jeśli ustawisz **wdrażanie akcji** do **skryptu wdrażania (.sql) do tworzenia i wdrażania bazy danych**, wartość będzie **True**.

> [!NOTE]
> Te ustawienia są skojarzone z konfiguracją konkretnej kompilacji i platformy. Na przykład, jeśli konfigurujesz ustawienia **debugowania** konfiguracji, a następnie opublikować przy użyciu **wersji** konfiguracji, ustawienia nie będą używane.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> W tym scenariuszu **wdrażanie akcji** powinna zawsze być ustawiona na **utworzyć skrypt wdrażania (.sql)**, ponieważ nie chcesz, aby program Visual Studio 2010 do wdrożenia bazy danych. Innymi słowy **DeployToDatabase** właściwość powinna zawsze być **False**.


Gdy **DeployToDatabase** właściwość zostanie określona, **/dd** przełącznika spowoduje zastąpienie właściwości tylko, jeśli wartość właściwości jest **false**:

- Jeśli **DeployToDatabase** właściwość **False**, i określeniu **/dd+** lub **/dd**, spowoduje zastąpienie VSDBCMD  **DeployToDatabase** właściwości i wdrażanie bazy danych.
- Jeśli **DeployToDatabase** właściwość **False**, i określeniu **/dd-** lub pominięta, VSDBCMD nie wdroży bazy danych.
- Jeśli **DeployToDatabase** właściwość **True**, VSDBCMD zignoruje przełącznika i wdrażanie bazy danych.
- Skrypt wdrażania jest generowany w każdym przypadku, niezależnie od tego, czy wdrażasz także bazy danych.

## <a name="conclusion"></a>Wniosek

W tym temacie podano Omówienie procesu kompilowania i wdrażania dla projektów bazy danych w programie Visual Studio 2010. Opisano również, jak używać VSDBCMD.exe za pomocą narzędzia MSBuild do obsługi wdrożenia bazy danych w skali przedsiębiorstwa.

Aby uzyskać więcej informacji na temat jak to działa w praktyce, zobacz [Dostosowywanie wdrożeń bazy danych dla wielu środowisk](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać informacji na temat sposobu dostosowywania wdrożenia bazy danych, tworząc plik konfiguracji wdrożenia osobne dla każdego środowiska, zobacz [Dostosowywanie wdrożeń bazy danych dla wielu środowisk](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Aby uzyskać wskazówki dotyczące sposobu konfigurowania członkostw roli bazy danych, uruchamiając skrypt po wdrożeniu, zobacz [członkostw roli bazy danych wdrażania w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Aby uzyskać wskazówki dotyczące zarządzania niektóre unikatowe wyzwania członkostwo baz danych nakładają, zobacz [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Te tematy w witrynie MSDN zapewniają szersze wskazówki i informacje dla projektów bazy danych programu Visual Studio i procesu wdrażania bazy danych:

- [Visual Studio 2010 projektów bazodanowych SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Zarządzanie zmianą bazy danych](https://msdn.microsoft.com/library/aa833404.aspx)
- [Porady: przygotowywanie bazy danych do wdrożenia z poziomu wiersza polecenia przy użyciu VSDBCMD. PLIK EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Omówienie bazy danych kompilacji i wdrażania](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-web-packages.md)
> [dalej](creating-and-running-a-deployment-command-file.md)
