---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Wdrażanie określonej kompilacji | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób wdrażania pakietów sieci web i skrypty bazy danych z określonego poprzednią kompilację do nowego miejsca docelowego, na przykład przedstawiający przejściowych lub produkcyjnych...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 312cd6f42463722b76996a3a848102946e0ca265
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832471"
---
<a name="deploying-a-specific-build"></a>Wdrażanie określonej kompilacji
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób wdrażania pakietów sieci web i skrypty bazy danych z określonego poprzednią kompilację do nowego miejsca docelowego, takie jak środowisku tymczasowym czy produkcyjnym.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w której procesu kompilacji i wdrażania jest kontrolowany przez dwa pliki projektu&#x2014;jeden zawierającej instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Do tej pory w tematach wymienionych w tym zestawie samouczków skupia się na temat sposobu tworzenia pakietu i wdrożenie aplikacji sieci web i baz danych w ramach jednego kroku lub zautomatyzowanego procesu. Jednak w niektórych typowych scenariuszy, należy wybrać wdrażanych zasobów z listy kompilacji w folderze wrzucania. Innymi słowy najnowszej kompilacji może nie być kompilację, której chcesz wdrożyć.

Rozważmy scenariusz ciągłej integracji (CI), opisane w poprzednim temacie [tworzenie kompilacji definicji, obsługuje wdrożenia](creating-a-build-definition-that-supports-deployment.md). Utworzono definicję kompilacji w Team Foundation Server (TFS) 2010. Za każdym razem, gdy deweloper sprawdza kod w programie TFS, Team Build będzie kompilacji kodu, tworzenie pakietów sieci web i skrypty bazy danych jako część procesu kompilacji, uruchom wszystkie testy jednostkowe i wdrażasz swoje zasoby w środowisku testowym. W zależności od zasad przechowywania, które zostały skonfigurowane podczas tworzenia definicji kompilacji TFS zachowa pewną liczbę poprzednich kompilacji.

![](deploying-a-specific-build/_static/image1.png)

Załóżmy, że po wykonaniu weryfikacji, sprawdzanie poprawności testów na jednym z tych kompilacji w środowisku testowym i wszystko jest gotowe do wdrożenia aplikacji w środowisku przejściowym. W międzyczasie deweloperzy mogą zaewidencjonowano nowego kodu. Nie chcesz ponownie skompilować rozwiązanie i wdrażanie w środowisku przejściowym, a nie chcesz wdrożenie najnowszej kompilacji w środowisku przejściowym. Zamiast tego którą chcesz wdrożyć specyficznej kompilacji, które zostały zweryfikowane i zweryfikowane na serwerach testowych.

Aby to osiągnąć, należy sprawdzić aparatu Microsoft Build Engine (MSBuild), gdzie można znaleźć pakietów sieci web i skryptów bazy danych, które wygenerowany konkretnej kompilacji.

## <a name="overriding-the-outputroot-property"></a>Zastępowanie właściwości OutputRoot

