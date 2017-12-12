---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: "Rozwiązanie kontaktów Menedżerze | Dokumentacja firmy Microsoft"
author: jrjlee
description: "Przykładowe rozwiązanie & #x 2014; rozwiązania z menedżerem skontaktuj się z & #x 2014; do reprezentowania aplikacji skali przedsiębiorstwa z realistyczne leve korzysta z tej serii samouczków..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b7f691a1ee855788f6a57616aea35d960e4c85c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="the-contact-manager-solution"></a>Rozwiązania z menedżerem kontaktu
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> To [serii samouczków](web-deployment-in-the-enterprise.md) używa przykładowe rozwiązanie & #x 2014; rozwiązania z menedżerem skontaktuj się z & #x 2014; do reprezentowania aplikacji skali przedsiębiorstwa z realistyczne poziom złożoności. W tym temacie przedstawiono rozwiązanie Menedżera skontaktuj się z, opisano najważniejsze składniki rozwiązania i identyfikuje wyzwania związane z wdrażaniem tego rodzaju aplikacji na różnych platformach docelowy w środowisku przedsiębiorstwa.
> 
> Podczas pracy z tematami w tych samouczkach służy rozwiązania kontaktów Menedżerze jako implementację odwołania, który pokazuje, jak można spełnić określonych wyzwania w scenariuszach wdrażania w przedsiębiorstwie. Następnym temacie [ustawienia zapasowej skontaktuj się z rozwiązania z menedżerem](setting-up-the-contact-manager-solution.md), opisano, jak pobrać i uruchomić rozwiązania na stacji roboczej developer.


## <a name="solution-overview"></a>Omówienie rozwiązania

Rozwiązanie kontaktów Menedżerze składa się z czterech poszczególnych projektów:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Jest to projekt aplikacji sieci web platformy ASP.NET MVC 3, który reprezentuje punkt wejścia dla rozwiązania. Zapewnia ona niektórych funkcji aplikacji sieci web podstawowych, takich jak zapewniając użytkownikom możliwość można tworzyć i wyświetlać szczegóły dotyczące kontaktu. Aplikacja korzysta z usługi Windows Communication Foundation (WCF) do zarządzania kontaktów i aplikacji usługi bazy danych programu ASP.NET do zarządzania, uwierzytelniania i autoryzacji.
- **ContactManager.Database**. Jest to projekt bazy danych programu Visual Studio. Projekt definiuje schemat bazy danych, dane kontaktowe magazynów.
- **ContactManager.Service**. Jest to projekt usługi sieci web WCF. Udostępnia usługi WCF, utworzyć punktu końcowego, który umożliwia obiekty wywołujące do wykonania, pobrać, aktualizowania i usuwania (CRUD) operacji na **ContactManager** bazy danych. Usługa zależy od **ContactManager** bazy danych i **ContactManager.Common.dll** zestawu.
- **ContactManager.Common**. Jest to projektu biblioteki klas. Usługi WCF, zależy od typów zdefiniowanych w tym zestawie.

Rozwiązanie zawiera również folder rozwiązania o nazwie publikowania. Zawiera różne pliki projektu niestandardowe i pliki poleceń, które pokazują, jak można kontrolować i manipulowania procesem kompilacji i wdrażania. Te są opisane bardziej szczegółowo w dalszej części tego samouczka.

Na poziomie pojęciach składniki rozwiązania dopasowania następująco:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Gdy aplikacja sieci web platformy ASP.NET MVC 3 używa dostawcy członkostwa ASP.NET, wszystkich stron w aplikacji sieci web zezwala na dostęp anonimowy. To wyraźnie nie jest realistyczne konfiguracji. Jednak rozwiązanie jest skonfigurowane w taki sposób, aby ułatwić do wdrożenia i przetestowania rozwiązania bez konfigurowania kont użytkowników i ról.


## <a name="deployment-challenges"></a>Związane z wdrożeniem

Rozwiązanie kontaktów Menedżerze przedstawiono kilka wyzwań wdrożenia, które są wspólne dla wiele scenariuszy wdrażania w przedsiębiorstwie:

- Rozwiązania składa się z wielu projektów zależnych. Należy wdrożyć te projekty jednocześnie.
- Parametry połączenia i punktów końcowych usługi muszą zostać zaktualizowane dla każdego środowiska, a w wielu przypadkach te informacje nie będą dostępne przez dewelopera.
- Podczas wdrażania **ContactManager** bazy danych do środowisk przemieszczania i produkcji, należy zachować istniejące dane na temat kolejnych wdrożeń.
- Podczas wdrażania bazy danych usług aplikacji ASP.NET, należy wdrożyć niektóre dane konfiguracji, ale pominąć wszystkie dane konta użytkownika.
- Projekty obejmują niektórych plików i folderów, które nie powinny zostać wdrożone. Należy wyłączyć te pliki i foldery z procesu wdrażania.
- Rozwiązanie musi do obsługi automatycznego wdrażania z serwera kompilacji Team Foundation Server (TFS).

## <a name="conclusion"></a>Wniosek

W tym temacie podano ogólne omówienie rozwiązania kontaktów Menedżerze i zidentyfikować niektóre wyzwania związane wdrożenia, które są wspólne dla wiele scenariuszy wdrażania w przedsiębiorstwie. Pozostałe tematy w tym samouczku opisano niektóre z metod, które można użyć, aby spełnić te problemy.

Następnym temacie [ustawienia zapasowej skontaktuj się z rozwiązania z menedżerem](setting-up-the-contact-manager-solution.md), opisano, jak pobrać i uruchomić rozwiązania na stacji roboczej developer.

>[!div class="step-by-step"]
[Poprzednie](web-deployment-in-the-enterprise.md)
[dalej](setting-up-the-contact-manager-solution.md)
