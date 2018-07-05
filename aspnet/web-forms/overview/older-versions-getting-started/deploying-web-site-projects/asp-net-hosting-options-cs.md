---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET obsługującego opcje (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Aplikacje sieci web platformy ASP.NET zwykle nie mają utworzony i testowane w lokalne Środowisko deweloperskie i muszą zostać wdrożone do o środowisku produkcyjnym...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 843db9bd1216c14d31f6fd7c142951a17849d7c2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399227"
---
<a name="aspnet-hosting-options-c"></a>Opcje hostingu platformy ASP.NET (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Aplikacji sieci web ASP.NET są zazwyczaj zaprojektowane, tworzone i testowane w lokalnym środowisku programowania i konieczności można wdrożyć w środowisku produkcyjnym, gdy wszystko będzie gotowe do wydania. Ten samouczek zawiera ogólne omówienie procesu wdrażania i służy jako wprowadzenie do tej serii samouczków.


## <a name="introduction"></a>Wprowadzenie

Aplikacje sieci Web są zazwyczaj zaprojektowane, utworzone i przetestowane w środowisku deweloperskim, który jest dostępny tylko dla programistów pracy w witrynie. Gdy aplikacja jest gotowa do wydania, jest przenoszony do środowiska produkcyjnego gdzie lokacji są dostępne dla wszystkich użytkowników Internetu. Ten proces wdrażania wprowadza wiele wyzwań:

- W środowisku produkcyjnym, musi istnieć i być prawidłowo skonfigurowane, przed wdrożeniem aplikacji ASP.NET; Ponadto w środowisku produkcyjnym muszą być przechowywane na bieżąco otrzymywać najnowsze poprawki zabezpieczeń.
- Prawidłowy zbiór plików znaczników, pliki kodu i pliki obsługi muszą zostać skopiowane ze środowiska projektowego do środowiska produkcyjnego. W przypadku aplikacji opartych na danych może to wymagać kopiowania schemat bazy danych i/lub dane, jak również.
- Może to być różnice konfiguracji między dwoma środowiskami. Serwer ciągu lub wiadomości e-mail połączenie bazy danych używane w środowisku programistycznym będzie prawdopodobnie inny niż w środowisku produkcyjnym. Co więcej zachowanie aplikacji może zależeć na środowisko. Na przykład po wystąpieniu błędu w trakcie opracowywania szczegóły błędu mogą być wyświetlane na ekranie, ale po wystąpieniu błędu w środowisku produkcyjnym, stronę błędu przyjazny dla użytkownika, które powinny być wyświetlane zamiast i szczegóły błędu pocztą e-mail do deweloperów.

Aby uniknąć pierwszego wyzwania — definiowanie i utrzymywanie w środowisku produkcyjnym — wiele osób i firm zlecają obsługę środowisk produkcyjnych, aby *dostawcy hostingu w sieci web*. Dostawca hostingu w sieci web to firma, która zarządza środowiska produkcyjnego w Twoim imieniu. Istnieją niezliczone web dostawców hosta, każdy z różnymi cenami i poziomów usług; w sekcji "Wyszukiwanie w sieci Web hosta Provider" porady dotyczące lokalizacji dostawcy usług.

Jest to pierwszy z serii samouczków, które wyglądają na etapy wdrażania aplikacji sieci web ASP.NET w środowisku produkcyjnym, zarządzana przez dostawcę hosta sieci web. W trakcie tego samouczka będziemy sprawdzać:

- Które pliki muszą zostać wdrożone do dostawcy hosta sieci web.
- Narzędzia do co usprawnia proces wdrażania.
- Jak wdrożyć bazę danych.
- Wskazówki dotyczące wdrażania bazy danych, która używa [dostawcy SQL na podstawie członkostwa i ról](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), oraz metody, aby mógł naśladować narzędzia administracyjnego witryny sieci Web w środowisku produkcyjnym.
- Strategie płynnie zaktualizowanie bazy danych w środowisku produkcyjnym ze zmianami wprowadzonymi podczas programowania.
- Techniki rejestrowanie błędów, występujących w produkcji i sposoby Powiadom deweloperów, gdy wystąpi błąd.

Te samouczki są dostosowane zwięzłe i zapewnić instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie przeprowadzi Cię przez proces. W tym samouczku inauguracyjnym zawiera omówienie procesu wdrażania platformy ASP.NET i porady dotyczące znajdowania dostawcy hostingu w sieci web. Zaczynajmy!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Omówienie procesu wdrażania platformy ASP.NET

Wdrażanie aplikacji ASP.NET mówiąc, obejmuje trzy kroki:

1. Konfigurowanie aplikacji sieci web, serwer sieci web i bazy danych w środowisku produkcyjnym.
2. Synchronizuj strony ASP.NET, pliki kodu, zestawów w `Bin` folder i pliki pomocnicze związane z kodu HTML, takich jak pliki CSS i JavaScript.
3. Synchronizuj schemat bazy danych i/lub danych.

Informacje o konfiguracji dla aplikacji sieci web znajduje się w `Web.config` pliku i zawiera parametry połączenia bazy danych, obsługa błędów kryteriów, adres URL poprawiania reguły i informacje o serwerze poczty e-mail. Często tych informacji różni się dla aplikacji w trakcie opracowywania w porównaniu z tej samej aplikacji w środowisku produkcyjnym. Na przykład przy tworzeniu aplikacji jest najlepiej użyć rozwoju bazy danych, tak aby nie testujesz względem produkcyjnej bazy danych. W wyniku parametry połączenia bazy danych zazwyczaj różnią się między aplikacjami, deweloperskich i produkcyjnych. Ze względu na różnice te części wdrożenia konieczne jest wprowadzenie zmian do informacji o konfiguracji aplikacji sieci web.

Oprócz zmian w konfiguracji aplikacji sieci web krok 1 również może pociągać za sobą konfiguracji dla serwera sieci web i bazy danych. Na przykład jeśli strona ASP.NET tworzy lub usuwa pliki z katalogu na serwerze sieci web serwera sieci web musi skonfigurować tak, aby zezwolić na te modyfikacje systemu plików. Podobnie może być uprawnień lub ustawienia uwierzytelniania, które należy podjąć w bazie danych.


Krok 2 obejmuje synchronizowanie zestaw zasadniczych stron ASP.NET i pliki obsługi między środowiskami deweloperskim i produkcyjnym. Zestawy ASP. Pliki związane z usługą NET, które muszą zostać zsynchronizowane między dwoma środowiskami zależy od typu projektu, zostanie utworzony w programie Visual Studio i dyskusji w następnym samouczku [ *określająca, które pliki muszą zostać wdrożone*](determining-what-files-need-to-be-deployed-cs.md). Samouczki trzecia i czwarta — [ *wdrażanie Twojej witryny przy użyciu protokołu FTP* ](deploying-your-site-using-an-ftp-client-cs.md) i [ *wdrażanie Twojej witryny przy użyciu programu Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) — Sprawdź różne narzędzia i techniki do synchronizowania tych plików.

Podczas kompilowania aplikacji opartych na danych, są zazwyczaj dwie bazy danych używane: jeden dla rozwoju i jeden w środowisku produkcyjnym. Podczas tworzenia aplikacji schemat rozwoju bazy danych może być modyfikowane w celu uwzględnienia nowych tabel, kolumn, procedury składowane i wyzwalacze lub może zostać zmodyfikowana, aby usunąć lub zmienić istniejące obiekty bazy danych. Między tych zmian czas i czas, w których aplikacja jest wdrażana w środowisku produkcyjnym deweloperskich i produkcyjnych baz danych nie są zsynchronizowane. Ten asynchroniczności musi być ustalone podczas procesu wdrażania. Te problemy zostaną sprawdzone w przyszłości samouczków.

## <a name="finding-a-web-host-provider"></a>Wyszukiwanie dostawcy hosta sieci Web

Aplikacje ASP.NET można wdrożyć na dowolnym serwerze sieci web, .NET Framework i Internet Information Services (IIS). Można hostować witrynę z komputera osobistego przy założeniu, że miał połączenie szerokopasmowe z Internetem i wie, jak skonfigurować router w taki sposób, aby umożliwić przychodzących żądań sieci web. Może również udostępnić lokacji z komputera w sieci intranet, jak wiele firm. Celem tych samouczków, jednak jest hostem witryny sieci Web u dostawcy usług hosta sieci web.

> [!NOTE]
> [Usługi IIS](https://www.iis.net/) to serwer sieci web przeznaczonych dla przedsiębiorstw firmy Microsoft. Jest dostarczany z-Home systemu Windows, takich jak Windows Server 2008 i niektóre wersje systemu Windows Vista. Nie trzeba zainstalować usługi IIS do obsługi aplikacji programu ASP.NET w środowisku deweloperskim, jak program Visual Studio obejmuje serwer sieci Web programu ASP.NET Development. Jednak serwer sieci Web programu ASP.NET Development akceptuje tylko połączenia lokalne i w związku z tym nie można używać w środowisku produkcyjnym.


Przed wdrożeniem witryny dostawcy hosta sieci web musi najpierw zdecyduj jaki firmy do prowadzenia działalności z. Dostępne są niezliczone firmy w portalu marketplace; hostingu w sieci web Wyszukaj "hostingu w sieci web firmy" zwraca więcej niż pięć milionów wyników. Jak można znaleźć ten, który jest odpowiedni dla Ciebie? Ulubiona Wyszukiwarka jest dobrym miejscem począwszy od witryn sieci Web, takich jak [TopHosts](http://www.tophosts.com/) i [HostCritique](http://www.hostcritique.net/), który Porównaj i zestaw różnych usług hostingu. Czy mogę poinformować o żadnych zaleceń dotyczących; współpracowników i współpracowników można również zadawać na zalecenia [hostingu Otwórz Forum](https://forums.asp.net/158.aspx) tutaj na [fora ASP.NET](https://forums.asp.net/).

Firm zajmujących się hostingiem w sieci Web zwykle oferują udostępnione plany hostingu i plany hostingu w wersji dedykowanej. Udostępniony hostingu jednej sieci web hostów serwera dziesiątki, jeśli nie setki różnych witryn sieci Web. Przy użyciu wyspecjalizowanego hostingu dzierżawy jest komputer z firmy, która służy lokacji i lokacji. Udostępnione planu hostingu może obejmować obsługę strony ASP.NET, pracy z baz danych Microsoft Access, 5 GB miejsca na dysku i 100 GB dotyczący miesięcznego ruchu przepustowości dla $9.95 miesięcznie. Inny plan hostingu udostępnionej może obejmują obsługę strony ASP.NET, dostęp do serwera bazy danych Microsoft SQL Server 2008, 10 GB miejsca na dysku i 250 GB przepustowości dotyczący miesięcznego ruchu cenie od 19,95 USD miesięcznie. Plany hostingu w wersji dedykowanej są zazwyczaj znacznie bardziej kosztowne i wyceny kilka dolarów kilkuset na miesiąc, ale zapewniają lepszą wydajność i większą kontrolę niż udostępnionej opcji hostingu. Jakiego planu, możesz wybrać zależy od Twojego budżetu, ilość ruchu odbiera witryny sieci Web i funkcji, możesz przewidywać będziesz potrzebować.

Dwie ważne uwagi podczas wybierania dostawcy hosta sieci web są dział obsługi klienta i jakości usługi. Jeśli masz pytanie lub problem z konfiguracją, jak długo trwa przesyłanie problem do działu pomocy technicznej hosta sieci web, dopóki nie uzyskasz odpowiedzi? Jak niezawodne są usługi firmy? Często mają awarii bazy danych? Jak często jego serwerem poczty e-mail przejdą w tryb offline? Można zawsze Pytaj firmy zawierają szczegółowe informacje dotyczące ich czas pracy i uzyskiwanie informacji o zasadach usługi swoich klientów, ale bardziej surefire sposobem jest poproś o opinię bieżącej i wcześniejszych klientów, można to zrobić za pośrednictwem forów w trybie online, grupy dyskusyjne i listservs wiadomości e-mail .

> [!NOTE]
> Niektóre firmy hostingu w sieci web skoncentrować swoją działalność na stosie określonej technologii, takich jak .NET lub [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, i **P** HP), dlatego należy upewnić się, że firmy, możesz wybrać obsługuje aplikacje platformy ASP.NET. Również Sprawdź, czy obsługują one wersję platformy ASP.NET, którego używasz do budowania aplikacji. I kompilowania aplikacji opartych na danych, upewnij się, że host sieci web zapewnia tego samego serwera bazy danych i wersji, którego używasz.


## <a name="summary"></a>Podsumowanie

Aplikacji sieci web ASP.NET są zazwyczaj zaprojektowane, tworzone i testowane w lokalne Środowisko deweloperskie. Gdy wersja jest gotowy do wydania, jest przenoszony do środowiska produkcyjnego. Choć jest możliwe do hosta ASP.NET witryn sieci Web na komputerze osobistym lub na serwerach w firmie, wielu firm i osób chce oddelegowanie hostingu dostawcy hosta sieci web.

W tej serii samouczków sprawdza, czy kroki wdrażania aplikacji ASP.NET do dostawcy hosta sieci web, eksplorowanie najczęstsze wyzwania. W tym samouczku oferowana ogólne omówienie procesu wdrażania platformy ASP.NET i udostępniła porady służące do znajdowania dostawca hosta w odpowiedniej sieci web. Następny samouczek analizuje pliki związane z ASP.NET muszą być kopiowane do środowiska produkcyjnego, w przypadku wdrażania witryny sieci Web.

Wszystkiego najlepszego programowania!

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Next](determining-what-files-need-to-be-deployed-cs.md)
