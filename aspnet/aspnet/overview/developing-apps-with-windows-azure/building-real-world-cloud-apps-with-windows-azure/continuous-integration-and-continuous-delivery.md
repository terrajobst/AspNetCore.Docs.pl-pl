---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: "Ciągłej integracji i ciągłego dostarczania (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4a5433a7dd70e27b59163822ba427b026c3f4ce0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Ciągłej integracji i ciągłego dostarczania (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Dwa pierwsze zalecane zostały wzorce procesu projektowania [zautomatyzować wszystko](automate-everything.md) i [kontroli źródła](source-control.md), i łączy je w trzecim wzorzec procesu. Ciągłej integracji (CI) oznacza, że zawsze, gdy projektant sprawdza się w kodzie do repozytorium źródłowe, kompilacja automatycznie zostanie wywołany. Ciągłego dostarczania (CD) przyjmuje jednym kroku dalsze: po pomyślnej kompilacji i testów jednostkowych zautomatyzowane, automatycznego wdrażania aplikacji w środowisku, gdzie są bardziej szczegółowe testowanie.

Chmura umożliwia minimalizuje to koszt obsługi środowiska testowego, ponieważ płacisz tylko za zasoby środowiska tak długo, jak używasz je. Proces CD można zdefiniować środowiska testowego, gdy zajdzie taka potrzeba, a środowiska można wyłączyć po zakończeniu testowania.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Ciągłej integracji i ciągłego dostarczania przepływu pracy

Ogólnie zaleca się wykonanie ciągłego dostarczania danych do programowania i środowiska przejściowe. Większości zespołów, nawet w firmie Microsoft, wymagają ręcznego procesu przeglądu i zatwierdzania w przypadku wdrożenia produkcyjnego. W środowisku produkcyjnym warto upewnić, że wdrożenie odbywa się podczas kluczowych pracowników z zespołu programistów dostępnych dla pomocy technicznej lub okresach mniejszym natężeniu ruchu. Ale ma nic, aby zapobiec całkowicie automatyzacji środowiska projektowania i testowania, dzięki czemu deweloper musi wykonać wszystkie zaewidencjonować zmiany i środowiska jest skonfigurowane do testowania akceptacji.

