---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Wykonywanie analizy warunkowej, jeśli wdrożenie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób przeprowadzania "co jeśli" (lub symulowanych) wdrożenia przy użyciu narzędzia do wdrażania sieci Web usług Internet Information Services (IIS) (Web Deploy) i V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f50053c3b231a1d174df51ce7aa4c6013192bca8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378102"
---
<a name="performing-a-what-if-deployment"></a>Wykonywanie wdrożenia "What If"
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób przeprowadzania "what if" (lub symulowanych) przy użyciu narzędzia do wdrażania sieci Web usług Internet Information Services (IIS) (Web Deploy) i VSDBCMD wdrożeń. Dzięki temu można ustalenie efektów logiki wdrożenia w środowisku określonego celu, aby rzeczywiście wdrożyć aplikację.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w której procesu kompilacji i wdrażania jest kontrolowany przez dwa pliki projektu&#x2014;jeden zawierającej instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Wykonywanie wdrożenia "What If" dla pakietów internetowych

Narzędzie Web Deploy zawiera funkcje, które umożliwia wykonywanie wdrożeń w "what if" (lub wersja próbna) trybu. Podczas wdrażania artefaktów w trybie "what if", narzędzie Web Deploy, generuje plik dziennika, tak, jakby były wykonywane wdrożenia, ale faktycznie nie zmienia żadnych na serwerze docelowym. Przeglądanie plików dziennika, może pomóc Ci zrozumieć, jaki wpływ wdrożenia będzie mieć na serwerze docelowym, w szczególności:

- Co będzie poproś o dodanie Cię.
- Jakie zostaną zaktualizowani.
- Co zostanie usunięty.

Ponieważ wdrożenia "what if" faktycznie nie zmienia żadnych na serwerze docelowym, co zawsze nie jest przewidzieć wdrożenia, zostanie wykonane pomyślnie.

Zgodnie z opisem w [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md), można wdrożyć pakietów sieci web za pomocą narzędzia Web Deploy na dwa sposoby&#x2014;za pomocą narzędzia wiersza polecenia MSDeploy.exe, bezpośrednio lub przez uruchomienie *. pliku deploy.cmd* pliku generuje procesu kompilacji.

Jeśli używasz programu MSDeploy.exe bezpośrednio, możesz uruchamiać wdrożenia "what if", dodając **whatif** flagi do swojej dyspozycji. Na przykład aby ocenić, co się stanie, jeśli wdrożono pakiet ContactManager.Mvc.zip w środowisku przejściowym, polecenie MSDeploy powinien wyglądać następująco:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Gdy jesteś zadowolony z wyników wdrożenia "what if", możesz usunąć **whatif** flagi do uruchamiania na żywo wdrożenia.

> [!NOTE]
> Aby uzyskać więcej informacji o opcjach wiersza polecenia programu MSDeploy.exe, zobacz [ustawienia operacji wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Jeśli używasz *. pliku deploy.cmd* pliku, możesz uruchomić wdrożenia "what if", umieszczając **/t** flaga Flaga (tryb wersji próbnej) zamiast **/y** Flaga ("yes" lub tryb aktualizacji) polecenie. Na przykład, można obliczyć, co się stanie, jeśli wdrożono pakiet ContactManager.Mvc.zip, uruchamiając *. pliku deploy.cmd* pliku polecenia powinny wyglądać następująco:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Gdy jesteś zadowolony z wyników wdrożenia "tryb wersji próbnej", możesz zastąpić **/t** flaga z **/y** flagi do uruchamiania na żywo wdrożenia:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia dla *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx). Jeśli uruchamiasz *. pliku deploy.cmd* plików bez określania flagi, wiersza polecenia wyświetli listę dostępnych flag.


## <a name="performing-a-what-if-deployment-for-databases"></a>Wykonywanie wdrożenia "What If" dla bazy danych

