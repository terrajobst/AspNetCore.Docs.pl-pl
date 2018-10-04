---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Rozproszonej pamięci podręcznej (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 26866ef9d13a198aab627ccf0f5e1ff3d2304427
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577551"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Rozproszonej pamięci podręcznej (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Poprzednim rozdziale przyjrzano obsługi błędów przejściowych i wymienione w pamięci podręcznej jako strategii wyłącznika. Ten rozdział zapewnia większej ilości informacji kontekstowych dotyczących buforowania, łącznie z ich używać i często używane wzorce dotyczące korzystania z niego i sposobie jego implementowania na platformie Azure.

## <a name="what-is-distributed-caching"></a>Co to jest rozproszonej pamięci podręcznej

Pamięć podręczna zapewnia wysoką przepływność, małe opóźnienia dostępu do danych najczęściej używanych aplikacji, za przechowywanie danych w pamięci. Dla aplikacji w chmurze najbardziej przydatne typ pamięci podręcznej jest rozproszonej pamięci podręcznej, co oznacza dane nie są przechowywane w pamięci serwera internetowego, ale od innych zasobów w chmurze, a dane w pamięci podręcznej będą dostępne dla wszystkich serwerów sieci web aplikacji (lub inne chmury maszyn wirtualnych tego ar e używanych przez aplikację).

![Diagram przedstawiający wielu serwerów sieci web, uzyskiwanie dostępu do tych samych serwerów pamięci podręcznej](distributed-caching/_static/image1.png)

Gdy aplikacja jest skalowana, dodając lub usuwając serwerów lub serwerów są zastępowane z powodu uaktualnienia lub błędy, dane w pamięci podręcznej pozostaje dostępna na każdym serwerze, który uruchamia aplikację.

Unikając duże opóźnienia dostępu do danych w trwałym magazynie danych, buforowanie może znacznie zwiększyć czas odpowiedzi aplikacji. Na przykład podczas pobierania danych z pamięci podręcznej jest znacznie szybsze niż z relacyjnej bazy danych.

Zaletą po stronie buforowania jest krótszy ruch do magazynu trwałego danych, co może spowodować obniżenie kosztów, gdy istnieją wyjście danych opłaty są pobierane za trwałym magazynie danych.

## <a name="when-to-use-distributed-caching"></a>Kiedy należy używać rozproszonej pamięci podręcznej

Buforowanie działa najlepiej w przypadku obciążeń aplikacji, wykonaj odczytu więcej niż zapisywania danych, a gdy model danych obsługuje organizacji klucz/wartość, która umożliwia przechowywanie i pobieranie danych w pamięci podręcznej. Jest również bardziej użyteczny, gdy użytkownicy aplikacji udostępniają wiele wspólnych danych; na przykład pamięć podręczna nie zapewnia korzyści tyle Jeśli każdy użytkownik zwykle pobiera dane, które użytkownik. Przykładem, gdzie pamięć podręczna może być bardzo korzystne jest katalog produktów, ponieważ dane nie zmieniają się często i wszyscy klienci są spojrzenie na te same dane.

Zaletą korzystania z pamięci podręcznej staje się coraz bardziej mierzalne bardziej umożliwia skalowanie aplikacji miarę limity przepływności i opóźnień opóźnienia magazynu trwałego danych więcej ograniczenie wydajności ogólnej aplikacji. Jednak zaimplementować, buforowanie innych powodów niż również pod względem wydajności. Dla danych, które nie musi być doskonale aktualne, gdy wyświetlane użytkownikowi dostępu do pamięci podręcznej może służyć jako wyłącznik po trwałym magazynie danych, nie odpowiada lub jest niedostępna.

## <a name="popular-cache-population-strategies"></a>Strategie populacji popularnej pamięci podręcznej

Aby można było pobierać dane z pamięci podręcznej, musisz zapisać ją tam najpierw. Istnieje kilka strategii w celu uzyskania danych, które są potrzebne do pamięci podręcznej:

- Na żądanie / pamięci podręcznej Aside

    Aplikacja podejmie próbę pobrania danych z pamięci podręcznej, a jeśli pamięci podręcznej nie ma danych ("Chybienia"), aplikacja przechowuje dane w pamięci podręcznej, więc, że będzie on dostępny przy następnym. Przy następnym aplikacja podejmie próbę uzyskania tych samych danych, znajdzie czego szukać w pamięci podręcznej ("Strzał"). Aby uniknąć pobierania dane w pamięci podręcznej, które uległy zmianie w bazie danych, unieważnienia pamięci podręcznej podczas wprowadzania zmian w magazynie danych.
- Wypychanie danych w tle

    Usługi w tle wypychanie danych do pamięci podręcznej w regularnych odstępach czasu i aplikacja zawsze ściąga z pamięci podręcznej. To świetnie działa podejście ze źródłami danych duże opóźnienie, które nie wymagają zawsze zwraca najnowsze dane.
- Wyłącznik

    Aplikacja zazwyczaj komunikuje się bezpośrednio z trwałym magazynie danych, ale gdy problemów z dostępnością znajdują się w trwałym magazynie danych, aplikacja pobiera dane z pamięci podręcznej. Dane mogą umieścić w pamięci podręcznej przy użyciu pamięci podręcznej specjalnie lub strategii wypychania danych w tle. Jest to błąd obsługi strategii, a nie strategii zwiększanie wydajności.

Aby zachować bieżące dane w pamięci podręcznej, można usunąć wpisy w pamięci podręcznej powiązane, gdy aplikacja tworzy, aktualizacji lub usuwa dane. Jeśli tak jest dla aplikacji, aby otrzymywać czasami dane, które są nieco nieaktualne, możesz polegać na czas wygaśnięcia można skonfigurować, aby ustawić limit pamięci podręcznej wiek, dane mogą być.

Możesz skonfigurować wygaśnięcie bezwzględne (ilość czasu od momentu utworzenia elementu pamięci podręcznej) lub przedłużanie ważności (ilość czasu od czasu ostatniego, do których uzyskano dostęp do elementu pamięci podręcznej). Bezwzględna wygaśnięcia jest używany, gdy są zależności na mechanizmie wygaśnięcia pamięci podręcznej, aby zapobiec danych stają się zbyt stare. W aplikacji napraw go firma Microsoft będzie ręcznie wykluczyć elementy starych pamięci podręcznej, a następnie użyjemy wygaśniecie, aby zapewnić najbardziej aktualne dane w pamięci podręcznej. Niezależnie od zasady wygasania, które wybierzesz pamięci podręcznej zostanie automatycznie eksmitować najstarsze elementy (co najmniej ostatnio używane lub LRU) po osiągnięciu limitu pamięci pamięci podręcznej.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Przykładowy kod odkładania do pamięci podręcznej dla aplikacji poprawka

W poniższym przykładowym kodzie możemy sprawdzić pamięci podręcznej najpierw podczas pobierania zadania rozwiązać go. Jeśli zadanie zostanie znaleziony w pamięci podręcznej, zostanie zwrócona. Jeśli nie zostanie znaleziony, możemy pobrać go w bazie danych i zapisz go w pamięci podręcznej. Czy wprowadzone zmiany do dodania w pamięci podręcznej w celu `FindTaskByIdAsync` metody są wyróżnione.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Podczas aktualizacji lub usunięcia zadania rozwiązać go masz unieważnienie (Usuń) buforowane zadanie. W przeciwnym razie przyszłość próbuje odczytać, że to zadanie będzie w dalszym ciągu uzyskać stare dane z pamięci podręcznej.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Oto przykłady ilustrują prostego kodu buforowania; pamięć podręczna nie została zaimplementowana do pobrania rozwiązać go projektu.

## <a name="azure-caching-services"></a>Pamięci podręcznej usługi Azure

Platforma Azure oferuje następujące usługi pamięci podręcznej: [usługi Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) i [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Usługa Azure Redis cache jest oparty na popularnej ["open source Redis Cache"](http://redis.io/) i jest pierwszym wyborem dla większości scenariuszy buforowania.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Za pomocą pamięci podręcznej dostawcy stanu sesji platformy ASP.NET

Jak wspomniano w [sieci web development najlepsze rozwiązania w zakresie rozdział](web-development-best-practices.md), najlepszym rozwiązaniem jest unikać stanu sesji. Jeśli aplikacja wymaga stanu sesji, dalej najlepszym rozwiązaniem jest unikać domyślnego dostawcę w pamięci, ponieważ nie umożliwiających skalowanie w poziomie (wielu wystąpień serwera sieci web). Dostawca stanu sesji ASP.NET programu SQL Server umożliwia lokacji, która działa na wielu serwerach sieci web, aby używać stanu sesji, ale wiąże się koszt duże opóźnienie w porównaniu do dostawcy usługi w pamięci. Najlepszym rozwiązaniem w przypadku używania stan sesji jest użycie dostawcy pamięci podręcznej, takich jak [dostawca stanu sesji dla usługi Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Podsumowanie

Przedstawiono sposób aplikacji naprawić implementacji buforowania, aby poprawić czas odpowiedzi i skalowalność i umożliwia korzystanie z aplikacji w dalszym ciągu można szybko reagujących operacji odczytu, gdy baza danych jest niedostępne. W [następny rozdział](queue-centric-work-pattern.md) pokażemy sposób dodatkowo poprawić skalowalność i aplikacji, nadal odpowiadać za operacje zapisu.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat buforowania zobacz następujące zasoby.

Dokumentacja

- [Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx). Oficjalna dokumentacja MSDN na temat buforowania na platformie Azure.
- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące buforowania i wzorzec z odkładaniem do pamięci podręcznej.
- [Przed uszkodzeniami: Wskazówki dotyczące architektury na temat odporności chmury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument, Marc Mercuri, Ulrich Homann i Andrew Townhill. Zobacz sekcję dotyczącą pamięci podręcznej.
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usługach Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Oficjalny dokument — Markiem Simmsem i Michael Thomassy. Zobacz sekcję dotyczącą rozproszonej pamięci podręcznej.
- [Rozproszonej pamięci podręcznej w ścieżce na skalowalność](https://msdn.microsoft.com/magazine/dd942840.aspx). Starsze artykuł w MSDN Magazine (2009), ale wyraźnego pisemnego wprowadzenie do rozproszonego buforowania w całości. Przechodzi do bardziej szczegółowe niż buforowania sekcje oficjalne dokumenty przed uszkodzeniami i najlepszych rozwiązań.

Wideo

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Dziewięć serii Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia widok 400-level jak zaprojektować aplikacje w chmurze. Ta seria skupia się na teorii i powodów dlaczego; szczegółowe instrukcje Zobacz Tworzenie dużych szeregi według — Markiem Simmsem. Zobacz Omówienie buforowania w odcinek 3 zaczynając od 1:24:14.
- [Tworzenie Big: Wnioski wyciągnięte z klientów platformy Azure — część I](https://channel9.msdn.com/Events/Build/2012/3-029). Simona Davies w tym artykule omówiono rozproszonej pamięci podręcznej zaczynając od 46:00. Podobnie jak przed uszkodzeniami serii, ale zbliża się bardziej szczegółowe informacje z instrukcjami. Prezentacja podano 31 października 2012 r., więc nie obejmuje ona usługę buforowania w funkcji Web Apps w usłudze Azure App Service, która została wprowadzona w 2013.

Przykładowy kod

- [Podstawy usługi na platformie Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja, która implementuje rozproszonej pamięci podręcznej. Zobacz wpis w blogu towarzyszący [podstawy usługi chmury — podstawy buforowania](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Poprzednie](transient-fault-handling.md)
> [dalej](queue-centric-work-pattern.md)
