---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Tworzenie aplikacji sieci Web w trybie Offline za pomocą sieci Web wdrażanie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób wykonania aplikacji sieci web w trybie offline na czas trwania zautomatyzowanego wdrażania z użyciem alert Wdr sieci Web usług Internet Information Services (IIS)...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 9b18a48914a74a7fea85278d0e601f6f84376b23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756771"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Wdrażanie przełączanie aplikacji internetowej w trybie Offline z sieci Web
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób wykonania aplikacji sieci web w trybie offline na czas trwania automatycznego wdrażania przy użyciu narzędzia wdrażania usług Internet Information Services (IIS) w sieci Web (Web Deploy). Użytkownicy, którzy przejdź do aplikacji sieci web są przekierowywane do *aplikacji\_offline.htm* pliku do czasu ukończenia wdrożenia.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

W wielu scenariuszach należy wykonać aplikację sieci web w trybie offline, podczas wprowadzania zmian do powiązanych składników, takich jak bazy danych lub usługi sieci web. Zazwyczaj w usługach IIS i platformy ASP.NET, można to zrobić, umieszczając plik o nazwie *aplikacji\_offline.htm* w folderze głównym aplikacji witryny sieci Web lub sieci web usług IIS. *Aplikacji\_offline.htm* plików to standardowy plik HTML i zwykle będzie zawierać prosty komunikat wniosku użytkownika, że witryna jest tymczasowo niedostępna z powodu konserwacji. Gdy *aplikacji\_offline.htm* plik istnieje w folderze głównym witryny sieci Web, usługi IIS przekierują automatycznie wszystkich żądań do pliku. Po wprowadzeniu wszystkich potrzebnych aktualizacji, Usuń *aplikacji\_offline.htm* plików i witryny sieci Web zostanie wznowione w zwykły sposób obsługiwania żądań.

Korzystając z narzędzia Web Deploy do przeprowadzania wdrożeń zautomatyzowanych lub pojedynczy krok do środowiska docelowego, możesz chcieć dołączyć Dodawanie i usuwanie *aplikacji\_offline.htm* pliku w procesie wdrażania. Aby to zrobić, należy wykonać te zadania wysokiego poziomu:

- W pliku projektu aparatu Microsoft Build Engine (MSBuild) służące do kontrolowania procesu wdrażania, Utwórz obiekt docelowy programu MSBuild, która kopiuje *aplikacji\_offline.htm* pliku na serwer docelowy, zanim wszystkie zadania wdrażania Rozpocznij.
- Dodaj inny docelowy programu MSBuild, który usuwa *aplikacji\_offline.htm* plików z serwera docelowego po ukończeniu wszystkich zadań wdrażania.
- W projekcie aplikacji sieci web, należy utworzyć *. wpp.targets* pliku, który zapewnia, że *aplikacji\_offline.htm* plik zostanie dodany do pakietu wdrożeniowego, gdy narzędzie Web Deploy jest wywoływany.