W tej sekcji założono, że używasz narzędzia VSDBCMD przeprowadzić wdrożenie przyrostowe, oparte na schemacie bazy danych. To podejście jest opisane bardziej szczegółowo w [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Zaleca się, że możesz zapoznać się z tego tematu przed zastosowaniem pojęcia opisane w tym miejscu.

Kiedy używasz VSDBCMD w **Wdróż** trybu, można użyć **/dd** (lub **/DeployToDatabase**) flaga kontroli VSDBCMD powoduje faktyczne wdrożenie bazy danych lub po prostu generuje skrypt wdrożenia. Jeśli wdrażasz pliku .dbschema jest to zachowanie:

- Jeśli określisz **/dd+** lub **/dd**, VSDBCMD wygeneruje skryptu wdrażania i wdrażanie bazy danych.
- Jeśli określisz **/dd-** lub pominięta, VSDBCMD spowoduje wygenerowanie skryptu wdrażania.

> [!NOTE]
> Jeśli wdrażasz .deploymanifest pliku, a nie plikiem .dbschema zachowanie **/dd** przełącznik jest o wiele bardziej skomplikowane. Zasadniczo VSDBCMD zignoruje wartość **/dd** przełącznika, jeśli plik .deploymanifest zawiera **DeployToDatabase** element z wartością **True**. [Wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md) opisano to zachowanie w całości.


Na przykład, można wygenerować skryptu wdrażania dla **ContactManager** bazy danych bez faktycznego wdrażania bazy danych, a polecenie VSDBCMD powinna wyglądać następująco:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD to narzędzie wdrażania bazy danych różnicowych i jako takie przez skrypt wdrażania jest generowana dynamicznie ma zawierać wszystkie niezbędne do aktualizacji bieżącej bazy danych, jeśli taki istnieje określony schemat polecenia SQL. Przeglądanie przez skrypt wdrażania to wygodny sposób, aby ustalić, jaki wpływ wdrożenia będą mogli korzystać w bieżącej bazie danych i danych, które zawiera. Na przykład możesz chcieć określić:

- Zostaną usunięte wszystkie istniejące tabele i czy, spowoduje utratę danych.
- Czy kolejność operacji niesie ze sobą ryzyko utraty danych, na przykład, jeśli podział lub scalania tabel.

Jeśli jesteś zadowolony z skryptu wdrażania, możesz powtórzyć VSDBCMD z **/dd+** flagę, aby wprowadzić zmiany. Alternatywnie możesz edytować skrypt wdrożeniowy do potrzeb, a następnie uruchomić go ręcznie na serwerze bazy danych.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrowanie funkcji "What If" pliki projektów niestandardowych

W bardziej złożonych scenariuszy wdrażania, możesz będziesz chciał użyć niestandardowy plik aparatu Microsoft Build Engine (MSBuild) projektu do hermetyzacji logikę kompilacji i wdrażania, zgodnie z opisem w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Na przykład w [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie *Publish.proj* pliku:

- Kompiluje rozwiązanie.
- Pakowanie i wdrażanie aplikacji ContactManager.Mvc, korzysta się z narzędzia Web Deploy.
- Pakowanie i wdrażanie aplikacji ContactManager.Service, korzysta się z narzędzia Web Deploy.
- Wdraża **ContactManager** bazy danych.

Po zintegrowaniu wdrożenia wielu pakietów sieci web i/lub baz danych w procesie pojedynczy krok w ten sposób może także opcję wykonania całego wdrożenia w trybie "what if".

*Publish.proj* plik pokazuje, jak to zrobić. Najpierw należy utworzyć właściwość do przechowywania wartości "what if":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


W tym przypadku został utworzony właściwość o nazwie **WhatIf** z wartością domyślną **false**. Użytkownicy mogą przesłaniać tę wartość przez ustawienie właściwości **true** w parametr wiersza polecenia, jak będzie wyświetlana wkrótce.

Następny etap to próby parametryzacji dowolnego narzędzia Web Deploy i VSDBCMD polecenia, aby odzwierciedlić flagi **WhatIf** wartości właściwości. Na przykład następny element docelowy (pobierane z *Publish.proj* pliku i uproszczonym) uruchamia *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web. Domyślnie, polecenie zawiera **/Y** switch ("yes" lub tryb aktualizacji). Jeśli **WhatIf** ustawiono **true**, zostanie zastąpiony przez **/t** przełącznika (wersji próbnej lub tryb "what if").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Podobnie następny element docelowy korzysta z narzędzia VSDBCMD wdrażanie bazy danych. Domyślnie **/dd** przełącznik nie jest dołączony. Oznacza to, że VSDBCMD spowoduje to wygenerowanie skryptu wdrażania, ale nie spowoduje wdrożenia bazy danych&#x2014;innymi słowy, "what if" scenariusza. Jeśli **WhatIf** nie ustawiono właściwości **true**, **/dd** przełącznik jest dodawany i VSDBCMD wdroży bazy danych.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Można użyć tej samej metody próby parametryzacji wszystkich odpowiednich poleceń w pliku projektu. Jeśli chcesz uruchamiać wdrożenie "what if" można następnie wystarczy podać **WhatIf** wartości właściwości w wierszu polecenia:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


W ten sposób można uruchomić wdrożenia "what if" dla wszystkich składników usługi projektu w jednym kroku.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób uruchamiania "what if" wdrożeń za pomocą narzędzia Web Deploy, VSDBCMD i MSBuild. Wdrożenie "what if" pozwala ocenić wpływ proponowanej wdrożenia przed faktycznie wprowadź zmiany w środowisku docelowym.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat składni wiersza polecenia narzędzia Web Deploy, zobacz [ustawienia operacji wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Aby uzyskać wskazówki dotyczące opcji wiersza polecenia, korzystając z *. pliku deploy.cmd* plików, zobacz [jak: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx). Aby uzyskać wskazówki dotyczące VSDBCMD składni wiersza polecenia, zobacz [VSDBCMD dokumentacja wiersza polecenia. Plik EXE (wdrożenia i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Poprzednie](advanced-enterprise-web-deployment.md)
> [dalej](customizing-database-deployments-for-multiple-environments.md)
