---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Wdrażanie pobierania aplikacji sieci Web w trybie Offline z sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób wykonania aplikacji sieci web w trybie offline na czas trwania automatycznego wdrażania przy użyciu alert Wdr sieci Web usług Internet Information Services (IIS)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 511201dc5646340b21023430fa319417f2b53ae2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Wdrażanie pobierania aplikacji sieci Web w trybie Offline z sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób wykonania aplikacji sieci web w trybie offline na czas trwania automatycznego wdrażania za pomocą narzędzia wdrażania usług Internet Information Services (IIS) w sieci Web (Web Deploy). Użytkownicy, którzy przejdź do aplikacji sieci web są przekierowywani do *aplikacji\_offline.htm* pliku do czasu ukończenia wdrożenia.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

W wiele scenariuszy należy przełączyć do aplikacji sieci web do trybu offline podczas wprowadzania zmian do powiązanych składników, takich jak bazy danych lub usługi sieci web. Zazwyczaj w usługach IIS i platformy ASP.NET, można to zrobić, umieszczając plik o nazwie *aplikacji\_offline.htm* w folderze głównym usług IIS witryna sieci Web lub aplikacji sieci web. *Aplikacji\_offline.htm* plików to standardowy plik HTML i zazwyczaj zawiera proste komunikat udzielanie porad użytkownika, że witryna jest tymczasowo niedostępna z powodu konserwacji. Gdy *aplikacji\_offline.htm* plik istnieje w folderze głównym witryny sieci Web, usługi IIS przekierują automatycznie żądań do pliku. Po zakończeniu wprowadzanie aktualizacji, należy usunąć *aplikacji\_offline.htm* plików i witryny sieci Web wznawia obsługiwać żądania w zwykły sposób.

Korzystając z narzędzia Web Deploy do przeprowadzania wdrożeń automatycznych lub pojedynczy krok w środowisku docelowym, można dołączyć do nich Dodawanie i usuwanie *aplikacji\_offline.htm* pliku w procesie wdrażania. Aby to zrobić, należy wykonać te zadania wysokiego poziomu:

- W pliku projektu Microsoft kompilacji Engine (MSBuild) służące do sterowania procesu wdrażania, Utwórz obiekt docelowy programu MSBuild, który kopiuje *aplikacji\_offline.htm* pliku na serwerze docelowym przed żadnych zadań wdrażania Rozpocznij.
- Dodaj inny element docelowy programu MSBuild, która usuwa *aplikacji\_offline.htm* pliku z serwera docelowego po zakończeniu wszystkich zadań wdrażania.
- W projekcie aplikacji sieci web, należy utworzyć *. wpp.targets* pliku, który zapewnia, że *aplikacji\_offline.htm* plik zostanie dodany do pakietu wdrożeniowego, gdy narzędzie Web Deploy jest wywoływany.

