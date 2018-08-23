---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Tworzenie definicji kompilacji, który obsługuje wdrażanie | Dokumentacja firmy Microsoft
author: jrjlee
description: Jeśli chcesz wykonywać dowolny rodzaj kompilacji w Team Foundation Server (TFS) 2010, należy utworzyć definicję kompilacji w projekcie zespołowym. Ten temat des...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33ebde3074603801945c676ace64b26ca5bbf44a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754851"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Tworzenie definicji kompilacji, która obsługuje wdrożenie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Jeśli chcesz wykonywać dowolny rodzaj kompilacji w Team Foundation Server (TFS) 2010, należy utworzyć definicję kompilacji w projekcie zespołowym. W tym temacie opisano sposób tworzenia nowej definicji kompilacji w programie TFS i sposobie kontrolowania wdrażania w Internecie jako część procesu kompilacji w kompilacji zespołu.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w której procesu kompilacji i wdrażania jest kontrolowany przez dwa pliki projektu&#x2014;jeden zawierającej instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Definicja kompilacji jest mechanizm, który kontroluje, jak i kiedy kompilacje miejsce w przypadku projektów zespołowych w programie TFS. Określa Każda definicja kompilacji:

- Czynności, które chcesz skompilować, takich jak pliki rozwiązania programu Visual Studio lub niestandardowe pliki projektu aparatu Microsoft Build Engine (MSBuild).
- Kryteria określające, gdy kompilacja powinno zająć miejsce, takich jak wyzwalacze ręczne, ciągłej integracji (CI) lub warunkowych zaewidencjonowań.
- Lokalizacja, do którego Team Build powinien wysyłać dane wyjściowe kompilacji, w tym artefaktów wdrożenia, takie jak pakiety programu web i skrypty bazy danych.
- Ilość czasu, który ma być przechowywana każdej kompilacji.
- Różne inne parametry procesu kompilacji.

