---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: Książka elektroniczna przedstawiono podejście na podstawie wzorców do tworzenia rozwiązań chmur rzeczywistych. Wzorce dotyczą programowanie proces również jako...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5a62818a2dc21128bb0a42a8b296ade460e7b060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a>Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Książka elektroniczna przedstawiono podejście na podstawie wzorców do tworzenia rozwiązań chmur rzeczywistych. Wzorce mają zastosowanie do procesu tworzenia, a także do architektury i praktyk kodowania.
> 
> Zawartość jest oparta na prezentacji opracowane przez Scott Guthrie i dostarczane przez niego na norweski konferencji Deweloperzy (NDC) w czerwca 2013 ([część 1](http://vimeo.com/68215538), [część 2](http://vimeo.com/68215602)), a w Australii Ed techniczna firmy Microsoft w Września 2013 ([część 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [część 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Wiele innych](more-patterns-and-guidance.md#acknowledgments) zaktualizowany i rozszerzony zawartości podczas przechodzenia go z wideo do formularza napisane.


## <a name="intended-audience"></a>Docelowa grupa odbiorców

Deweloperzy, którzy są zastanawiasz się, jak tworzeniu aplikacji w chmurze, biorąc pod uwagę przeniesienie do chmury, lub dopiero zaczynasz korzystać z chmury programowanie znajdziesz tutaj krótkie omówienie najważniejszych pojęć i wskazówki, które są im potrzebne, aby dowiedzieć się. Pojęcia są przedstawiane za pomocą konkretnych przykłady i każdego rozdziału łącza do innych zasobów, aby uzyskać więcej szczegółowych informacji. Przykłady i linki do dodatkowych zasobów są platform firmy Microsoft oraz usług, ale przedstawiono zasady dotyczą innych środowiska deweloperskie dla sieci web i usług w chmurze, jak również w środowiskach.

Deweloperów, którzy są już tworzeniu aplikacji w chmurze może się okazać, że w tym miejscu pomysły, które pomogą sprawić, że będą szanse. Każdego rozdziału w serii można odczytać niezależnie, aby można było wybrać i wybierz tematy, które interesują Cię.

Każdy, kto obserwowane Scott Guthrie *tworzenia rzeczywistych aplikacji w chmurze platformy Azure* prezentacji i chce bardziej szczegółowe informacje i zaktualizowane informacje zostanie ustalone, które w tym miejscu.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Wzorce projektowe w chmurze

Książka elektroniczna objaśniono trzunastu zalecane wzorce dla rozwoju chmury. "Wzorzec" jest używana w tym miejscu szeroko oznacza zalecany sposób postępowania: jak najlepiej przejść o tworzeniu, projektowania i kodowania aplikacji w chmurze. Są to klucza wzorców, które pomogą Ci "obniżenia do pit sukcesu" ich wykonanie.

- [Automatyzowanie wszystko](automate-everything.md).

    - Aby zmaksymalizować wydajność i zminimalizować liczbę błędów w procesach powtarzających się za pomocą skryptów.
    - Pokaz: Skrypty zarządzania platformy Azure.
- [Kontroli źródła](source-control.md). 

    - Konfigurowanie rozgałęziania struktury w kontroli źródła do ułatwienia DevOps przepływu pracy.
    - Pokaz: dodania skryptów do kontroli źródła.
    - Pokaz: utrzymywanie poufnych danych poza kontrolą źródła.
    - Pokaz: Użyj narzędzia Git w programie Visual Studio.
- [Ciągłej integracji i dostarczania](continuous-integration-and-continuous-delivery.md). 

    - Automatyzowanie kompilowanie i wdrażanie z każdym zaewidencjonowania kontroli źródła.
- [Programowanie najlepsze rozwiązania w sieci Web](web-development-best-practices.md). 

    - Zachowaj bezstanowych warstwa sieci web.
    - Pokaz: skalowanie i automatyczne skalowanie aplikacji sieci Web w usłudze Azure App Service.
    - Unikaj stanu sesji.
    - Za pomocą CDN rezerwowe, gdy element CDN jest niedostępny.
    - Użyj modelu programowania asynchronicznego.
    - Pokaz: asynchronicznego w programie ASP.NET MVC i Entity Framework.
- [Logowanie jednokrotne](single-sign-on.md). 

    - Wprowadzenie do usługi Azure Active Directory.
    - Pokaz: tworzenie aplikacji platformy ASP.NET, która używa usługi Azure Active Directory.
- [Opcje magazynu danych](data-storage-options.md). 

    - Typy magazynów danych.
    - Jak wybrać magazyn danych po prawej stronie.
    - Pokaz: Baza danych Azure SQL.
- [Strategie partycjonowanie danych](data-partitioning-strategies.md). 

    - Partycjonowania danych pionowo, poziomo czy oba rodzaje, aby ułatwić skalowanie relacyjnej bazy danych.
- [Magazyn obiektów blob niestrukturalnych](unstructured-blob-storage.md). 

    - Przechowywanie plików w chmurze przy użyciu usługi blob.
    - Pokaz: przy użyciu magazynu obiektów blob w aplikacji rozwiązać.
- [Projekt po awarii](design-to-survive-failures.md). 

    - Typy błędów.
    - Zakres awarii.
    - Opis umowy SLA.
- [Monitorowanie i dane telemetryczne](monitoring-and-telemetry.md). 

    - Dlaczego należy zarówno kupić aplikacji telemetrii i napisać własny kod do Instrumentacja aplikacji.
    - Pokaz: Usługi New Relic na platformie Azure
    - Pokaz: rejestrowanie kodu w aplikacji rozwiązać.
    - Pokaz: iniekcji zależności w aplikacji rozwiązać.
    - Pokaz: Obsługa wbudowanych rejestrowania na platformie Azure.
- [Obsługa błędów przejściowych](transient-fault-handling.md). 

    - Użyj inteligentne logiki ponawiania/wycofania celu złagodzenia skutków błędów przejściowych.
    - Pokaz: ponawiania/wycofania programie Entity Framework 6.
- [Rozproszone buforowanie](distributed-caching.md). 

    - Poprawić skalowalność i ograniczyć koszty transakcji bazy danych przy użyciu buforowania rozproszonego.
- [Wzorzec skoncentrowane kolejki pracy](queue-centric-work-pattern.md). 

    - Włączyć wysoką dostępność i poprawić skalowalność przez słabo sprzężenia warstw web i proces roboczy.
    - Pokaz: Kolejek usługi Azure storage w aplikacji rozwiązać.
- [Więcej usług w chmurze, wskazówki i wzorce aplikacji](more-patterns-and-guidance.md).
- [Dodatek: Przykładowa aplikacja Fix It](the-fix-it-sample-application.md)

    - Znane problemy
    - Najlepsze praktyki
    - Jak pobrać, tworzenia, uruchamiania i wdrażania.

Te wzorce stosowane do wszystkich środowisk chmury, ale firma Microsoft będzie ilustrują je za pomocą przykładów oparte na technologii firmy Microsoft i usług, takich jak Visual Studio Team Foundation Service, ASP.NET i usługi Azure.

Tego pozostałej części w tym rozdziale przedstawiono poprawka przykładowej aplikacji i aplikacje sieci Web w środowisku chmury Azure App Service, uruchomionym aplikacji Usuń w.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Napraw go Przykładowa aplikacja

Większość zrzuty ekranu i przykłady kodu pokazano Książka elektroniczna są oparte na aplikacji poprawka pierwotnie opracowane przez [Scott Guthrie](https://weblogs.asp.net/scottgu/) aby zademonstrować wzorce programowania aplikacji zalecane chmury i rozwiązania.

![Poprawka strony głównej aplikacji](introduction/_static/image1.png)

Przykładowa aplikacja jest elementem pracy, system biletów. Jeśli potrzebujesz coś stałej utworzenia biletu i przypisz go do osoby, a inne Zaloguj się, a Zobacz biletów przypisane do nich i biletów zostać oznaczone jako zakończone po zakończeniu pracy.

Jest standardowe projektu sieci web programu Visual Studio. Jest oparty na programie ASP.NET MVC, a korzysta z bazy danych programu SQL Server. Można uruchomić lokalnie w usługach IIS Express, a można wdrożyć na platformie Azure witryny sieci Web do uruchamiania w chmurze. Możesz zalogować się przy użyciu uwierzytelniania formularzy i lokalnej bazy danych lub za pomocą dostawcy społecznościowych, takich jak Google. (Później firma Microsoft będzie także pokazują, jak przeprowadzić logowania za pomocą konta organizacyjnego usługi Active Directory.)

![Zaloguj się na stronie](introduction/_static/image2.png)

Po zalogowaniu w można utworzyć biletu, przypisz je do innej i Przekaż obraz ma być uzyskać stałej.

![Utwórz zadanie poprawka](introduction/_static/image3.png)

![Poprawka utworzone zadanie](introduction/_static/image4.png)

Można śledzić postęp utworzonych elementów roboczych, zobacz biletów przypisane, Wyświetl szczegóły biletu oraz elementy Oznacz jako ukończone.

Jest to bardzo prosta aplikacja z punktu widzenia funkcji, ale będzie widoczny sposób tworzenia go tak, aby można skalować do milionów użytkowników i będą odporne na błędy bazy danych i zakończenia połączenia. Widoczny będzie również sposób tworzenia przepływu pracy zautomatyzowane i elastyczne programowanie, dzięki czemu można uruchomić proste i zapewnić skuteczniejszych aplikacji przez iteracja cykl programowania wydajne i szybko.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplikacje sieci Web w usłudze aplikacji Azure

Używane dla aplikacji Usuń środowisko chmury jest usługą platformy Azure, który nazywamy witryn sieci Web. Ta usługa jest sposób można udostępniać własnych aplikacji sieci web na platformie Azure, bez konieczności tworzenia maszyn wirtualnych i ich aktualizowanie, zainstalowanie i skonfigurować usługi IIS itp. Firma Microsoft hostem witryny na naszych maszyn wirtualnych i automatycznie udostępnić kopii zapasowej i odzyskiwania i innych usług dla Ciebie. Usługi witryn sieci Web działa z platformy ASP.NET, Node.js, PHP i Python. Umożliwia wdrażanie bardzo szybko przy użyciu programu Visual Studio, narzędzia Web Deploy, FTP, Git lub TFS. Zazwyczaj jest tylko kilka sekund między po uruchomieniu wdrożenia i czas udostępnienia aktualizacji przez Internet. Wszystkie bezpłatnie rozpocząć pracę, a można skalować miarę zwiększania się ruchu.

W tle w aplikacjach sieci Web w usłudze Azure App Service udostępnia wiele składników architektury i funkcje, które trzeba by utworzyć samodzielnie, jeśli zostały przechodzi do hostowania witryny sieci web za pomocą usług IIS na własnych maszynach wirtualnych. Jeden składnik jest wdrożenie punktu końcowego, który umożliwia skonfigurowanie usług IIS i automatycznie instaluje aplikację na dowolną liczbę maszyn wirtualnych, jak chcesz uruchomić witryny.

![Usługa wdrażania](introduction/_static/image5.png)

Gdy użytkownik trafi witryny sieci web, ich maszyn wirtualnych usług IIS nie osiągnęła bezpośrednio, komputery przechodzą [Routing żądań aplikacji (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) usługi równoważenia obciążenia. Można użyć własnych serwerów, ale w tym miejscu zaletą jest, że ich są ustawione automatycznie. Używają heurystyki inteligentnego, który bierze pod uwagę czynników, takich jak sesji koligacji, głębokość kolejki w usługach IIS, a użycie procesora CPU na każdym komputera do bezpośredniego ruch do maszyn wirtualnych hostujących witryny sieci web.

![Moduł równoważenia obciążenia ARR](introduction/_static/image6.png)

Jeśli komputer ulegnie awarii, Azure automatycznie pobiera go z obrót, obraca się nowe wystąpienie maszyny Wirtualnej i uruchamia kierowanie ruchem do nowego wystąpienia--wszystko bez dół czasu dla aplikacji.

![Automatyczne odzyskiwanie po awarii komputera](introduction/_static/image7.png)

Wszystko to odbywa się automatycznie. To wszystko, co należy zrobić, tworzenie witryny sieci web i wdrażanie aplikacji, przy użyciu programu Windows PowerShell, Visual Studio lub portalu zarządzania Azure.

Szybkie i łatwe samouczek krok po kroku, który pokazuje, jak utworzyć aplikację sieci web w programie Visual Studio i wdrożyć ją do witryny sieci Web platformy Azure, zobacz [wprowadzenie do platformy Azure i ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Podsumowanie

To wprowadzenie udostępnił listę tematów, które będzie obejmować książce, zrzuty ekranu aplikacji przykładowej i krótkie omówienie aplikacji sieci Web w środowisku chmury Azure App Service. Jedną z zalet dużą rozwoju aplikacji w chmurze oraz jest łatwo automatyzować powtarzalne zadania, takie jak tworzenie środowiska testowego i wdrażanie kodu. Jak to zrobić będący przedmiotem [następny rozdział](automate-everything.md).

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji na temat tematy omówione w tym rozdziale zobacz następujące zasoby.

Dokumentacja:

- [Web Apps w usłudze aplikacji Azure](https://azure.microsoft.com/services/app-service/web/). Strona portalu dokumentacji platformy Azure o aplikacjach sieci Web.
- [Sieci Web aplikacji, usługi w chmurze i maszyn wirtualnych: kiedy należy używać?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, jak pokazano w tym rozdziale jest tylko jeden z trzech sposobów, które mogą korzystać z aplikacji sieci web na platformie Azure. W tym artykule wyjaśniono różnice między trzy sposoby oraz zapewnia wskazówki na temat wybierania który z nich jest odpowiednia dla danego scenariusza. Podobnie jak witryn sieci Web usługi w chmurze jest funkcją PaaS systemu Azure. Maszyny wirtualne są funkcją IaaS. Aby uzyskać informacje o PaaS i IaaS, zobacz [opcje danych](data-storage-options.md#paasiaas) działu.

Filmy wideo:

- [Scott Guthrie rozpoczyna się od kroku 0 - co to jest system operacyjny chmury Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architektura witryn sieci Web — z Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Ustawienia wewnętrzne witryny sieci Web platformy Azure z Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
