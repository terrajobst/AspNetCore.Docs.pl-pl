---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Dodawanie zawartości do kontroli źródła | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób dodawania zawartości do kontroli źródła w Team Foundation Server (TFS) 2010. Opisano w nim sposób dodawania rozwiązań i projektów do projek zespołu...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838374"
---
<a name="adding-content-to-source-control"></a>Dodawanie zawartości do kontroli źródła
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób dodawania zawartości do kontroli źródła w Team Foundation Server (TFS) 2010. Opisano w nim sposób dodawania rozwiązań i projektów do projektu zespołowego w programie TFS, i wyjaśnia, jak dodać zależności zewnętrznych, takich jak platform ani zestawów do kontroli źródła.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

## <a name="task-overview"></a>Omówienie zadań

W większości przypadków można dodać zawartości do kontroli źródła należy każdy członek zespołu deweloperów. Aby dodać rozwiązanie do kontroli źródła w programie TFS, należy wykonać następujące czynności wysokiego poziomu:

- Połącz z projektem zespołowym.
- Struktury folderów projektu zespołowego na serwerze są mapowane na strukturę folderów na komputerze lokalnym.
- Dodaj rozwiązanie i jego zawartość do kontroli źródła.
- Dodawanie wszelkich zależności zewnętrznych do kontroli źródła.

W tym temacie pokazują sposób wykonania tych procedur.

