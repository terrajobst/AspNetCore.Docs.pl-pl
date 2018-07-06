---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Projektowanie pod kątem przetrwania awarii (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 29a430a223fb62d9530ea00a60d458a6dbc5199a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820658"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Projektowanie po awarii (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Jedną z rzeczy, o których należy przemyśleć podczas kompilowania dowolny typ aplikacji, ale szczególnie taki, który zostanie uruchomiony w chmurze, gdy wiele osób będą używać, jest sposób projektowania aplikacji, dzięki czemu mogą bezpiecznie obsługiwać awarie i w dalszym ciągu dostarczać wartość jak możliwe. Mając wystarczająco dużo czasu, przebieg procesu się nie udaje się w dowolnym środowisku lub w każdym systemie oprogramowania. Jak aplikację pod kątem obsługi tych sytuacji Określa, jak zły usługi klienci otrzymają oraz ile czasu trzeba poświęcić na analizowanie i rozwiązywanie problemów.

## <a name="types-of-failures"></a>Typy błędów

Istnieją dwie kategorie podstawowe błędy, które można obsłużyć inaczej:

- Przejściowy naprawiać błędów, takich jak problemy z połączeniem sieciowym sporadycznie.
- Trwałej błędów, które wymagają interwencji.

W przypadku błędów przejściowych można zaimplementować zasady ponawiania, aby upewnić się, że większość czas odzyskiwania aplikacji, szybko i automatycznie. Twoi klienci mogą zauważą nieznacznie dłuższy czas odpowiedzi, ale w przeciwnym razie nie ulegnie zmianie. Pokażemy niektóre sposoby obsługi tych błędów w [obsługi błędów przejściowych rozdział](transient-fault-handling.md).

Dla trwałe błędy, można zaimplementować monitorowanie i rejestrowanie funkcji, które powiadamia niezwłocznie problemów i które ułatwia analizą głównych przyczyn problemów. Pokażemy niektóre sposoby na bieżąco te rodzaje błędów w [rozdział monitorowanie i dane telemetryczne](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Zakres awarii

Masz również wziąć pod uwagę zakres awarii — czy dotyczy jednej maszynie, całą usługę, takich jak bazy danych SQL lub magazynu lub cały region.

![Zakres awarii](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Błędy maszyny

Na platformie Azure serwer nie powiodło się, jest automatycznie zastępowany przez nowy i aplikacji w chmurze dobrze zaprojektowanego sprawności tego rodzaju błąd, szybko i automatycznie. Wcześniej podkreślić, firma Microsoft skalowalność zalet warstwy sieci web o bezstanowa i łatwość odzyskiwania z serwera nie powiodło się jest kolejną korzyścią wynikającą z statelessness. Łatwość odzyskiwania jest również jedną z zalet platformy jako usługi (PaaS) funkcji, takich jak bazy danych SQL Database i Azure App Service Web Apps. Awarie sprzętowe są rzadkie, ale kiedy występują one, czy usługi te określają je automatycznie; nie trzeba nawet pisać kod obsługujący maszyny błędy podczas korzystania z jednej z tych usług.

### <a name="service-failures"></a>Błędy usługi

Aplikacje w chmurze zazwyczaj używają wielu usług. Na przykład aplikacja naprawić korzysta z usługi SQL Database, usługi Storage i aplikacji sieci web jest wdrażana w usłudze Azure App Service. Co będzie aplikacji zrobić, jeśli jedna z usług, których zależysz zakończy się niepowodzeniem? Dla niektórych błędów usługi przyjazną "Niestety, spróbuj ponownie później" komunikat może być najlepsze, możesz zrobić. Ale w wielu scenariuszach możesz to zrobić lepiej. Na przykład gdy magazyn danych zaplecza nie działa, możesz można akceptować dane wejściowe użytkownika, wyświetlać "Twoje żądanie zostało odebrane" i przechowywać dane wejściowe wiadomo gdzie jeszcze tymczasowo; następnie podczas service, konieczne będzie działać ponownie, można pobrać danych wejściowych i go przetworzyć.

[Wzorzec pracy skoncentrowany na kolejkach](queue-centric-work-pattern.md) rozdziale pokazano jeden ze sposobów postępowania w tym scenariuszu. Aplikacja naprawić przechowuje zadania w usłudze SQL Database, ale nie musi on zakończyć pracy, gdy baza danych SQL nie działa. W tym rozdziale opisano sposób przechowywania danych wejściowych użytkownika dla zadania w kolejce i używać procesu roboczego do odczytywania kolejki i aktualizacji zadania podrzędnego. Jeśli SQL działa, możliwość tworzenia zadania usuń go pozostanie nienaruszona; proces roboczy może poczekać i przetwarzać nowych zadań w przypadku, gdy baza danych SQL jest dostępna.

### <a name="region-failures"></a>Błędy regionu

Cały regionów może zakończyć się niepowodzeniem. Klęski żywiołowe może zniszczyć centrum danych, może uzyskać spłaszczona za meteor, wiersza magistrali w centrum danych można było obciąć przez rolnika zakopanie krowy z backhoe itp. Co należy zrobić, jeśli aplikacja jest hostowana w stricken centrum danych? Istnieje możliwość skonfigurować aplikację na platformie Azure do jednoczesnego uruchamiania w wielu regionach, aby w przypadku awarii w jednym, możesz nadal działa w innym regionie. Takie błędy są bardzo rzadko, a większość aplikacji nie szybkiego dostępu za pośrednictwem przeszkód niezbędne do zapewnienia ciągłości usługi za pośrednictwem tego rodzaju błędów. Sekcja zasobów na końcu rozdział informacje na temat zachować swoją aplikację, które są dostępne nawet za pośrednictwem awarię regionu.

Celem usługi Azure jest zapewnienie obsługi wszystkich tych rodzajów błędów o wiele prostsze i pojawi się kilka przykładów jak nam idzie, w następujących rozdziałach.

## <a name="slas"></a>Umowy SLA

Ludzie często Posłuchaj o umów dotyczących poziomu usług (SLA) w środowisku chmury. Zasadniczo są to obietnic wchodzące w firmach o jak niezawodnej usługi. Dostępność na poziomie 99,9% czasu zgodnie z umową SLA oznacza, że możesz oczekiwać usługi działają poprawnie przez 99,9% czasu. Jest dość typowy wartość umowy SLA i dźwięki, takich jak bardzo wysoką liczbę, ale nie zakładane dół czas, jaki. faktycznie wynosi 1%. Poniżej przedstawiono tabelę, która pokazuje jak długi przestój różnych procentach umowy SLA, kwota przez rok, miesiąc i tydzień.

![Tabela umowy SLA](design-to-survive-failures/_static/image2.png)

Dlatego dostępność na poziomie 99,9% czasu zgodnie z umowy SLA oznacza, że usługa może nie działać 8.76 godzin roku lub 43,2 minut w miesiącu. To więcej czasu niż większość osób weź pod uwagę. Tak więc jako deweloper chcesz należy pamiętać, że pewien czas przestoju i obsługiwał go w taki sposób, bezpieczne. W pewnym momencie ktoś będzie korzystać z aplikacji, usługa ma być wyłączone i chcesz zminimalizować negatywny wpływ tego na klienta.

Co należy wiedzieć na temat umowy SLA jest jakim przedziale czasu odnosi się do: powoduje zegara Rozpoczynanie resetowania co tydzień, co miesiąc lub co rok? Na platformie Azure możemy teraz zresetować zegar co miesiąc, co jest dla Ciebie lepsza roczne umowy SLA, roczne umów SLA można ukryć odnotowane przesuwając z serii dobre miesięcy.

Oczywiście możemy zawsze Oczekuj lepiej niż SLA; zwykle będzie znacznie mniej, niż w dół. Gwarantuje to, jeśli mamy problem nigdy nie dłużej niż maksymalny czas przestoju możesz poprosić dla pieniędzy. Kwotę, wracając prawdopodobnie nie ma w pełni kompensuje znaczenie biznesowe nadmiarowe czas przestoju, ale ten aspekt umowy SLA działa jako zasady wymuszania i informuje o tym, że traktujemy on bardzo poważnie.

### <a name="composite-slas"></a>Złożone umowy SLA

Ważne jest, aby zastanów się, gdy rozważasz umowy SLA jest wpływ przy użyciu wielu usług w aplikacji, z każda usługa zawierająca osobne umowy SLA. Na przykład aplikacja naprawić korzysta z usługi Azure App Service Web Apps, Azure Storage i SQL Database. Oto ich numery umowy SLA w dniu ta Książka elektroniczna jest zapisywana w grudnia 2013:

![Umowa SLA witryny sieci Web, Storage, SQL Database](design-to-survive-failures/_static/image3.png)

Co to jest maksymalny czas, czego można oczekiwać dla aplikacji, w oparciu o te umowom SLA przestoju? Może się wydawać, że czas dół są takie same najgorszy wartość procentowa umów SLA lub 99,9% w tym przypadku. Byłoby wartość true, jeśli wszystkie trzy usługi zawsze nie powiodło się w tym samym czasie, ale która niekoniecznie co rzeczywiście się dzieje. Każda usługa może zakończyć się niepowodzeniem niezależnie w różnym czasie, więc trzeba obliczyć złożona umowa SLA mnożąc poszczególnych numerów umowy SLA.

![Złożona umowa SLA](design-to-survive-failures/_static/image4.png)

Dzięki czemu aplikacja może działać nie tylko 43,2 minut miesięcznie, ale 3 razy ta kwota, 108 minut w miesiącu — i nadal mieścić się w granicach umowy SLA platformy Azure.

Ten problem nie jest unikatowa na platformie Azure. Firma Microsoft zapewnia faktycznie Najlepsza chmura SLA dowolnej usługi w chmurze dostępna i będziesz mieć podobne problemy, aby się zająć, jeśli używasz dowolnego dostawcy usług w chmurze. Co to powoduje zaznaczenie jest znaczenie zastanawiać się, jak projektowanie aplikacji do obsługi błędów usługi nieuniknione poprawnie, ponieważ one mogą odbywa się wystarczająco często mających wpływ na klientów lub użytkowników.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Umowy SLA chmury w porównaniu do środowiska czas przestoju przedsiębiorstwa

Osoby czasami powiedzieć "W swojej aplikacji w organizacji nigdy nie mam te problemy." Jeśli zadasz pytanie o czas, jaki dół miesięcznie faktycznie posiadany zwykle mówią, "Dobrze, zdarza się czasami." I jeśli zadasz pytanie częstotliwość wprowadzanych, "Czasami musimy utworzyć kopię zapasową lub zainstalować nowe oprogramowanie serwera lub aktualizacji." Oczywiście, który zlicza czas przestoju. Większości aplikacji firmowych, chyba że są one szczególnie o kluczowym znaczeniu są faktycznie w dół dla więcej niż dozwolone przez umowy SLA usługi czas. Ale jeśli jesteś odpowiedzialny za i kontrolę nad jego serwera i infrastruktury, zwykle możesz mniej angst dół razy. W środowisku chmury jest zależny od kogoś innego i nie wiesz, co się dzieje, dzięki czemu może przeważnie, aby uzyskać bardziej niepokoju go.

Gdy przedsiębiorstwa uzyskuje większy procent czasu, niż uzyskiwanie chmury umowy SLA, im jego spędzając znacznie więcej pieniędzy na sprzęcie. Usługa w chmurze można to zrobić, ale musi opłaty za dużo więcej dla swoich usług. Zamiast tego należy korzystać z zalet usługi ekonomiczne i projektowania oprogramowania, tak aby błędy nieuniknione spowodować zakłócenie minimalną dla klientów. Zadanie jako Projektant aplikacji chmury nie jest tak wiele w celu uniknięcia błędów, aby uniknąć katastrofy i można to zrobić, skupiając się na oprogramowanie, nie na sprzęcie. Natomiast aplikacjami w przedsiębiorstwie Dokładamy wszelkich starań zmaksymalizować średniego czasu między awariami, aplikacje w chmurze Dokładamy wszelkich starań zminimalizować Średni czas do odzyskania.

### <a name="not-all-cloud-services-have-slas"></a>Nie wszystkie usługi w chmurze ma umowy SLA

Pamiętaj również, nie każdej usługi w chmurze nawet zgodnie z umową SLA. Jeśli Twoja aplikacja jest zależna od usługi, za pomocą żadnej gwarancji czasu, może być w dół znacznie dłużej, niż może imagine. Na przykład jeśli włączysz Zaloguj się do witryny, korzystając z mediów społecznościowych, takich jak Facebook lub Twitter, skontaktuj się z dostawcy usług, aby dowiedzieć się, jeśli jest objęta umową SLA i może się okazać tam nie. Ale jeśli usługa uwierzytelniania ulegnie awarii lub nie obsługuje liczby żądań, które generują go, klientów z zablokowanym dostępem do aplikacji. Mogą być wyłączone, dni lub dłużej. Twórcy jedną nową aplikację oczekiwano setki milionów plików do pobrania i zależna uwierzytelniania serwisu Facebook — ale nie komunikować się z usługi Facebook, przed przejściem na żywo i wykrytych za późno, które nie było żadnej umowy SLA dla usługi.

### <a name="not-all-downtime-counts-toward-slas"></a>Nie wszystkie liczniki czas przestoju kierunku umowy SLA

Niektóre usługi w chmurze celowo może odmówić usługi, jeśli aplikacja używa nadmiernej je. Jest to nazywane *ograniczania*. Jeśli usługa jest objęta umową SLA, należy go stanu warunki, w których może być ograniczona i projekt aplikacji należy uniknąć tych warunków i odpowiednio reagować na ograniczenia wydajności, jeśli występuje on. Na przykład jeśli żądania do usługi zaczynają się niepowodzeniem, jeśli klient przekroczy pewną liczbę na sekundę, chcesz upewnij się, że automatyczne ponawianie prób nie występują tak szybko, spowodują ograniczania kontynuować. Więcej na temat ograniczania przepustowości w odpowiemy [obsługi błędów przejściowych rozdział](transient-fault-handling.md).

## <a name="summary"></a>Podsumowanie

W tym rozdziale próbował łatwiej wykorzystać, dlaczego rzeczywistych aplikacji w chmurze musi być zaprojektowany do poprawnego działania po awarii. Począwszy od [następny rozdział](monitoring-and-telemetry.md), pozostałe wzorców w tej serii przejść do bardziej szczegółowo kilka strategii, można użyć, aby to zrobić:

- Masz dobre [monitorowanie i telemetria](monitoring-and-telemetry.md), dzięki czemu możesz dowiedzieć się szybko o błędach, które wymagają interwencji, a użytkownik ma wystarczające informacje, aby je rozwiązać.
- [Obsługa błędów przejściowych](transient-fault-handling.md) przez zaimplementowanie logiki ponowień inteligentne tak, aby aplikacja automatycznie odzyskuje po można i powraca do [wyłącznik](transient-fault-handling.md#circuitbreakers) logiki, gdy nie jest.
- Użyj [rozproszonej pamięci podręcznej](distributed-caching.md) aby zminimalizować problemy przepływności, opóźnienia i połączenie z dostępu do bazy danych.
- Implementowanie luźne sprzężenia za pośrednictwem [wzorzec pracy skoncentrowany na kolejkach](queue-centric-work-pattern.md), dzięki czemu Twoje fronton aplikacji mogą w dalszym ciągu działać po zaplecza nie działa.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji zobacz dalszych rozdziałach tę książkę elektroniczną i następujące zasoby.

Dokumentacja:

- [Przed uszkodzeniami: Wskazówki dotyczące architektury na temat odporności chmury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument, Marc Mercuri, Ulrich Homann i Andrew Townhill. Wersja strony sieci Web przed uszkodzeniami serii filmów wideo.
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usługach Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument — Markiem Simmsem i Michael Thomassy.
- [Wskazówki techniczne ciągłości biznesowej platformy Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Oficjalny dokument Patrick Wickline i Jason Roth.
- [Odzyskiwanie po awarii i wysoka dostępność dla aplikacji platformy Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Oficjalny dokument przez Michaela McKeown, Hanu Kommalapati i Jason Roth.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące wdrażania centrum danych Multi, wzorzec wyłącznika.
- [Pomoc techniczna platformy Azure — umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).
- [Ciągłość prowadzenia działalności biznesowej w usłudze Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentacja usługi o wysokiej dostępności i odzyskiwania funkcji usługi SQL Database.
- [Wysoka dostępność i odzyskiwanie po awarii programu SQL Server na maszynach wirtualnych platformy Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Filmy wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Dziewięć serii Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia informacje o szczegółowo pojęcia i zasady dotyczące architektury w sposób bardzo dostępny i interesujące z historii z doświadczenia zespołu doradczego klientów firmy Microsoft (CAT) z klientów. Odcinki 1 do 8 konstrukcyjnym szczegółowo powody do projektowania aplikacji w chmurze po awarii. Zobacz też monitowania dyskusję na temat ograniczania przepustowości w odcinek 2 zaczynając od 49:57, omówienie punktów awarii i trybów awarii w odcinek 2 zaczynając od 56:05 i dyskusji wyłączników w odcinek 3 zaczynając od 40:55.
- [Tworzenie Big: Wnioski wyciągnięte z klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). — Markiem Simmsem omawia projektowanie pod kątem błędów i instrumentacji wszystko. Podobnie jak przed uszkodzeniami serii, ale zbliża się bardziej szczegółowe informacje z instrukcjami.

> [!div class="step-by-step"]
> [Poprzednie](unstructured-blob-storage.md)
> [dalej](monitoring-and-telemetry.md)