W tym temacie pokazują sposób wykonania tych procedur. Zadania i wskazówki, w tym temacie założono, że utworzono już rozwiązanie, które zawiera co najmniej jeden projekt aplikacji sieci web i użyć pliku niestandardowego projektu do kontrolowania procesu wdrażania zgodnie z opisem w [wdrażanie w Internecie w Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternatywnie, można użyć [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie, aby pracować z przykładami w tym temacie.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Dodawanie aplikacji\_pliku projektu aplikacji sieci Web w trybie Offline

Pierwsze zadanie, które należy wykonać, jest dodanie *aplikacji\_offline* plik do projektu aplikacji sieci web:

- Aby zapobiec pliku nie zakłócają proces programowania (nie chcesz aplikacja ma być trwale w trybie offline), należy wywołać go coś innego niż *aplikacji\_offline.htm*. Na przykład nazwać plik *aplikacji\_offline template.htm*.
- Aby zapobiec są wdrażane jako plik — jest, należy ustawić akcji kompilacji **Brak**.

**Aby dodać aplikację\_pliku projektu aplikacji sieci web w trybie offline**

1. Otwórz swoje rozwiązanie w programie Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, kliknij prawym przyciskiem myszy projekt aplikacji sieci web, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.
3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **strony HTML**.
4. W **nazwa** wpisz **aplikacji\_offline template.htm**, a następnie kliknij przycisk **Dodaj**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Dodaj kilka prostych HTML, aby zawiadomić użytkowników o tym, że aplikacja jest niedostępna, a następnie zapisz plik. Nie ma żadnych znaczników po stronie serwera (na przykład wszystkie znaczniki, które mają prefiks "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy nowy plik, a następnie kliknij przycisk **właściwości**.
7. W **właściwości** okna w **Build Action** wiersz, wybierz opcję **Brak**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Wdrażanie i usuwania aplikacji\_pliku w trybie Offline

Następnym krokiem jest do modyfikowania logika wdrożenia, aby skopiować plik na serwer docelowy, na początku procesu wdrażania i usunąć go na końcu.

> [!NOTE]
> Następna procedura zakłada, że używasz niestandardowego pliku projektu MSBuild kontrolować proces wdrażania, zgodnie z opisem w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Jeśli jest wdrażany bezpośrednio z programu Visual Studio, należy użyć innego podejścia. Jedno takie podejście w tym artykule opisano Sayed Ibrahim Hashimi [jak wykonać Twojej aplikacji w trybie Offline podczas publikowania w sieci Web](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Aby wdrożyć *aplikacji\_w trybie offline* pliku do docelowej witryny usług IIS, należy wywołać za pomocą MSDeploy.exe [narzędzia Web Deploy **contentPath** dostawcy](https://technet.microsoft.com/library/dd569034(WS.10).aspx). **ContentPath** dostawca obsługuje zarówno ścieżki fizycznej katalogu i ścieżek witryny sieci Web lub aplikację usług IIS, co staje się on idealnym wyborem w przypadku synchronizacji plików między folderem projektu programu Visual Studio i aplikacji sieci web usług IIS. Aby wdrożyć plik, polecenie MSDeploy powinien wyglądać następująco:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Aby usunąć plik z lokacji docelowej na końcu procesu wdrażania, polecenie MSDeploy powinien wyglądać następująco:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Aby zautomatyzować te polecenia jako część procesu kompilacji i wdrożenia, należy zintegrować niestandardowego pliku projektu MSBuild. W następnej procedurze opisano jak to zrobić.

**Wdrażanie i usuwanie aplikacji\_pliku w trybie offline**

1. W programie Visual Studio 2010 należy otworzyć plik projektu MSBuild, który kontroluje proces wdrażania. W [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie to *Publish.proj* pliku.
2. W katalogu głównym **projektu** elementu, Utwórz nową **PropertyGroup** elementu do przechowywania zmiennych dotyczących *aplikacji\_offline* wdrożenia:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot** właściwość jest zdefiniowana w innym miejscu w *Publish.proj* pliku. Oznacza to lokalizacja folderu głównego elementu zawartość źródłową względem bieżącej ścieżki&#x2014;innymi słowy, powiązane z lokalizacją *Publish.proj* pliku.
4. **ContentPath** dostawca nie będzie akceptować względnych ścieżek plików, więc należy uzyskać ścieżkę bezwzględną do pliku źródłowego, przed jego wdrożeniem. Możesz użyć [converttoabsolutepath —](https://msdn.microsoft.com/library/bb882668.aspx) zadanie, aby to zrobić.
5. Dodaj nową **docelowej** elementu o nazwie **GetAppOfflineAbsolutePath**. Ten element docelowy, użyj **converttoabsolutepath —** zadania, aby uzyskać ścieżkę bezwzględną do *aplikacji\_w trybie offline do szablonu* pliku w folderze projektu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Ten element docelowy ma ścieżkę względną do *aplikacji\_w trybie offline do szablonu* pliku w folderze projektu i zapisuje go do nowej właściwości jako bezwzględną ścieżkę do pliku. **BeforeTargets** atrybut określa, że ten element docelowy w celu wykonania przed **DeployAppOffline** docelowych, które utworzysz w następnym kroku.
7. Dodaj nowy obiekt docelowy o nazwie **DeployAppOffline**. W ramach tego obiektu docelowego, wywołaj polecenie MSDeploy.exe, która wdraża usługi *aplikacji\_w trybie offline* pliku do docelowego serwera sieci web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. W tym przykładzie **ContactManagerIisPath** właściwość jest zdefiniowana w innym miejscu w pliku projektu. Jest to po prostu ścieżka aplikacji IIS, w postaci *[nazwa witryny sieci Web usług IIS] / [Nazwa aplikacji]*. W tym warunku w elemencie docelowym umożliwia użytkownikom *aplikacji\_offline* wdrożenia lub wyłączyć, zmieniając wartość właściwości lub podając parametr wiersza polecenia.
9. Dodaj nowy obiekt docelowy o nazwie **DeleteAppOffline**. W ramach tego obiektu docelowego, wywołaj polecenie MSDeploy.exe, które powoduje usunięcie Twojego *aplikacji\_w trybie offline* pliku docelowego serwera sieci web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Ostatnim zadaniem jest wywołania te nowe obiekty docelowe w odpowiednich punktach podczas wykonywania pliku projektu. Można to zrobić na różne sposoby. Na przykład w *Publish.proj* pliku **FullPublishDependsOn** właściwość określa listę elementów docelowych, które muszą być wykonywane w kolejności, kiedy **FullPublish** domyślne docelowy jest wywoływana.
11. Modyfikowanie pliku projektu MSBuild do wywołania **DeployAppOffline** i **DeleteAppOffline** obiekty docelowe w odpowiednich miejscach w procesie publikowania.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Po uruchomieniu niestandardowego pliku projektu MSBuild *aplikacji\_offline* plik zostanie wdrożony na serwerze, natychmiast po pomyślnej kompilacji. Następnie będzie można usunąć z serwera po zakończeniu wszystkich zadań wdrażania.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Dodawanie aplikacji\_pliku w trybie Offline do pakietów wdrożeniowych

W zależności od tego, jak można skonfigurować wdrożenie, istniejących zawartości w lokalizacji docelowej usługi IIS aplikacji sieci web&#x2014;takich jak *aplikacji\_offline.htm* pliku&#x2014;mogą zostać usunięte automatycznie, gdy wdrożysz internetowy pakiet do miejsca docelowego. Aby upewnić się, że *aplikacji\_offline.htm* plik pozostaje w miejscu w czasie trwania operacji wdrożenia, musisz uwzględnić plik w obrębie samego pakietu wdrożeniowego sieci web, dodatkowo wdrażanie pliku bezpośrednio na początku proces wdrażania.

- Jeśli wykonano poprzednich zadań w tym temacie, będziesz dodano *aplikacji\_offline.htm* plik do projektu aplikacji sieci web w ramach innej nazwy pliku (użyliśmy *aplikacji\_ w trybie offline template.htm*) i będzie mieć Ustaw akcję kompilacji **Brak**. Zmiany te są konieczne, aby zapobiec plik z nie zakłócają programowanie i debugowanie. Dlatego należy dostosować proces pakowania, aby upewnić się, że *aplikacji\_offline.htm* plik jest uwzględniony w pakiecie wdrożeniowym sieci web.

Web potok publikowania (WPP) używa listy elementów o nazwie **FilesForPackagingFromProject** umożliwia tworzenie listy plików, które powinny być uwzględnione w pakiecie wdrożeniowym sieci web. Zawartość sieci web pakiety można dostosować, dodając własne elementy do tej listy. Aby to zrobić, należy wykonać następujące czynności wysokiego poziomu:

1. Tworzenie niestandardowego projektu pliku o nazwie *.wpp.targets [Nazwa projektu]* w tym samym folderze co plik projektu.

    > [!NOTE]
    > *. Wpp.targets* plik ma znaleźć się w tym samym folderze co plik projektu aplikacji sieci web&#x2014;na przykład *ContactManager.Mvc.csproj*&#x2014;, a nie w tym samym folderze co niestandardowe pliki projektu, których używasz do kontrolowania procesu kompilacji i wdrażania.
2. W *. wpp.targets* plików, Utwórz nowy obiekt docelowy programu MSBuild, który jest wykonywany *przed* **CopyAllFilesToSingleFolderForPackage** docelowej. Jest to element docelowy WPP, który tworzy listę rzeczy, które należy uwzględnić w pakiecie.
3. W nowej lokalizacji docelowej, należy utworzyć **ItemGroup** elementu.
4. W **ItemGroup** elementu Dodawanie **FilesForPackagingFromProject** elementu, a następnie określ *aplikacji\_offline.htm* pliku.

*. Wpp.targets* pliku powinna wyglądać następująco:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Oto kluczowe punkty Uwaga w tym przykładzie:

- **BeforeTargets** atrybut wstawia ten element docelowy do WPP, określając, który ma być wykonany bezpośrednio przed **CopyAllFilesToSingleFolderForPackage** docelowej.
- **FilesForPackagingFromProject** elementu używa **DestinationRelativePath** wartości metadanych można zmienić nazwy pliku z *aplikacji\_offline template.htm* Aby *aplikacji\_offline.htm* jest dodawany do listy.

Następna procedura dowiesz się, jak dodać to *. wpp.targets* plik do projektu aplikacji sieci web.

**Aby dodać. wpp.targets pliku do pakietu wdrożeniowego sieci web**

1. Otwórz swoje rozwiązanie w programie Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, kliknij prawym przyciskiem myszy węzeł projektu aplikacji sieci web (na przykład **ContactManager.Mvc**), wskaż polecenie **Dodaj**, a następnie kliknij przycisk **Nowy element**.
3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **pliku XML** szablonu.
4. W **nazwa** wpisz *[Nazwa projektu] ***.wpp.targets** (na przykład **ContactManager.Mvc.wpp.targets**), a następnie kliknij przycisk **Dodaj**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Jeśli dodasz nowy element do węzła głównego projektu, plik jest tworzony w tym samym folderze co plik projektu. Można to sprawdzić, otwierając folder w Eksploratorze Windows.
5. W pliku należy dodać znaczniki MSBuild opisanych powyżej.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Zapisz i Zamknij *.wpp.targets [Nazwa projektu]* pliku.

Przy kolejnym uruchomieniu kompilowanie i tworzenie pakietu projektu aplikacji sieci web, potok WPP automatycznie wykryje *. wpp.targets* pliku. *Aplikacji\_offline template.htm* plik zostanie uwzględniony w wynikowej pakiecie wdrożeniowym sieci web jako *aplikacji\_offline.htm*.

> [!NOTE]
> W przypadku niepowodzenia wdrożenia *aplikacji\_offline.htm* plik pozostanie w miejscu, a aplikacja będzie pozostawać w trybie offline. Jest to zazwyczaj żądane zachowanie. Aby wyświetlić aplikację trybu online, możesz usunąć *aplikacji\_offline.htm* plików z serwera sieci web. Alternatywnie, jeśli Usuń wszelkie błędy i uruchom pomyślne wdrożenie *aplikacji\_offline.htm* plik zostanie usunięty.


## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób wykonania aplikacji sieci web w trybie offline na czas trwania rozmieszczania, poprzez publikowanie *aplikacji\_offline.htm* pliku na serwer docelowy, na początku procesu wdrażania i usuwanie na stronie Zakończenie. Również przedstawione instrukcje obejmują *aplikacji\_offline.htm* plików w pakiecie wdrożeniowym sieci web.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia pakietów i proces wdrażania, zobacz [budowanie i projektów aplikacji sieci Web pakietu](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [konfigurowania parametrów wdrożenia pakietu internetowego](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), i [ Wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Jeśli publikowanie aplikacji sieci web bezpośrednio z programu Visual Studio, a nie przy użyciu metody pliku niestandardowego projektu MSBuild opisane w tych samouczkach, musisz użyć nieco innego podejścia do wykonania aplikacji w trybie offline podczas publikowania proces. Aby uzyskać więcej informacji, zobacz [sposób wykonania aplikacji sieci web w trybie offline podczas publikowania](https://go.microsoft.com/?linkid=9805135) (wpis na blogu).

> [!div class="step-by-step"]
> [Poprzednie](excluding-files-and-folders-from-deployment.md)
> [dalej](running-windows-powershell-scripts-from-msbuild-project-files.md)
