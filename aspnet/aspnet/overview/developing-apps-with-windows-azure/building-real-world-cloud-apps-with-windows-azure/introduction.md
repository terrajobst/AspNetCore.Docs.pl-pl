---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: Ta książka elektroniczna przeprowadzi Cię przez to podejście oparte na wzorcach do tworzenia rozwiązań w chmurze w rzeczywistych warunkach. Wzorce dotyczą procesu opracowywania oraz aby...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: c496e93d7517bc187514d5fa2dfa90d29c5f47f9
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576316"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Ta książka elektroniczna przeprowadzi Cię przez to podejście oparte na wzorcach do tworzenia rozwiązań w chmurze w rzeczywistych warunkach. Wzorce dotyczą procesu opracowywania oraz architektury i kodowania.
> 
> Zawartość jest oparta na prezentacji, opracowane przez Scotta Guthriego i przedstawił na norweskiej przez niego na norweskiej deweloperów (ndc Developers Conference) w czerwcu 2013 ([część 1](http://vimeo.com/68215538), [część 2](http://vimeo.com/68215602)) i Microsoft Tech Ed Australia Września 2013 r ([część 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [część 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Wiele innych](more-patterns-and-guidance.md#acknowledgments) aktualizowana i rozszerzana podczas była ona z wideo na formę tekstową.


## <a name="intended-audience"></a>Odbiorców

Deweloperzy, którzy są zastanawiasz się programowaniem dla chmury, rozważasz przejście do chmury lub rozpoczynasz projektowanie aplikacji w chmurze znajduje się w tym miejscu Zwięzłe omówienie najważniejszych koncepcji i praktyk, które są im potrzebne, aby dowiedzieć się. Pojęcia zostały zilustrowane konkretnymi przykładami i każdego łącza rozdziału, do innych zasobów, aby uzyskać więcej szczegółowych informacji. Przykłady i linki do dodatkowych zasobów dotyczą usług oraz struktur firmy Microsoft, ale przedstawione zasady dotyczą innych struktur projektowania sieci web i również w środowiskach w chmurze.

Deweloperzy, którzy już się programowaniem dla chmury może się okazać, że w tym miejscu pomysły, które pomogą zwiększyć ich osiągania sukcesów. Każdy rozdział w serii może zostać odczytany niezależnie, dzięki czemu można wybrać i wybrać polecenie tematów, które interesują Cię.

Każdy, kto obserwowane Scott Guthrie *tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure* prezentacji oraz profesjonalistą, bardziej szczegółowe informacje i zaktualizowane informacje znajdzie, które w tym miejscu.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Wzorce programowania w chmurze

Ta książka elektroniczna wyjaśnia trzunastu zalecana wzorce projektowania aplikacji w chmurze. "Wzorzec" jest używany w tym miejscu szeroko oznacza zalecany sposób, aby wykonywały: najlepszy sposób dotyczące programowania, projektowania i programowania aplikacji w chmurze. Są to kluczowe wzorce, które pomogą Ci "można podzielić na pit powodzenia", po ich wykonaniu.

- [Automatyzowanie wszystkiego](automate-everything.md).

    - Aby zmaksymalizować wydajność i zminimalizować błędy w procesach powtarzających się za pomocą skryptów.
    - Pokaz: Skrypty zarządzania systemu Azure.
- [Kontrola źródła](source-control.md). 

    - Skonfiguruj rozgałęziona struktura w kontroli źródła, aby ułatwić przepływ pracy DevOps.
    - Pokaz: Dodaj skrypty do kontroli źródła.
    - Pokaz: utrzymywanie poufnych danych wyewidencjonowane z kontroli źródła.
    - Pokaz: Użyj usługi Git w programie Visual Studio.
- [Ciągła integracja i dostarczanie](continuous-integration-and-continuous-delivery.md). 

    - Automatyzuj kompilowanie i wdrażanie za pomocą każdego ewidencjonowania kontroli źródła.
- [Najlepsze praktyki programowania aplikacji w sieci Web](web-development-best-practices.md). 

    - Zachowaj warstwy internetowej bezstanowe.
    - Pokaz: skalowanie i automatyczne skalowanie aplikacji sieci Web w usłudze Azure App Service.
    - Należy unikać stanu sesji.
    - Za pomocą sieci CDN rezerwowe, gdy sieć CDN jest niedostępny.
    - Użyj modelu programowania asynchronicznego.
    - Pokaz: asynchronicznych w ASP.NET MVC i platformy Entity Framework.
- [Logowanie jednokrotne](single-sign-on.md). 

    - Wprowadzenie do usługi Azure Active Directory.
    - Pokaz: tworzenie aplikacji platformy ASP.NET, która używa usługi Azure Active Directory.
- [Opcje przechowywania danych](data-storage-options.md). 

    - Typy magazynów danych.
    - Jak wybrać odpowiedniego magazynu danych.
    - Pokaz: Usługi Azure SQL Database.
- [Strategie partycjonowania danych](data-partitioning-strategies.md). 

    - Partycjonowanie danych, pionowo, poziomo lub obydwa elementy w ułatwienia, skalowanie relacyjnej bazy danych.
- [Magazyn obiektów blob bez struktury](unstructured-blob-storage.md). 

    - Store plików w chmurze przy użyciu usługi blob service.
    - Pokaz: przy użyciu usługi blob storage w aplikacji naprawić.
- [Projektowanie pod kątem przetrwania awarii](design-to-survive-failures.md). 

    - Typy błędów.
    - Zakres awarii.
    - Zrozumienie umowy SLA.
- [Monitorowanie i telemetria](monitoring-and-telemetry.md). 

    - Dlaczego należy zarówno kupowanie aplikacji dane telemetryczne i Napisz własny kod do instrumentacji aplikacji.
    - Pokaz: New Relic dla platformy Azure
    - Pokaz: logowania aplikacji naprawić kod.
    - Pokaz: wstrzykiwanie zależności w aplikacji naprawić.
    - Pokaz: Obsługa wbudowane funkcje rejestrowania na platformie Azure.
- [Obsługa błędu przejściowego](transient-fault-handling.md). 

    - Użyj inteligentne logikę ponawiania/wycofywania, aby zminimalizować wpływ błędów przejściowych.
    - Pokaz: ponawiania/wycofywania w Entity Framework 6.
- [Rozproszonej pamięci podręcznej](distributed-caching.md). 

    - Zwiększanie skalowalności i obniżenie kosztów transakcji bazy danych za pomocą rozproszonej pamięci podręcznej.
- [Wzorzec pracy skoncentrowany na kolejkach](queue-centric-work-pattern.md). 

    - Włączyć wysoką dostępność i zwiększanie skalowalności przez luźno sprzężenia warstw web i proces roboczy.
    - Pokaz: Kolejki usługi Azure storage w aplikacji naprawić.
- [Więcej w chmurze aplikacji wzorce i wskazówki](more-patterns-and-guidance.md).
- [Dodatek: Przykładowa aplikacja Fix It](the-fix-it-sample-application.md)

    - Znane problemy
    - Najlepsze praktyki
    - Jak pobrać, tworzenia, uruchamiania i wdrażania.

Te wzorce stosowane do wszystkich środowisk w chmurze, ale firma Microsoft będzie pokazują je przy użyciu przykładów na podstawie technologii firmy Microsoft i usług, takich jak Visual Studio, Team Foundation Service, ASP.NET i Azure.

Ta pozostałej w tym rozdziale wprowadza rozwiązać go przykładowej aplikacji i aplikacji sieci Web w środowisku chmury Azure App Service, które aplikacja naprawić jest uruchamiana w.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Napraw go przykładowej aplikacji

Większość zrzuty ekranu i przykłady kodu zamieszczone w tej książce elektronicznej opierają się na aplikacji rozwiązać go pierwotnie opracowana przez [Scott Guthrie](https://weblogs.asp.net/scottgu/) aby zademonstrować wzorce programowania aplikacji w chmurze zalecane i praktyk.

![Napraw strona główna aplikacji](introduction/_static/image1.png)

Przykładowa aplikacja jest element roboczy prosty, system biletów. Jeśli musisz coś, co jest stała, Utwórz bilet i przypisz ktoś oraz innym osobom może zalogować się i zobacz bilety przypisane do nich i oznaczyć biletów, jako ukończone po zakończeniu pracy.

Jest to standardowy projekt sieci web programu Visual Studio. Bazuje na platformie ASP.NET MVC, a korzysta z bazy danych programu SQL Server. Jego można uruchomić lokalnie w usługach IIS Express i którą można wdrożyć do witryny sieci Web platformy Azure do uruchamiania w chmurze. Możesz zalogować się przy użyciu uwierzytelniania formularzy i lokalną bazą danych lub za pomocą mediów społecznościowych, takich jak Google. (Później także pokażemy jak zalogować się przy użyciu konta organizacyjnego usługi Active Directory.)

![Zaloguj się na stronie](introduction/_static/image2.png)

Po zalogowaniu w można utworzyć bilet, przypisać go do innej i Przekaż obraz ma zostać naprawione.

![Utwórz zadanie poprawka](introduction/_static/image3.png)

![Napraw utworzono zadanie](introduction/_static/image4.png)

Można śledzić postępy elementów roboczych, który został utworzony, zobacz bilety przypisana, Wyświetl szczegóły biletu i Oznacz elementy jako ukończone.

Jest to bardzo prosta aplikacja z punktu widzenia funkcji, ale będzie zobaczysz, jak utworzyć ją tak, aby przeprowadzać skalowanie do milionów użytkowników i będą odporne na błędy bazy danych i zakończenia połączenia. Widoczna będzie również sposób tworzenia zautomatyzowanych i elastyczne rozwój przepływu roboczego, co pozwala w prosty sposób Rozpocznij pracę i zapewnić lepsze i lepsze aplikacji przez iteracja cyklu tworzenia oprogramowania, szybko i wydajnie.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplikacje sieci Web w usłudze Azure App Service

W środowisku chmury używana dla aplikacji naprawić to usługa platformy Azure, który nazywamy witryn sieci Web. Ta usługa to sposób, możesz hostować własną aplikację internetową na platformie Azure bez konieczności tworzenia maszyn wirtualnych i ich aktualizowanie, zainstalować i skonfigurować usługi IIS itp. Firma Microsoft hostowanie witryny w naszych maszyn wirtualnych i automatycznie udostępnić kopii zapasowych i odzyskiwania oraz innych usług. Usługa witryny sieci Web w programach ASP.NET, Node.js, PHP i Python. Pozwala on na wdrażanie bardzo szybko przy użyciu programu Visual Studio, narzędzia Web Deploy, FTP, Git lub TFS. Zazwyczaj jest tylko kilka sekund pomiędzy po uruchomieniu wdrożenia aktualizacji jest dostępna za pośrednictwem Internetu. Wszystkie bezpłatnie rozpocząć pracę i możesz skalować w górę ze wzrostem natężenia ruchu.

W tle aplikacji sieci Web w usłudze Azure App Service udostępnia wiele składników architektury i funkcje, które trzeba samodzielnie tworzyć, jeśli miało hostować witrynę sieci web za pomocą usług IIS na własnych maszynach wirtualnych. Jeden składnik to wdrożenie punktu końcowego, który umożliwia skonfigurowanie usług IIS i automatycznie instaluje aplikację na tyle maszyn wirtualnych, jak chcesz uruchomić witryny.

![Usługa wdrażania](introduction/_static/image5.png)

Gdy użytkownik trafi w witrynie sieci web, te maszyny wirtualne usług IIS nie trafią bezpośrednio, komputery przechodzą za pośrednictwem [Routing żądań aplikacji (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) moduły równoważenia obciążenia. Można ich użyć z własnymi serwerami, ale w tym miejscu zaletą jest na to, że są one skonfigurowane dla Ciebie automatycznie. Używają heurystyki inteligentnego, która ma pod uwagę czynniki, takie jak koligacja sesji, głębokość kolejki w usługach IIS, a użycie procesora CPU na każdym maszyny do kierowania ruchu do maszyn wirtualnych hostujących witryny sieci web.

![Moduł równoważenia obciążenia ARR](introduction/_static/image6.png)

Jeśli maszyna ulegnie awarii, Azure automatycznie ściągany z obrotu, uruchamia nowe wystąpienie maszyny Wirtualnej i rozpoczyna kierowanie ruchu do nowego wystąpienia--wszystko bez dół czasu dla aplikacji.

![Automatyczne odzyskiwanie po awarii maszyny](introduction/_static/image7.png)

Wszystko to odbywa się automatycznie. To wszystko, co należy zrobić, Utwórz witrynę sieci web i wdrażanie aplikacji, za pomocą programu Windows PowerShell, Visual Studio lub portalu zarządzania systemu Azure.

Aby uzyskać szybki i łatwy samouczek krok po kroku, który pokazuje, jak utworzyć aplikację sieci web w programie Visual Studio i wdrożyć ją do witryny sieci Web platformy Azure, zobacz [Rozpoczynanie pracy z platformą Azure i programu ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Podsumowanie

To wprowadzenie udostępnił listę tematów, które obejmie książki, zrzuty ekranu przykładowej aplikacji i krótkie omówienie aplikacji sieci Web w środowisku chmury w usłudze Azure App Service. Jedną z zalet tworzenia aplikacji w i w chmurze jest łatwe zautomatyzować powtarzalne zadania, takie jak tworzenie środowiska testowego i wdrażanie kodu. Jak to zrobić, to znaczy, temat [następny rozdział](automate-everything.md).

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat omówione w tym rozdziale tematy zobacz następujące zasoby.

Dokumentacja:

- [Aplikacji sieci Web w usłudze Azure App Service](https://azure.microsoft.com/services/app-service/web/). Strona portalu dokumentacji platformy Azure o usłudze Web Apps.
- [Usługi w chmurze, maszyny wirtualne i aplikacje sieci Web: kiedy korzystać z czego?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, jak pokazano w tym rozdziale jest tylko jeden z trzech sposobów, które można uruchamiać aplikacje sieci web na platformie Azure. W tym artykule wyjaśniono różnice między trzy sposoby i zawiera wskazówki dotyczące sposobu wybierania, która jest odpowiednia dla danego scenariusza. Podobnie jak witryny sieci Web usług w chmurze jest funkcja PaaS platformy Azure. Maszyny wirtualne są funkcją IaaS. Opis rozwiązania paas a IaaS, zawiera [opcje danych](data-storage-options.md#paasiaas) rozdziale.

Filmy wideo:

- [Scott Guthrie rozpoczyna się od kroku 0 — co to jest systemu operacyjnego w chmurze platformy Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architektura witryn sieci Web — za pomocą Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Wewnętrzne witryny sieci Web systemu Azure za pomocą i Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
