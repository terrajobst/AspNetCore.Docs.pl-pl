---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "Wdrażanie określonej kompilacji | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano sposób wdrażania pakietów sieci web i skryptów bazy danych z określonych ostatniej kompilacji do nowego miejsca docelowego, takich jak środowiska tymczasowym czy produkcyjnym..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: afac083c96c1396ad60275fcb55a0ec9c4c0bd44
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-specific-build"></a>Wdrażanie określonej kompilacji
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak wdrażać pakiety sieci web i skryptów bazy danych z określonych ostatniej kompilacji do nowego miejsca docelowego, takich jak środowisku tymczasowym czy produkcyjnym.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w które procesem kompilacji i wdrażania są kontrolowane przez dwa pliki projektu & #x 2014; IE ne instrukcjami kompilacji, które są stosowane do każdego środowiska docelowego i jeden zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Do tej pory tematy w tym zestawie samouczek koncentruje się na temat sposobu tworzenia, pakowanie i wdrażanie aplikacji sieci web i baz danych jako część pojedynczy krok lub zautomatyzowanego procesu. Jednak w niektórych typowych scenariuszy, należy wybrać zasoby, które można wdrożyć z listy kompilacji w folderze upuszczania. Innymi słowy ostatniej kompilacji nie może być kompilacji, który chcesz wdrożyć.

Rozważmy scenariusz ciągłej integracji (CI) opisana w poprzedniej sekcji, [tworzenie kompilacji definicji czy obsługuje wdrożenia](creating-a-build-definition-that-supports-deployment.md). Po utworzeniu definicję kompilacji w Team Foundation Server (TFS) 2010. Za każdym razem, gdy dewelopera sprawdza kod do programu TFS, Team Build będzie kompilacji kodu, tworzenie pakietów sieci web i skryptów bazy danych jako część procesu kompilacji, uruchom wszystkie testy jednostkowe i wdrażanie zasobów w środowisku testowym. W zależności od zasad przechowywania skonfigurowane podczas tworzenia definicji kompilacji TFS zachowują pewną liczbę poprzednich wersjach.

![](deploying-a-specific-build/_static/image1.png)

Załóżmy, że zostało wykonane weryfikacji i sprawdzania poprawności testów na jednym z tych kompilacje w środowisku testowym i wszystko jest gotowe do wdrożenia aplikacji w środowisku przemieszczania. Tymczasem deweloperzy mogą mieć zaewidencjonowany nowy kod. Nie chcesz ponownie skompiluj rozwiązanie, a następnie wdrożyć do środowiska pomostowego, a nie chcesz wdrożyć ostatniej kompilacji w środowisku przemieszczania. Zamiast tego którą chcesz wdrożyć określonej kompilacji, które zostały sprawdzone i zatwierdzone na serwerach testowym.

W tym celu należy sprawdzić, gdzie można znaleźć pakietów sieci web i skryptów bazy danych, które wygenerowanych przez kompilację określonych aparat kompilacji firmy Microsoft (MSBuild).

## <a name="overriding-the-outputroot-property"></a>Zastępowanie właściwości OutputRoot

