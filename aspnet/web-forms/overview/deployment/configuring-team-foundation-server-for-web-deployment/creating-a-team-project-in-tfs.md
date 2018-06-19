---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Tworzenie nowego projektu zespołowego w programie TFS | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880430"
---
<a name="creating-a-team-project-in-tfs"></a>Tworzenie nowego projektu zespołowego w programie TFS
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

## <a name="task-overview"></a>Omówienie zadań

Do udostępniania i użyć nowego projektu zespołowego w programie TFS, musisz wykonać następujące ogólne kroki:

- Przyznaj uprawnienia do użytkownika, który spowoduje utworzenie nowego projektu zespołowego.
- Tworzenie projektu zespołowego.
- Przyznaj uprawnienia do członków zespołu, którzy będą pracować w projekcie.
- Sprawdź w części zawartości.

W tym temacie opisano, jak do wykonania tych procedur i zidentyfikuje użytkownicy i role zadania, które mogą być odpowiedzialne za każdej procedury. Należy pamiętać, że, w zależności od struktury organizacji każdego z tych zadań mogą być odpowiedzialność inną osobę.

Zadania i wskazówki, w tym temacie założono, czy zostały zainstalowane i skonfigurowane TFS i utworzono kolekcję projektów zespołowych w trakcie procesu konfiguracji. Aby uzyskać więcej informacji na temat tych założeń i bardziej ogólne informacje od scenariusza, zobacz [Konfiguracja kompilacji serwera TFS dla Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Udzielanie uprawnień osobom Twórca projektu zespołowego

Aby było możliwe utworzenie nowego projektu zespołowego, potrzebne są następujące uprawnienia:

- Musi mieć **tworzenie nowych projektów** uprawnień na warstwie aplikacji TFS. Uprawnienie to można przyznać zwykle przez dodanie użytkowników do **Administratorzy kolekcji projektów** grupy TFS. **Team Foundation Administratorzy** globalna grupa zawiera również to uprawnienie.
- Musi mieć uprawnienia do tworzenia nowej witryny zespołu w zbiorze witryn programu SharePoint, odpowiadającej kolekcji projektów zespołowych TFS. Uprawnienie to można przyznać zwykle przez dodanie użytkownika do grupy programu SharePoint z **Pełna kontrola** zbioru witryn prawa w programie SharePoint.
- Jeśli używasz funkcji programu SQL Server Reporting Services, musi być członkiem **Team Foundation Content Manager** w usługach Reporting Services.

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

Zazwyczaj osoba lub grupa, która zarządza wdrożenia TFS wykonuje także tych procedur.

Ponieważ jest to wysoko uprzywilejowane zestaw uprawnień, nowych projektów zespołowych są zazwyczaj tworzone przez mały podzbiór użytkowników odpowiedzialnego za administrowanie wdrożenia TFS. Deweloperzy będą nie zwykle można udzielić wymaganych uprawnień do tworzenia nowych projektów zespołowych.

### <a name="grant-permissions-in-tfs"></a>Udzielanie uprawnień w programie TFS

Jeśli chcesz umożliwić użytkownikom tworzenie nowych projektów zespołowych, pierwszego zadania wysokiego poziomu jest dodanie użytkownika do **Administratorzy kolekcji projektów** grupy dla kolekcji projektów zespołowych.

**Aby dodać użytkownika do grupy Administratorzy kolekcji projektów**

1. Na serwerze TFS na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Team Foundation Server 2010**, a następnie kliknij przycisk **Team Foundation Konsola administracyjna**.
2. W widoku drzewa nawigacji rozwiń węzeł **warstwy aplikacji**, a następnie kliknij przycisk **kolekcje projektów zespołowych**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. W **kolekcje projektów zespołowych** okienku, wybierz opcję chcesz zarządzać kolekcji projektów zespołowych.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Na **ogólne** , kliknij pozycję **członkostwa w grupie**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. W **grupę globalną** okno dialogowe, wybierz opcję **Administratorzy kolekcji projektów** grupy, a następnie kliknij przycisk **właściwości**.
6. W **Team Foundation Server grupy właściwości** okno dialogowe, wybierz opcję **użytkownika systemu Windows lub grupy**, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, aby można było tworzyć nowe projekty zespołowe, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. W **Team Foundation Server grupy właściwości** okno dialogowe, kliknij przycisk **OK**.
9. W **grupę globalną** okno dialogowe, kliknij przycisk **Zamknij**.

### <a name="grant-permissions-in-sharepoint-services"></a>Udzielanie uprawnień w programie SharePoint Services

Następnie należy przyznać uprawnienia użytkownika do tworzenia nowych lokacji zespołu w kolekcji witryn programu SharePoint, która odnosi się do kolekcji projektów zespołowych TFS.

**Aby przyznać uprawnienia Pełna kontrola do zbioru witryn programu SharePoint**

1. W konsoli administracyjnej Team Foundation Server na **kolekcje projektów zespołowych** wybierz kolekcji projektów zespołowych, którymi chcesz zarządzać.
2. Na **witryny programu SharePoint** karcie, zanotuj wartość ustawienia **bieżącej lokalizacji domyślnej witryny** adresu URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Otwórz program Internet Explorer, a następnie przejdź do adresu URL zanotowaną w kroku 2.

    > [!NOTE]
    > Jeśli nie zalogowaniu się do systemu Windows jako użytkownik, który utworzył kolekcji projektów zespołowych, należy zalogować się do programu SharePoint jako ten użytkownik. Aby kontynuować.
4. Na **Akcje witryny** menu, kliknij przycisk **ustawienia lokacji**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Na **ustawienia lokacji** w obszarze **uprawnienia użytkowników i**, kliknij przycisk **osoby i grupy**.
6. W okienku nawigacji po lewej stronie kliknij **grup**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Na **osoby i grupy: wszystkie grupy** kliknij przycisk **Konfigurowanie grup w tej witrynie**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Może pojawić się <strong>HTTP 404 — Nie znaleziono</strong> błąd z powodu podwójnego usterki kodowania protokołu HTTP. W takim przypadku należy zastąpić adres URL to:   
   > [<em>adres URL zbioru witryn</em>] /\_layouts/permsetup.aspx  
   > Na przykład:  
   > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. Na **Konfigurowanie grup w tej witrynie** strony, Dodaj użytkownika, który spowoduje utworzenie projektów zespołowych do **właścicieli** grupy, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektu zespołowego](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Tworzenie nowego projektu zespołowego i dodawanie użytkowników

Gdy masz wystarczające uprawnienia, możesz użyć **Team Explorer** okna w Visual Studio 2010 do utworzenia nowego projektu zespołowego. Takie podejście udostępnia kreatora, który zbiera wszystkie wymagane informacje i wykonuje niezbędnych zadań w programie TFS, SharePoint i SQL Server Reporting Services. Ponadto należy udzielić uprawnień do nowego projektu zespołowego, aby członkowie zespołu deweloperów umożliwiające dodawanie i modyfikowanie zawartości.

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

Zazwyczaj administratora TFS lub kierownik zespołu deweloperów wykonuje te procedury.

### <a name="create-a-new-team-project"></a>Tworzenie nowego projektu zespołowego

W następnej procedurze opisano sposób tworzenia nowego projektu zespołowego w TFS 2010.

**Do utworzenia nowego projektu zespołowego**

1. Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft Visual Studio 2010**, kliknij prawym przyciskiem myszy **programu Microsoft Visual Studio 2010**, a następnie kliknij przycisk **Uruchom jako administrator**.

    > [!NOTE]
    > Jeżeli nie uruchomisz programu Visual Studio 2010 jako administrator, Kreator nowego projektu zespołowego zakończy się niepowodzeniem w ostatnim kroku.
2. Jeśli **Kontrola konta użytkownika** zostanie wyświetlone okno dialogowe, kliknij przycisk **tak**.
3. W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.

    > [!NOTE]
    > Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki od 4 do 7.
4. W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.
5. W **Dodaj lub usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.
6. W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia TFS, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. W **Dodaj lub usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.
8. W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia TFS ma się połączyć, wybierz zespół projektu kolekcji, które chcesz dodać do, a następnie kliknij przycisk **Connect**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. W **Team Explorer** okna, kliknij prawym przyciskiem myszy zespołu kolekcji projektów, a następnie kliknij przycisk **nowego projektu zespołowego**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. W **nowego projektu zespołowego** okno dialogowe, podaj nazwę i opis dla projektu zespołowego, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Jeśli projekt zawiera spacje, po dojściu do użycia usług Internet Information Services (IIS) Narzędzie wdrażania Web (Web Deploy) do wdrożenia pakietu w ścieżce danych wyjściowych może stają przed problemy. Spacje w ścieżce może utrudnić znacznie więcej do uruchomienia poleceń narzędzia Web Deploy.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Na **wybierz szablon procesu** wybierz szablon procesu, który ma być używany do zarządzania procesem tworzenia, a następnie kliknij pozycję **dalej**.

    > [!NOTE]
    > Aby uzyskać więcej informacji dotyczących szablonów procesów dla serwera TFS, zobacz [szablonów procesów i narzędzi](https://msdn.microsoft.com/vstudio/aa718795).
12. Na **ustawienia witryny zespołu** , pozostaw domyślne ustawienie bez zmian, a następnie kliknij przycisk **dalej**.
13. To ustawienie powoduje utworzenie lub identyfikuje witryna zespołu programu SharePoint, który jest skojarzony z projektem zespołowym TFS. Zespół deweloperów można użyć tej lokacji do zarządzania dokumentacji, uczestniczyć w wątkach na dyskusję, tworzenie stron typu wiki i wykonywania różnych zadań, które nie są związane z kodem. Aby uzyskać więcej informacji, zobacz [interakcje między produktów programu SharePoint i serwera Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Na **Określ ustawienia kontroli źródła** , pozostaw domyślne ustawienie bez zmian, a następnie kliknij przycisk **dalej**.
15. To ustawienie określa lub tworzy lokalizacji w hierarchii folderów TFS, który będzie pełnił rolę folderu głównego dla zawartości.
16. Na **Potwierdź ustawienia projektu zespołowego** kliknij przycisk **Zakończ**.
17. Gdy nowego projektu zespołowego została pomyślnie utworzona, na **utworzyć projektu zespołowego** kliknij przycisk **Zamknij**.

### <a name="add-users-to-a-team-project"></a>Dodawanie użytkowników do projektu zespołowego

Teraz, po utworzeniu nowego projektu zespołowego, można przyznać uprawnień użytkowników, aby włączyć je, aby rozpocząć dodawanie i współpracy nad zawartości.

**Aby dodać użytkowników do projektu zespołowego**

1. W programie Visual Studio 2010 w **Team Explorer** okna, kliknij prawym przyciskiem myszy projektu zespołowego, wskaż pozycję **ustawienia projektu zespołowego**, a następnie kliknij przycisk **członkostwa w grupie**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Aby umożliwić użytkownikowi na dodawanie, modyfikowanie i usuwanie kodu pod kontrolą źródła, należy dodać go do **współautorzy** grupy.
3. W **grupy projektów** okno dialogowe, wybierz opcję **współautorzy** grupy, a następnie kliknij przycisk **właściwości**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. W **Team Foundation Server grupy właściwości** okno dialogowe, wybierz opcję **użytkownika systemu Windows lub grupy**, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, które chcesz dodać do projektu zespołowego, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. W **Team Foundation Server grupy właściwości** okno dialogowe, kliknij przycisk **OK**.
7. W **grupy projektów** okno dialogowe, kliknij przycisk **Zamknij**.

## <a name="conclusion"></a>Wniosek

W tym momencie jest gotowe do użycia nowego projektu zespołowego, a zespół deweloperów można rozpocząć dodawanie zawartości i współpracy nad procesem tworzenia.

Następnym temacie [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md), w tym artykule opisano sposób dodawania zawartości do kontroli źródła.

## <a name="further-reading"></a>Dalsze informacje

Szerszych wskazówki dotyczące tworzenia projektów zespołowych w programie TFS, zobacz [utworzenia projektu zespołowego](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektu zespołowego](https://msdn.microsoft.com/library/dd547204.aspx). Aby uzyskać więcej informacji na temat dodawania użytkowników do zespołów i projektów, zobacz [Dodawanie użytkowników do zespołów i projektów](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-team-foundation-server-for-web-deployment.md)
> [dalej](adding-content-to-source-control.md)
