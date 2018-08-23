---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Rozwiązywanie problemów z procesem tworzenia pakietów | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak zbierać szczegółowe informacje na temat procesu tworzenia pakietów za pomocą właściwości EnablePackageProcessLoggingAndAssert w M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22be1ccc5a1ec52d143bedffd79264918c1a45e1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754467"
---
<a name="troubleshooting-the-packaging-process"></a>Rozwiązywanie problemów z procesem tworzenia pakietów
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak zbierać szczegółowe informacje na temat procesu tworzenia pakietów, za pomocą **EnablePackageProcessLoggingAndAssert** właściwość aparatu Microsoft Build Engine (MSBuild).
> 
> Po ustawieniu **EnablePackageProcessLoggingAndAssert** właściwości **true**, program MSBuild będzie:
> 
> - Dodaj dodatkowe informacje na temat procesu tworzenia pakietów w dziennikach kompilacji.
> - Rejestruje błędy pod pewnymi warunkami, na przykład, jeśli zduplikowane pliki znajdują się na liście pakietu.
> - Utwórz katalog dzienników w *ProjectName*\_pakietu folder i użyć go do rejestrowania informacji o plikach one pakowania.
> 
> Jeśli proces pakowania, kończy się niepowodzeniem lub pakietów wdrażania sieci web nie zawierają pliki, których można oczekiwać, można użyć tych informacji rozwiązywać ten proces i pinpoint gdzie przebieg procesu problem.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** właściwości tylko wtedy, gdy tworzysz projekt przy użyciu **debugowania** konfiguracji. Właściwość jest ignorowana w innych konfiguracjach.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Opis właściwości EnablePackageProcessLoggingAndAssert

[Budowanie i projektów aplikacji sieci Web pakietu](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) opisano, jak Web potok publikowania (WPP) zawiera zbiór elementów docelowych MSBuild, które rozszerzają funkcjonalność programu MSBuild i włącz ją zintegrować z sieci Web usług Internet Information Services (IIS) Narzędzia Deployment (Web Deploy). Podczas pakowania projektu aplikacji sieci web wywoływania WPP elementów docelowych.

Wiele z tych celów WPP obejmują logikę warunkową, która rejestruje dodatkowe informacje podczas **EnablePackageProcessLoggingAndAssert** właściwość jest ustawiona na **true**. Na przykład, jeśli przejrzysz **pakietu** obiektu docelowego, widać tworzy katalog dziennika dodatkowe i zapisuje listę plików do pliku tekstowego, jeśli **EnablePackageProcessLoggingAndAssert** jest równa **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Obiekty docelowe WPP są zdefiniowane w *Microsoft.Web.Publishing.targets* pliku w folderze % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Możesz otworzyć ten plik i przejrzyj obiekty docelowe w programie Visual Studio 2010 lub dowolnym edytorze XML. Należy zadbać, nie należy modyfikować zawartości pliku.


## <a name="enabling-the-additional-logging"></a>Włączanie dodatkowe rejestrowanie

Należy podać wartość **EnablePackageProcessLoggingAndAssert** właściwości na różne sposoby, w zależności od tego, jak skompilować projekt.

Jeśli tworzysz projekt w wierszu polecenia, należy podać wartość **EnablePackageProcessLoggingAndAssert** właściwość jako argument wiersza polecenia:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Jeśli używasz pliku niestandardowego projektu do tworzenia projektów, można dołączyć **EnablePackageProcessLoggingAndAssert** wartość w **właściwości** atrybutu **MSBuild**zadań:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Jeśli używasz definicji kompilacji Team Foundation Server (TFS) do tworzenia projektów, należy podać wartość **EnablePackageProcessLoggingAndAssert** właściwość **argumenty MSBuild** wiersz:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia i konfigurowania definicji kompilacji, zobacz [tworzenie kompilacji definicji, obsługuje wdrożenia](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Alternatywnie, jeśli chcesz uwzględnić pakiet z każdą kompilacją, można zmodyfikować plik projektu dla projektu aplikacji sieci web ustawić **EnablePackageProcessLoggingAndAssert** właściwości **true**. Należy dodać właściwości do pierwszego **PropertyGroup** element w obrębie pliku .csproj lub .vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Przeglądanie plików dziennika

Podczas kompilowania i projektu aplikacji sieci web za pomocą pakietu **EnablePackageProcessLoggingAndAssert** równa **true**, MSBuild tworzy dodatkowy folder o nazwie logowania *ProjectName* \_Folderu pakietu. Folder dziennika zawiera różne pliki:

![](troubleshooting-the-packaging-process/_static/image2.png)

Listę plików, które są wyświetlane będą się różnić zależnie od elementów w projekcie i proces kompilacji. Jednak te pliki są zwykle używane do rejestrowania listę plików, które zbiera potok WPP pakowania na różnych etapach procesu:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* plik zawiera listę plików, które zbiera MSBuild do pakowania, zanim zostaną usunięte wszystkie pliki, które są określone do wykluczenia.
- *AfterExcludeFilesFilesList.txt* plik zawiera listę zmodyfikowanych plików, po usunięciu wszystkie pliki, które są określone do wykluczenia.

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat wykluczanie plików i folderów z procesem tworzenia pakietów, zobacz [wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* plik listy plików zebranych pakowania po każdym *Web.config* przekształceń, które mogły zostać wykonane. Na tej liście, a wszelkie charakterystyczne dla konfiguracji *Web.config* przekształcanie plików, takie jak *Web.Debug.config* i *Web.Release.config*, są wykluczane z listy plików pakowanie. Pojedynczy przekształcane *Web.config* znajduje się w ich miejscu.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* plik zawiera listę plików po parametry połączenia w *Web.config* ustawiać parametry pliku. Jest to proces, który pozwala zastąpić parametry połączenia za pomocą odpowiednich ustawień w środowisku docelowym podczas wdrażania pakietu.
- *Prepackage.txt* plik zawiera ukończone prekompilacyjne listę plików do uwzględnienia w pakiecie.

> [!NOTE]
> Nazwy dodatkowe pliki dziennika zwykle odpowiadają WPP elementów docelowych. Możesz przejrzeć te obiekty docelowe, sprawdzając *Microsoft.Web.Publishing.targets* pliku w folderze % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Jeśli zawartość pakietu sieci web nie odpowiadają oczekiwaniom, przeglądania tych plików może być przydatny sposób identyfikacji w momencie w kwestii przetwarzania poszło źle.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano, jak można użyć **EnablePackageProcessLoggingAndAssert** właściwości w programie MSBuild rozwiązywania problemów z procesem tworzenia pakietów. Go opisano różne sposoby, w którym można podać wartości właściwości, aby proces kompilacji i jej opisano dodatkowe informacje, które są rejestrowane, gdy właściwość jest ustawiona **true**.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat korzystania z niestandardowych plików projektu MSBuild do kontrolowania procesu wdrażania, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na temat potok WPP i jak zarządza procesem tworzenia pakietów, zobacz [budowanie i projektów aplikacji sieci Web pakietu](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Aby uzyskać wskazówki, jak wykluczyć określone pliki i foldery z pakiety wdrażania sieci web, zobacz [wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](running-windows-powershell-scripts-from-msbuild-project-files.md)
