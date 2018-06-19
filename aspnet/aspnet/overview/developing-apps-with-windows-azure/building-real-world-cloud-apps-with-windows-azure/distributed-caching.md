---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Rozproszonej pamięci podręcznej (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 3600200f9bb705ccf66c859547668bdf8e89d97a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868675"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Rozproszone buforowania (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Poprzedni rozdział przeglądał obsługi błędu przejściowego i wymienionych buforowanie jako strategii wyłącznika. Ten rozdział zawiera więcej informacji uzupełniających na temat buforowania, tym, kiedy należy używać go typowe wzorce dotyczących korzystania z niego oraz sposób jej wdrożeniu na platformie Azure.

## <a name="what-is-distributed-caching"></a>Co to jest dystrybuowane buforowanie

Pamięć podręczną zapewnia wysokiej przepływności, małych opóźnieniach dostępu do danych aplikacji powszechnie używanych przez przechowywanie danych w pamięci. Dla aplikacji w chmurze najbardziej przydatne typ pamięci podręcznej jest rozproszonej pamięci podręcznej, co oznacza dane nie są przechowywane w pamięci serwera poszczególnych sieci web, ale od innych zasobów w chmurze, a buforowane dane będą dostępne dla wszystkich serwerów sieci web aplikacji (lub innych usług w chmurze maszyn wirtualnych tej ar e używany przez aplikację).

![Diagram przedstawiający wielu serwerów sieci web podczas uzyskiwania dostępu do tych samych serwerów pamięci podręcznej](distributed-caching/_static/image1.png)

Gdy aplikacja skaluje przez dodawanie lub usuwanie serwerów lub gdy serwery są zastępowane z powodu uaktualnienia lub błędy, pamięci podręcznej dane pozostają dostępne dla każdego serwera, który uruchamia aplikację.

Dzięki unikaniu duże opóźnienia dostępu do danych magazynu danych, buforowanie mogą znacznie zwiększyć czas odpowiedzi aplikacji. Na przykład pobieranie danych z pamięci podręcznej jest znacznie szybsza niż z relacyjnej bazy danych.

Zaletą po stronie buforowanie jest zmniejszenie ruchu w magazynie danych, co może spowodować obniżenie kosztów, gdy istnieją wyjście danych opłaty za magazyn danych.

## <a name="when-to-use-distributed-caching"></a>Kiedy należy używać buforowania rozproszonego

Buforowanie działa najlepiej dla obciążeń aplikacji, który nie odczytu więcej niż zapisywanie danych i gdy modelu danych obsługuje organizacja klucz/wartość, która umożliwia przechowywanie i pobieranie danych w pamięci podręcznej. Jest również bardziej przydatne w przypadku aplikacji użytkownicy udostępniają wiele wspólnych danych; na przykład pamięci podręcznej nie będzie zapewniają jako wiele korzyści, jeśli każdy użytkownik zwykle pobiera dane unikatowe dla tego użytkownika. Przykład gdy buforowanie może być bardzo przydatne jest katalog produktów, ponieważ dane nie zmienia się często i szukasz wszystkich klientów w tych samych danych.

Zaletą buforowania staje się coraz bardziej mierzalnych skaluje więcej aplikacji, jak limity przepustowości i opóźnień opóźnienia magazynu trwałego danych stanie więcej limit na ogólną wydajnością. Jednak zaimplementować, buforowanie innego powodu niż również wydajności. Dla danych, które nie muszą być dokładnie aktualne, gdy pokazywana użytkownikowi dostępu do pamięci podręcznej może służyć jako wyłącznika, dla, gdy magazyn danych jest odpowiadać lub jest niedostępny.

## <a name="popular-cache-population-strategies"></a>Strategie populacji popularnych pamięci podręcznej

Aby można było pobierać dane z pamięci podręcznej, należy go przechowywać najpierw. Istnieje kilka strategii pobierania danych, które są potrzebne do pamięci podręcznej:

- Na żądanie / pamięci podręcznej Zarezerwuj

    Aplikacja próbuje pobrać dane z pamięci podręcznej, a gdy pamięci podręcznej nie ma danych ("Chybienia"), aplikacja przechowuje dane w pamięci podręcznej, dzięki czemu będzie on dostępny przy następnym. Znajdzie przy następnym aplikacja próbuje pobrać te same dane co szuka w pamięci podręcznej ("trafienie"). Aby uniknąć pobierania buforowane dane, które uległy zmianie w bazie danych, unieważnienie pamięci podręcznej podczas wprowadzania zmian w magazynie danych.
- Wypychanie danych w tle

    Usługi w tle wypychanie danych do pamięci podręcznej w regularnych odstępach czasu i aplikacji zawsze ściąga z pamięci podręcznej. To podejście Wielkiej współpracuje ze źródłami danych duże opóźnienia, które nie wymagają zawsze zwraca najnowsze dane.
- Wyłącznika

    Aplikacji zwykle komunikuje się bezpośrednio z magazynem danych, ale gdy magazyn danych ma problemów z dostępnością, aplikacja pobiera dane z pamięci podręcznej. Dane może umieścić w pamięci podręcznej przy użyciu pamięci podręcznej Odłóż lub strategii wypychania danych tła. Jest to błąd obsługi strategii zamiast strategii zwiększenie wydajności.

Aby aktualność danych w pamięci podręcznej, można usunąć wpisy w pamięci podręcznej powiązane, gdy aplikacja tworzy aktualizacje, lub usuwa dane. Jeśli tak jest czasami pobrać dane, które są nieco nieaktualne aplikacji, może polegać na czas wygaśnięcia można skonfigurować, aby ustawić limit wiek pamięci podręcznej, dane mogą być.

Można skonfigurować wygaśnięcia bezwzględne (ilość czasu od czasu utworzenia elementu pamięci podręcznej) lub przedłużanie ważności (ilość czasu od czasu ostatniego dostępu do niego element pamięci podręcznej). Bezwzględna wygaśnięcia jest używany, gdy są zależnie od mechanizmu wygaśnięcia pamięci podręcznej, aby zapobiec dane zbyt stare. W aplikacji napraw go firma Microsoft będzie ręcznie Wyklucz elementy starych pamięci podręcznej i użyjemy wygaśniecie do najbardziej aktualne dane są przechowywane w pamięci podręcznej. Niezależnie od zasady wygasania, możesz wybrać pamięci podręcznej zostanie automatycznie Wyklucz najstarsze elementy (najmniej ostatnio używane lub LRU) po osiągnięciu limitu pamięci podręcznej.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Przykładowy kod Zarezerwuj pamięć podręczna poprawka aplikacji

W poniższym przykładowym kodzie sprawdzamy pamięci podręcznej najpierw podczas pobierania zadania napraw go. Jeśli zadanie zostanie znaleziony w pamięci podręcznej, zostanie zwrócona Jeśli nie zostanie znaleziony, możemy pobrać go z bazy danych i umieść ją w pamięci podręcznej. Czy wprowadzone zmiany do dodania, buforowanie, aby `FindTaskByIdAsync` wyróżniono metody.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Podczas aktualizacji lub usunięcia zadania napraw go, masz unieważnić zadań (Usuń) zapisane w pamięci podręcznej. W przeciwnym razie przyszłości próbuje odczytać się, że to zadanie będzie nadal mieć stare dane z pamięci podręcznej.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Oto przykłady ilustrujący prostego kodu buforowania; buforowanie nie została zaimplementowana do pobrania poprawka projektu.

## <a name="azure-caching-services"></a>Buforowanie usług Azure

Platforma Azure oferuje następujące usługi buforowania: [pamięć podręczna Redis Azure](https://msdn.microsoft.com/library/dn690523.aspx) i [Azure zarządzanych w pamięci podręcznej](https://msdn.microsoft.com/library/dn386094.aspx). Pamięć podręczna Redis Azure jest oparta na popularnych [Otwórz źródło pamięci podręcznej Redis](http://redis.io/) i jest pierwszy wybór dla większości scenariuszy buforowania.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Stan sesji ASP.NET przy użyciu dostawcy pamięci podręcznej

Jak wspomniano w [sieci web development najlepszych rozwiązań rozdział](web-development-best-practices.md), najlepszym rozwiązaniem jest unikać stanu sesji. Jeśli aplikacja wymaga stanu sesji, dalej najlepszym rozwiązaniem jest unikać domyślnego dostawcy w pamięci, ponieważ nie umożliwiających skalowania w poziomie (wielu wystąpień serwera sieci web). Dostawca stanu sesji ASP.NET SQL Server umożliwia lokacji, która działa na wielu serwerach sieci web do korzystania ze stanu sesji, ale wiąże się z kosztami duże opóźnienie w porównaniu do dostawcy w pamięci. Najlepszym rozwiązaniem, jeśli zajdzie potrzeba korzystania ze stanu sesji jest użycie dostawcy pamięci podręcznej, takich jak [dostawcę stanu sesji dla usługi pamięć podręczna Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Podsumowanie

Przedstawiono sposób aplikacji Usuń można zaimplementować buforowanie, aby zwiększyć czas reakcji i skalowalność i aby umożliwić aplikacji nadal jest elastyczny dla operacji odczytu, gdy baza danych jest niedostępny. W [następnego rozdziału](queue-centric-work-pattern.md) zostanie omówiony sposób dodatkowo poprawić skalowalność i aplikacji w dalszym ciągu odpowiadać dla operacji zapisu.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji na temat buforowania zobacz następujące zasoby.

Dokumentacja

- [Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx). Oficjalnej dokumentacji MSDN na buforowanie na platformie Azure.
- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki buforowanie i wzorzec Zarezerwuj pamięci podręcznej.
- [Przed uszkodzeniami: Wskazówki dotyczące architektury chmury odporność](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument Mercuri wytłoków, Ulrich Homann i Andrew Townhill. Zobacz sekcję dotyczącą buforowanie.
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę na usług w chmurze Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Oficjalny dokument przez moduły SIMM znaku i Michael Thomassy. Zobacz sekcję dotyczącą systemy buforowania rozproszonego.
- [Rozproszone buforowanie na ścieżkę do skalowalność](https://msdn.microsoft.com/magazine/dd942840.aspx). Starsze artykuł MSDN Magazine (2009), ale wyraźnie napisane wprowadzenie do buforowania rozproszonego ogólnie; przechodzi w stan głębokość więcej niż sekcji buforowania oficjalne dokumenty przed uszkodzeniami i najlepsze rozwiązania.

Wideo

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia widok poziomu 400 sposobu projektowania aplikacji w chmurze. Ta seria skupia się na teorii i powodów, dla których Dlaczego; szczegółowe instrukcje Zobacz budynku Big serii przez moduły SIMM znaku. Zawiera omówienie buforowania w epizodu 3, zaczynając od 1:24:14.
- [Big budynku: Wnioski uzyskane od klientów platformy Azure — część I](https://channel9.msdn.com/Events/Build/2012/3-029). Davies Simona omówiono rozproszonej pamięci podręcznej zaczynając od 46:00. Podobnie jak seria przed uszkodzeniami, ale przechodzi w szczegółowe instrukcje. Prezentacji podano 31 października 2012 r., więc nie obejmują usługi buforowania aplikacji sieci Web w usłudze Azure App Service, który został wprowadzony w 2013.

Przykładowy kod

- [Podstawy usługi na platformie Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowej aplikacji, która implementuje systemy buforowania rozproszonego. Zobacz wpis w blogu towarzyszący [podstawy usługi w chmurze — podstawy buforowanie](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Poprzednie](transient-fault-handling.md)
> [dalej](queue-centric-work-pattern.md)
