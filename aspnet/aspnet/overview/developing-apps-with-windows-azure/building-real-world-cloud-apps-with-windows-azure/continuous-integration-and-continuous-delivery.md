---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Ciągła integracja i ciągłe dostarczanie (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 787aacfd843f5f72e567670d601fb036a2c474bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754622"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Ciągła integracja i ciągłe dostarczanie (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Dwa pierwsze zaleca wzorce procesu programowania zostały [Automatyzowanie wszystkiego](automate-everything.md) i [kontroli źródła](source-control.md), i łączy je w trzecim wzorzec procesu. Ciągłą integrację (CI) oznacza, że zawsze wtedy, gdy programista zaewidencjonuje kodu do repozytorium źródłowego, kompilacja zostaje automatycznie wyzwolona. Ciągłe dostarczanie (CD) zajmuje to jeden krok dalej: po pomyślnej kompilacji i automatyczne testy jednostkowe, automatyczne wdrażanie aplikacji w środowisku, w których wykonują bardziej szczegółowe testowanie.

Chmura umożliwia zminimalizowanie kosztów utrzymania środowiska testowego, ponieważ płacisz tylko zasobów środowiska tak długo, jak jest używany. Proces ciągłego Dostarczania można skonfigurować środowisko testowe, gdy będą potrzebne, a może zająć szczegółów środowiska, gdy wszystko będzie gotowe testowania.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Przepływ pracy ciągłej integracji i ciągłego dostarczania

Ogólnie zaleca się wykonanie ciągłe dostarczanie do tworzenia i środowisk przejściowych. Większość zespołów, nawet w firmie Microsoft wymagają ręcznego procesu przeglądu i zatwierdzania wdrożenia produkcyjnego. W środowisku produkcyjnym warto upewnić się, że wdrożenie to następuje najważniejszych osób w zespole rozwoju dostępnych dla pomocy technicznej lub w okresach o małym natężeniu ruchu. Ale ma nic do uniemożliwić całkowicie Automatyzacja środowiska deweloperskie i testowe, tak, aby wszystko, co deweloper musi wykonać zaewidencjonować zmianę oraz środowisko zostało skonfigurowane do testy odbiorcze.

Na poniższym diagramie z [Microsoft Patterns and Practices e-book, ciągłego dostarczania](http://aka.ms/ReleasePipeline) przedstawia Typowy przepływ pracy. Kliknij obraz, aby zobaczyć, jak to pełny rozmiar w jego oryginalnego kontekstu.

[![Przepływ pracy ciągłego dostarczania](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Jak chmura umożliwia ekonomiczne ciągłej integracji i ciągłego wdrażania

Automatyzowanie tych procesów na platformie Azure jest łatwe. Ponieważ używasz wszystko w chmurze, nie trzeba kupować serwerów ani zarządzania nimi dla kompilacji lub środowiska testowego. I nie trzeba czekać na serwer można było wykonać test na. Z każdą kompilacją, jak można Przyspiesz środowisko testowe na platformie Azure przy użyciu skryptu automatyzacji, testów akceptacyjnych wykonywania lub więcej testów szczegółowe przed nim, a następnie po zakończeniu, po prostu go zlikwidujesz. I jeśli uruchomisz wyłącznie tego serwera do 2 godzin lub 8 godzin lub dni, kwotę, które trzeba płacić za jej minimalny, ponieważ teraz płacić tylko za czas, który faktycznie działa maszynę. Na przykład środowisko wymagane poprawki aplikacji po prostu koszt wynosi około 1% na godzinę po przejściu jednowarstwowa z poziomu bezpłatne. W ciągu miesiąca po przeprowadzeniu tylko środowiska na godzinę w czasie, środowiska testowego będzie prawdopodobnie kosztować mniej niż latte, który można kupić w kawę.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

Usługa VSTS zapewnia szereg funkcji, które ułatwiają tworzenie aplikacji od planowania do wdrożenia.

- Obsługuje ona kontroli źródła (scentralizowany) TFVC i Git (rozproszone).
- Oferuje ona usługi kompilacji elastycznych, co oznacza dynamicznie tworzy serwerów kompilacji, gdy zajdzie taka potrzeba i pobiera je w dół po ich zakończeniu. Możesz automatycznie uruchamiał kompilację, gdy ktoś sprawdza zmiany kodu źródłowego i nie trzeba mieć przydzielić i płacić za własnych serwerów kompilacji, leżących bezczynności przez większość czasu. Usługa kompilacji jest bezpłatna, tak długo, jak nie może przekraczać liczbę kompilacji. Spodziewasz się zrobić dużą liczbę kompilacji, mogą płacić za mały dodatkowych serwerów kompilacji zastrzeżone.
- Obsługuje ona ciągłe dostarczanie na platformę Azure.
- Obsługuje testy obciążeniowe automatycznych. Testowanie obciążenia mają kluczowe znaczenie dla aplikacji w chmurze, ale często jest zaniedbania, dopóki nie jest za późno. Testowanie obciążeniowe symuluje intensywnie korzysta z aplikacji przez tysiące użytkowników, dzięki któremu można znaleźć wąskie gardła i poprawianie przepływności — przed publikacją aplikacji do środowiska produkcyjnego.
- Obsługuje ona współpraca pokoju zespołu, który umożliwia współpracę w czasie rzeczywistym komunikacji i współpracy dla małych zespołów agile.
- Obsługuje ona elastyczne zarządzanie projektami.


Aby uzyskać więcej informacji na temat ciągłą integrację i ciągłe dostarczanie funkcji usługi VSTS, zobacz [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Jeśli potrzebujesz do zarządzania projektami setką kompleksowych, współpracę zespołową i rozwiązania kontroli źródła, zapoznaj się z usługi VSTS. Usługa jest dostępna bezpłatnie dla maksymalnie 5 użytkowników i możesz zasubskrybować na stronie [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Podsumowanie

Pierwsze trzy rozwoju wzorce chmury zostały o sposobie wdrażania z procesu opracowywania powtarzalne, niezawodne i przewidywalne, przy użyciu czasu cyklu niski. W [następny rozdział](web-development-best-practices.md) Zaczniemy Przyjrzyj się wzorców architektury i kodowania.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji sieci web w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Zobacz też następujące zasoby:

- [Tworzenie potoku wersji przy użyciu serwera Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Książka elektroniczna praktyczne laboratoria oraz przykładowy kod przez Microsoft Patterns and Practices, zapewnia szczegółowe wprowadzenie do ciągłego dostarczania. Obejmuje korzystanie z programu Visual Studio Lab Management i Release Management programu Visual Studio.
- [ALM Rangers DevOps, narzędzia i wskazówki](https://aka.ms/vsarsolutions/). ALM Rangers wprowadzone DevOps Workbench przykładowe pomocnika rozwiązanie i praktyczne wskazówki we współpracy z wzorców &amp; książki rozwiązania *tworzenie potoku wydania z programem TFS 2012*, jako doskonały sposób na rozpoczęcie Pojęcia związane z DevOps uczenia &amp; Release Management dla programu TFS 2012, jak i do testów laboratoryjnych. Wskazówki przedstawiono sposób twórz raz i wdrażanie w wielu środowiskach.
- [Testowanie dostarczania ciągłego w programie Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Książka elektroniczna przez Microsoft Patterns and Practices, wyjaśnia, jak zintegrować testy zautomatyzowane za pomocą ciągłego dostarczania.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Kod źródłowy tak zaprojektowane, aby przechwycić kompilacji z serwera TFS (na podstawie etykiety), skompiluj go, spakujesz ją, ktoś w roli DevOps, aby skonfigurować określone aspekty i wypchnąć je do platformy Azure. Narzędzie do śledzenia procesu wdrażania w celu umożliwienia operacji "wycofanie" wcześniej wdrożonej wersji. Narzędzie nie ma zależności zewnętrznych i może działać w autonomicznej przy użyciu interfejsów API programu TFS i zestawu Azure SDK.
- [Ciągłe dostarczanie: Wiarygodne oprogramowanie zwalnia poprzez tworzenie, testowanie i automatyzację wdrażania](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Książka autorstwa Jez Humble.
- [Zwolnij go! Projektowanie i wdrażanie oprogramowania gotowe do produkcji](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Książka autorstwa Nygarda T. Michael.

> [!div class="step-by-step"]
> [Poprzednie](source-control.md)
> [dalej](web-development-best-practices.md)
