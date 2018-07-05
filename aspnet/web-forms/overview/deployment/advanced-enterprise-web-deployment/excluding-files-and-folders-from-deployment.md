---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Wykluczanie plików i folderów z wdrożenia | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak można wykluczyć pliki i foldery z pakietu wdrożeniowego sieci web podczas kompilacji i pakietów projektu aplikacji sieci web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c50352d423f41f84677dbf048e74088214340f3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382585"
---
<a name="excluding-files-and-folders-from-deployment"></a>Wykluczanie plików i folderów z wdrożenia
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak można wykluczyć pliki i foldery z pakietu wdrożeniowego sieci web podczas kompilacji i pakietów projektu aplikacji sieci web.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="overview"></a>Omówienie

Podczas kompilowania projektu aplikacji sieci web w programie Visual Studio 2010 w sieci Web potok publikowania (WPP) umożliwia rozszerzanie tego procesu kompilacji upakowanie swoje skompilowanej aplikacji sieci web w pakiecie można wdrożyć w sieci web. Można następnie użyć narzędzia wdrażania usług Internet Information Services (IIS) w sieci Web (Web Deploy) Aby wdrożyć ten pakiet sieci web do zdalnego serwera sieci web usług IIS lub zaimportować pakiet ręcznie za pomocą Menedżera usług IIS. Ten proces pakowania została wyjaśniona w [budowanie i projektów aplikacji sieci Web pakietu](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

W jaki sposób można kontrolować, które pobiera ujęte w sieci web pakietu? Ustawienia projektu w programie Visual Studio, za pomocą podstawowego pliku projektu, zapewnia odpowiedniego sterowania dla wielu scenariuszy. Jednak w niektórych przypadkach można dostosować zawartość sieci web pakietu do określonego miejsca docelowego środowiska. Na przykład możesz chcieć dołączyć folder plików dziennika, podczas wdrażania aplikacji w środowisku testowym, ale wykluczyć folder, w przypadku wdrażania aplikacji w środowisku tymczasowym lub produkcyjnym. W tym temacie pokazano jak to zrobić.

## <a name="what-gets-included-by-default"></a>Co pobiera domyślnie?

Po skonfigurowaniu właściwości projektu aplikacji sieci web w programie Visual Studio, **elementów, aby wdrożyć** listy na **pakowaniu/publikowaniu Web** strona pozwala określić, jakie mają zostać uwzględnione w danym wdrożeniu sieci web pakiet. Domyślnie jest ono ustawione na **tylko pliki potrzebne do uruchomienia tej aplikacji**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Po wybraniu **tylko pliki potrzebne do uruchomienia tej aplikacji**, potok WPP próbuje określić, które pliki powinny zostać dodane do pakietu sieci web. Możliwości obejmują:

- Wyświetla wszystkie kompilacje dla projektu.
- Wszystkie pliki oznaczone przy użyciu akcji kompilacji **zawartości**.

> [!NOTE]
> Logikę, która określa, które pliki do uwzględnienia znajduje się w tym pliku:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Wykluczenie określonych plików i folderów

W niektórych przypadkach można bardziej precyzyjną kontrolę nad tym, którzy są wdrożone pliki i foldery. Jeśli wiesz, że pliki, które chcesz wykluczyć wyprzedzenia czasu i wykluczenia mają zastosowanie do wszystkich środowisk docelowej, można po prostu ustaw **Build Action** każdego pliku **Brak**.

**Aby wykluczyć określone pliki z wdrożenia**

1. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **właściwości**.
2. W **właściwości** okna w **Build Action** wiersz, wybierz opcję **Brak**.

Jednak to podejście nie zawsze jest wygodne. Na przykład możesz chcieć różne pliki, które i foldery są uwzględnione, zgodnie ze środowiska docelowego, a poza programem Visual Studio. Na przykład w przykładowym rozwiązaniu Contact Manager Przyjrzyj się zawartości projektu ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Wewnętrzny folder zawiera niektóre skrypty SQL, które programistka używa do tworzenia, usuwania i wypełnić lokalnych baz danych do celów programistycznych. Żadne postanowienie w tym folderze nie powinny być wdrażane na środowisku tymczasowym czy produkcyjnym.
- Folder skryptów zawiera kilka plików JavaScript. Wiele z tych plików są uwzględniane wyłącznie do obsługi debugowania lub dostarczyć IntelliSense w programie Visual Studio. Nie, niektóre z tych plików powinny być wdrażane w środowiskach przejściowych lub produkcyjnych. Jednak można je wdrożyć w środowisku testowym dla deweloperów w celu ułatwienia rozwiązywania problemów.

Chociaż można manipulować plików projektu, które mają zostać wykluczone z określonych plików i folderów, jest łatwiejszy sposób. Potok WPP obejmuje mechanizm Wyklucz pliki i foldery, tworząc listy elementów o nazwie **ExcludeFromPackageFolders** i **ExcludeFromPackageFiles**. Ten mechanizm można rozszerzyć przez dodanie własne elementy do tych list. Aby to zrobić, należy wykonać następujące czynności wysokiego poziomu:

1. Tworzenie niestandardowego projektu pliku o nazwie *.wpp.targets [Nazwa projektu]* w tym samym folderze co plik projektu.

    > [!NOTE]
    > *. Wpp.targets* plik ma znaleźć się w tym samym folderze co plik projektu aplikacji sieci web&#x2014;na przykład *ContactManager.Mvc.csproj*&#x2014;, a nie w tym samym folderze co niestandardowe pliki projektu, których używasz do kontrolowania procesu kompilacji i wdrażania.
2. W *. wpp.targets* Dodaj **ItemGroup** elementu.
3. W **ItemGroup** elementu Dodawanie **ExcludeFromPackageFolders** i **ExcludeFromPackageFiles** elementy, aby wykluczyć określone pliki i foldery, zgodnie z potrzebami.

Jest to podstawowa struktura *. wpp.targets* pliku:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Należy pamiętać, że każdy element zawiera element metadanych jednostki o nazwie **FromTarget**. Jest to opcjonalna wartość, która nie ma wpływu na proces kompilacji; Służy ona po prostu wskaż, dlaczego zostały pominięte określone pliki lub foldery, jeśli ktoś przegląda dzienniki kompilacji.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Wykluczanie plików i folderów z pakietu sieci Web

Następna procedura dowiesz się, jak dodać *. wpp.targets* plik do projektu aplikacji sieci web i jak za pomocą pliku wykluczanie określonych plików i folderów z pakietu sieci web podczas kompilowania projektu.

**Aby wykluczyć pliki i foldery z pakietu wdrożeniowego sieci web**

1. Otwórz swoje rozwiązanie w programie Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, kliknij prawym przyciskiem myszy węzeł projektu aplikacji sieci web (na przykład **ContactManager.Mvc**), wskaż polecenie **Dodaj**, a następnie kliknij przycisk **Nowy element**.
3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **pliku XML** szablonu.
4. W **nazwa** wpisz *[Nazwa projektu] ***.wpp.targets** (na przykład **ContactManager.Mvc.wpp.targets**), a następnie kliknij przycisk **Dodaj**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Jeśli dodasz nowy element do węzła głównego projektu, plik jest tworzony w tym samym folderze co plik projektu. Można to sprawdzić, otwierając folder w Eksploratorze Windows.
5. W pliku należy dodać **projektu** elementu i **ItemGroup** elementu:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Jeśli chcesz wykluczyć foldery ze pakiet sieci web, należy dodać **ExcludeFromPackageFolders** elementu **ItemGroup** elementu:

   1. W **Include** atrybutu, podaj rozdzieloną średnikami listę folderów, które chcesz wykluczyć.
   2. W **FromTarget** elementu metadanych zapewniają mająca znaczenie wartość, aby wskazać, dlaczego foldery są jest wyłączone, takie jak nazwa *. wpp.targets* pliku.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Jeśli chcesz wykluczyć pliki z pakietu sieci web, należy dodać **ExcludeFromPackageFiles** elementu **ItemGroup** elementu:

   1. W **Include** atrybutu, podaj rozdzieloną średnikami listę plików, które chcesz wykluczyć.
   2. W **FromTarget** elementu metadanych zapewniają mająca znaczenie wartość, aby wskazać, dlaczego plików trwa wykluczonych, takie jak nazwa *. wpp.targets* pliku.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *.Wpp.targets [Nazwa projektu]* pliku powinna teraz wyglądać następująco:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Zapisz i Zamknij *.wpp.targets [Nazwa projektu]* pliku.

Przy kolejnym uruchomieniu kompilowanie i tworzenie pakietu projektu aplikacji sieci web, potok WPP automatycznie wykryje *. wpp.targets* pliku. Wszystkie pliki i foldery, które określiłeś nie będą uwzględniane w pakiecie sieci web.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób wykluczania określonych plików i folderów, podczas tworzenia pakietu sieci web, tworząc niestandardowe *. wpp.targets* pliku w tym samym folderze co plik projektu aplikacji sieci web.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat korzystania z niestandardowych plików projektu aparatu Microsoft Build Engine (MSBuild) do kontrolowania procesu wdrażania, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na temat tworzenia pakietów i proces wdrażania, zobacz [budowanie i projektów aplikacji sieci Web pakietu](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [konfigurowania parametrów wdrożenia pakietu internetowego](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), i [ Wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Poprzednie](deploying-membership-databases-to-enterprise-environments.md)
> [dalej](taking-web-applications-offline-with-web-deploy.md)