Zadania i wskazówki, w tym temacie założono, że utworzono już nowego projektu zespołowego TFS można zarządzać zawartością. Aby uzyskać więcej informacji na temat tworzenia nowego projektu zespołowego, zobacz [tworzenia projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Osób wykonujących te procedury?

W większości przypadków należy dodać i modyfikować zawartości w ramach projektów zespołowych określonych każdy członek zespołu deweloperów.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Łączenie z projektem zespołowym i Tworzenie mapowania folderu

Przed dodaniem żadnej zawartości do kontroli źródła, należy połączyć z projektem zespołowym i utworzyć mapowania między struktury folderów na serwerze i system plików na komputerze lokalnym.

**Aby nawiązać połączenie z projektem zespołowym i Mapuj ścieżkę lokalną**

1. Na stacji roboczej dewelopera Otwórz program Visual Studio 2010.
2. W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.

    > [!NOTE]
    > Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki od 3 do 6.
3. W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.
4. W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.
5. W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.
7. W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia programu TFS, aby nawiązać połączenie, wybierz zespół projektu, kolekcji, wybierz projekt zespołowy, do którego chcesz dodać do, a następnie kliknij **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. W **Team Explorer** , rozwiń węzeł projektu zespołowego, a następnie kliknij dwukrotnie ikonę **kontroli źródła**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Na **Eksploratora kontroli źródła** kliknij pozycję **niezamapowane**.

    ![](adding-content-to-source-control/_static/image4.png)
10. W **mapy** dialogowym **lokalny folder** pola, przejdź do (lub Utwórz) folder lokalny do działania jako folder główny projektu zespołowego, a następnie kliknij przycisk **mapy**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Po wyświetleniu monitu, aby pobrać pliki źródłowe, kliknij przycisk **tak**.

    ![](adding-content-to-source-control/_static/image6.png)

W tym momencie zmapowano folderu po stronie serwera dla projektu zespołowego do folderu lokalnego na stacji roboczej dewelopera. Również pobraniu istniejącą zawartość z projektu zespołowego na strukturze folderów lokalnych. Możesz teraz rozpocząć dodawanie własnych zawartości do kontroli źródła.

## <a name="add-projects-and-solutions-to-source-control"></a>Dodaj projekty i rozwiązania do kontroli źródła

Aby dodać projekty i rozwiązania do kontroli źródła, należy najpierw przenieść je do zamapowany folder dla projektu zespołowego na komputerze lokalnym. Następnie można sprawdzić w zawartości do synchronizowania swoje dodatki z serwerem.

**Aby dodać projekty do kontroli źródła**

1. Na stacji roboczej dewelopera należy przenieść swoje projekty i rozwiązania do odpowiedniej lokalizacji w ramach struktury zamapowany folder dla projektu zespołowego.

    > [!NOTE]
    > Wiele organizacji ma preferowanym sposobem sposób organizowania projektów i rozwiązań w kontroli źródła. Aby uzyskać wskazówki dotyczące struktury folderów, zobacz [How to: struktura Your źródłowych folderów kontroli w Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Otwórz rozwiązanie w programie Visual Studio 2010.
3. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Dodaj rozwiązanie do kontroli źródła**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > W niektórych przypadkach, w zależności od tego, jak Twoja organizacja lubi struktury zawartości w programie TFS może być konieczne dodanie projekty do kontroli źródła osobno, aby zapewnić bardziej precyzyjną kontrolę nad sposób organizowania kodu źródłowego.
4. Upewnij się, że **Eksploratora kontroli źródła** karty wyświetla zawartość zostały dodane w ramach struktury folderów serwera dla projektu zespołowego.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **Eksploratora kontroli źródła** karty wyświetla zawartość przy użyciu żadne dalsze monity nie ponieważ rozwiązanie jest dodawane do zamapowany folder w lokalnym systemie plików. Jeśli Twoje rozwiązanie w niezmapowanej lokalizacji, użytkownik jest proszony o Określ lokalizacje folderów w TFS i lokalnego systemu plików.
5. Na **Eksploratora kontroli źródła** na karcie **folderów** okienku kliknij prawym przyciskiem myszy projektu zespołowego (na przykład **ContactManager**), a następnie kliknij przycisk **Zaewidencjonuj Oczekujące zmiany**.
6. W **Zaewidencjonuj — pliki źródłowe** okno dialogowe, wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.

    ![](adding-content-to-source-control/_static/image9.png)

W tym momencie dodano rozwiązania do kontroli źródła w programie TFS.

## <a name="add-external-dependencies-to-source-control"></a>Dodaj zależności zewnętrznych do kontroli źródła

Po dodaniu projektu lub rozwiązania do kontroli źródła, wszystkie pliki i foldery znajdujące się w projekcie lub rozwiązaniu także zostaną dodane. Jednak w wielu przypadkach, projekty i rozwiązania polegają również na zależności zewnętrzne, np. lokalnych zestawów, aby działać prawidłowo. Musisz dodać takich zasobów, do kontroli źródła, aby umożliwić kompilacji zespołowej i inni członkowie zespołu deweloperów pomyślnie kompilacji kodu.

Na przykład struktura folderów dla Contact Manager przykładowe rozwiązanie zawiera folder o nazwie pakietów. Zawiera zestaw i różne zasoby pomocnicze dla ADO.NET Entity Framework 4.1. Packages folder nie jest częścią rozwiązania Contact Manager, ale to rozwiązanie nie zostanie pomyślnie skompilowany bez niego. Aby włączyć Team Build skompilować rozwiązanie, należy dodać packages folder do kontroli źródła.

> [!NOTE]
> Włączenie packages folder jest typowy dla co się stanie po dodaniu Entity Framework lub podobne zasoby do rozwiązania przy użyciu rozszerzenia NuGet dla programu Visual Studio 2010.


**Aby dodać zawartość — projekt do kontroli źródła**

1. Upewnij się, że elementy, które chcesz dodać (np. packages folder) znajdują się w odpowiedniej lokalizacji w ramach zamapowany folder w lokalnym systemie plików.
2. W programie Visual Studio 2010 w **Team Explorer** , rozwiń węzeł projektu zespołowego, a następnie kliknij dwukrotnie ikonę **kontroli źródła**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Na **Eksploratora kontroli źródła** na karcie **folderów** okienku, wybierz folder, który zawiera element lub elementy użytkownik ma zostać dodana.
4. Kliknij przycisk **Dodaj elementy do folderu** przycisku.

    ![](adding-content-to-source-control/_static/image11.png)
5. W **Dodaj do kontroli źródła** okna dialogowego Wybierz folder lub elementy, które chcesz dodać, a następnie kliknij przycisk **dalej**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Na **wykluczone elementy** , a następnie wybierz wszystkie wymagane elementy, które zostały automatycznie wyłączone (na przykład zestawy), a następnie kliknij przycisk **Uwzględnij elementy**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Na **elementy do dodania** kartę, upewnij się, że są wyświetlane wszystkie pliki, które chcesz dołączyć, a następnie kliknij **Zakończ**.

    ![](adding-content-to-source-control/_static/image14.png)
8. W **Eksploratora kontroli źródła** okna, kliknij przycisk **Zaewidencjonuj** przycisku.

    ![](adding-content-to-source-control/_static/image15.png)
9. W **Zaewidencjonuj — pliki źródłowe** okno dialogowe, wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.

W tym momencie dodano zależności zewnętrznych dla Twojego rozwiązania do kontroli źródła.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano, jak nawiązać połączenie z projektem zespołowym, mapowanie strukturę folderów i dodawanie zawartości do kontroli źródła. Aby uzyskać więcej informacji na temat pracy z elementami pod kontrolą źródła, zobacz [przy użyciu systemu kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).

Następny temat [Konfigurowanie serwera kompilacji TFS dla wdrażania w Internecie](configuring-a-tfs-build-server-for-web-deployment.md), w tym artykule opisano sposób przygotowania serwera TFS Team Build do kompilowania i wdrażania rozwiązania.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe informacje na temat pracy z kontrolą źródła w programie TFS, zobacz [przy użyciu systemu kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-team-project-in-tfs.md)
> [dalej](configuring-a-tfs-build-server-for-web-deployment.md)
