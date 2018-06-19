---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Wykonywanie co, jeśli wdrożenie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób wykonywania "co w przypadku" (lub symulowane) wdrożenia przy użyciu narzędzia do wdrażania sieci Web usług Internet Information Services (IIS) (Web Deploy) i V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879988"
---
<a name="performing-a-what-if-deployment"></a>Wykonywanie wdrożenia "Co w przypadku"
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób wykonywania "co w przypadku" (lub symulowane) przy użyciu narzędzia do wdrażania sieci Web usług Internet Information Services (IIS) (Web Deploy) i VSDBCMD wdrożeń. Dzięki temu można określić skutków logiki wdrożenia w środowisku określonego elementu docelowego, przed wdrożeniem faktycznie aplikacji.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji i wdrożenia dwa pliki projektu&#x2014;jeden zawierającego instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Wykonywanie wdrożenia "Co w przypadku" dla pakietów sieci Web

Narzędzie Web Deploy zawiera funkcje, które umożliwia wykonywanie wdrożeń w "co w przypadku" (lub wersji próbnej) trybu. Podczas wdrażania artefaktów w trybie "co w przypadku" Narzędzia Web Deploy generuje plik dziennika, tak jakby były przeprowadzone wdrożenie, ale faktycznie nie zmienia sytuacji na serwerze docelowym. Przeglądanie pliku dziennika może pomóc Ci zrozumieć, jaki wpływ wdrożenia będzie mieć na serwerze docelowym, w szczególności:

- Co spowoduje zostaną dodane.
- Jakie będzie aktualizowany.
- Jakie zostaną usunięte.

Ponieważ wdrożenia "co w przypadku" faktycznie nie zmienia sytuacji na serwerze docelowym, jakie zawsze nie jest prognozowania, czy wdrożenie powiedzie się.

Zgodnie z opisem w [wdrażanie pakietów sieci Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), można wdrożyć pakietów sieci web za pomocą narzędzia Web Deploy na dwa sposoby&#x2014;za pomocą narzędzia wiersza polecenia programu MSDeploy.exe bezpośrednio lub przez uruchomienie *. pliku deploy.cmd* pliku czy generuje procesu kompilacji.

Jeśli używasz programu MSDeploy.exe bezpośrednio, dodając można uruchomić wdrożenia "co w przypadku" **-whatif** flagi do polecenia. Na przykład aby ocenić, co się stanie po wdrożeniu pakietu ContactManager.Mvc.zip w środowisku przemieszczania, polecenie MSDeploy powinien wyglądać następująco:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Po zakończeniu wyniki wdrożenia "co w przypadku" można usunąć **-whatif** flagi uruchamiania wdrożenia na żywo.

> [!NOTE]
> Aby uzyskać więcej informacji o opcjach wiersza polecenia programu MSDeploy.exe, zobacz [ustawienia operację wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Jeśli używasz *. pliku deploy.cmd* plików, można uruchomić wdrożenia "co w przypadku" umieszczając **/t** flaga Flaga (trybu wersji próbnej) zamiast **/y** Flaga ("tak", lub tryb aktualizacji) w polecenie. Na przykład, aby obliczyć, co się stanie po wdrożeniu pakietu ContactManager.Mvc.zip, uruchamiając *. pliku deploy.cmd* pliku polecenia powinien wyglądać następująco:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Po zakończeniu wyników wdrożenia "Tryb próbne", można zastąpić **/t** flaga z **/y** flagi uruchamiania wdrożenia na żywo:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia dla *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrożenia pakietu za pomocą pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx). Po uruchomieniu *. pliku deploy.cmd* pliku bez określenia flagi, wiersz polecenia spowoduje wyświetlenie listy dostępnych flag.


## <a name="performing-a-what-if-deployment-for-databases"></a>Wykonywanie wdrożenia "Co w przypadku" dla bazy danych

