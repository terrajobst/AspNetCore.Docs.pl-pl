---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Tworzenie i uruchamianie wdrożenia plik poleceń | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób tworzenia pliku poleceń, które pozwoli uruchamiać wdrażania przy użyciu aparatu Microsoft Build Engine (MSBuild), pliki projektu w jednym kroku ponownie...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 121df7482f7cbd70b191e518bae791f0642ccc7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756336"
---
<a name="creating-and-running-a-deployment-command-file"></a>Tworzenie i uruchamianie pliku poleceń wdrażania
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób tworzenia pliku polecenia, które pozwoli uruchamiać wdrażania przy użyciu aparatu Microsoft Build Engine (MSBuild), pliki projektu jako pojedynczy krok, powtarzalnego procesu.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [Contact Manager](the-contact-manager-solution.md) rozwiązania&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie procesu kompilacji](understanding-the-build-process.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="process-overview"></a>Omówienie procesu

W tym temacie dowiesz się, jak tworzenie i uruchamianie pliku poleceń, który używa tych plików projektu do wykonywania powtarzalne wdrożenia w środowisku docelowym. Zasadniczo pliku poleceń po prostu musi zawierać polecenia MSBuild który:

- Informuje program MSBuild, aby wykonać niezależnie od środowiska *Publish.proj* pliku.
- Informuje *Publish.proj* pliku, plik, który zawiera ustawienia specyficzne dla środowiska projektu i gdzie można znaleźć go.

## <a name="create-an-msbuild-command"></a>Tworzenie polecenia programu MSBuild

Zgodnie z opisem w [objaśnienie procesu kompilacji](understanding-the-build-process.md), plik projektu o specyficznych dla środowiska&#x2014;na przykład *Env Dev.proj*&#x2014;zaprojektowano w celu zaimportowania do niezależny od środowiska *Publish.proj* plik w czasie kompilacji. Razem te dwa pliki zapewniają kompletny zestaw instrukcji, które kazać programowi MSBuild, jak tworzyć i wdrażać rozwiązania.

*Publish.proj* plików używa **zaimportować** elementu, aby zaimportować plik projektu specyficznego dla danego środowiska.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Jako takie korzystając z MSBuild.exe do tworzenia i wdrażania rozwiązania Contact Manager, należy:

- Uruchamianie MSBuild.exe na *Publish.proj* pliku.
- Określ lokalizację pliku projektu specyficznymi dla środowiska, podając parametr wiersza polecenia o nazwie **TargetEnvPropsFile**.

Aby to zrobić, polecenia MSBuild powinien wyglądać następująco:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


W tym miejscu jest prostym kroku, aby przejść do wdrożenia powtarzalne, pojedynczy krok. Wszystko, co należy zrobić, jest dodanie polecenia MSBuild do pliku .cmd. W przypadku rozwiązania Contact Manager folderu publikowania zawiera plik o nazwie *Dev.cmd Publikuj* dokładnie ten wykonujący.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl** przełącznik powoduje, że program MSBuild utworzy plik dziennika o nazwie *msbuild.log* w katalogu roboczym, w której wywołano MSBuild.exe.


W przypadku wdrażania lub ponownego wdrożenia rozwiązania Contact Manager, wszystko, czego potrzebujesz w celu uruchomienia *Dev.cmd Publikuj* pliku. Po uruchomieniu pliku MSBuild wykonują następujące czynności:

- Kompiluj wszystkie projekty w rozwiązaniu.
- Generuj pakiety do wdrożenia sieci web dla projektów aplikacji sieci web.
- Generowanie plików .dbschema i .deploymanifest dla projektów bazy danych.
- Wdrażanie pakietów internetowych do serwera sieci web.
- Wdrażanie bazy danych na serwerze bazy danych.

## <a name="run-the-deployment"></a>Uruchom wdrożenie

Po utworzeniu pliku poleceń w środowisku docelowym, należy wykonać całe wdrożenie, po prostu uruchamiając plik.

**Aby wdrożyć to rozwiązanie Contact Manager do danego środowiska testowego**

1. Na stacji roboczej dewelopera, otwórz Eksploratora Windows, a następnie przejdź do lokalizacji *Dev.cmd Publikuj* pliku.
2. Kliknij dwukrotnie plik, aby go uruchomić.
3. Jeśli **Otwórz plik — ostrzeżenie o zabezpieczeniach** pojawi się okno dialogowe, kliknij przycisk **Uruchom**.
4. Czy ustawienia konfiguracji i serwerów testu zostały skonfigurowane poprawnie, w oknie wiersza polecenia wyświetli **kompilacja powiodła się** komunikatu, gdy program MSBuild zakończył przetwarzanie plików projektu.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Jeśli po raz pierwszy, udało Ci się wdrożyć rozwiązanie do tego środowiska, musisz dodać konto komputera serwera sieci web test, aby **db\_datawriter** i **db\_datareader**ról na **ContactManager** bazy danych. Ta procedura jest opisana w [Konfigurowanie serwera bazy danych dla wdrożenia publikowania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Musisz przypisać te uprawnienia, podczas tworzenia bazy danych. Domyślnie proces kompilacji nie będzie ponownie utworzyć bazy danych przy każdym wdrożeniu&#x2014;zamiast tego porównuje istniejącą bazę danych do najnowszej schematu i zmiany tylko wymagane. W rezultacie tylko należy zamapować te role bazy danych wdrażania rozwiązania po raz pierwszy.
6. Otwórz program Internet Explorer i przejdź pod adres URL aplikacji Contact Manager (na przykład `http://testweb1:85/ContactManager/`).
7. Sprawdź, czy aplikacja działa zgodnie z oczekiwaniami, i możliwe będzie dodawanie kontaktów.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Wniosek

Tworzenie pliku poleceń, zawierający instrukcje MSBuild oferuje szybki i łatwy sposób tworzenia i wdrażania rozwiązania wielu projektów w środowisku określonego miejsca docelowego. Jeśli zachodzi potrzeba wielokrotnie wdrażać rozwiązania dla wielu środowisk docelowej, można utworzyć wiele plików poleceń. W każdym pliku polecenia polecenie MSBuild utworzy tego samego pliku projektu w wersji uniwersalnej, ale będą wpisywać, plik inny projekt specyficznymi dla środowiska. Na przykład pliku polecenia, aby opublikować z deweloperem lub środowiska testowego może zawierać tego polecenia programu MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Plik poleceń do publikowania w środowisku przejściowym może zawierać tego polecenia programu MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania pliki projektu specyficznego dla środowiska dla środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Zastępowanie właściwości lub ustawiając różnych innymi przełącznikami w poleceniu programu MSBuild, można również dostosować proces kompilacji dla każdego środowiska. Aby uzyskać więcej informacji, zobacz [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Poprzednie](deploying-database-projects.md)
> [dalej](manually-installing-web-packages.md)