Na poniższym diagramie z [Microsoft Patterns and Practices Książka elektroniczna o ciągłego dostarczania](http://aka.ms/ReleasePipeline) przedstawiono typowy przepływ pracy. Kliknij obraz, aby zobaczyć jego pełnym rozmiarze w jego oryginalnej kontekstu.

[![Ciągłego dostarczania przepływu pracy](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Jak chmury umożliwia tworzenie ekonomicznych CI i dysku CD

Automatyzowanie te procesy na platformie Azure jest bardzo proste. Ponieważ używasz wszystko w chmurze, nie trzeba kupować i zarządzać serwerami dla kompilacji lub środowiska testowego. I nie trzeba czekać na serwerze można było wykonać test na. Przy każdej kompilacji, które należy wykonać można pokrętła środowiska testowego na platformie Azure przy użyciu skryptu automatyzacji, akceptacji wykonywania testów lub więcej testów szczegółowe na nim, a następnie po wykonaniu tych właśnie zerwanie go. I po uruchomieniu tego serwera tylko przez 2 godziny lub 8 godzin lub dziennie, kwotę, które trzeba płacić za jego minimalny, ponieważ użytkownik opłaca tylko raz, faktycznie uruchomioną maszynę. Na przykład środowiska wymagane przez tę poprawkę aplikacji zasadniczo koszt wynosi około 1% na godzinę przejście jedną warstwę z poziom wolnego. W ciągu miesiąca po przeprowadzeniu tylko środowiska na godzinę w czasie, środowiska testowego będzie prawdopodobnie tańszy niż latte, który można kupić w Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS zapewnia szereg funkcji, aby ułatwić projektowanie aplikacji z planowania wdrożenia.

- Obsługuje ona zarówno Git (rozproszone) i TFVC (scentralizowane) do kontroli źródła.
- Oferuje usługi kompilacji elastyczne, co oznacza dynamicznie tworzy serwery kompilacji, gdy są potrzebne i przeniesiony w dół po zakończeniu instalacji. Można automatycznie rozpocząć poza kompilacji podczas kontroli w zmiany kodu źródłowego i nie trzeba mieć przydzielić i opłacać własnych serwerów kompilacji, które znajdują się bezczynności w większości przypadków. Usługa kompilacji jest bezpłatna, dopóki nie może przekraczać określonej liczby kompilacji. Jeśli do dużej liczby kompilacji, można opłacać małego dodatkowe serwery kompilacji zastrzeżone.
- Obsługuje ona ciągłego dostarczania danych do platformy Azure.
- Obsługuje ona testów automatycznych obciążenia. Testów obciążenia jest krytyczne dla aplikacji w chmurze, ale jest często zaniedbania, dopóki nie jest za późno. Testów obciążenia symuluje intensywnie korzysta z aplikacji przez tysięcy użytkowników, dzięki któremu można znaleźć wąskich gardeł i zwiększyć przepustowość — przed udostępnieniem aplikacji do środowiska produkcyjnego.
- Współpraca pokoju zespołu, ułatwiający komunikację w czasie rzeczywistym i współpracy dla małych zespołów agile go obsługuje.
- Obsługuje ona zwinnego zarządzania projektami.


Aby uzyskać więcej informacji o ciągłej integracji i dostarczania funkcje programu VSTS, zobacz [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Jeśli szukasz zarządzania projektu gotowe współpraca z zespołem i rozwiązanie do kontroli źródła, zapoznaj się z usługi VSTS. Usługa jest bezpłatna dla użytkowników do 5, a można założyć dla niego w [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Podsumowanie

Pierwszy wzorce programowania trzy chmury zostały temat należy wdrożyć proces powtarzalne, niezawodnych, przewidywalnych programowanie z niskim cyklu. W [następnego rozdziału](web-development-best-practices.md) Rozpoczniemy przyjrzeć się wzorce architektury i kodowania.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji sieci web w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Zobacz też następujące zasoby:

- [Tworzenie potoku wersji z Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Książka elektroniczna, materiały laboratoriów i przykładowy kod przez Microsoft Patterns and Practices, szczegółowe wprowadzenie do ciągłego dostarczania. Obejmuje korzystanie z programu Visual Studio Lab Management i Visual Studio Release Management.
- [DevOps Rangers ALM narzędzi i wskazówek](https://aka.ms/vsarsolutions/). ALM Rangers wprowadzone DevOps Workbench przykładowe pomocnika rozwiązanie i praktyczne wskazówki we współpracy z wzorców &amp; Podręcznik rozwiązań *tworzenie potoku wersji z programem TFS 2012*, jako doskonały sposób na rozpoczęcie Learning pojęcia DevOps &amp; Release Management do TFS 2012 i rozpocząć testowany. Wskazówki pokazano, jak utworzyć aplikację raz i wdrożyć w wielu środowiskach.
- [Testowanie pod kątem ciągłego dostarczania w programie Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Książka elektroniczna przez Microsoft Patterns and Practices, wyjaśniono, jak zintegrować testów automatycznych z ciągłego dostarczania.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Kod źródłowy narzędzie przeznaczone do przechwytywania kompilacji z TFS (oparte na etykiecie), jego tworzenia, pakiet go umożliwić innym osobom w roli DevOps, aby skonfigurować określone aspekty go i wypchnąć go na platformie Azure. Narzędzie do śledzenia procesu wdrażania w celu umożliwienia operacji "Przywracanie" do wersji wdrożonej wcześniej. To narzędzie nie ma zależności zewnętrznych i mogą działać z autonomicznej przy użyciu interfejsów API TFS i zestawu Azure SDK.
- [Ciągłego dostarczania: Niezawodnej oprogramowania wydawanych przez proces kompilacji, testów i automatyzacji wdrażania](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Podręcznik przez Jez skromny.
- [Zwolnij go! Projektowanie i wdrażanie oprogramowania gotowe do produkcji](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Podręcznik przez Michael T. Nygard.

>[!div class="step-by-step"]
[Poprzednie](source-control.md)
[dalej](web-development-best-practices.md)