W [przykładowe rozwiązanie](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* pliku deklaruje właściwość o nazwie **OutputRoot**. Jak sugeruje nazwa, jest to folder główny, który zawiera wszystko, co jest generowany przez proces kompilacji. W *Publish.proj* plików, można zobaczyć które **OutputRoot** właściwość odwołuje się do lokalizacji głównej dla wszystkich zasobów wdrożenia.

> [!NOTE]
> **OutputRoot** jest nazwą właściwości często używane. Pliki projektu Visual C# i Visual Basic również zadeklarować tej właściwości, aby przechowywać główną lokalizację dla wszystkie dane wyjściowe kompilacji.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Jeśli chcesz, aby plik projektu do wdrożenia pakietów sieci web i bazy danych skryptów z innej lokalizacji&#x2014;danych wyjściowych poprzedniej kompilacji serwera TFS, takich jak&#x2014;po prostu musisz przesłonić **OutputRoot** właściwości. Na serwerze programu Team Build, powinny ustawić wartość właściwości do folderu kompilacji odpowiednie. Jeśli podczas uruchamiania programu MSBuild w wierszu polecenia, można określić wartość dla **OutputRoot** jako argument wiersza polecenia:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


W praktyce, czy też chcesz pominąć **kompilacji** docelowej&#x2014;Brak punktu tworzenia rozwiązania, jeśli nie planujesz używania dane wyjściowe kompilacji. Można to zrobić przez określenie obiektów docelowych, które chcesz wykonać z poziomu wiersza polecenia:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Jednak w większości przypadków należy wbuduj logikę wdrożenia w definicji kompilacji serwera TFS. Dzięki temu użytkownicy z **ustawiania kompilacji w kolejce** uprawnień do wyzwalania wdrożenia z żadnej instalacji programu Visual Studio z połączeniem z serwerem TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Tworzenie definicji kompilacji, aby wdrożyć określonej kompilacji

Następna procedura zawiera opis sposobu tworzenia definicji kompilacji, która umożliwia użytkownikom Wyzwalaj wdrożenia w środowisku przejściowym, za pomocą jednego polecenia.

W takim przypadku nie ma definicji kompilacji, aby faktycznie zbudować prawie wszystko&#x2014;po prostu chcesz, aby wykonać logikę wdrożenia w pliku projektu niestandardowe. *Publish.proj* plik zawiera logikę warunkową, z pominięciem **kompilacji** docelowy, jeśli plik jest uruchomiona w kompilacji zespołu. Jest to realizowane przy ocenie wbudowanych **BuildingInTeamBuild** właściwość, która jest automatycznie ustawiana na **true** po uruchomieniu pliku projektu w programie Team Build. W rezultacie możesz pominąć procesu kompilacji i po prostu uruchomić plik projektu do Wdróż istniejącą kompilację.

**Aby utworzyć definicję kompilacji, aby ręcznie wyzwolić wdrożenia**

1. W programie Visual Studio 2010 w **Team Explorer** oknie rozwinięciu węzła projektu zespołu, kliknij prawym przyciskiem myszy **kompilacje**, a następnie kliknij przycisk **Nowa definicja kompilacji**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Na **ogólne** kartę, nadaj nazwę definicji kompilacji (na przykład **DeployToStaging**) i opcjonalny opis.
3. Na **wyzwalacza** zaznacz **ręczne — zaewidencjonowania nie wyzwalają nowej kompilacji**.
4. Na **ustawienia domyślne kompilacji** na karcie **Kopiuj dane wyjściowe kompilacji do następującego folderu docelowego** wpisz ścieżkę Universal Naming Convention (UNC) folderu wrzucania (na przykład  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Na **procesu** na karcie **plik procesu kompilacji** listy rozwijanej, pozostaw **DefaultTemplate.xaml** wybrane. Jest to jeden z domyślnych szablonów procesów kompilacji, które poproś o dodanie Cię do wszystkich nowych projektów zespołowych.
6. W **parametrów procesu kompilacji** tabelę, kliknij opcję w **elementy do kompilacji** wiersza, a następnie kliknij przycisk **wielokropka** przycisku.

    ![](deploying-a-specific-build/_static/image4.png)
7. W **elementy do kompilacji** okno dialogowe, kliknij przycisk **Dodaj**.
8. W **elementów typu** listy rozwijanej wybierz **pliki projektu MSBuild**.
9. Przejdź do lokalizacji pliku niestandardowego projektu za pomocą którego możesz kontrolować proces wdrażania, wybierz plik, a następnie kliknij przycisk **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. W **elementy do kompilacji** okno dialogowe, kliknij przycisk **OK**.
11. W **parametrów procesu kompilacji** tabeli, a następnie rozwiń **zaawansowane** sekcji.
12. W **argumenty MSBuild** wiersz, określ lokalizację pliku projektu określonego środowiska i dodać jego symbol zastępczy dla lokalizacji folderu kompilacji:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Należy zastąpić **OutputRoot** wartość za każdym razem, gdy tworzysz kolejkę kompilacji. Zostało to opisane w następnej procedurze.
13. Kliknij przycisk **Zapisz**.

Gdy użytkownik zainicjuje kompilacji, należy zaktualizować **OutputRoot** właściwości, aby wskazywał kompilacji, którą chcesz wdrożyć.

**Aby wdrożyć konkretnej kompilacji z definicji kompilacji**

1. W **Team Explorer** okna, kliknij prawym przyciskiem myszy definicji kompilacji, a następnie kliknij przycisk **Zakolejkuj nową kompilację**.

    ![](deploying-a-specific-build/_static/image7.png)
2. W **Zakolejkuj kompilację** dialogowym **parametry** kartę, a następnie rozwiń **zaawansowane** sekcji.
3. W **argumenty MSBuild** wiersz, zastąp wartość **OutputRoot** właściwość o lokalizację folderu kompilacji. Na przykład:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Pamiętaj zawierać końcowego ukośnika na końcu ścieżki do folderu kompilacji.
4. Kliknij przycisk **kolejki**.

Podczas dodawania kompilacji do kolejki, plik projektu zostanie wdrożona skryptów bazy danych i sieci web pakiety z poziomu folderu upuszczania kompilacji podana w **OutputRoot** właściwości.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób publikowania zasobów wdrożenia, takie jak pakiety programu web i skrypty bazy danych, z konkretną poprzedniej kompilacji, przy użyciu modelu wdrażania pliku projektu podziału. Jego wyjaśniono, jak zastąpić **OutputRoot** właściwość i jak włączenie logiki wdrożenia do TFS definicji kompilacji.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji, zobacz [tworzenie Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) i [zdefiniować proces kompilacji](https://msdn.microsoft.com/library/ms181715.aspx). Aby uzyskać więcej wskazówek dotyczących kolejkowania kompilacji, zobacz [kolejkować kompilację](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-build-definition-that-supports-deployment.md)
> [dalej](configuring-permissions-for-team-build-deployment.md)
