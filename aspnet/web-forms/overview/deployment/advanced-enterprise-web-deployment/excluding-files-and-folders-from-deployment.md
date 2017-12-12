---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: "Wykluczanie plików i folderów z wdrożenia | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano, jak można wykluczyć pliki i foldery z pakietu wdrożeniowego sieci web podczas kompilacji i pakietu projektu aplikacji sieci web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 80810415bac473a58f60110fb9d08772e0627bd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="excluding-files-and-folders-from-deployment"></a>Wykluczanie plików i folderów z wdrożenia
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak można wykluczyć pliki i foldery z pakietu wdrożeniowego sieci web podczas kompilacji i pakietu projektu aplikacji sieci web.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="overview"></a>Omówienie

Podczas kompilowania projektu aplikacji sieci web w programie Visual Studio 2010 Web potok publikowania (WPP) umożliwia rozszerzenie tego procesu kompilacji według opakowania Twojej skompilowanej aplikacji sieci web do pakietu sieci web do wdrożenia. Można następnie użyć narzędzia wdrażania sieci Internet Information Services (IIS) (Web Deploy) do wdrożenia tego pakietu sieci web do zdalnego serwera sieci web usług IIS lub zaimportować pakiet ręcznie za pomocą Menedżera usług IIS. Ten proces tworzenia pakietu zostało wyjaśnione w dokumencie [budynku i projekty aplikacji sieci Web pakowania](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

W jaki sposób można kontrolować, które pobiera ujęte w sieci web pakietu? Ustawienia projektu programu Visual Studio za pomocą źródłowego pliku projektu, zapewniają wystarczającą kontrolę nad wiele scenariuszy. Jednak w niektórych przypadkach można dostosować zawartość pakietu sieci web do określonego miejsca docelowego środowisk. Na przykład można umieścić folder plików dziennika podczas wdrażania aplikacji w środowisku testowym, ale podczas wdrażania aplikacji w środowisku tymczasowym czy produkcyjnym można wykluczyć folder. W tym temacie opisano, jak to zrobić.

## <a name="what-gets-included-by-default"></a>Pobiera co włączone domyślnie?

Po skonfigurowaniu właściwości projektu aplikacji sieci web w programie Visual Studio, **elementy do wdrożenia** listy na **pakowaniu/publikowaniu Web** umożliwia określenie, co chcesz uwzględnić w danym wdrożeniu sieci web pakiet. Domyślnie ta jest równa **tylko pliki potrzebne do uruchomienia tej aplikacji**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Po wybraniu **tylko pliki potrzebne do uruchomienia tej aplikacji**, WPP spróbuje określić, które pliki powinny zostać dodane do pakietu sieci web. Możliwości obejmują:

- Wyświetla wszystkie kompilacje dla projektu.
- Wszystkie pliki oznaczone przy użyciu akcji kompilacji **zawartości**.

> [!NOTE]
> Logiki, która określa, które pliki są zawarte w tym pliku:   
> *%ProgramFiles%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Wykluczenie określonych plików i folderów

W niektórych przypadkach należy bardziej precyzyjną kontrolę, w którym są wdrażane pliki i foldery. Jeśli wiesz, że pliki, które chcesz wykluczyć wyprzedzenia czas i wykluczenia ma zastosowanie do wszystkich środowisk docelowej, można po prostu **Akcja kompilacji** każdego pliku na **Brak**.

**Aby wykluczyć określone pliki z wdrożenia**

1. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **właściwości**.
2. W **właściwości** okna w **Akcja kompilacji** wierszu, wybierz opcję **Brak**.

Jednak ta metoda nie zawsze jest wygodne. Na przykład może zajść potrzeba różnią się, które pliki i foldery znajdują się w zależności od środowiska docelowego i z zewnętrznego programu Visual Studio. Na przykład w kontaktów Menedżerze przykładowe rozwiązanie Przyjrzyjmy się zawartość ContactManager.Mvc projektu:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Wewnętrzny folder zawiera niektóre skrypty SQL, deweloper używane do tworzenia, drop i wypełnić lokalnych baz danych do celów programistycznych. Nic w tym folderze powinny zostać wdrożone w środowisku tymczasowym czy produkcyjnym.
- Folder skryptów zawiera kilka plików JavaScript. Wiele z tych plików znajdują się wyłącznie do obsługi debugowania lub podaj IntelliSense w Visual Studio. Niektóre z tych plików należy nie można wdrożyć w środowisku tymczasowym czy produkcyjnym. Można jednak wdrożyć je do środowiska testowego deweloperów w celu ułatwienia rozwiązywania problemów.

Mimo że można manipulować plików projektu, które mają zostać wykluczone z określonych plików i folderów, jest prostsze. Mechanizm wykluczyć pliki i foldery, tworzenie list elementów o nazwie obejmuje WPP **ExcludeFromPackageFolders** i **ExcludeFromPackageFiles**. Ten mechanizm można rozszerzyć przez dodanie własne elementy do tych list. Aby to zrobić, należy wykonać następujące ogólne kroki:

1. Utwórz plik niestandardowe projektu o nazwie *.wpp.targets [Nazwa projektu]* w tym samym folderze co plik projektu.

    > [!NOTE]
    > *. Wpp.targets* plik ma znaleźć się w tym samym folderze co plik projektu aplikacji sieci web danej #x 2014; na przykład *ContactManager.Mvc.csproj*& #x 2014; a nie w tym samym folderze wszystkich pliki projektu niestandardowych służy do sterowania kompilacji i procesu wdrażania.
2. W *. wpp.targets* plików, dodawanie **ItemGroup** elementu.
3. W **ItemGroup** elementu, Dodaj **ExcludeFromPackageFolders** i **ExcludeFromPackageFiles** elementy do wykluczenia z określonych plików i folderów, zgodnie z wymaganiami.

Jest to podstawowa struktura *. wpp.targets* pliku:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Należy pamiętać, że każdy element zawiera element metadanych elementu o nazwie **FromTarget**. Jest to wartość opcjonalna, która nie ma wpływu na proces kompilacji; Służy on po prostu wskaż, dlaczego określone pliki lub foldery zostały pominięte Jeśli ktoś przegląda dzienniki kompilacji.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Wykluczanie plików i folderów z pakietu sieci Web

Następna procedura przedstawia sposób dodawania *. wpp.targets* plik do projektu aplikacji sieci web i sposobu użycia pliku do wykluczenia określonych plików i folderów z pakietu sieci web podczas kompilowania projektu.

**Aby wykluczyć pliki i foldery z pakietu wdrożeniowego sieci web**

1. Otwórz rozwiązanie w Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, kliknij prawym przyciskiem myszy węzeł projektu aplikacji sieci web (na przykład **ContactManager.Mvc**), wskaż polecenie **Dodaj**, a następnie kliknij przycisk **Nowy element**.
3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **pliku XML** szablonu.
4. W **nazwa** wpisz *[Nazwa projektu]***. wpp.targets** (na przykład **ContactManager.Mvc.wpp.targets**), a następnie kliknij przycisk  **Dodaj**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Jeśli dodajesz nowy element do węzła głównego projektu, plik jest tworzony w tym samym folderze co plik projektu. Można to sprawdzić, otwierając folder w Eksploratorze Windows.
5. W pliku, należy dodać **projektu** elementu i **ItemGroup** elementu:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Jeśli chcesz wykluczyć foldery ze pakietu sieci web, Dodaj **ExcludeFromPackageFolders** elementu **ItemGroup** elementu:

    1. W **Include** atrybutu, podaj Rozdzielana średnikami lista folderów do wykluczenia.
    2. W **FromTarget** elementu metadanych, podaj wartość łatwy do rozpoznania, aby wskazać, dlaczego jest wykluczany folderów, takie jak nazwa *. wpp.targets* pliku.

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Jeśli chcesz wykluczyć pliki z pakietu sieci web, Dodaj **ExcludeFromPackageFiles** elementu **ItemGroup** elementu:

    1. W **Include** atrybutu, podaj rozdzieloną średnikami listę plików, które chcesz wykluczyć.
    2. W **FromTarget** elementu metadanych, podaj wartość łatwy do rozpoznania, aby wskazać, dlaczego jest wykluczany plików, takie jak nazwa *. wpp.targets* pliku.

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *.Wpp.targets [Nazwa projektu]* plik powinien teraz wyglądać następująco:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Zapisz i Zamknij *.wpp.targets [Nazwa projektu]* pliku.

Przy kolejnym uruchomieniu pakiet i kompilacji projektu aplikacji sieci web, WPP automatycznie wykryje *. wpp.targets* pliku. Wszystkie pliki i foldery, które określiłeś nie będą uwzględniane w pakietu sieci web.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób wykluczenie określonych plików i folderów podczas tworzenia pakietu sieci web przez utworzenie niestandardowego *. wpp.targets* pliku w tym samym folderze co plik projektu aplikacji sieci web.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na przy użyciu niestandardowe pliki projektu Microsoft kompilacji Engine (MSBuild), aby kontrolować proces wdrażania, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji dotyczących tworzenia pakietów i proces wdrażania, zobacz [budynku i projekty aplikacji sieci Web pakowania](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [konfigurowania parametrów wdrażania pakietu sieci Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), i [ Wdrażanie pakietów sieci Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

>[!div class="step-by-step"]
[Poprzednie](deploying-membership-databases-to-enterprise-environments.md)
[dalej](taking-web-applications-offline-with-web-deploy.md)
