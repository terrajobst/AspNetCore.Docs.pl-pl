---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano najważniejsze kwestie związane z i wyzwania, które będą potrzebne do pokonania po zainicjowaniu obsługi administracyjnej baz danych usług aplikacji ASP.NET (częściej stosowana...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 307375843c51f31d3d8ae0f2ef0a17a3e58d3a64
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752870"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano najważniejsze kwestie i wyzwania, że będzie potrzebny do pokonania, kiedy aprowizowanie aplikacji ASP.NET usług baz danych (określone częściej baz danych członkostwa) w środowiskach testowych, przejściowych lub produkcyjnych. Zawiera również opis metod, których można użyć, aby spełnić te wyzwania.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Co to są problemy podczas wdrażania bazy danych członkostwa?

W większości przypadków gdy opracowanie strategii wdrażania dla bazy danych, pierwszą rzeczą, jaką należy wziąć pod uwagę jest jakie dane mają zostać wdrożone. W środowisku deweloperskim lub testowym można wdrożyć dane konta użytkownika, aby ułatwić szybkie i łatwe testowanie. W środowisku tymczasowym czy produkcyjnym jest bardzo mało prawdopodobne, że chcesz wdrożyć danych konta użytkownika.

Niestety baz danych członkostwa ASP.NET wprowadzenie niektórych określonych wyzwania, które podjęciu tej decyzji o wiele bardziej złożonych:

- Wdrożenie tylko schematy spowoduje pozostawienie bazy danych członkostwa w działającym stanie. Jest to spowodowane członkowskiej bazie danych zawiera dane konfiguracyjne (w **aspnet\_SchemaVersions** tabeli), bazy danych wymaga, aby funkcjonować. W efekcie wykonujesz wdrożenie tylko do schematu bazy danych członkostwa w celu wykluczenia dane konto użytkownika, należy uruchomić skrypt po wdrożeniu, aby dodać dane konfiguracji programu essential.
- W zależności od sposobu skonfigurowania bazy danych członkostwa dostawcy członkostwa może używać klucza komputera w celu szyfrowania haseł i przechowywać je w bazie danych. W tym przypadku danych z konta użytkownika, wdrożone z bazą danych staje się bezużyteczny na serwerze docelowym. Z tego powodu wdrażania danych konta użytkownika nie jest obsługiwanym scenariuszem.

## <a name="choosing-a-membership-database-strategy"></a>Wybieranie strategii bazy danych członkostwa

W przypadku wybrania, jak wykonać aprowizację bazy danych członkostwa w środowisku przedsiębiorstwa serwera, użyj następujących wytycznych:

- Wszędzie tam, gdzie to możliwe, nie należy wdrażać baz danych członkostwa. Zamiast tego należy utworzyć bazy danych członkostwa ręcznie na docelowym serwerze bazy danych. Jeśli nie zostały dostosowane schemat bazy danych członkostwa, możesz po prostu utworzyć nowe konto na miejscu na docelowym przy użyciu [narzędzia rejestracji serwera SQL platformy ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Jeśli nie jest dostępna opcja, ale aby wdrożyć bazę danych członkostwa&#x2014;na przykład, jeśli wprowadzono rozległych modyfikacji schematu bazy danych jest&#x2014;powinien wykonać wdrożenie tylko do schematu bazy danych członkostwa do wykluczenia dane konto użytkownika, a następnie Uruchom skrypt po wdrożeniu, aby dodać wszelkie dane konfiguracyjne wymagane. Szeroka wskazówki można znaleźć w tych metod w [jak: wdrożyć ASP.NET członkostwa bazy danych bez łącznie z kont użytkowników](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Należy pamiętać, że *schematu członkowskiej bazie danych prawdopodobnie może być stosunkowo statycznych*. Nawet jeśli dostosowano bazy danych członkostwa jest mało prawdopodobne, że musisz zaktualizować schemat w regularnych odstępach czasu&#x2014;nie będzie można zmienić za pomocą taką samą częstotliwością, jak kod w aplikacji sieci web lub projektu bazy danych. Jako takie nie należy dołączyć wszystkie procesy zautomatyzowane lub pojedynczy krok wdrażania bazy danych członkostwa.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Aktualizowanie schematu bazy danych członkostwa przy użyciu VSDBCMD

Jeśli zmodyfikujesz struktury bazy danych członkostwa po wdrożeniu pierwszego może nie chcieć użyć narzędzia wdrażania usług Internet Information Services (IIS) w sieci Web (Web Deploy) do ponownego wdrożenia bazy danych. Funkcja wdrażania bazy danych w narzędzia Web Deploy nie obejmuje możliwości, aby wprowadzić aktualizacje różnicowych docelowej bazy danych&#x2014;zamiast tego narzędzia Web Deploy należy porzucić i ponownie utworzyć bazy danych. Oznacza to, że tracisz istniejących danych konta użytkownika, który jest zazwyczaj niepożądanych w środowiskach przejściowych lub produkcyjnych.

Alternatywą jest użycie narzędzia VSDBCMD do zaktualizowania schematu docelowej bazy danych. VSDBCMD obejmuje dwie ważne funkcje. Po pierwsze można było zaimportować schematu istniejącą bazę danych do pliku .dbschema. Po drugie jego można wdrożyć pliku .dbschema istniejącą bazę danych jako różnicowej aktualizację, co oznacza, że wykonuje tylko zmiany potrzebne do docelowej bazy danych na bieżąco, a nie utracić wszystkie dane.

Następujące ogólne kroki można użyć do zaktualizowania schematu bazy danych członkostwa:

1. Użyj VSDBCMD **importu** akcji, aby wygenerować plik .dbschema dla źródłowej bazy danych członkostwa. Ta procedura jest opisana w [porady: Importowanie schematu w wierszu polecenia](https://msdn.microsoft.com/library/dd172135.aspx).
2. Użyj VSDBCMD **Wdróż** akcji, aby wdrożyć plik .dbschema do docelowej bazy danych członkostwa. Ta procedura jest opisana w [VSDBCMD dokumentacja wiersza polecenia. Plik EXE (wdrożenia i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano niektóre wyzwania, które mogą się spodziewać, gdy trzeba aprowizować baz danych członkostwa ASP.NET w różnych środowiskach docelowych. W szczególności wyjaśnić, dlaczego tylko schematy wdrożenia spowoduje pozostawienie bazy danych członkostwa w działającym stanie i dlaczego wdrażanie danych konta użytkownika nie jest obsługiwane. Temat również przedstawione wskazówki na temat sposobu obsługi administracyjnej, wdrażanie i aktualizowanie baz danych członkostwa w różnych scenariuszach.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej wskazówki i przykłady dotyczące używania VSDBCMD, zobacz [VSDBCMD dokumentacja wiersza polecenia. Plik EXE (wdrożenia i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx) i [porady: Importowanie schematu w wierszu polecenia](https://msdn.microsoft.com/library/dd172135.aspx). Aby uzyskać więcej informacji na temat korzystania z aspnet\_regsql.exe do tworzenia baz danych członkostwa, zobacz [narzędzia rejestracji serwera SQL platformy ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Aby uzyskać bardziej ogólne wskazówki na temat wdrażania baz danych członkostwa, zobacz [jak: wdrożyć ASP.NET członkostwa bazy danych bez łącznie z kont użytkowników](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Poprzednie](deploying-database-role-memberships-to-test-environments.md)
> [dalej](excluding-files-and-folders-from-deployment.md)