W tym temacie opisano sposób wykonywania tych procedur. Zadania i wskazówki, w tym temacie założono, zostały już utworzone rozwiązania, który zawiera co najmniej jeden projekt aplikacji sieci web i używać pliku projektu niestandardowych kontrolować proces wdrażania, zgodnie z opisem w [Web Deployment w Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternatywnie można użyć [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie, które należy wykonać w przykładach w temacie.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Dodawanie aplikacji\_plików w trybie Offline do projektu aplikacji sieci Web

Pierwszym zadaniem, które należy wykonać, jest dodanie *aplikacji\_w trybie offline* plik do projektu aplikacji sieci web:

- Aby zapobiec zakłócać proces tworzenia pliku (nie chcesz aplikacja ma być trwale w trybie offline), należy wywołać go coś innego niż *aplikacji\_offline.htm*. Można na przykład nazwę pliku *aplikacji\_w trybie offline template.htm*.
- Aby zapobiec wdrażane jako plik — jest, należy ustawić akcji kompilacji **Brak**.

**Aby dodać aplikację\_plików w trybie offline do projektu aplikacji sieci web**

1. Otwórz rozwiązanie w Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, kliknij prawym przyciskiem myszy projekt aplikacji sieci web, wskaż pozycję **Dodaj**, a następnie kliknij przycisk **nowy element**.
3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **strony HTML**.
4. W **nazwa** wpisz **aplikacji\_w trybie offline template.htm**, a następnie kliknij przycisk **Dodaj**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Dodaj prosty kod HTML, aby zawiadomić użytkowników o tym, że aplikacja jest niedostępna, a następnie zapisz plik. Nie ma żadnych tagów po stronie serwera (na przykład wszystkie tagi, które są poprzedzane prefiksem "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy nowy plik, a następnie kliknij przycisk **właściwości**.
7. W **właściwości** okna w **Akcja kompilacji** wierszu, wybierz opcję **Brak**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Wdrażanie i usuwanie aplikacji\_plików w trybie Offline

Następnym krokiem jest do modyfikowania logiki wdrożenia, aby skopiować plik do serwera docelowego na początku procesu wdrażania i usunąć go na końcu.

> [!NOTE]
> Następna procedura przyjęto założenie, że używasz niestandardowego pliku projektu MSBuild do kontrolowania procesu wdrażania zgodnie z opisem w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Jeśli wdrażasz bezpośrednio z programu Visual Studio, należy użyć innej metody. Sayed Ibrahim Hashimi opisuje jedno takie podejście w [jak wykonać Twojej aplikacji w tryb Offline podczas publikowania w sieci Web](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Aby wdrożyć *aplikacji\_w trybie offline* pliku do docelowej witryny usług IIS, należy wywołać przy użyciu MSDeploy.exe [narzędzia Web Deploy **contentPath** dostawcy](https://technet.microsoft.com/library/dd569034(WS.10).aspx). **ContentPath** dostawca obsługuje zarówno ścieżki katalogu fizycznego i ścieżki witryny sieci Web lub aplikacji usług IIS, co staje się on idealnym wyborem w przypadku synchronizacji plików między folderu projektu programu Visual Studio i aplikacji sieci web usług IIS. Do wdrożenia pliku, polecenia MSDeploy powinien wyglądać następująco:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Aby usunąć plik z lokacji docelowej po zakończeniu procesu wdrażania, polecenia MSDeploy powinien wyglądać następująco:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Aby zautomatyzować tych poleceń jako część procesu kompilacji i wdrożenia, należy zintegrować niestandardowego pliku projektu MSBuild. W następnej procedurze opisano jak to zrobić.

**Do wdrożenia i Usuń aplikację\_plików w trybie offline**

1. W programie Visual Studio 2010 należy otworzyć pliku projektu MSBuild, która kontroluje procesu wdrażania. W [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie to *Publish.proj* pliku.
2. W folderze głównym **projektu** elementu, Utwórz nową **PropertyGroup** element do przechowywania zmienne *aplikacji\_w trybie offline* wdrożenia:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot** właściwość jest zdefiniowana w innym miejscu w *Publish.proj* pliku. Wskazuje on lokalizację folderu głównego dla zawartości źródłowej względem bieżącej ścieżki&#x2014;innymi słowy, względną wobec lokalizacji *Publish.proj* pliku.
4. **ContentPath** dostawcy nie akceptuje względne ścieżki do pliku, więc należy uzyskać ścieżka bezwzględna do pliku źródłowego, zanim będzie można go wdrożyć. Można użyć [converttoabsolutepath —](https://msdn.microsoft.com/library/bb882668.aspx) zadań w tym celu.
5. Dodaj nową **docelowej** elementu o nazwie **GetAppOfflineAbsolutePath**. W obrębie tego celu użyć **converttoabsolutepath —** zadanie w celu uzyskania ścieżki bezwzględnej do *aplikacji\_w trybie offline szablonu* pliku w folderze projektu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Ten element docelowy ma ścieżkę względną do *aplikacji\_w trybie offline szablonu* pliku w folderze projektu i zapisuje go na nową właściwość jako ścieżka bezwzględna do pliku. **BeforeTargets** atrybut określa, że ten element docelowy do uruchomienia przed **DeployAppOffline** docelowych, które zostaną utworzone w następnym kroku.
7. Dodaj nowy cel o nazwie **DeployAppOffline**. W obrębie tego celu, wywołaj polecenie MSDeploy.exe, która wdraża Twojej *aplikacji\_w trybie offline* pliku do docelowego serwera sieci web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. W tym przykładzie **ContactManagerIisPath** właściwość jest zdefiniowana w innym miejscu w pliku projektu. Jest to po prostu ścieżka aplikacji IIS, w formie *[nazwa witryny sieci Web usług IIS] / [Nazwa aplikacji]*. W tym warunku w elemencie docelowym umożliwia użytkownikom przełącznika *aplikacji\_w trybie offline* wdrożenia lub wyłączyć, zmieniając wartość właściwości lub podanie parametru wiersza polecenia.
9. Dodaj nowy cel o nazwie **DeleteAppOffline**. W obrębie tego celu, wywołaj polecenie MSDeploy.exe, która usuwa z *aplikacji\_w trybie offline* plik docelowy serwer sieci web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Ostatnim zadaniem jest do wywołania te nowe elementy docelowe w odpowiednich miejscach podczas wykonywania pliku projektu. Można to zrobić na różne sposoby. Na przykład w *Publish.proj* pliku **FullPublishDependsOn** właściwość określa listę elementów docelowych, które muszą być wykonywane w kolejności, kiedy **FullPublish** domyślne docelowy jest wywoływany.
11. Modyfikowanie pliku projektu MSBuild do wywołania **DeployAppOffline** i **DeleteAppOffline** elementy docelowe w odpowiednich miejscach w procesie publikowania.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Po uruchomieniu niestandardowego pliku projektu MSBuild *aplikacji\_w trybie offline* plik zostanie wdrożony na serwerze natychmiast po pomyślnej kompilacji. Następnie będzie można usunąć z serwera po zakończeniu wszystkich zadań wdrażania.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Dodawanie aplikacji\_plików w trybie Offline do pakietów wdrożeniowych

W zależności od konfiguracji wdrożenia, istniejących zawartości w miejscu docelowym usług IIS aplikacji sieci web&#x2014;jak *aplikacji\_offline.htm* pliku&#x2014;mogą zostać usunięte automatycznie, podczas wdrażania sieci web pakiet do tego miejsca docelowego. Aby upewnić się, że *aplikacji\_offline.htm* plik pozostaje w miejscu na czas trwania rozmieszczania, należy także uwzględnić plik w obrębie samego pakietu wdrożeniowego sieci web wdrażanie pliku bezpośrednio na początku proces wdrażania.

- Jeśli po wykonaniu poprzednich zadań w tym temacie, będzie dodano *aplikacji\_offline.htm* plik do projektu aplikacji sieci web w innej nazwy pliku (użyliśmy *aplikacji\_ w trybie offline template.htm*) i będzie została wybrana akcja kompilacji **Brak**. Te zmiany są niezbędne w celu zapobieżenia pliku z zakłócać programowanie i debugowanie. W związku z tym musisz dostosować proces tworzenia pakietu, aby upewnić się, że *aplikacji\_offline.htm* plik znajduje się w pakiecie wdrożeniowym sieci web.

Web potok publikowania (WPP) używa listy elementów o nazwie **FilesForPackagingFromProject** do tworzenia listy plików, które powinny być uwzględnione w pakiecie wdrożeniowym sieci web. Zawartość pakietów sieci web można dostosować, dodając własne elementy do tej listy. Aby to zrobić, należy wykonać następujące ogólne kroki:

1. Utwórz plik niestandardowe projektu o nazwie *.wpp.targets [Nazwa projektu]* w tym samym folderze co plik projektu.

    > [!NOTE]
    > *. Wpp.targets* plik ma znaleźć się w tym samym folderze co plik projektu aplikacji sieci web&#x2014;na przykład *ContactManager.Mvc.csproj*&#x2014;, a nie w tym samym folderze co niestandardowe pliki projektu, umożliwiające sterowanie procesem kompilacji i wdrażania.
2. W *. wpp.targets* pliku, Utwórz nowy obiekt docelowy MSBuild, która wykonuje *przed* **CopyAllFilesToSingleFolderForPackage** docelowej. Jest to element docelowy WPP, który umożliwia tworzenie listy rzeczy do uwzględnienia w pakiecie.
3. Tworzenie nowego docelowego **ItemGroup** elementu.
4. W **ItemGroup** elementu, Dodaj **FilesForPackagingFromProject** elementu i określ *aplikacji\_offline.htm* pliku.

*. Wpp.targets* pliku powinna wyglądać następująco:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Poniżej przedstawiono najważniejsze notatki w tym przykładzie:

- **BeforeTargets** atrybutu wstawia ten element docelowy do WPP, określając, który ma być wykonany bezpośrednio przed **CopyAllFilesToSingleFolderForPackage** docelowej.
- **FilesForPackagingFromProject** elementu używa **DestinationRelativePath** wartości metadanych można zmienić nazwy pliku z *aplikacji\_w trybie offline template.htm* Aby *aplikacji\_offline.htm* jest dodawany do listy.

Następnej procedury przedstawiono sposób dodawania to *. wpp.targets* plik do projektu aplikacji sieci web.

**Aby dodać. wpp.targets pliku do pakietu wdrożeniowego sieci web**

1. Otwórz rozwiązanie w Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, kliknij prawym przyciskiem myszy węzeł projektu aplikacji sieci web (na przykład **ContactManager.Mvc**), wskaż polecenie **Dodaj**, a następnie kliknij przycisk **Nowy element**.
3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **pliku XML** szablonu.
4. W **nazwa** wpisz *[Nazwa projektu] ***.wpp.targets** (na przykład **ContactManager.Mvc.wpp.targets**), a następnie kliknij przycisk **Dodaj**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Jeśli dodajesz nowy element do węzła głównego projektu, plik jest tworzony w tym samym folderze co plik projektu. Można to sprawdzić, otwierając folder w Eksploratorze Windows.
5. W pliku należy dodać znaczników MSBuild opisanych powyżej.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Zapisz i Zamknij *.wpp.targets [Nazwa projektu]* pliku.

Przy kolejnym uruchomieniu pakiet i kompilacji projektu aplikacji sieci web, WPP automatycznie wykryje *. wpp.targets* pliku. *Aplikacji\_w trybie offline template.htm* plików mają być uwzględnieni w wynikowej pakietu wdrożeniowego sieci web jako *aplikacji\_offline.htm*.

> [!NOTE]
> W przypadku niepowodzenia wdrożenia *aplikacji\_offline.htm* plik pozostanie w miejscu i aplikacji pozostanie w trybie offline. Zazwyczaj jest to zachowanie. Aby przenieść aplikację do trybu online, możesz usunąć *aplikacji\_offline.htm* pliku z serwera sieci web. Alternatywnie, jeśli Popraw błędy i uruchom pomyślne wdrożenie *aplikacji\_offline.htm* plik zostanie usunięty.


## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób wykonania aplikacji sieci web w trybie offline na czas trwania rozmieszczania, poprzez publikowanie *aplikacji\_offline.htm* pliku na serwerze docelowym na początku procesu wdrażania i usunięcie go w Zakończenie. Również objęte sposobu uwzględniania *aplikacji\_offline.htm* plików w pakiecie wdrożeniowym sieci web.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji dotyczących tworzenia pakietów i proces wdrażania, zobacz [budynku i projekty aplikacji sieci Web pakowania](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [konfigurowania parametrów wdrażania pakietu sieci Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), i [ Wdrażanie pakietów sieci Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Jeśli publikowanie aplikacji sieci web bezpośrednio z programu Visual Studio, a nie za pomocą podejście pliku niestandardowe projektu MSBuild opisane w tych samouczkach, konieczne będzie użycie nieco inny sposób w celu przejęcia aplikacji w trybie offline podczas publikowania proces. Aby uzyskać więcej informacji, zobacz [sposób wykonania aplikacji sieci web w trybie offline podczas publikowania](https://go.microsoft.com/?linkid=9805135) (wpis w blogu).

> [!div class="step-by-step"]
> [Poprzednie](excluding-files-and-folders-from-deployment.md)
> [dalej](running-windows-powershell-scripts-from-msbuild-project-files.md)
