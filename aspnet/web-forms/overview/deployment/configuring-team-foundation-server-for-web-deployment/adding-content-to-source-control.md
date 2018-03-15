---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Dodawanie zawartości do kontroli źródła | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano sposób dodawania zawartości do kontroli źródła w Team Foundation Server (TFS) 2010. Opisuje sposób dodawania rozwiązania i projekty do projek zespołu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="adding-content-to-source-control"></a>Dodawanie zawartości do kontroli źródła
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób dodawania zawartości do kontroli źródła w Team Foundation Server (TFS) 2010. Opisuje sposób dodawania rozwiązania i projekty do projektu zespołowego w programie TFS, oraz wyjaśniono sposób dodawania zależności zewnętrzne, takie jak struktury lub zestawów do kontroli źródła.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

## <a name="task-overview"></a>Omówienie zadań

W większości przypadków można dodawać zawartości do kontroli źródła należy każdy członek zespołu deweloperów. Aby dodać rozwiązanie do kontroli źródła w programie TFS, musisz wykonać następujące ogólne kroki:

- Połącz z projektem zespołowym.
- Struktura folderów projektu zespołowego na serwerze są mapowane na strukturę folderów na komputerze lokalnym.
- Dodaj rozwiązanie i jego zawartość do kontroli źródła.
- Dodaj wszelkie zależności zewnętrzne do kontroli źródła.

W tym temacie opisano sposób wykonywania tych procedur.

