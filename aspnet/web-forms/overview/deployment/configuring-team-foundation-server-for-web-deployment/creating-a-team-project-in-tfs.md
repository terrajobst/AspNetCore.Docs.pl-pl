---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Tworzenie projektu zespołowego w TFS | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 9218a22ff221dc7067662c58ccd3e758fca493b7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755952"
---
<a name="creating-a-team-project-in-tfs"></a>Tworzenie projektu zespołowego w TFS
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

## <a name="task-overview"></a>Omówienie zadań

Aby aprowizować i użyć nowego projektu zespołowego w programie TFS, należy wykonać następujące czynności wysokiego poziomu:

- Przyznaj uprawnienia do użytkownika, który spowoduje utworzenie nowego projektu zespołowego.
- Utwórz projekt zespołowy.
- Przyznaj uprawnienia do członków zespołu, którzy będą działać w projekcie.
- Zaewidencjonuj odpowiednią zawartość.

W tym temacie opisano, jak do wykonania tych procedur, a następnie zidentyfikuje, użytkownikami i rolami zadania, które mogą być odpowiedzialne za każdej procedury. Należy pamiętać, że w zależności od struktury organizacji, każdy z tych zadań może być odpowiedzialność innej osobie.

Zadania i wskazówki, w tym temacie założono, został zainstalowany i skonfigurowany TFS, i czy utworzono kolekcję projektów zespołowych w ramach procesu konfiguracji. Aby uzyskać więcej informacji na temat tych założeń i bardziej ogólne informacje od scenariusza, zobacz [skonfigurować serwer kompilacji TFS dla wdrażania w Internecie](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Udziel uprawnień twórcę projektu zespołowego

W celu utworzenia nowego projektu zespołowego, potrzebne są następujące uprawnienia:

- Konieczne jest posiadanie **tworzyć nowe projekty** uprawnień w warstwie aplikacji TFS. Uprawnienie to można przyznać zwykle przez dodanie użytkowników do **Administratorzy kolekcji projektów** grupy serwera TFS. **Administratorzy Team Foundation** globalna grupa zawiera również to uprawnienie.
- Musi mieć uprawnienia do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, która odpowiada kolekcji projektów zespołowych TFS. Uprawnienie to można przyznać zwykle przez dodanie użytkownika do grupy programu SharePoint przy użyciu **Pełna kontrola** zbioru witryn prawa w programie SharePoint.
- Jeśli używasz funkcji programu SQL Server Reporting Services, musisz być członkiem **Menedżer zawartości Team Foundation** roli w usługach Reporting Services.

### <a name="who-performs-these-procedures"></a>Osób wykonujących te procedury?

Zazwyczaj osoba lub grupa, która administruje wdrożenia serwera TFS oraz wykonuje te procedury.

Ponieważ jest to wysoko uprzywilejowane zestaw uprawnień, nowych projektów zespołowych są zwykle tworzone przez mały podzbiór użytkowników odpowiedzialnych za administrowanie wdrożenia programu TFS. Deweloperzy nie zwykle uzyska uprawnienia wymagane do utworzenia nowych projektów zespołowych.

### <a name="grant-permissions-in-tfs"></a>Udziel uprawnień w programie TFS

Jeśli chcesz umożliwić użytkownikowi tworzenie nowych projektów zespołowych, jest dodanie użytkownika do pierwszego zadania wysokiego poziomu **Administratorzy kolekcji projektów** grupy dla kolekcji projektów zespołowych.

**Aby dodać użytkownika do grupy Administratorzy kolekcji projektów**

1. Na serwerze TFS na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Team Foundation Server 2010**, a następnie kliknij przycisk **Team Foundation Konsola administracyjna**.
2. W widoku drzewa nawigacji rozwiń węzeł **warstwy aplikacji**, a następnie kliknij przycisk **kolekcji projektów zespołowych**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. W **kolekcji projektów zespołowych** wybierz opcję zespołu projektu zbierania, którymi chcesz zarządzać.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Na **ogólne** kliknij pozycję **członkostwa w grupie**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. W **grup globalnych** okno dialogowe, wybierz opcję **Administratorzy kolekcji projektów** grupy, a następnie kliknij przycisk **właściwości**.
6. W **właściwości grupy programu Team Foundation Server** okno dialogowe, wybierz opcję **Windows użytkownik lub grupa**, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, aby móc tworzyć nowych projektów zespołowych kliknij **Sprawdź nazwy**, a następnie kliknij przycisk **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. W **właściwości grupy programu Team Foundation Server** okno dialogowe, kliknij przycisk **OK**.
9. W **grup globalnych** okno dialogowe, kliknij przycisk **Zamknij**.

### <a name="grant-permissions-in-sharepoint-services"></a>Udziel uprawnień w programie SharePoint Services

Następnie należy przyznać uprawnienia użytkownika do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, który odpowiada kolekcji projektów zespołowych TFS.

**Aby przyznać uprawnienia Pełna kontrola zbioru witryn programu SharePoint**

1. W w konsoli administracyjnej Team Foundation Server na **kolekcji projektów zespołowych** wybierz kolekcji projektów zespołowych, którymi chcesz zarządzać.
2. Na **witryny programu SharePoint** kartę, zanotuj wartość ustawienia **bieżąca domyślna lokalizacja witryny** adresu URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Otwórz program Internet Explorer, a następnie przejdź do adresu URL zanotowaną w kroku 2.

    > [!NOTE]
    > Jeśli użytkownik są nie zalogowany do Windows jako użytkownik, który utworzył kolekcji projektów zespołowych, należy zalogować się do programu SharePoint jako ten użytkownik. Aby kontynuować.
4. Na **Akcje witryny** menu, kliknij przycisk **ustawienia lokacji**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Na **ustawienia lokacji** w obszarze **użytkownikami i uprawnieniami**, kliknij przycisk **osoby i grupy**.
6. W okienku nawigacji po lewej stronie kliknij **grup**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Na **osoby i grupy: wszystkie grupy** kliknij **Konfigurowanie grup dla tej witryny**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Może pojawić się <strong>HTTP 404 Nie znaleziono</strong> błąd z powodu podwójnego kodowania błędów HTTP. W takim przypadku należy zastąpić adres URL to:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Na przykład:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Na **Konfigurowanie grup dla tej witryny** Dodaj użytkownika, który spowoduje utworzenie projektów zespołowych pod kątem **właścicieli** grupy, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w obrębie kolekcji projektu zespołowego, zobacz [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Tworzenie nowego projektu zespołowego i dodawanie użytkowników

Gdy masz odpowiednie uprawnienia, możesz użyć **Team Explorer** okna w programie Visual Studio 2010 do utworzenia nowego projektu zespołowego. To podejście oferuje bezpieczny kreatora, który zbiera wszystkie wymagane informacje i wykonuje niezbędne zadania w programie TFS, SharePoint i SQL Server Reporting Services. Należy również przyznać uprawnienia nowego projektu zespołowego do członków zespołu deweloperów umożliwiające dodawanie i modyfikowanie zawartości.

### <a name="who-performs-these-procedures"></a>Osób wykonujących te procedury?

Zazwyczaj administrator programu TFS lub lider zespołu deweloperów wykonuje te procedury.

### <a name="create-a-new-team-project"></a>Tworzenie nowego projektu zespołowego

W następnej procedurze opisano sposób tworzenia nowego projektu zespołowego w TFS 2010.

**Aby utworzyć nowy projekt zespołowy**

1. Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Visual Studio 2010**, kliknij prawym przyciskiem myszy **Microsoft Visual Studio 2010**, a następnie kliknij przycisk **Uruchom jako administrator**.

    > [!NOTE]
    > Jeśli nie uruchomisz programu Visual Studio 2010 jako administrator, Kreator nowego projektu zespołowego zakończy się niepowodzeniem w ostatnim kroku.
2. Jeśli **Kontrola konta użytkownika** pojawi się okno dialogowe, kliknij przycisk **tak**.
3. W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.

    > [!NOTE]
    > Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki 4 – 7.
4. W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.
5. W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.
6. W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.
8. W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia programu TFS, aby nawiązać połączenie, wybierz zespół projektu kolekcji, które chcesz dodać do, a następnie kliknij przycisk **Connect**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. W **Team Explorer** okna, kliknij prawym przyciskiem myszy kolekcji projektów zespołu, a następnie kliknij przycisk **nowego projektu zespołowego**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. W **nowego projektu zespołowego** okno dialogowe, podaj nazwę i opis dla projektu zespołowego, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Jeśli Twój projekt zespołowy zawiera spacje, może twarzy niektóre problemy, po dojściu do korzystania z narzędzia wdrażania Web Internet Information Services (IIS) (Web Deploy) do wdrażania pakietów ze ścieżki danych wyjściowych. Miejsca do magazynowania w ścieżce może utrudnić znacznie więcej poleceń narzędzia Web Deploy.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Na **wybierz szablon procesu** wybierz szablon procesu, który zarządza procesem opracowywania, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat szablonów procesów programu TFS, zobacz [szablonów procesów i narzędzi](https://msdn.microsoft.com/vstudio/aa718795).
12. Na **ustawienia witryny zespołu** strony, pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.
13. To ustawienie tworzy lub identyfikuje zespołu witryny programu SharePoint, który jest skojarzony z projektem zespołowym TFS. Twój zespół deweloperów można użyć tej lokacji do zarządzania dokumentacją, uczestniczą w wątkach dyskusji, tworzenie strony typu wiki i wykonywania różnych zadań, które nie są związane z kodem. Aby uzyskać więcej informacji, zobacz [interakcje między produktów programu SharePoint i serwera Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Na **Określ ustawienia kontroli źródła** strony, pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.
15. To ustawienie określa lub tworzy lokalizacji w hierarchii folderów TFS, który będzie pełnił rolę folderu głównego dla zawartości.
16. Na **Potwierdź ustawienia projektu zespołowego** kliknij **Zakończ**.
17. Gdy nowy projekt zespołowy pomyślnym utworzeniu na **utworzyć projektu zespołowego** kliknij **Zamknij**.

### <a name="add-users-to-a-team-project"></a>Dodawanie użytkowników do projektu zespołowego

Teraz, po utworzeniu nowego projektu zespołowego, można przyznać uprawnienia do użytkowników w celu umożliwienia im rozpocząć dodawanie i współpracę nad zawartością.

**Aby dodać użytkowników do projektu zespołowego**

1. W programie Visual Studio 2010 w **Team Explorer** okna, kliknij prawym przyciskiem myszy projekt zespołowy, wskaż opcję **ustawienia projektu zespołowego**, a następnie kliknij przycisk **członkostwa w grupie**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Aby umożliwić użytkownikowi na dodawanie, modyfikowanie i usuwanie kodu pod kontrolą źródła, należy dodać go do **współautorzy** grupy.
3. W **grupy projektów** okno dialogowe, wybierz opcję **współautorzy** grupy, a następnie kliknij przycisk **właściwości**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. W **właściwości grupy programu Team Foundation Server** okno dialogowe, wybierz opcję **Windows użytkownik lub grupa**, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, które chcesz dodać do projektu zespołowego, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. W **właściwości grupy programu Team Foundation Server** okno dialogowe, kliknij przycisk **OK**.
7. W **grupy projektów** okno dialogowe, kliknij przycisk **Zamknij**.

## <a name="conclusion"></a>Wniosek

W tym momencie nowego projektu zespołowego jest gotowy do użycia i Twojego zespołu deweloperów rozpocząć dodawanie zawartości i współpracę w procesie tworzenia aplikacji.

Następny temat [dodawania zawartości do kontroli źródła](adding-content-to-source-control.md), w tym artykule opisano sposób dodawania zawartości do kontroli źródła.

## <a name="further-reading"></a>Dalsze informacje

Szerszy wskazówki dotyczące tworzenia projektów zespołowych w programie TFS, zobacz [tworzenia projektu zespołowego](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w obrębie kolekcji projektu zespołowego, zobacz [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx). Aby uzyskać więcej informacji na temat dodawania użytkowników do zespołów i projektów, zobacz [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-team-foundation-server-for-web-deployment.md)
> [dalej](adding-content-to-source-control.md)
