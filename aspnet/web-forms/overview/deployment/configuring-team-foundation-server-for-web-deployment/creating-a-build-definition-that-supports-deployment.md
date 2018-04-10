---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Tworzenie definicji kompilacji, który obsługuje wdrażanie | Dokumentacja firmy Microsoft
author: jrjlee
description: Jeśli chcesz wykonywać dowolny rodzaj kompilacji w Team Foundation Server (TFS) 2010, należy utworzyć definicję kompilacji w projekcie zespołowym. Ten temat des...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Tworzenie definicji kompilacji, która obsługuje wdrożenia
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Jeśli chcesz wykonywać dowolny rodzaj kompilacji w Team Foundation Server (TFS) 2010, należy utworzyć definicję kompilacji w projekcie zespołowym. W tym temacie opisano, jak utworzyć nową definicję kompilacji w programie TFS i sterowanie wdrożenia sieci web jako część procesu kompilacji w Team Build.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji i wdrożenia dwa pliki projektu&#x2014;jeden zawierającego instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Definicja kompilacji jest mechanizm, który określa, jak i kiedy kompilacje wystąpić dla projektów zespołowych w programie TFS. Określa każdej definicji kompilacji:

- Czynności, które chcesz utworzyć, takich jak pliki rozwiązania Visual Studio lub niestandardowe pliki projektu Microsoft kompilacji Engine (MSBuild).
- Kryteria określające po kompilacji powinien mieć miejsce, takie jak ręczne wyzwalaczy, ciągłej integracji (CI), lub warunkowych zaewidencjonowań.
- Lokalizacja, do której Team Build należy wysłać wyjścia kompilacji, w tym artefakty wdrażania pakietów sieci web i skryptów bazy danych.
- Ilość czasu, które mają być przechowywane każdej kompilacji.
- Różnych innych parametrów procesu kompilacji.

