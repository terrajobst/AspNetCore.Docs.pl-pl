---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: "Rozwiązywanie problemów z procesu tworzenia pakietów | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano, jak szczegółowe informacje na temat procesu tworzenia pakietów można zebrać za pomocą właściwości EnablePackageProcessLoggingAndAssert w M..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 977077357eb5774193a40c55fabee9733dd5ab2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-the-packaging-process"></a>Rozwiązywanie problemów z procesu tworzenia pakietów
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak szczegółowe informacje na temat procesu tworzenia pakietów można zebrać za pomocą **EnablePackageProcessLoggingAndAssert** właściwości w aparat kompilacji firmy Microsoft (MSBuild).
> 
> Podczas ustawiania **EnablePackageProcessLoggingAndAssert** właściwości **true**, będzie MSBuild:
> 
> - Dodaj dodatkowe informacje na temat procesu tworzenia pakietów w dziennikach kompilacji.
> - Dziennik błędów w niektórych warunkach, na przykład, jeśli zduplikowane pliki znajdują się na liście pakietów.
> - Utwórz katalog dziennika w *ProjectName*\_pakietu folderze i używać go do rejestrowania informacji o plikach pakowane.
> 
> Proces tworzenia pakietu kończy się niepowodzeniem, czy pakiety wdrażania sieci web nie mogą zawierać pliki, które powinny, można użyć tych informacji rozwiązywać procesu do punktu przyczepienia której elementy będą nieprawidłowe.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** właściwość działa tylko w przypadku tworzenia Twój projekt używający **debugowania** konfiguracji. Właściwość jest ignorowana w innych konfiguracji.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Opis właściwości EnablePackageProcessLoggingAndAssert

[Kompilowanie i projekty aplikacji sieci Web pakowania](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) opisano, jak sieci Web potok publikowania (WPP) zapewnia zbiór docelowych elementów MSBuild, które zapewniają rozszerzenie funkcjonalności programu MSBuild i włącz ją zintegrować z sieci Web usług Internet Information Services (IIS) Narzędzia Deployment (Web Deploy). Podczas pakowania projektu aplikacji sieci web jest wywoływanie WPP elementów docelowych.

Wiele z tych celów WPP obejmują logikę warunkową zaloguje się dodatkowe informacje podczas **EnablePackageProcessLoggingAndAssert** właściwość jest ustawiona na **true**. Na przykład, jeśli należy przejrzeć **pakietu** obiektu docelowego widać tworzy katalog dziennika dodatkowe i zapisuje do pliku tekstowego z listą plików, jeśli **EnablePackageProcessLoggingAndAssert** jest równa **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Obiekty docelowe WPP są zdefiniowane w *Microsoft.Web.Publishing.targets* plików w folderze % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Możesz otworzyć ten plik i przejrzyj elementy docelowe w Visual Studio 2010 lub dowolnego edytora XML. Należy zadbać, aby nie modyfikować zawartość pliku.


## <a name="enabling-the-additional-logging"></a>Włączanie rejestrowania dodatkowe

Należy podać wartość **EnablePackageProcessLoggingAndAssert** właściwości na różne sposoby, w zależności od tego, jak utworzyć projekt.

W przypadku tworzenia projektu z poziomu wiersza polecenia, należy podać wartość **EnablePackageProcessLoggingAndAssert** właściwość jako argument wiersza polecenia:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Jeśli używasz pliku projektu niestandardowych do tworzenia projektów, możesz uwzględnić **EnablePackageProcessLoggingAndAssert** wartość w **właściwości** atrybutu **MSBuild**zadań:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Jeśli używasz definicji kompilacji Team Foundation Server (TFS) do tworzenia projektów, należy podać wartość **EnablePackageProcessLoggingAndAssert** właściwości w **argumenty programu MSBuild** wiersza:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia i konfigurowania definicje kompilacji, zobacz [tworzenie kompilacji definicji czy obsługuje wdrożenia](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Alternatywnie, jeśli chcesz uwzględnić pakietu w każdej kompilacji, można zmodyfikować pliku projektu dla projektu aplikacji sieci web ustawić **EnablePackageProcessLoggingAndAssert** właściwości **true**. Właściwość należy dodać do pierwszej **PropertyGroup** elementu w pliku .csproj lub .vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Przeglądanie plików dziennika

Podczas tworzenia i pakietów z projektu aplikacji sieci web **EnablePackageProcessLoggingAndAssert** ustawioną **true**, MSBuild tworzy dodatkowy folder o nazwie logowania *ProjectName* \_Folderu pakietu. Folder dziennika zawiera różne pliki:

![](troubleshooting-the-packaging-process/_static/image2.png)

Lista plików, które są wyświetlane zależą od elementów w projekcie i procesu kompilacji. Jednak te pliki są zwykle używane do rejestrowania lista plików, które zbiera WPP dla na różnych etapach procesu tworzenia pakietów:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* plik zawiera listę plików, które zbiera MSBuild dla pakowania, zanim zostaną usunięte wszystkie pliki, które są określone do wykluczenia.
- *AfterExcludeFilesFilesList.txt* plik zawiera listę zmodyfikowany plik po usunięciu wszystkie pliki, które są określone do wykluczenia.

    > [!NOTE]
    > Aby uzyskać więcej informacji na wykluczanie plików i folderów z procesu tworzenia pakietów, zobacz [z wyjątkiem plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* plik zawiera listę plików zbierane na potrzeby pakowania po dowolnym *Web.config* transformacje mogły zostać wykonane. Na tej liście żadnych konfiguracji specyficznych dla *Web.config* przekształcanie plików, takie jak *Web.Debug.config* i *Web.Release.config*, są wykluczane z listy plików dla Tworzenie pakietów. Pojedynczy przekształcone *Web.config* znajduje się w ich miejscu.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* plik zawiera listę plików po parametry połączenia w *Web.config* pliku została sparametryzowana. Jest to proces, który umożliwia wymianę parametry połączenia z odpowiednich ustawień dla środowiska docelowego podczas wdrażania pakietu.
- *Prepackage.txt* plik zawiera ukończone prekompilacyjnego lista plików do uwzględnienia w pakiecie.

> [!NOTE]
> Nazwy plików dziennika dodatkowe zwykle odpowiadają WPP elementy docelowe. Możesz przejrzeć następujących elementów docelowych, sprawdzając *Microsoft.Web.Publishing.targets* plików w folderze % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Jeśli zawartość pakietu sieci web nie odpowiadają oczekiwaniom, przeglądania tych plików może być wygodny sposób na określenie, jakie punktu w kwestii procesu poszło źle.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano, jak używasz **EnablePackageProcessLoggingAndAssert** właściwości w programie MSBuild rozwiązywać proces tworzenia pakietu. Go wyjaśniono różne sposoby, w którym można podać wartość właściwości do procesu tworzenia i go opisane dodatkowe informacje, które są rejestrowane, gdy wartość właściwości **true**.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat używania niestandardowe pliki projektu MSBuild kontrolować proces wdrażania, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na WPP i jak zarządza proces tworzenia pakietu, zobacz [budynku i projekty aplikacji sieci Web pakowania](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Aby uzyskać wskazówki dotyczące sposobu wykluczenie określonych plików i folderów z sieci web pakiety wdrożeniowe, zobacz [z wyjątkiem plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md).

>[!div class="step-by-step"]
[Poprzednie](running-windows-powershell-scripts-from-msbuild-project-files.md)
