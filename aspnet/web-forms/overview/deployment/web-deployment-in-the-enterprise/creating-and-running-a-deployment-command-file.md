---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Tworzenie i uruchamianie wdrożenia polecenie Plik | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób tworzenia pliku poleceń, które pozwoli uruchamiać wdrażania przy użyciu plików projektu Microsoft kompilacji Engine (MSBuild) jako pojedynczy krok...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: e5fb034a67bc9f2ea549af269eae51a49acc4d98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891181"
---
<a name="creating-and-running-a-deployment-command-file"></a>Tworzenie i uruchamianie pliku poleceń wdrażania
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób tworzenia pliku poleceń, które pozwoli uruchamiać wdrażania przy użyciu plików projektu Microsoft kompilacji Engine (MSBuild) jako pojedynczy krok i powtarzalnej proces.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [kontaktów Menedżerze](the-contact-manager-solution.md) rozwiązania&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis procesu kompilacji](understanding-the-build-process.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="process-overview"></a>Omówienie procesu

W tym temacie nauczysz się, jak utworzyć i uruchomić plik polecenia używane te pliki projektu do wykonywania powtarzalnych wdrożenia do środowiska docelowego. Zasadniczo pliku polecenia po prostu musi zawierać polecenia programu MSBuild który:

- Informuje MSBuild, aby wykonać niezależny od środowiska *Publish.proj* pliku.
- Określa, że *Publish.proj* pliku, plik, który zawiera ustawienia projektu określonego środowiska i gdzie można znaleźć go.

## <a name="create-an-msbuild-command"></a>Utwórz polecenie MSBuild

Zgodnie z opisem w [opis procesu kompilacji](understanding-the-build-process.md), plik projektu określonego środowiska&#x2014;na przykład *Env Dev.proj*&#x2014;zaprojektowano w celu zaimportowania do środowiska — funkcja *Publish.proj* plik w czasie kompilacji. Te dwa pliki Podaj ze sobą, kompletny zestaw instrukcje określające sposób tworzenia i wdrażania rozwiązania programu MSBuild.

*Publish.proj* plików używa **zaimportować** element, aby zaimportować plik projektu określonego środowiska.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Tak korzystając z programu MSBuild.exe do tworzenia i wdrażania rozwiązania Contact Manager, musisz:

- Uruchom MSBuild.exe na *Publish.proj* pliku.
- Określ lokalizację pliku projektu określonego środowiska, podając parametr wiersza polecenia o nazwie **TargetEnvPropsFile**.

W tym celu polecenia programu MSBuild powinien wyglądać następująco:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


W tym miejscu jest prosty krok, aby przejść do wdrożenia powtarzalne, pojedynczy krok. Wszystko, co należy zrobić to dodanie polecenia programu MSBuild w pliku .cmd. W rozwiązaniu Menedżera skontaktuj się z folderu publikowania zawiera plik o nazwie *Dev.cmd publikowania* wykonuje dokładnie to.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl** przełącznik powoduje, że program MSBuild, aby utworzyć plik dziennika o nazwie *msbuild.log* w katalogu roboczym, w którym wywołano MSBuild.exe.


Aby wdrożyć lub ponownego wdrożenia rozwiązania menedżera kontaktu, wszystko co należy zrobić Uruchom *Dev.cmd publikowania* pliku. Po uruchomieniu pliku będzie MSBuild:

- Tworzenie wszystkich projektów w rozwiązaniu.
- Generowanie pakietów sieci web można wdrożyć dla projektów aplikacji sieci web.
- Generuj pliki .dbschema i .deploymanifest dla projektów bazy danych.
- Wdrażanie pakietów sieci web na serwerze sieci web.
- Wdrażania bazy danych na serwerze bazy danych.

## <a name="run-the-deployment"></a>Uruchom wdrożenie

Po utworzeniu pliku poleceń dla środowiska docelowego, należy wykonać całego wdrożenia po prostu, uruchamiając plik.

**Aby wdrożyć rozwiązanie kontaktów Menedżerze do danego środowiska testowego**

1. Na stacji roboczej dewelopera Otwórz Eksploratora Windows, a następnie przejdź do lokalizacji *Dev.cmd publikowania* pliku.
2. Kliknij dwukrotnie plik, aby go uruchomić.
3. Jeśli **Otwórz plik — ostrzeżenie o zabezpieczeniach** zostanie wyświetlone okno dialogowe, kliknij przycisk **Uruchom**.
4. Jeśli Twoje ustawienia konfiguracji i serwery testu są skonfigurowane poprawnie, okna wiersza polecenia wyświetli **kompilacji zakończyło się pomyślnie** podczas MSBuild zakończył przetwarzanie plików projektu.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Jeśli jest to w tym środowisku wdrożono rozwiązanie po raz pierwszy, musisz dodać konto komputera serwera sieci web testów, aby **db\_datawriter** i **db\_datareader**ról na **ContactManager** bazy danych. Ta procedura jest opisana w [Konfiguracja serwera bazy danych dla wdrożenia publikowania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Należy przypisać te uprawnienia, podczas tworzenia bazy danych. Domyślnie proces kompilacji nie zostanie ponownie bazy danych przy każdym wdrożeniu&#x2014;zamiast tego porównuje istniejącą bazę danych do najnowszej schematu i wprowadź zmiany wymagane. W związku z tym należy tylko należy zamapować te role bazy danych podczas pierwszego wdrażania rozwiązania.
6. Otwórz program Internet Explorer i przejdź do adresu URL aplikacji, skontaktuj się z Menedżera (na przykład `http://testweb1:85/ContactManager/`).
7. Sprawdź, czy aplikacja działa zgodnie z oczekiwaniami i możliwe będzie dodawanie kontaktów.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Wniosek

Tworzenie pliku poleceń instrukcjami MSBuild zapewnia szybki i łatwy sposób tworzenia i wdrażania rozwiązania wielu projektów w środowisku określonego miejsca docelowego. Jeśli potrzebujesz wielokrotnie wdrażać rozwiązania w wielu środowiskach, w miejsce docelowe, można utworzyć wiele plików poleceń. W każdym pliku polecenia polecenie MSBuild utworzy tego samego pliku uniwersalnych projektów, ale będzie określać plik inny projekt określonego środowiska. Na przykład użyć pliku polecenia publikować deweloperem lub środowiska testowego może zawierać tego polecenia programu MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Plik polecenia do publikowania w środowisku przemieszczania mogą zawierać tego polecenia programu MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Aby uzyskać wskazówki dotyczące sposobu dostosowywania pliki projektu określonego środowiska dla środowiska serwera, zobacz [konfigurowania właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Proces kompilacji dla każdego środowiska można również dostosować przez zastępowanie właściwości albo ustawienie różnych przełączników inne polecenia programu MSBuild. Aby uzyskać więcej informacji, zobacz [informacje w wierszu polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Poprzednie](deploying-database-projects.md)
> [dalej](manually-installing-web-packages.md)