W tej sekcji założono, że używasz narzędzia VSDBCMD wdrożenie bazy danych przyrostowej, oparte na schemacie. Takie podejście jest opisany bardziej szczegółowo w [wdrażania projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Zaleca się, że należy zapoznać się z tego tematu przed zastosowaniem pojęcia opisane w tym miejscu.

Jeśli używasz VSDBCMD w **Wdróż** tryb, można użyć **/dd** (lub **/DeployToDatabase**) flaga kontroli VSDBCMD faktycznie wdraża bazy danych lub po prostu generuje skrypt wdrożenia. W przypadku instalowania plików .dbschema, jest to zachowanie:

- Jeśli określisz **/dd+** lub **/dd**, VSDBCMD wygenerowania skryptu wdrażania i wdrażania bazy danych.
- Jeśli określisz **/dd-** lub pominięta, VSDBCMD wygeneruje tylko skryptu wdrażania.

> [!NOTE]
> W przypadku instalowania plików .deploymanifest zamiast pliku .dbschema zachowanie **/dd** przełącznik jest znacznie bardziej skomplikowane. Zasadniczo VSDBCMD zignoruje wartość **/dd** przełącznika, jeśli plik .deploymanifest zawiera **DeployToDatabase** element o wartości **True**. [Wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md) opisano to zachowanie w całości.


Na przykład, aby wygenerować skryptu wdrażania dla **ContactManager** bazy danych bez faktycznie wdrażania bazy danych, a polecenie VSDBCMD powinna wyglądać następująco:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD to narzędzie wdrażania różnicowej bazy danych i jako taki skrypt wdrożenia dynamicznie jest generowany zawiera wszystkie niezbędne do aktualizacji bieżącej bazy danych, jeśli istnieje wpis z określonym schematem polecenia SQL. Przeglądanie skrypt wdrożenia to wygodny sposób, aby określić, jaki wpływ wdrożenia ma w bieżącej bazie danych i danych, które zawiera. Na przykład można określić:

- Zostaną usunięte wszystkie istniejące tabele, i czy który spowoduje utratę danych.
- Czy kolejność operacji niesie ryzyko utraty danych, na przykład, jeśli jest dzielenie i scalanie tabel.

W przypadku modyfikowania skryptu wdrażania, można powtórzyć VSDBCMD z **/dd+** flagę, aby wprowadzić zmiany. Alternatywnie można edytować skrypt wdrożenia w celu zgodnie z wymaganiami, a następnie uruchom go ręcznie na serwerze bazy danych.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrowanie funkcji "Co w przypadku" pliki projektu niestandardowych

W bardziej złożonych scenariuszach wdrażania, należy do użycia niestandardowego pliku projektu Microsoft kompilacji Engine (MSBuild) hermetyzacji logiki kompilowanie i wdrażanie, zgodnie z opisem w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Na przykład w [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie, *Publish.proj* pliku:

- Buduje rozwiązanie.
- Używa narzędzia Web Deploy pakietu i wdrażania aplikacji ContactManager.Mvc.
- Używa narzędzia Web Deploy pakietu i wdrażania aplikacji ContactManager.Service.
- Wdraża **ContactManager** bazy danych.

Po zintegrowaniu wdrożenia wielu pakietów sieci web i/lub baz danych do procesu pojedynczy krok w ten sposób również możesz opcja wykonywania całego wdrożenia w trybie "co w przypadku".

*Publish.proj* pliku pokazano, jak to zrobić. Najpierw należy utworzyć właściwość do przechowywania wartości "co w przypadku":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


W takim przypadku zostanie utworzona właściwość o nazwie **WhatIf** z wartością domyślną **false**. Użytkownicy mogą zastąpić tę wartość przez ustawienie właściwości **true** parametru wiersza polecenia, jak wyświetlone wkrótce.

Kolejnego etapu ma parametryzacja dowolnego narzędzia Web Deploy i VSDBCMD polecenia, aby odzwierciedlić flagi **WhatIf** wartości właściwości. Na przykład dalej docelowego (pobierane z *Publish.proj* plików i uproszczone) uruchamia *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web. Domyślnie polecenie zawiera **/Y** switch ("tak", lub tryb aktualizacji). Jeśli **WhatIf** ustawiono **true**, zostanie zastąpiony przez **/T** przełącznika (wersja próbna lub tryb "co w przypadku").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Podobnie element docelowy dalej używa narzędzia VSDBCMD do wdrażania bazy danych. Domyślnie **/dd** przełącznik nie jest dołączony. Oznacza to, że VSDBCMD wygeneruje skryptu wdrażania, ale nie spowoduje wdrożenia bazy danych&#x2014;innymi słowy, "co w przypadku" scenariusza. Jeśli **WhatIf** nie ustawiono właściwości **true**, **/dd** dodawanej przełącznik i VSDBCMD wdroży bazy danych.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Można użyć tej samej metody, aby parametryzacja wszystkich odpowiednich poleceń w pliku projektu. Jeśli chcesz uruchamiać wdrożenie "co w przypadku" można następnie wystarczy podać **WhatIf** wartości właściwości w wierszu polecenia:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


W ten sposób można uruchomić wdrożenia "co w przypadku" dla wszystkich składników projektu w jednym kroku.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób uruchamiania "co w przypadku" wdrożenia przy użyciu narzędzia Web Deploy, VSDBCMD i MSBuild. Wdrożenie "co w przypadku" umożliwia ocenę wpływu proponowanych wdrożenia przed wprowadzeniem faktycznie żadnych zmian w środowisku docelowym.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji o składni wiersza polecenia narzędzia Web Deploy, zobacz [ustawienia operację wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Wskazówki dotyczące opcji wiersza polecenia, korzystając z *. pliku deploy.cmd* plików, zobacz [porady: Zainstaluj wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx). Aby uzyskać wskazówki dotyczące VSDBCMD składni wiersza polecenia, zobacz [dotyczące wiersza polecenia dla VSDBCMD. EXE (wdrożenia i importowania schematu)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Poprzednie](advanced-enterprise-web-deployment.md)
> [dalej](customizing-database-deployments-for-multiple-environments.md)