Zadania i wskazówki, w tym temacie założono, zostało już utworzone nowych projektów zespołowych TFS, aby zarządzać zawartością. Aby uzyskać więcej informacji na temat tworzenia nowego projektu zespołowego, zobacz [Tworzenie nowego projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

W większości przypadków należy można dodawać i modyfikować zawartość w projektach zespołowych określonych każdego członka zespołu deweloperów.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Łączenie z projektem zespołowym i Tworzenie mapowania folderu

Przed dodaniem żadnej zawartości do kontroli źródła, należy połączyć z projektem zespołowym i utworzyć mapowanie między struktury folderów na serwerze i system plików na komputerze lokalnym.

**Połącz z projektem zespołowym i mapowanie ścieżki lokalnej**

1. Na stacji roboczej dewelopera Otwórz program Visual Studio 2010.
2. W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.

    > [!NOTE]
    > Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki od 3 do 6.
3. W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.
4. W **Dodaj lub usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.
5. W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia TFS, a następnie kliknij przycisk **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. W **Dodaj lub usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.
7. W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia TFS ma się połączyć, wybierz zespół projektu kolekcji, które chcesz dodać do projektu zespołowego, a następnie kliknij przycisk **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. W **Team Explorer** , rozwiń węzeł projektu zespołowego, a następnie kliknij dwukrotnie ikonę **kontroli źródła**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Na **Eksploratora kontroli źródła** , kliknij pozycję **nie jest zamapowany**.

    ![](adding-content-to-source-control/_static/image4.png)
10. W **mapy** okna dialogowego, **folder lokalny** polu, przejdź do (lub Utwórz) folder lokalny do działania jako folder główny dla projektu zespołowego, a następnie kliknij przycisk **mapy**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Po wyświetleniu monitu, aby pobrać pliki źródłowe, kliknij przycisk **tak**.

    ![](adding-content-to-source-control/_static/image6.png)

W tym momencie zamapowaniu folder po stronie serwera dla projektu zespołowego do folderu lokalnego na stacji roboczej developer. Możesz również pobrano istniejącą zawartość z projektu zespołowego do struktury folderu lokalnego. Można teraz uruchomić dodać własną zawartość do kontroli źródła.

## <a name="add-projects-and-solutions-to-source-control"></a>Dodaj projekty i rozwiązania do kontroli źródła

Aby dodać projekty i rozwiązania do kontroli źródła, należy najpierw przenieść je do zamapowany folder dla projektu zespołowego na komputerze lokalnym. Następnie można sprawdzić w zawartości do synchronizowania z dodatków na serwerze.

**Aby dodać projekty do kontroli źródła**

1. Na stacji roboczej developer Przenieś swoje projekty i rozwiązania do odpowiedniej lokalizacji w strukturze zamapowany folder dla projektu zespołowego.

    > [!NOTE]
    > Wiele organizacji ma preferowana metoda sposób organizowania projektów i rozwiązań w kontroli źródła. Aby uzyskać instrukcje na temat struktury folderów, zobacz [jak: struktura Your źródła formantu folderów na serwerze Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Otwórz rozwiązanie w Visual Studio 2010.
3. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Dodaj rozwiązanie do kontroli źródła**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > W niektórych przypadkach, w zależności od tego, jak organizacji lubi struktury zawartości w programie TFS może być konieczne dodanie projekty do kontroli źródła osobno w celu zapewnienia bardziej precyzyjną kontrolę nad organizowania kodu źródłowego.
4. Sprawdź, czy **Eksploratora kontroli źródła** kartę Wyświetla zawartość zostały dodane w ramach struktury folderów serwera dla projektu zespołowego.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **Eksploratora kontroli źródła** kartę Wyświetla zawartość z żadne dodatkowe monitowania, ponieważ dodana rozwiązania do zamapowany folder w lokalnym systemie plików. Jeśli rozwiązanie znajduje się w niezmapowanej lokalizacji, użytkownik jest proszony o Określ lokalizację folderu w TFS i lokalnego systemu plików.
5. Na **Eksploratora kontroli źródła** karcie **folderów** okienku kliknij prawym przyciskiem myszy projektu zespołowego (na przykład **ContactManager**), a następnie kliknij przycisk **Zaewidencjonuj Oczekujące zmiany**.
6. W **Zaewidencjonuj — pliki źródłowe** okno dialogowe, wprowadź komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.

    ![](adding-content-to-source-control/_static/image9.png)

W tym momencie dodano rozwiązania do kontroli źródła w programie TFS.

## <a name="add-external-dependencies-to-source-control"></a>Dodaj zależności zewnętrzne do kontroli źródła

Po dodaniu projektu lub rozwiązania do kontroli źródła, wszystkie pliki i foldery w projekcie lub rozwiązaniu również zostaną dodane. Jednak w partii przypadków, projekty i rozwiązania również polegać na zależności zewnętrzne, takie jak lokalne zestawy, do poprawnego działania. Konieczne jest dodanie tych zasobów, do kontroli źródła, aby umożliwić Team Build i innych członków zespołu deweloperów pomyślnie kompilacji kodu.

Na przykład struktura folderów dla kontaktów Menedżerze przykładowe rozwiązanie zawiera folder o nazwie pakietów. Zawiera zestaw i różnych zasobów pomocnicze dla ADO.NET Entity Framework 4.1. Folder pakietów nie jest częścią rozwiązania kontaktów Menedżerze, ale rozwiązanie nie zostanie pomyślnie kompilacji bez niego. Aby włączyć Team Build w celu skompilowania rozwiązania, należy dodać folder pakietów do kontroli źródła.

> [!NOTE]
> Włączenie folderu pakietów jest typowe dla co się stanie po dodaniu Entity Framework lub podobnych zasobów, do rozwiązania przy użyciu rozszerzenia NuGet dla programu Visual Studio 2010.


**Aby dodać zawartość-projekt do kontroli źródła**

1. Upewnij się, że elementy, które chcesz dodać (na przykład folder pakietów) są w odpowiedniej lokalizacji folderu mapowane w lokalnym systemie plików.
2. W programie Visual Studio 2010 w **Team Explorer** , rozwiń węzeł projektu zespołowego, a następnie kliknij dwukrotnie ikonę **kontroli źródła**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Na **Eksploratora kontroli źródła** karcie **folderów** okienku, wybierz folder, który zawiera element lub elementy możesz mają zostać dodane.
4. Kliknij przycisk **Dodaj elementy do folderu** przycisku.

    ![](adding-content-to-source-control/_static/image11.png)
5. W **Dodaj do kontroli źródła** okno dialogowe Wybierz folder lub elementy, które chcesz dodać, a następnie kliknij przycisk **dalej**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Na **wykluczone elementy** , a następnie wybierz wszystkie wymagane elementy, które zostały automatycznie wyłączone (na przykład zestawy), a następnie kliknij przycisk **obejmują elementy**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Na **elementy do dodania** Sprawdź, czy są wyświetlane wszystkie pliki, które chcesz dołączyć, a następnie kliknij pozycję **Zakończ**.

    ![](adding-content-to-source-control/_static/image14.png)
8. W **Eksploratora kontroli źródła** okna, kliknij przycisk **Zaewidencjonuj** przycisku.

    ![](adding-content-to-source-control/_static/image15.png)
9. W **Zaewidencjonuj — pliki źródłowe** okno dialogowe, wprowadź komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.

W tym momencie dodano zależności zewnętrznych dla rozwiązania do kontroli źródła.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób Połącz z projektem zespołowym, mapy struktury folderów i Dodaj do kontroli źródła zawartości. Aby uzyskać więcej informacji na temat pracy z elementami pod kontrolą źródła, zobacz [przy użyciu kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).

Następnym temacie [Konfigurowanie kompilacji serwera TFS dla Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), w tym artykule opisano sposób przygotowania serwera TFS Team Build do tworzenia i wdrażania rozwiązania.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe informacje na temat pracy z kontroli źródła w programie TFS, zobacz [przy użyciu kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).

>[!div class="step-by-step"]
[Poprzednie](creating-a-team-project-in-tfs.md)
[dalej](configuring-a-tfs-build-server-for-web-deployment.md)
