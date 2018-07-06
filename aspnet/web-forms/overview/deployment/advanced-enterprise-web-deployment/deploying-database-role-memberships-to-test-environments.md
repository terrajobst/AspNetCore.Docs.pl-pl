---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Wdrażanie członkostw roli bazy danych w środowiskach testowych | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób dodawania kont użytkowników do ról bazy danych jako część wdrożenia rozwiązania w środowisku testowym. Podczas wdrażania rozwiązania zawierającego...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a690d99df7a19c422fb217544ec183c311d1796f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828036"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Wdrażanie członkostw roli bazy danych w środowiskach testowych
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób dodawania kont użytkowników do ról bazy danych jako część wdrożenia rozwiązania w środowisku testowym.
> 
> Podczas wdrażania rozwiązania zawierającego projekt bazy danych w środowisku tymczasowym lub produkcyjnym zwykle nie chcesz dla deweloperów, aby zautomatyzować dodawanie kont użytkowników do ról bazy danych. W większości przypadków deweloper nie będzie wiadomo, które konta użytkowników, należy dodać do ról bazy danych, a tych wymagań można zmienić w dowolnym momencie. Jednak podczas wdrażania rozwiązania zawierającego projekt bazy danych do rozwoju lub środowiska testowego, ta sytuacja jest zazwyczaj zamiast innego:
> 
> - Deweloper zazwyczaj ponownie wdraża rozwiązanie na bieżąco, często kilka razy dziennie.
> - Bazy danych ponownie jest zwykle tworzony przy każdym wdrożeniu, co oznacza, że użytkownicy bazy danych muszą być tworzone i dodawane do ról po każdym wdrożeniu.
> - Deweloper zazwyczaj ma pełną kontrolę nad środowiska deweloperskie lub testowe docelowego.
> 
> W tym scenariuszu jest często korzystne automatycznie utworzyć bazy danych użytkowników i przypisywanie członkostw roli bazy danych jako część procesu wdrażania.
> 
> Kluczowym czynnikiem jest to, że ta operacja musi być warunkowych opartych na środowisku docelowym. Jeśli wdrażasz tymczasowym lub w środowisku produkcyjnym, należy pominąć operację. Jeśli jest wdrażany z deweloperem lub środowiska testowego, którą chcesz wdrożyć członkostwa w roli bez dalszej interwencji. W tym temacie opisano jeden podejścia, którego można użyć, aby rozwiązać ten problem.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

W tym temacie założono, że:

- Użyj podejście split projektu plików do wdrożenia rozwiązania, zgodnie z opisem w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Wywoływanie VSDBCMD z pliku projektu, aby wdrożyć projekt bazy danych, zgodnie z opisem w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Tworzenie użytkowników bazy danych i przypisywanie członkostw roli podczas wdrażania projektu bazy danych w środowisku testowym, należy:

- Utwórz skrypt Transact Structured Query Language (język Transact-SQL), który sprawia, że zmiany w bazie danych to konieczne.
- Utwórz obiekt docelowy aparatu Microsoft Build Engine (MSBuild), który używa narzędzia sqlcmd.exe, aby uruchomić skrypt SQL.
- Konfigurowanie plików projektu, aby wywołać element docelowy podczas wdrażania rozwiązania w środowisku testowym.

W tym temacie pokazują sposób wykonywania każdego z tych procedur.

## <a name="scripting-the-database-role-memberships"></a>Obsługa skryptów członkostw roli bazy danych

Wiele różnych sposobów, można utworzyć skrypt języka Transact-SQL i w dowolnym miejscu możesz wybrać. Jest to najłatwiejsza metoda do tworzenia skryptów w ramach rozwiązania usługi w programie Visual Studio 2010.

**Aby utworzyć skrypt SQL**

1. W **Eksploratora rozwiązań** oknie rozwinięciu węzła projektu bazy danych.
2. Kliknij prawym przyciskiem myszy **skrypty** folderu, wskaż **Dodaj**, a następnie kliknij przycisk **nowy Folder**.
3. Typ **testu** jako nazwa folderu, a następnie naciśnij klawisz Enter.
4. Kliknij prawym przyciskiem myszy **testu** folderu, wskaż **Dodaj**, a następnie kliknij przycisk **skryptu**.
5. W **Dodaj nowy element** okna dialogowego pole, nadaj nazwę opisową skryptu (na przykład **AddRoleMemberships.sql**), a następnie kliknij przycisk **Dodaj**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. W *AddRoleMemberships.sql* plików, Dodawanie instrukcji języka Transact-SQL:

    1. Utwórz użytkownika bazy danych dla identyfikatora logowania programu SQL Server, który będzie miał dostęp bazy danych.
    2. Dodaj użytkownika bazy danych do żadnej roli wymaganej bazy danych.
7. Plik powinien wyglądać następująco:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Zapisz plik.

## <a name="executing-the-script-on-the-target-database"></a>Wykonanie skryptu do docelowej bazy danych

