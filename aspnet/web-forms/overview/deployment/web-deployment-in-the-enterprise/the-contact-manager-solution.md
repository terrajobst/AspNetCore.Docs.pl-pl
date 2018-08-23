---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Rozwiązanie Contact Manager | Dokumentacja firmy Microsoft
author: jrjlee
description: Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014;rozwiązania Contact Manager&#x2014;do reprezentowania aplikacji skali przedsiębiorstwa przy użyciu realistycznej leve...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: bc2fe88f4566d9fa144abcd2239f9008a1aefe83
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753088"
---
<a name="the-contact-manager-solution"></a>Rozwiązanie Contact Manager
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> To [serii samouczków](web-deployment-in-the-enterprise.md) używa rozwiązania przykładowe&#x2014;rozwiązania Contact Manager&#x2014;do reprezentowania aplikacji skali przedsiębiorstwa przy użyciu realistycznej stopień złożoności. W tym temacie przedstawiono rozwiązanie Contact Manager, w tym artykule opisano najważniejsze składniki rozwiązania i identyfikuje wyzwania we wdrażaniu tego rodzaju aplikacji na różnych platformach docelowy w środowisku przedsiębiorstwa.
> 
> Podczas pracy z tematami w tych samouczkach, można użyć rozwiązania Contact Manager jako implementację referencyjną, który demonstruje, jak spełniasz specyficznych trudności związanych w scenariuszach wdrażania w przedsiębiorstwie. Następny temat [ustawienie zapasowej rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md), w tym artykule opisano sposób pobierania i uruchom rozwiązanie na stacji roboczej dewelopera.


## <a name="solution-overview"></a>Omówienie rozwiązania

Rozwiązanie Contact Manager składa się z czterech poszczególnych projektów:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Jest to projektu aplikacji sieci web ASP.NET MVC 3, który reprezentuje punkt wejścia dla rozwiązania. Oferuje ona niektóre funkcje aplikacji web podstawowych, takich jak zapewniając użytkownikom możliwość tworzenia i wyświetlania szczegółów dotyczących kontaktu ds. Aplikacja korzysta z usługi Windows Communication Foundation (WCF) do zarządzania kontaktami i aplikacji usługi bazy danych programu ASP.NET, aby zarządzać uwierzytelnianiem i autoryzacją.
- **ContactManager.Database**. Jest to projekt bazy danych programu Visual Studio. Projekt definiuje schemat bazy danych, dane kontaktowe magazynów.
- **ContactManager.Service**. Jest to projekt usługi sieci web WCF. Udostępnia usługi WCF, utworzyć punkt końcowy, który umożliwia obiektom wywołującym do wykonania, pobieranie, aktualizowanie i usuwanie operacji (CRUD) na **ContactManager** bazy danych. Usługa zależy od **ContactManager** bazy danych i **ContactManager.Common.dll** zestawu.
- **ContactManager.Common**. Jest to projekt biblioteki klas. Usługa WCF opiera się na typy zdefiniowane w tym zestawie.

Rozwiązanie obejmuje również folder rozwiązania o nazwie publikowania. Zawiera różne pliki projektów niestandardowych i pliki poleceń, które pokazują, jak można kontrolować i manipulowania procesu kompilacji i wdrażania. Są one omówione bardziej szczegółowo w dalszej części tego samouczka.

Na poziomie koncepcyjny rozwiązania wzajemne dopasowanie składników następująco:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Gdy aplikacja sieci web ASP.NET MVC 3 używa dostawcy członkostwa platformy ASP.NET, wszystkich stron w aplikacji sieci web zezwala na dostęp anonimowy. To wyraźnie nie jest realistyczne konfiguracji. Jednak rozwiązanie jest skonfigurowana tak, w ten sposób, aby ułatwić wdrażanie i testowanie rozwiązania bez konieczności konfigurowania kont użytkowników i ról.


## <a name="deployment-challenges"></a>Wyzwania związane z wdrożeniem

Rozwiązanie Contact Manager ilustruje kilka wyzwania związane z wdrożeniem, które są wspólne dla wielu scenariuszy wdrażania w przedsiębiorstwie:

- To rozwiązanie składa się z wielu projektów zależnych. Należy wdrożyć te projekty jednocześnie.
- Parametry połączenia i punktów końcowych usługi muszą zostać zaktualizowane dla każdego środowiska, a w wielu przypadkach te informacje nie będą dostępne dla dewelopera.
- Podczas wdrażania **ContactManager** bazy danych do środowisk przemieszczania i produkcji, należy zachować istniejące dane na kolejne wdrożenia.
- Podczas wdrażania bazy danych usług aplikacji ASP.NET, należy wdrożyć dane konfiguracji, ale pominięto żadnych danych konta użytkownika.
- Projekty obejmują niektórych plików i folderów, które nie powinny być wdrażane. Należy wyłączyć te pliki i foldery z procesu wdrażania.
- Rozwiązanie musi do obsługi automatycznego wdrażania z serwera kompilacji Team Foundation Server (TFS).

## <a name="conclusion"></a>Wniosek

W tym temacie podano ogólne omówienie rozwiązania Contact Manager i zidentyfikować niektóre wyzwania związane z używaniem wdrożeniem, które są wspólne dla wielu scenariuszy wdrażania w przedsiębiorstwie. Pozostałe tematy w tym samouczku opisano niektóre z metod, które można użyć, aby spełnić te wyzwania.

Następny temat [ustawienie zapasowej rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md), w tym artykule opisano sposób pobierania i uruchom rozwiązanie na stacji roboczej dewelopera.

> [!div class="step-by-step"]
> [Poprzednie](web-deployment-in-the-enterprise.md)
> [dalej](setting-up-the-contact-manager-solution.md)