> [!NOTE]
> Aby uzyskać więcej informacji o definicje kompilacji, zobacz [Definiowanie procesu kompilacji](https://msdn.microsoft.com/library/ms181715.aspx).


W tym temacie opisano sposób tworzenia definicji kompilacji, która używa elementu konfiguracji, tak aby kompilacji jest wyzwalane, gdy projektant sprawdza w nową zawartość. Jeśli kompilacja zakończy się powodzeniem, usługa kompilacji uruchamia plik projektu niestandardowe, aby wdrożyć rozwiązanie do środowiska testowego.

Gdy wyzwolić kompilację, te akcje muszą się zdarzyć:

- Najpierw należy Team Build należy utworzyć rozwiązanie. W ramach tego procesu Team Build wywoła publikowania potoku sieci Web (WPP) do generowania pakietów wdrażania w sieci web dla poszczególnych projektów aplikacji sieci web w rozwiązaniu. Team Build będzie również uruchomić wszystkie testy jednostek skojarzonych z rozwiązaniem.
- W przypadku niepowodzenia kompilacji rozwiązania Team Build powinna przyjmować żadnych dodatkowych czynności. Niepowodzenia testu jednostki powinny być traktowane jako niepowodzenie kompilacji.
- Jeśli kompilacja rozwiązania zakończy się powodzeniem, Team Build należy uruchomić pliku projektu niestandardowego, który kontroluje wdrażania rozwiązania. W ramach tego procesu Team Build wywoła (Web Deploy) umożliwia instalowanie aplikacji spakowanych sieci web na serwerach sieci web docelowych Narzędzie wdrażania Web usług Internet Information Services (IIS), a zostaną wywołane uruchamianie tworzenia bazy danych za pomocą narzędzia VSDBCMD.exe skrypty na serwerach docelowych bazy danych.

Przedstawiono to proces:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Menedżera skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie obejmuje niestandardowego pliku projektu MSBuild *Publish.proj*, który można uruchomić z programu MSBuild lub Team Build. Zgodnie z opisem w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ten plik projektu definiuje logikę, która wdraża Twoje pakiety sieci web i baz danych w środowisku docelowym. Plik zawiera logikę pomijające budynku i proces tworzenia pakietu, jeśli została uruchomiona w Team Build, pozostawiając tylko zadania wdrażania do uruchomienia. To jest, ponieważ podczas wdrażania w ten sposób można zautomatyzować, zazwyczaj należy zapewnić pomyślnie skompilowaniem rozwiązania i przekazuje wszystkie testy jednostkowe, przed rozpoczęciem procesu wdrażania.

Następnej sekcji opisano sposób wykonania tego procesu, tworząc nową definicję kompilacji.

> [!NOTE]
> Ta procedura&#x2014;, w której jeden automatycznego procesu kompilacji, testy i wdraża rozwiązanie&#x2014;może być najbardziej nadaje się do wdrożenia, aby przetestować środowisk. W przypadku środowisk przemieszczania i produkcji prawdopodobnie znacznie więcej chcesz wdrożyć zawartość z poprzedniej kompilacji, która już zweryfikować i sprawdzić poprawności w środowisku testowym. Takie podejście jest opisana w następnym temacie [wdrażanie określonej kompilacji](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Kto wykonuje tę procedurę?

Zazwyczaj administratora TFS wykonuje tę procedurę. W niektórych przypadkach kierownik zespołu deweloperów może mieć odpowiedzialność dla kolekcji projektów zespołowych TFS. Aby utworzyć nową definicję kompilacji, musisz być członkiem **Administratorzy kompilacji kolekcji projektów** grupy dla kolekcji projektów zespołowych, która zawiera rozwiązania.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Tworzenie definicji kompilacji dla elementu konfiguracji i wdrażania

Następna procedura opisuje sposób tworzenia definicji kompilacji, która wyzwala elementu konfiguracji. Jeśli kompilacja zakończy się powodzeniem, rozwiązanie jest wdrożone przy użyciu logiki w pliku projektu MSBuild niestandardowych.

**Aby utworzyć definicję kompilacji dla elementu konfiguracji i wdrażania**

1. W programie Visual Studio 2010 w **Team Explorer** okna, rozwiń węzeł projektu zespołowego, kliknij prawym przyciskiem myszy **kompilacje**, a następnie kliknij przycisk **nową definicję kompilacji**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Na **ogólne** karcie, nadaj nazwę definicji kompilacji (na przykład **DeployToTest**) i opcjonalny opis.
3. Na **wyzwalacza** , a następnie wybierz kryteria, na których chcesz wyzwalają nowej kompilacji. Na przykład, jeśli chcesz Skompiluj rozwiązanie i wdrożyć środowiska testowego każdorazowego dewelopera sprawdza w nowy kod, wybierz **ciągłej integracji**.
4. Na **kompilacji domyślne** karcie **Kopiuj dane wyjściowe kompilacji do następującego folderu docelowego** wpisz ścieżkę Universal Naming Convention (UNC) z folderu docelowego (na przykład  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Ta lokalizacja przechowywania przechowuje kilka kompilacji, w zależności od skonfigurowanych zasad przechowywania. Aby opublikować artefaktów wdrożenia z określonej kompilacji w środowisku tymczasowym czy produkcyjnym, należy to znajdziesz je.
5. Na **procesu** karcie **pliku procesu kompilacji** listy rozwijanej, pozostaw **DefaultTemplate.xaml** wybrane. Jest to jeden z szablonów domyślnych procesów kompilacji, które zostaną dodane do wszystkich nowych projektów zespołowych.
6. W **parametrów procesu kompilacji** tabeli, kliknij przycisk **elementów do kompilacji** wiersza, a następnie kliknij przycisk **wielokropka** przycisku.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. W **elementów do kompilacji** okno dialogowe, kliknij przycisk **Dodaj**.
8. Przejdź do lokalizacji pliku rozwiązania, a następnie kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. W **elementów do kompilacji** okno dialogowe, kliknij przycisk **Dodaj**.
10. W **elementów typu** listy rozwijanej wybierz **pliki projektu MSBuild**.
11. Przejdź do lokalizacji pliku projektu niestandardowe, z którym można kontrolować proces wdrażania, wybierz plik, a następnie kliknij **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **Elementów do kompilacji** okno dialogowe powinna zostać wyświetlona dwa elementy. Kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Na **procesu** karcie **parametrów procesu kompilacji** tabeli, a następnie rozwiń **zaawansowane** sekcji.
14. W **argumenty programu MSBuild** wiersz, należy dodać żadnych argumentów wiersza polecenia programu MSBuild który *albo* elementów do tworzenia wymaga. W scenariuszu rozwiązania Menedżera skontaktuj się z tych argumentów są wymagane:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. W tym przykładzie:

    1. **DeployOnBuild = true** i **DeployTarget = pakiet** argumenty są wymagane podczas kompilowania rozwiązania z menedżerem kontaktu. To powoduje, że program MSBuild tworzenia pakietów wdrożeniowych sieci web po utworzeniu każdego projektu aplikacji sieci web, zgodnie z opisem w [budynku i projekty aplikacji sieci Web pakowania](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. **TargetEnvPropsFile** argumentu jest wymagany w przypadku kompilowania *Publish.proj* pliku. Ta właściwość wskazuje lokalizację pliku konfiguracji specyficznych dla danego środowiska, zgodnie z opisem w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Na **zasady przechowywania** skonfiguruj kompilacje ile każdego typu, aby zachować zgodnie z potrzebami.
17. Kliknij przycisk **zapisać**.

## <a name="queue-a-build"></a>Kolejkowanie kompilacji

W tym momencie utworzono co najmniej jedną nową definicję kompilacji. Proces kompilacji, zdefiniowane przez użytkownika będzie uruchamiana zgodnie z wyzwalaczy, określona w definicji kompilacji.

Jeśli skonfigurowano definicję kompilacji, aby użyć elementu konfiguracji, można sprawdzić definicję kompilacji na dwa sposoby:

- Sprawdź w części zawartości do projektu zespołowego do wyzwalania automatycznego kompilacji.
- Kolejka kompilacji ręcznie.

**Aby ręcznie kolejki kompilację**

1. W **Team Explorer** okna, kliknij prawym przyciskiem myszy definicję kompilacji, a następnie kliknij przycisk **Tworzenie nowej kolejki**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. W **kolejka kompilacji** okno dialogowe, przejrzyj właściwości kompilacji, a następnie kliknij przycisk **kolejki**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Aby przejrzeć postęp i wyniki kompilacji&#x2014;niezależnie od tego, czy zostało wyzwolone ręcznie lub automatycznie&#x2014;kliknij dwukrotnie definicję kompilacji w **Team Explorer** okna. Spowoduje to otwarcie **Eksplorator kompilacji** kartę.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

W tym miejscu można rozwiązywać problemy kompilacji nie powiodło się. Po dwukrotnym kliknięciu poszczególnych kompilacji, możesz wyświetlić informacje podsumowujące i kliknij, aby szczegółowe pliki dzienników.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Tych informacji umożliwia rozwiązywanie problemów z niepowodzeniem kompilacji i rozwiązania dowolnych problemów przed podjęciem próby innej kompilacji.

> [!NOTE]
> Kompilacje, które wykonywania logiki wdrożenia są prawdopodobnie niepowodzeniem, dopóki serwer kompilacji udzielono uprawnień wymaganych w środowisku docelowym. Aby uzyskać więcej informacji, zobacz [konfigurowania uprawnień dla zespołu wdrażania kompilacji](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorowanie procesu kompilacji

TFS oferuje szeroką gamę funkcji, które pomagają monitorować proces kompilacji. Na przykład TFS można wysłać wiadomości e-mail lub wyświetlić alerty w obszarze powiadomień paska zadań po ukończeniu kompilacji. Aby uzyskać więcej informacji, zobacz [wykonywania i tworzy Monitor](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób tworzenia definicji kompilacji w programie TFS. Definicji kompilacji jest skonfigurowana dla elementu konfiguracji, więc proces kompilacji jest uruchamiana zawsze, gdy projektant sprawdza w zawartości do projektu zespołowego. Definicja kompilacji wykonuje niestandardowego pliku projektu MSBuild wdrażać pakiety sieci web i skryptów bazy danych w środowisku serwera docelowego.

Aby automatycznego wdrażania do pomyślnego jako część procesu kompilacji należy udzielić odpowiednich uprawnień do konta usługi kompilacji na docelowej serwerów sieci web i bazy danych serwera docelowego. Końcowe tematu w tym samouczku [konfigurowania uprawnień dla zespołu wdrażania kompilacji](configuring-permissions-for-team-build-deployment.md), opisano sposób identyfikowania i konfigurowania uprawnień wymaganych do automatycznego wdrażania z serwera Team Build.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia definicje kompilacji, zobacz [utworzyć podstawową definicję kompilacji](https://msdn.microsoft.com/library/ms181716.aspx) i [Definiowanie procesu kompilacji](https://msdn.microsoft.com/library/ms181715.aspx). Aby uzyskać więcej pomocy w przypadku kompilacji usługi kolejkowania wiadomości, zobacz [kolejki kompilację](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-tfs-build-server-for-web-deployment.md)
> [dalej](deploying-a-specific-build.md)