> [!NOTE]
> Aby uzyskać więcej informacji na temat definicji kompilacji, zobacz [zdefiniować proces kompilacji](https://msdn.microsoft.com/library/ms181715.aspx).


W tym temacie pokazano sposób tworzenia definicji kompilacji, który używa ciągłej integracji, tak aby kompilacja zostaje wyzwolona, gdy deweloper wprowadza nową zawartość. Jeśli kompilacja zakończy się powodzeniem, usługa kompilacji jest uruchamiany pliku niestandardowego projektu, aby wdrożyć to rozwiązanie w środowisku testowym.

Po wyzwoleniu kompilacji, działania te należy wykonać:

- Team Build należy najpierw skompilować rozwiązanie. W ramach tego procesu Team Build wywoła Web Publishing potoku (WPP) do generowania pakietów wdrażania sieci web dla wszystkich projektów aplikacji sieci web w rozwiązaniu. Team Build będzie również uruchomić wszystkie testy jednostkowe skojarzonego z rozwiązaniem.
- Jeśli kompilacja rozwiązania kończy się niepowodzeniem, Team Build powinien wykonywać żadnych dalszych czynności. Niepowodzenie testów jednostkowych powinny być traktowane jako niepowodzenie kompilacji.
- Kompilacja rozwiązania kończy się powodzeniem, Team Build powinien uruchamiać pliku niestandardowego projektu, który kontroluje wdrażania rozwiązania. W ramach tego procesu Team Build wywoła narzędzie Internet Information Services (IIS) Web Deployment (Web Deploy), instalacja aplikacji spakowane w sieci web na serwerach docelowych w sieci web i będzie go wywoływać uruchamianie tworzenia bazy danych za pomocą narzędzia VSDBCMD.exe skrypty na serwerach docelowych w bazie danych.

Obrazuje to proces:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie zawiera niestandardowy plik projektu MSBuild *Publish.proj*, którą można uruchomić z programu MSBuild lub Team Build. Zgodnie z opisem w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ten plik projektu zdefiniowana logika, która wdraża swoich pakietów sieci web i baz danych w środowisku docelowym. Plik zawiera logikę, które pomija budynku i proces pakowania, jeśli działa on w programie Team Build, pozostawiając tylko zadania wdrażania do uruchomienia. To jest, ponieważ podczas automatyzacji wdrażania w ten sposób zwykle należy upewnić się, że rozwiązanie zostanie skompilowane pomyślnie i przekazuje wszystkie testy jednostkowe, przed rozpoczęciem procesu wdrażania.

Następnej sekcji opisano sposób wykonania tego procesu, tworząc nową definicję kompilacji.

> [!NOTE]
> Ta procedura&#x2014;w której automatycznego jednego procesu kompilacji, testów i wdraża to rozwiązanie&#x2014;może być najbardziej nadaje się do wdrożenia do środowisk testowych. W przypadku środowisk przemieszczania i produkcji możesz znacznie bardziej prawdopodobne, aby wdrożyć zawartość z poprzedniej kompilacji, która została już zweryfikowana i sprawdzone w środowisku testowym. Takie podejście jest opisane w następnym temacie [wdrażanie określonej kompilacji](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Ta procedura osób wykonujących?

Zazwyczaj administratora TFS wykonuje tę procedurę. W niektórych przypadkach lider zespołu deweloperów może potrwać odpowiedzialności dla kolekcji projektów zespołowych w programie TFS. Aby utworzyć nową definicję kompilacji, musisz być członkiem **Administratorzy kompilacji kolekcji projektów** grupy dla kolekcji projektu zespołowego, który zawiera rozwiązania.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Utwórz definicję kompilacji ciągłej integracji i wdrażania

Następna procedura zawiera opis sposobu tworzenia definicji kompilacji, która wyzwala ciągłej integracji. Jeśli kompilacja zakończy się powodzeniem, rozwiązanie będzie wdrożone przy użyciu logiki w niestandardowy plik projektu programu MSBuild.

**Aby utworzyć definicję kompilacji ciągłej integracji i wdrażania**

1. W programie Visual Studio 2010 w **Team Explorer** oknie rozwinięciu węzła projektu zespołu, kliknij prawym przyciskiem myszy **kompilacje**, a następnie kliknij przycisk **Nowa definicja kompilacji**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Na **ogólne** kartę, nadaj nazwę definicji kompilacji (na przykład **DeployToTest**) i opcjonalny opis.
3. Na **wyzwalacza** , a następnie wybierz kryteria, na których chcesz Wyzwalaj nową kompilację. Na przykład, jeśli chcesz skompilować rozwiązanie i wdrażania do środowiska testowego, za każdym razem, gdy programista zaewidencjonuje nowy kod, wybierz **ciągłej integracji**.
4. Na **ustawienia domyślne kompilacji** na karcie **Kopiuj dane wyjściowe kompilacji do następującego folderu docelowego** wpisz ścieżkę Universal Naming Convention (UNC) folderu wrzucania (na przykład  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Kilka kompilacji, w zależności od skonfigurowanych zasad przechowywania są przechowywane w tej lokalizacji docelowej. Jeśli chcesz opublikować artefakty wdrażania z kompilacji w określonym w tymczasowym lub produkcyjnym środowisku, to której można je znaleźć.
5. Na **procesu** na karcie **plik procesu kompilacji** listy rozwijanej, pozostaw **DefaultTemplate.xaml** wybrane. Jest to jeden z domyślnych szablonów procesów kompilacji, które poproś o dodanie Cię do wszystkich nowych projektów zespołowych.
6. W **parametrów procesu kompilacji** tabelę, kliknij opcję w **elementy do kompilacji** wiersza, a następnie kliknij przycisk **wielokropka** przycisku.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. W **elementy do kompilacji** okno dialogowe, kliknij przycisk **Dodaj**.
8. Przejdź do lokalizacji pliku rozwiązania, a następnie kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. W **elementy do kompilacji** okno dialogowe, kliknij przycisk **Dodaj**.
10. W **elementów typu** listy rozwijanej wybierz **pliki projektu MSBuild**.
11. Przejdź do lokalizacji pliku niestandardowego projektu za pomocą którego możesz kontrolować proces wdrażania, wybierz plik, a następnie kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **Elementy do kompilacji** okno dialogowe powinna zostać wyświetlona dwa elementy. Kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Na **procesu** na karcie **parametrów procesu kompilacji** tabeli, a następnie rozwiń **zaawansowane** sekcji.
14. W **argumenty MSBuild** wierszy, dodawać żadnych argumentów wiersza polecenia programu MSBuild, *albo* wymaga elementów do kompilacji. W tym scenariuszu rozwiązania Contact Manager wymagane są następujące argumenty:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. W tym przykładzie:

    1. **DeployOnBuild = true** i **DeployTarget = pakiet** argumenty są wymagane w przypadku, gdy tworzysz rozwiązanie Contact Manager. To powoduje, że program MSBuild, aby utworzyć pakiety wdrażania sieci web po utworzeniu każdego projektu aplikacji sieci web zgodnie z opisem w [budowanie i projektów aplikacji sieci Web pakietu](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. **TargetEnvPropsFile** argument jest wymagany, gdy tworzysz *Publish.proj* pliku. Ta właściwość wskazuje lokalizację pliku konfiguracji specyficznych dla środowiska, zgodnie z opisem w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Na **zasady przechowywania** skonfiguruj, jak wiele kompilacji dla każdego typu, aby zachować zgodnie z potrzebami.
17. Kliknij przycisk **Zapisz**.

## <a name="queue-a-build"></a>Kolejkowanie kompilacji

Na tym etapie utworzono przynajmniej jedną nową definicję kompilacji. Proces kompilacji, który zdefiniowano teraz zostanie uruchomiona zgodnie z wyzwalaczy, które określiłeś w definicji kompilacji.

Jeśli skonfigurowano definicję kompilacji, aby użyć elementu konfiguracji, możesz sprawdzić swoją definicję kompilacji, na dwa sposoby:

- Zaewidencjonuj część zawartości do projektu zespołowego w celu wyzwolenia automatycznych kompilacji.
- Ręcznie Kolejkowanie kompilacji.

**Aby ręcznie zakolejkować kompilację**

1. W **Team Explorer** okna, kliknij prawym przyciskiem myszy definicji kompilacji, a następnie kliknij przycisk **Zakolejkuj nową kompilację**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. W **Zakolejkuj kompilację** okno dialogowe, sprawdź właściwości kompilacji, a następnie kliknij **kolejki**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Aby przejrzeć postęp i wyniki kompilacji&#x2014;niezależnie od tego, czy zostało wyzwolone ręcznie lub automatycznie&#x2014;kliknij dwukrotnie definicję kompilacji w **Team Explorer** okna. Spowoduje to otwarcie **Build Explorer** kartę.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

W tym miejscu można rozwiązywać niepowodzenia kompilacji. Dwukrotne kliknięcie jednej z kompilacji można wyświetlić podsumowanie i kliknij, aby szczegółowe pliki dzienników.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Rozwiązywanie problemów z kompilacji nie powiodło się i rozwiązać wszelkie problemy, przed podjęciem próby innej kompilacji, można użyć tych informacji.

> [!NOTE]
> Kompilacje, które są wykonywane logiki wdrożenia prawdopodobnie może zakończyć się niepowodzeniem, dopóki serwer kompilacji udzielono wszystkie uprawnienia wymagane w środowisku docelowym. Aby uzyskać więcej informacji, zobacz [Konfigurowanie uprawnień dla wdrożenia kompilacji zespołu](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorowanie procesu kompilacji

TFS udostępnia szeroki zakres funkcji, które ułatwiają monitorowanie procesu kompilacji. Na przykład TFS można wysyłać wiadomości e-mail lub wyświetlić alerty w obszarze powiadomień paska zadań po zakończeniu kompilacji. Aby uzyskać więcej informacji, zobacz [uruchamiania i monitorowanie kompilacji](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób tworzenia definicji kompilacji w programie TFS. Definicja kompilacji jest skonfigurowana do ciągłej integracji, aby proces kompilacji jest uruchamiany w każdym przypadku, gdy programista zaewidencjonuje zawartości do projektu zespołowego. Definicja kompilacji wykonuje niestandardowego pliku projektu MSBuild do wdrażania pakietów sieci web i skrypty bazy danych do środowiska serwera docelowego.

Aby automatycznego wdrażania została wykonana pomyślnie, jako część procesu kompilacji należy przyznać odpowiednie uprawnienia do konta usługi kompilacji na serwerach sieci web docelowego i docelowy serwer bazy danych. Ostatnim temacie w tym samouczku [Konfigurowanie uprawnień dla wdrożenia kompilacji zespołu](configuring-permissions-for-team-build-deployment.md), w tym artykule opisano sposób identyfikowania i konfigurowania uprawnień wymaganych do automatycznego wdrażania z serwera Team Build.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji, zobacz [tworzenie Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) i [zdefiniować proces kompilacji](https://msdn.microsoft.com/library/ms181715.aspx). Aby uzyskać więcej wskazówek dotyczących kolejkowania kompilacji, zobacz [kolejkować kompilację](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-tfs-build-server-for-web-deployment.md)
> [dalej](deploying-a-specific-build.md)