W idealnym przypadku należy uruchomić wszystkie wymagane skrypty języka Transact-SQL w ramach skryptu powdrożeniowego podczas wdrażania projektu bazy danych. Jednak skrypty powdrożeniowe nie pozwalają na wykonywanie logiki warunkowe na podstawie konfiguracje rozwiązania lub właściwości kompilacji. Alternatywą jest uruchamiać skrypty SQL bezpośrednio w pliku projektu MSBuild, tworząc **docelowej** element, który wykonuje polecenie sqlcmd.exe. Można użyć tego polecenia, aby uruchomić skrypt do docelowej bazy danych:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia sqlcmd, zobacz [Narzędzie sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Zanim to polecenie można osadzić w obiekt docelowy programu MSBuild, należy wziąć pod uwagę pod jakimi warunkami ma skrypt do uruchomienia:

- Docelowa baza danych musi istnieć przed zmianą jego członkostwa w roli. Jako takie, musisz uruchomić ten skrypt *po* wdrożenie bazy danych.
- Musisz obejmują warunku, dlatego, że skrypt jest wykonywane tylko dla środowisk testowych.
- Jeśli korzystasz z wdrożenia "what if"&#x2014;innymi słowy, jeśli masz generowania skryptów wdrażania, ale nie uruchomione ich&#x2014;nie należy uruchomić skrypt SQL.

Jeśli używasz podziału podejście pliku projektu, opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), jak pokazano w przykładowe rozwiązanie Contact Manager, można podzielić instrukcje kompilacji skryptu SQL następująco:

- Wszystkie wymagane właściwości specyficzne dla środowiska, wraz z właściwość, która określa, czy mają zostać wdrożone uprawnień, należy go w pliku projektu specyficznymi dla środowiska (na przykład *Env Dev.proj*).
- Element docelowy programu MSBuild, wraz z dowolnej właściwości, które nie zmieni się między środowiskami docelowy powinien przeprowadzić w pliku projektu uniwersalnej (na przykład *Publish.proj*).

W pliku projektu specyficznymi dla środowiska należy zdefiniować nazwę serwera bazy danych, nazwa docelowej bazy danych i właściwość typu Boolean, która umożliwia użytkownikowi określenie, czy mają zostać wdrożone członkostwa w roli.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


W pliku projektu uniwersalnej należy podać lokalizację pliku wykonywalnym sqlcmd i lokalizację skryptu SQL, który chcesz uruchomić. Te właściwości pozostaną takie same, niezależnie od tego, w środowisku docelowym. Należy również utworzyć obiekt docelowy programu MSBuild do wykonania polecenia sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Zwróć uwagę, Dodaj lokalizację pliku wykonywalnym sqlcmd jako właściwość statyczna, ponieważ może to być przydatne do innych celów. Z kolei należy zdefiniować lokalizację skryptu SQL i składnia polecenia sqlcmd jako właściwości dynamicznych w ciągu docelowym, ponieważ nie będą wymagane przed docelowy jest wykonywany. W tym przypadku **DeployTestDBPermissions** docelowy będzie można wykonać tylko, jeśli te warunki są spełnione:

- **DeployTestDBRoleMemberships** właściwość jest ustawiona na **true**.
- Użytkownik nie określił **WhatIf = true** flagi.

Na koniec nie zapomnij wywołać element docelowy. W *Publish.proj* pliku, można to zrobić, dodając element docelowy do listy zależności dla domyślnej **FullPublish** docelowej. Należy upewnić się, że **DeployTestDBPermissions** docelowy nie jest wykonywany do momentu **PublishDbPackages** został on wykonany.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Wniosek

W tym temacie opisano jeden ze sposobów, w którym można dodać użytkowników bazy danych i członkostwo w rolach jako akcję powdrożeniową podczas wdrażania projektu bazy danych. Jest to zazwyczaj przydatne w przypadku, gdy regularnie ponownie utworzyć bazę danych w środowisku testowym, ale należy ich unikać zazwyczaj, gdy wdrażanie baz danych w środowiskach przejściowych lub produkcyjnych. Jako takie należy upewnić się, że używasz niezbędne logikę warunkową tak, aby użytkownicy bazy danych i członkostwo w rolach są tworzone tylko moment jest właściwy to zrobić.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat używania VSDBCMD wdrażania projektów bazy danych, zobacz [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać wskazówki dotyczące Dostosowywanie wdrożeń bazy danych dla środowisk inną docelową, zobacz [Dostosowywanie wdrożeń bazy danych dla wielu środowisk](customizing-database-deployments-for-multiple-environments.md). Aby uzyskać więcej informacji na temat korzystania z niestandardowych plików projektu MSBuild do kontrolowania procesu wdrażania, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na temat opcji wiersza polecenia sqlcmd, zobacz [Narzędzie sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Poprzednie](customizing-database-deployments-for-multiple-environments.md)
> [dalej](deploying-membership-databases-to-enterprise-environments.md)
