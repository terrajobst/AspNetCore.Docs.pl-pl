---
title: ASP.NET Core najlepsze rozwiązania w zakresie wydajności
author: mjrousos
description: Porady dotyczące zwiększania wydajności aplikacji platformy ASP.NET Core i unikanie typowych problemów z wydajnością.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 11/29/2018
uid: performance/performance-best-practices
ms.openlocfilehash: ced86dbc2d6f40b503493eda122d8977d6df7035
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52453028"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core najlepsze rozwiązania w zakresie wydajności

Przez [Mike Rousos](https://github.com/mjrousos)

Ten temat zawiera wytyczne dotyczące wydajności, najlepsze rozwiązania z platformą ASP.NET Core.

<a name="hot"></a>
<!-- TODO review hot code paths is jargon that won't MT (machine translate) and is not well defined for native speakers. --> W tym dokumencie ścieżki gorąca kod jest zdefiniowany jako ścieżka kodu, który jest często nazywany i gdzie większość czasu wykonywania występuje. Ścieżki kodu gorąca zwykle ograniczają skalowanie aplikacji oraz wydajności.

## <a name="cache-aggressively"></a>Agresywne pamięci podręcznej

Pamięć podręczna została omówiona w różnych części tego dokumentu. Aby uzyskać więcej informacji, zobacz [buforują odpowiedzi w programie ASP.NET Core](xref:performance/caching/index).

## <a name="avoid-blocking-calls"></a>Unikanie zablokowania wywołania

Aplikacje platformy ASP.NET Core, powinny zostać tak zaprojektowane, aby przetwarzać wiele żądań jednocześnie. Asynchroniczne interfejsy API umożliwiają małych puli wątków, aby obsłużyć tysiące jednoczesnych żądań przez bez oczekiwania na blokuje wywołania. Bez czekania na długotrwałych synchroniczne zakończenie zadania, wątek może działać względem innego żądania.

Typowe problem z wydajnością w aplikacji platformy ASP.NET Core nie blokuje wywołania, które może być asynchroniczne. Wiele synchroniczne wywołania blokowania prowadzi do [wyczerpania puli wątków](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) i zmniejszenie czasu odpowiedzi.

**Nie**:

* Blokuj wykonywanie asynchronicznych, wywołując [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) lub [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Uzyskania blokady w typowych ścieżek kodu. Aplikacje platformy ASP.NET Core to większość wydajne, gdy zaprojektowana do równoległego uruchamiania kodu.

**Czy**:

* Wprowadź [hot ścieżek kodu](#hot) asynchronicznego.
* Asynchroniczne wywoływanie dostępu do danych i długotrwałych operacji interfejsów API.
* Należy kontrolera/Razor strony akcje asynchroniczne. Cały stos wywołań, musi być asynchroniczne w celu skorzystania z [async/await](/dotnet/csharp/programming-guide/concepts/async/) wzorców.

Program profilujący, takich jak [narzędzia PerfView](https://github.com/Microsoft/perfview) może służyć do wyszukiwania dla wątków, często dodawane do [puli wątków](/windows/desktop/procthread/thread-pool). `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` Zdarzeń wskazuje wątek, dodawane do puli wątków. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Zminimalizować, alokacje dużego obiektu

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core --> [Modułu zbierającego elementy bezużyteczne .NET Core](/dotnet/standard/garbage-collection/) zarządza alokacją i zwalnianiem pamięci automatycznie w aplikacji platformy ASP.NET Core. Automatyczne wyrzucanie elementów bezużytecznych zazwyczaj oznacza, że programistów nie potrzebuje już martwić się o sposób lub czas pamięć jest zwalniana. Jednak czyszczenie nieużywanych obiektów ma czas procesora CPU, dzięki czemu deweloperzy należy zminimalizować alokowania obiektów w [hot ścieżek kodu](#hot). Wyrzucanie elementów bezużytecznych jest szczególnie kosztowne w dużych obiektów (w bajtach > 85 K). Duże obiekty są przechowywane na [stertę dużego obiektu](/dotnet/standard/garbage-collection/large-object-heap) i wymagają pełnego (generacji 2) wyrzucanie elementów bezużytecznych, aby wyczyścić. W przeciwieństwie do generacji 0 i kolekcji generacji 1 kolekcji generacji 2 wymaga wykonywanie aplikacji, aby tymczasowo zawieszone. Częste alokacji i dezalokacji dużych obiektów może spowodować niespójne wydajności.

Zalecenia:

* **Czy** należy wziąć pod uwagę buforowanie dużych obiektów, które są często używane. Buforowanie dużych obiektów zapobiega alokacje kosztowne.
* **Czy** puli buforów przy użyciu [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) do przechowywania dużych tablic.
* **Nie** przydzielić wiele, krótkotrwałych dużych obiektów na [hot ścieżek kodu](#hot).

Problemy z pamięcią, takich jak poprzedni można zdiagnozować, przeglądając wyrzucania elementów bezużytecznych (GC) statystyki w [narzędzia PerfView](https://github.com/Microsoft/perfview) i badanie:

* Czas wstrzymania kolekcji wyrzucania elementów.
* Jaki procent czasu procesora poświęconego na w wyrzucania elementów bezużytecznych.
* Ile wyrzucania elementów bezużytecznych są generacji 0, 1 i 2.

Aby uzyskać więcej informacji, zobacz [wyrzucania elementów bezużytecznych i wydajność](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Optymalizowanie dostępu do danych

Interakcje z magazynem danych lub inne usługi zdalnej są często najwolniejsze część aplikacji ASP.NET Core. Odczytywanie i zapisywanie danych efektywnie jest krytyczny dla dobrą wydajność.

Zalecenia:

* **Czy** asynchroniczne wywoływanie dostępu do wszystkich danych interfejsów API.
* **Nie** pobieranie większej ilości danych niż jest to konieczne. Zapisywanie zapytań, aby zwrócić tylko dane, które są niezbędne dla bieżącego żądania HTTP.
* **Czy** należy wziąć pod uwagę buforowanie często uzyskać dostępu do danych pobranych z bazy danych lub usługi zdalnej, jeśli jest dopuszczalne dane, które mogą być nieco nieaktualne. W zależności od scenariusza, prawdopodobnie używasz [MemoryCache](xref:performance/caching/memory) lub [DistributedCache](xref:performance/caching/distributed). Aby uzyskać więcej informacji, zobacz [buforują odpowiedzi w programie ASP.NET Core](xref:performance/caching/index).
* Zminimalizuj liczbę rund sieciowych. Celem jest, aby pobrać wszystkie dane, które będą potrzebne w pojedynczym wywołaniu, a nie kilka wywołań.
* **Czy** użyj [nie śledzenia zapytań](/ef/core/querying/tracking#no-tracking-queries) w Entity Framework Core przy uzyskiwaniu dostępu do danych dla celów tylko do odczytu. EF Core może zwrócić wyniki zapytania śledzenia nie bardziej efektywnie.
* **Czy** filtr i agregacji zapytań LINQ (przy użyciu `.Where`, `.Select`, lub `.Sum` instrukcji, na przykład), aby filtrowanie odbywa się przez bazę danych.
* **Czy** należy wziąć pod uwagę, że programu EF Core jest rozpoznawana jako niektórych operatorów zapytań, na komputerze klienckim, co może prowadzić do wykonywania zapytań nieefektywne. Aby uzyskać więcej informacji, zobacz [problemy z wydajnością oceny klienta](/ef/core/querying/client-eval#client-evaluation-performance-issues)
* **Nie** użyj kwerend projekcji w kolekcjach, które mogą skutkować wykonywania "N + 1" zapytania SQL. Aby uzyskać więcej informacji, zobacz [optymalizacji skorelowany podzapytań](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Zobacz [EF o wysokiej wydajności](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) w przypadku metod, które może poprawić wydajność w aplikacjach o dużej skali:

* [Buforowanie typu DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Zapytania skompilowane jawnie](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Zalecane jest mierzenie wpływu poprzednich metod o wysokiej wydajności, przed podjęciem bazy kodu. Dodatkowe złożoność zapytania skompilowane nie może spowodować zwiększenie wydajności.

Zapytanie, może zostać wykryte problemy, przeglądając czas poświęcony na uzyskiwanie dostępu do danych za pomocą [usługi Application Insights](/azure/application-insights/app-insights-overview) lub za pomocą narzędzi profilowania. Większość baz danych także udostępnić dane statystyczne dotyczące często wykonywanych zapytań.

## <a name="pool-http-connections-with-httpclientfactory"></a>Połączenia HTTP puli HttpClientFactory

Mimo że [HttpClient](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) implementuje `IDisposable` interfejsu, ma oznaczało to być ponownie używane. Zamknięte `HttpClient` wystąpień zamykaj gniazda w `TIME_WAIT` stanie krótkim czasie. W związku z tym jeśli ścieżka kodu, które tworzy i usuwa `HttpClient` obiektów jest często używany, aplikacja może wyczerpać dostępne gniazda. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) wprowadzono w programie ASP.NET Core 2.1 jako rozwiązanie tego problemu. Obsługuje ona korzystanie z puli połączeń HTTP w celu zoptymalizowania wydajności i niezawodności.

Zalecenia:

* **Nie** tworzenie i usuwanie `HttpClient` wystąpień bezpośrednio.
* **Czy** użyj [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) można pobrać `HttpClient` wystąpień. Aby uzyskać więcej informacji, zobacz [HttpClientFactory Użyj, aby zaimplementować odporne na błędy żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Zachowaj typowe szybkie ścieżek kodu

Chcesz, aby cały kod szybkiego działania, ale ścieżki często dzwonić kodu są najbardziej krytycznych w celu optymalizacji:

* Składniki oprogramowania pośredniczącego w potoku przetwarzania żądań aplikacji, szczególnie oprogramowanie pośredniczące, uruchom na wczesnym etapie potoku. Te składniki mieć duży wpływ na wydajność.
* Kod, który jest wykonywany dla każdego żądania lub wiele razy na żądanie. Na przykład rejestrowanie niestandardowe, obsługi autoryzacji lub inicjowania usługi przejściowy.

Zalecenia:

* **Nie** składników oprogramowania pośredniczącego niestandardowych za pomocą długotrwałych zadań.
* **Czy** korzystać z wydajności narzędzi profilowania (takich jak [Visual Studio, narzędzia diagnostyczne](/visualstudio/profiling/profiling-feature-tour) lub [narzędzia PerfView](https://github.com/Microsoft/perfview)) do identyfikowania [hot ścieżek kodu](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Wykonaj długotrwałych zadań poza żądania HTTP

Większość żądań do aplikacji ASP.NET Core może być obsługiwany przez kontroler lub modelu strony wywoływania niezbędne usługi i zwracania odpowiedzi HTTP. Dla niektórych żądań, które obejmują długotrwałych zadań lepiej jest zapewnienie asynchronicznej procesu całą odpowiedź na żądanie.

Zalecenia:

* **Nie** poczekaj, aż długotrwałych zadań do wykonania w ramach zwykłych przetwarzania żądania HTTP.
* **Czy** należy wziąć pod uwagę, obsługi długotrwałych żądań z [usługi w tle](/aspnet/core/fundamentals/host/hosted-services) lub poza procesem, za pomocą [funkcji platformy Azure](/azure/azure-functions/). Kończenie pracy poza procesem jest szczególnie korzystne w przypadku zadań intensywnie wykorzystujące procesor CPU.
* **Czy** użyj opcji komunikacji w czasie rzeczywistym, takich jak [SignalR](xref:signalr/introduction) do komunikacji z klientami asynchronicznie.

## <a name="minify-client-assets"></a>Zmniejszenie zasobów klienta

Aplikacje platformy ASP.NET Core za pomocą złożonych połączeń frontonach często służą wielu JavaScript, CSS lub pliki obrazów. Można zwiększyć wydajność żądań ładowania początkowego za:

* Tworzenie pakietów, które łączy wiele plików w jedno.
* Minifikacja, co zmniejsza rozmiar plików przez.

Zalecenia:

* **Czy** korzystanie z platformy ASP.NET Core [wbudowaną obsługę](xref:client-side/bundling-and-minification) tworzenie pakietów i minifikacja zasobów klienta.
* **Czy** należy wziąć pod uwagę innych narzędzi innych firm, takich jak [Gulp](uid:client-side/bundling-and-minification#consume-bundleconfigjson-from-gulp) lub [Webpack](https://webpack.js.org/) dla bardziej złożonego zarządzania zasobami klienta.

## <a name="use-the-latest-aspnet-core-release"></a>Użyj najnowszej wersji platformy ASP.NET Core

Każdą nową wersją programu ASP.NET zawiera ulepszenia wydajności. Optymalizacje w .NET Core i ASP.NET Core oznacza, że nowsze wersje będą przewyższyć starszych wersji. Na przykład, .NET Core 2.1 dodano obsługę dla skompilowanych wyrażeń regularnych i skorzystały z [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). Platforma ASP.NET Core 2.2 dodano obsługę protokołu HTTP/2. Jeśli wydajność ma najwyższy priorytet, należy rozważyć uaktualnienie do najnowszej wersji platformy ASP.NET Core.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Minimalizuj wyjątków

Wyjątki powinny być rzadko. Zgłaszania i przechwytywania wyjątków jest powolne względem innych wzorców przepływu kodu. W związku z tym wyjątki nie używać programu Normalny przepływ sterowania.

Zalecenia:

* **Nie** Użyj zgłaszanie lub przechwytywanie wyjątków jako środek przepływu normalnego programu, szczególnie w przypadku ścieżek kodu dostępu.
* **Czy** obejmują logikę w aplikacji, aby wykrywać i obsługiwać sytuacje, które mogłyby spowodować wyjątek.
* **Czy** zgłosić lub przechwycić wyjątki dotyczące niezwykłych lub nieoczekiwanymi warunkami.

Narzędzia do diagnostyki aplikacji (np. usługi Application Insights) może pomóc ustalić występujących wyjątków w aplikacji, która może mieć wpływ na wydajność.