W [przykładowe rozwiązanie](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* pliku deklaruje właściwość o nazwie **OutputRoot**. Jak wynika z nazwy, to folderu głównego, który zawiera wszystkie elementy, które generuje procesu kompilacji. W *Publish.proj* pliku widać, że **OutputRoot** właściwość odwołuje się do lokalizacji głównej dla wszystkich zasobów wdrożenia.

> [!NOTE]
> **OutputRoot** jest nazwą właściwości często używane. Pliki projektu Visual C# i Visual Basic również zadeklarować tej właściwości do przechowywania katalog główny dla wszystkich plików wyjściowych kompilacji.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Jeśli chcesz, aby plik projektu do wdrażania pakietów sieci web i skryptów bazy danych z innej lokalizacji & #x 2014; takich jak dane wyjściowe poprzedniego kompilacji TFS & #x 2014; musisz po prostu zastąpić **OutputRoot** właściwości. Wartość właściwości należy ustawić do folderu odpowiednich kompilacji na serwerze programu Team Build. Jeśli podczas uruchamiania programu MSBuild w wierszu polecenia, można określić wartość dla **OutputRoot** jako argument wiersza polecenia:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


W praktyce, czy też chcesz pominąć **kompilacji** docelowej & #x 2014; nie ma żadnych punktu tworzenia rozwiązania, jeśli nie zamierzasz korzystać z danych wyjściowych kompilacji. Można to zrobić, określając obiektów docelowych, które chcesz wykonać z poziomu wiersza polecenia:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Jednak w większości przypadków należy do tworzenia logiki wdrożenia do definicji kompilacji TFS. Dzięki temu użytkownicy z **kolejka kompilacji** uprawnień, aby wyzwolić wdrożenie z żadnej instalacji programu Visual Studio z połączeniem z serwerem TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Tworzenie definicji kompilacji do wdrożenia w określonej kompilacji

Następna procedura opisuje sposób tworzenia definicji kompilacji, która umożliwia użytkownikom wyzwalacza wdrożenia w środowisku przemieszczania za pomocą jednego polecenia.

W takim przypadku nie ma definicji kompilacji zbudowaniem niczego & #x 2014; możesz po prostu go do wykonania w pliku projektu niestandardowej logiki wdrożenia. *Publish.proj* plik zawiera logikę warunkową, z pominięciem **kompilacji** docelowych, jeśli plik jest uruchomiona w Team Build. Jest to możliwe dzięki wbudowanej oceny **BuildingInTeamBuild** właściwość, która jest automatycznie ustawiana **true** po uruchomieniu pliku projektu w Team Build. W związku z tym można pominąć proces kompilacji i wystarczy uruchomić plik projektu do kompilacji istniejącego wdrożenia.

**Aby utworzyć definicję kompilacji, aby wyzwolić wdrożenie ręcznie**

1. W programie Visual Studio 2010 w **Team Explorer** okna, rozwiń węzeł projektu zespołowego, kliknij prawym przyciskiem myszy **kompilacje**, a następnie kliknij przycisk **nową definicję kompilacji**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Na **ogólne** karcie, nadaj nazwę definicji kompilacji (na przykład **DeployToStaging**) i opcjonalny opis.
3. Na **wyzwalacza** wybierz opcję **ręcznego — zaewidencjonowania nie wyzwalają nowej kompilacji**.
4. Na **kompilacji domyślne** karcie **Kopiuj dane wyjściowe kompilacji do następującego folderu docelowego** wpisz ścieżkę Universal Naming Convention (UNC) z folderu docelowego (na przykład  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Na **procesu** karcie **pliku procesu kompilacji** listy rozwijanej, pozostaw **DefaultTemplate.xaml** wybrane. Jest to jeden z szablonów domyślnych procesów kompilacji, które zostaną dodane do wszystkich nowych projektów zespołowych.
6. W **parametrów procesu kompilacji** tabeli, kliknij przycisk **elementów do kompilacji** wiersza, a następnie kliknij przycisk **wielokropka** przycisku.

    ![](deploying-a-specific-build/_static/image4.png)
7. W **elementów do kompilacji** okno dialogowe, kliknij przycisk **Dodaj**.
8. W **elementów typu** listy rozwijanej wybierz **pliki projektu MSBuild**.
9. Przejdź do lokalizacji pliku projektu niestandardowe, z którym można kontrolować proces wdrażania, wybierz plik, a następnie kliknij **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. W **elementów do kompilacji** okno dialogowe, kliknij przycisk **OK**.
11. W **parametrów procesu kompilacji** tabeli, a następnie rozwiń **zaawansowane** sekcji.
12. W **argumenty programu MSBuild** wiersz, określ lokalizację pliku projektu określonego środowiska i dodać symbol zastępczy dla lokalizacji folderu kompilacji:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Należy zastąpić **OutputRoot** wartość za każdym razem, gdy kolejka kompilacji. Ten temat znajdują się w następnej procedurze.
13. Kliknij przycisk **zapisać**.

Gdy wyzwolić kompilację, musisz zaktualizować **OutputRoot** właściwości, aby wskazywał kompilacji, którą chcesz wdrożyć.

**Aby wdrożyć określonej kompilacji z definicji kompilacji**

1. W **Team Explorer** okna, kliknij prawym przyciskiem myszy definicję kompilacji, a następnie kliknij przycisk **Tworzenie nowej kolejki**.

    ![](deploying-a-specific-build/_static/image7.png)
2. W **kolejka kompilacji** na okna dialogowego **parametry** karcie, rozwiń **zaawansowane** sekcji.
3. W **argumenty programu MSBuild** wiersz, zastąp wartość **OutputRoot** właściwość o lokalizację folderu kompilacji. Na przykład:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Należy uwzględnić kreskę ułamkową odwróconą na końcu ścieżki do folderu kompilacji.
4. Kliknij przycisk **kolejki**.

Gdy kolejka kompilacji, plik projektu zostanie wdrożona skryptów bazy danych i sieci web pakiety z folderu przechowywania kompilacji podana w **OutputRoot** właściwości.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób publikowania zasobów wdrożenia, takie jak pakiety sieci web i skryptów bazy danych, z poprzedniej określonej kompilacji przy użyciu modelu wdrażania pliku projektu podziału. Go wyjaśniono sposób przesłonięcia **OutputRoot** właściwości i jak włączyć logiki wdrożenia do TFS definicji kompilacji.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia definicje kompilacji, zobacz [utworzyć podstawową definicję kompilacji](https://msdn.microsoft.com/en-us/library/ms181716.aspx) i [Definiowanie procesu kompilacji](https://msdn.microsoft.com/en-us/library/ms181715.aspx). Aby uzyskać więcej pomocy w przypadku kompilacji usługi kolejkowania wiadomości, zobacz [kolejki kompilację](https://msdn.microsoft.com/en-us/library/ms181722.aspx).

>[!div class="step-by-step"]
[Poprzednie](creating-a-build-definition-that-supports-deployment.md)
[dalej](configuring-permissions-for-team-build-deployment.md)
