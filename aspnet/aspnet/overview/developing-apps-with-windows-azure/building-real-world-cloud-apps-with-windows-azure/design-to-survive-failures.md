---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Projekt po awarii (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: "Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: a0ee790da07c99cdb1279a6bca637a4ce8076e84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Projektowanie po awarii (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Jest jednym z elementów, trzeba myśleć o podczas budowania dowolnego typu aplikacji, ale szczególnie taki, który zostanie uruchomiony w chmurze, w którym wiele osób będzie używany, sposób projektowania aplikacji, dzięki czemu można bezpiecznie obsłużyć awarie i w dalszym ciągu dostarczać wartość jak możliwe. Mając wystarczająco dużo czasu, przebiegu się nie udaje się w dowolnym środowisku lub dowolnego systemu oprogramowania. Sposób obsługi tych sytuacji w Twojej aplikacji określa, jak zły otrzyma klientów i czas, jaki trzeba poświęcić analizowanie i rozwiązywanie problemów.

## <a name="types-of-failures"></a>Typy błędów

Istnieją dwie kategorie podstawowych błędów, które należy obsługiwać inaczej:

- Przejściowy samonaprawiania błędów, takich jak problemy z połączeniem sieciowym tymczasowymi.
- Trwałej błędów, które wymagają interwencji.

Przejściowych błędów można zaimplementować zasady ponawiania, aby upewnić się, że większość czasu aplikacji odzyskuje szybko i automatycznie. Klienci zauważyć nieco więcej czasu odpowiedzi, ale w przeciwnym razie nie będzie mieć wpływ. Poniżej opisano kilka metod obsługi tych błędów w [obsługi błędów przejściowych rozdział](transient-fault-handling.md).

Dla reguły trwałe niepowodzenia, można zaimplementować monitorowanie i rejestrowanie funkcji, który powiadamia niezwłocznie o problemach i które ułatwia analiza głównych przyczyn. Poniżej opisano kilka metod zwiększające u góry tego rodzaju błędów w [rozdział monitorowanie i dane telemetryczne](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Błąd zakresu

Masz również myśleć o zakresie awarii — czy ma to wpływ na jednym komputerze, całej usługi, takie jak bazy danych SQL lub magazynu lub całego regionu.

![Błąd zakresu](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Błędy maszyn

Na platformie Azure serwer nie powiodło się automatycznie zastępowane nową i aplikacji w chmurze dobrze zaprojektowanego odzyskuje z tego typu awarii, automatycznie i szybko. Wcześniej podkreślić, firma Microsoft skalowalność zalet warstwy sieci web bezstanowe i kolejną zaletą statelessness jest łatwość odzyskiwania z serwera nie powiodło się. Łatwość odzyskiwania jest również jedną z zalet platforma jako usługa (PaaS) funkcje, takie jak bazy danych SQL i aplikacje sieci Web usługi aplikacji Azure. Awarie sprzętowe występują rzadko, ale podczas występowania się, że te usługi obsługi je automatycznie. nawet nie trzeba napisać kod obsługujący maszyny błędy podczas korzystania z jednej z tych usług.

### <a name="service-failures"></a>Awarie usług

Aplikacje w chmurze zazwyczaj korzystają z wielu usług. Na przykład aplikacja napraw korzysta z usługi SQL Database, usługa Magazyn i wdrażania aplikacji sieci web w usłudze Azure App Service. Co spowoduje aplikacji zrobić po awarii jednego z usługi, które zależą od? Niektóre błędy usługi przyjazną nazwę w polu "Niestety, spróbuj ponownie później" komunikat może być najlepiej możesz zrobić. Jednak w wielu scenariuszach można wykonać lepiej. Na przykład po magazynie danych zaplecza nie działa, można akceptowanie danych wejściowych użytkownika, wyświetlanie "zostały odebrane żądanie" i przechowywać dane wejściowe wiadomo gdzie else tymczasowo; następnie podczas usługi należy działa, możesz pobrać dane wejściowe i go przetworzyć.

[Skoncentrowane kolejki wzorzec pracy](queue-centric-work-pattern.md) rozdziale przedstawiono jeden sposób obsługi tego scenariusza. Aplikacja poprawka przechowuje zadania w bazie danych SQL, ale nie musi zakończyć pracy, gdy baza danych SQL nie działa. W tym rozdziale przedstawiono będzie sposób przechowywania danych wejściowych użytkownika dla tego zadania w kolejce i używać procesu roboczego do odczytywania kolejki i zaktualizować zadania. Jeśli SQL działa, możliwość tworzenia zadania poprawka nie mają wpływu; proces roboczy może czekać i przetworzyć nowe zadania, gdy baza danych SQL jest dostępna.

### <a name="region-failures"></a>Błędy regionu

Cały regionów może zakończyć się niepowodzeniem. Klęski żywiołowej może spowodować utratę centrum danych, może pobrać spłaszczone przez meteor wierszu magistrali do centrum danych można wyciąć przez rolnika zakopanie krowy Łyżka koparki itd. Jeśli aplikacja znajduje się w centrum danych stricken co chcesz zrobić? Istnieje możliwość skonfigurowania aplikacji na platformie Azure jednoczesną w wielu regionach, dzięki czemu jest awarii jednego, w przypadku kontynuowania uruchomiona w innym regionie. Takie błędy są bardzo rzadko wystąpienia, a większość aplikacji nie przejść za pomocą hoops niezbędne do zapewnienia ciągłości usługi za pośrednictwem tego rodzaju błędów. Zobacz sekcję zasobów na końcu rozdziale informacji na temat sposobu na utrzymanie aplikacji dostępne nawet za pośrednictwem awarii regionu.

Celem Azure jest upewnienie się, obsługa wszystkich tego rodzaju błędów znacznie łatwiejsze i zobaczysz jak możemy jest operacją w poniższych rozdziałach przykłady.

## <a name="slas"></a>Umowy SLA

Użytkownicy często otrzymywać informacje o umów dotyczących poziomu usług (SLA) w środowisku chmury. Zasadniczo te są ze zobowiązania wchodzące w firmach o jak niezawodnej usługi. 99,9%, SLA oznacza można spodziewać usługi działają poprawnie 99,9% czasu. Jest dość typowy wartość SLA dźwięki, takie jak bardzo duża liczba, ale nie może zrealizować dół czas, jaki. rzeczywista wynosi 1%. Oto jest tabela przedstawiająca przestoju, ile różnych procentach umowy SLA, ilość przez rok, miesiąc i tydzień.

![Tabela umowy SLA](design-to-survive-failures/_static/image2.png)

Dlatego 99,9%, SLA oznacza usługi mogą być wyłączone 8.76 godziny roku lub 43,2 minut w miesiącu. To więcej czasu niż należy pamiętać, większość osób. Dlatego jako deweloper chcesz należy pamiętać, że pewne czas przestoju i jego obsługa w sposób bezpieczne. W pewnym momencie ktoś ma używać aplikacji, usługi ma być wyłączone i chcesz zminimalizować negatywny wpływ na tym na klienta.

Informacje o umowie SLA jest to jakim przedziale czasu odnosi się do: jest resetowane zegar co tydzień, co miesiąc lub co rok? Na platformie Azure resetowana zegar każdego miesiąca, która jest lepszym rozwiązaniem dla Ciebie niż roczne umowy SLA, ponieważ corocznych SLA można ukryć zły miesięcy przesuwając z serii dobrej miesięcy.

Oczywiście możemy zawsze Oczekuj lepszą wydajność niż SLA; zwykle będzie znacznie mniej niż w dół. Gwarantuje to, czy jeśli kiedykolwiek dół przez czas dłuższy niż maksymalna wartość czas przestoju poproś dla pieniędzy. Kwotę, wracając prawdopodobnie nie w pełni kompensuje znaczenie biznesowe przekroczenia czasu, ale element umowy SLA działa jako zasadach wymuszania i informuje o tym, że firma Microsoft zabrać bardzo poważnie.

### <a name="composite-slas"></a>Złożone umowy SLA

Ważne jest, aby traktować podczas przeglądania SLA jest wpływ przy użyciu wielu usług w aplikacji z każdej usługi mających oddzielne umowy SLA. Na przykład aplikacja napraw korzysta z aplikacji sieci Web usługi aplikacji Azure, Azure Storage i bazy danych SQL. Poniżej przedstawiono numerów umowy SLA w dniu Książka elektroniczna jest zapisywana w grudniu 2013:

![Baza danych SQL witryny sieci Web, magazynu, umowy SLA](design-to-survive-failures/_static/image3.png)

Co to jest maksymalny czasu, który można oczekiwać aplikacji oparte na tych umów SLA usługi? Może się okazać, że dół czasu są takie same najgorszy wartość procentowa umów SLA lub 99,9% w takim przypadku. Który będzie mieć wartość true, jeśli wszystkie trzy usługi zawsze nie powiodło się w tym samym czasie, ale, która nie jest zawsze faktycznie wpływ. Każda usługa może zakończyć się niepowodzeniem niezależnie w różnym czasie, dlatego należy obliczyć złożonego SLA mnożąc poszczególnych numerów umowy dotyczącej poziomu usług.

![Złożone umowy SLA](design-to-survive-failures/_static/image4.png)

Dzięki aplikacji może działać nie tylko 43,2 minut miesiąca, ale 3 razy wielkość, 108 minut miesięcznie — i nadal mieścić się w granicach umowy SLA platformy Azure.

Ten problem nie jest unikatowa na platformie Azure. Udostępniamy faktycznie najlepsze chmury SLA wszystkie dostępne usługi w chmurze i będziesz mieć problemy podobne do postępowania w przypadku korzystania z usługi w chmurze dowolnego dostawcy. To wyróżnia jest znaczenie myślenia o sposobie projektowania aplikacji do obsługi błędów usługi nieuniknione poprawnie, ponieważ może być wystarczająco często wpłynąć na klientów lub użytkowników.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>W porównaniu do obsługi czas przestoju enterprise SLA chmury

Czasami powiedzieć osób, "W mojej aplikacji przedsiębiorstwa I nigdy nie mają te problemy." Ile dół raz w ciągu miesiąca od faktycznie mają, zwykle mówią, "Również zdarza się to od czasu do czasu." I jak często od, wprowadzanych który "Czasami musimy wykonać kopię zapasową lub zainstalowania nowego serwera lub aktualizacji oprogramowania." Oczywiście, które zlicza czas przestoju. Większości aplikacji firmowych, chyba że są one szczególnie krytycznym są faktycznie dół więcej niż dozwolone przez naszych umów SLA usługi czas. Jednak gdy jest serwera i infrastruktury i ponosisz odpowiedzialność i kontrolę nad jego, często uznać mniej angst dół razy. W środowisku chmury jest zależny od innego użytkownika i nie można ustalić co się dzieje, dlatego może być większe, aby uzyskać więcej niepokoju go.

Gdy jednostka osiągnięcie większej procent czasu nie możesz uzyskać z chmury umowy SLA, robią go przez wydatków znacznie większą oszczędność pieniędzy na sprzęcie. Usługi w chmurze można to zrobić, ale będzie musiał obciążania wielu innych swoich usług. Zamiast tego należy korzystać z ekonomicznego usługi i zaprojektować oprogramowania tak, aby nieuniknione błędy powodują minimalna zakłócenia dla klientów. Zadanie Projektant aplikacji chmury nie jest tak wiele w celu uniknięcia błędu, aby uniknąć katastrofy i można to zrobić, koncentrujących się na oprogramowanie, a nie na sprzęcie. Dlatego aplikacje przedsiębiorstwa Dokładamy wszelkich starań zmaksymalizować Średni czas między awariami, aplikacje w chmurze starań, aby zminimalizować czas do odzyskania.

### <a name="not-all-cloud-services-have-slas"></a>Nie wszystkie usługi w chmurze ma umowy SLA

Pamiętaj także że nie wszystkie usługi w chmurze ma nawet umowy SLA. Jeśli aplikacja jest zależna od usługi z żadnej gwarancji, czasu, można w dół do tej pory dłużej, niż może Wyobraź sobie. Na przykład jeśli włączysz logowania do witryny przy użyciu dostawcy społecznościowych, takich jak Facebook lub Twitter, skontaktuj się z dostawcy usług, aby dowiedzieć się, jeśli istnieje umowy dotyczącej poziomu usług i może być tam nie jest jednym. Jednak jeśli z usługą uwierzytelniania ulegnie awarii, nie obsługuje odebranej liczby żądań, które generują go z aplikacji zablokowanym klientów. Mogą być wyłączone przez dni lub dłużej. Twórcy jedną nową aplikację oczekiwano setki milionów pliki do pobrania i jest zależna na uwierzytelniania serwisu Facebook —, ale przed przejściem za późno na żywo i odnalezionych nie zwróć się do usługi Facebook, które nie było umowy dotyczącej poziomu usług dla tej usługi.

### <a name="not-all-downtime-counts-toward-slas"></a>Nie wszystkie liczniki przestoju kierunku umowy SLA

Niektóre usługi w chmurze może celowo odmowy usługi, jeśli aplikacja nadmiernie korzysta z nich. Ta metoda jest wywoływana *ograniczania*. Jeśli usługa ma umowy SLA, powinien on stanu warunki, w których może zdławionych i projekt aplikacji należy uniknąć tych warunków i odpowiednio zareagować na ograniczenie, jeśli występuje. Na przykład jeśli żądania do usługi kończą się niepowodzeniem po przekroczeniu pewną liczbę na sekundę, chcesz upewnij się, że automatyczne ponawianie prób nie występują tak szybko, one powodować ograniczenie kontynuować. Firma Microsoft zapewnia więcej na temat ograniczania przepustowości w [obsługi błędów przejściowych rozdział](transient-fault-handling.md).

## <a name="summary"></a>Podsumowanie

W tym rozdziale próbował pomóc należy pamiętać, dlaczego aplikacji w chmurze rzeczywistych musi być przeznaczone do poprawnego działania po awarii. Począwszy od [następnego rozdziału](monitoring-and-telemetry.md), pozostałe wzorce w tej serii przejdź na temat kilka strategii, można użyć w tym:

- Dobre ma [monitorowania i dane telemetryczne](monitoring-and-telemetry.md), aby sprawdzić szybko o błędach, które wymagają interwencji i zawiera wystarczających informacji potrzebnych do ich rozwiązywania.
- [Obsługa błędów przejściowych](transient-fault-handling.md) zaimplementowanie Logika ponawiania inteligentnego, dzięki czemu aplikacja odzyskuje automatycznie po można i powraca do [wyłącznika](transient-fault-handling.md#circuitbreakers) logiki, gdy nie jest.
- Użyj [rozproszonej pamięci podręcznej](distributed-caching.md) aby zminimalizować problemy przepływności, opóźnienia i połączenie z dostępem do bazy danych.
- Implementowanie utracić sprzężenia za pośrednictwem [wzorzec skoncentrowane kolejki pracy](queue-centric-work-pattern.md), aby kontynuować pracę podczas wewnętrznej nie działa z fronton aplikacji.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji zobacz dalszych rozdziałach Książka elektroniczna i następujących zasobów.

Dokumentacja:

- [Przed uszkodzeniami: Wskazówki dotyczące architektury chmury odporność](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx). Oficjalny dokument Mercuri wytłoków, Ulrich Homann i Andrew Townhill. Wersja strony sieci Web seria filmów przed uszkodzeniami.
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę na usług w chmurze Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). Oficjalny dokument przez moduły SIMM znaku i Michael Thomassy.
- [Wskazówki techniczne ciągłości biznesowej Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh873027.aspx). Oficjalny dokument Patrick Wickline i Jason Roth.
- [Odzyskiwanie po awarii i wysoką dostępność aplikacji Azure](https://msdn.microsoft.com/en-us/library/windowsazure/dn251004.aspx). Oficjalny dokument Michael McKeown, Hanu Kommalapati i Jason Roth.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Wskazówki dotyczące wdrażania centrum danych Zobacz Multi, wyłącznika wzorca.
- [Pomoc techniczna platformy Azure - umów dotyczących poziomu usług](https://azure.microsoft.com/support/legal/sla/).
- [Ciągłość prowadzenia działalności biznesowej w bazie danych Azure SQL](https://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx). Dokumentację dotyczącą bazy danych SQL wysokiej dostępności i odzyskiwaniem po awarii funkcji odzyskiwania.
- [Wysoka dostępność i odzyskiwanie po awarii dla programu SQL Server na maszynach wirtualnych Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj870962.aspx).

Filmy wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia informacje o szczegółowo pojęcia i architektury zasad w sposób bardzo dostępny i interesujące z wątków z doświadczenia zespół Advisory klienta firmy Microsoft (CAT) z konkretnymi klientami. Odcinki 1 i 8 przechodzi szczegółowo w przyczyny projektowania aplikacji w chmurze po awarii. Zobacz też kolejnych dyskusji ograniczania przepustowości w epizodu 2, zaczynając od 49:57 dyskusji punktów awarii i tryby awarii w epizodu 2, zaczynając od 56:05 i Omówienie podziałów obwód w epizodu 3, zaczynając od 40:55.
- [Kompilowanie Big: Wnioski uzyskane od klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). Oznacz moduły SIMM wspomniana projektowanie pod kątem awarii i Instrumentacja wszystko. Podobnie jak seria przed uszkodzeniami, ale przechodzi w szczegółowe instrukcje.

>[!div class="step-by-step"]
[Poprzednie](unstructured-blob-storage.md)
[dalej](monitoring-and-telemetry.md)
