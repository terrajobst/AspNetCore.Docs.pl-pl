---
title: ASP.NET Core najlepsze rozwiązania w zakresie wydajności
author: mjrousos
description: Porady dotyczące zwiększania wydajności aplikacji platformy ASP.NET Core i unikanie typowych problemów z wydajnością.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 05/10/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 7651dff18f98c60057660c8946c3daa66d272f6a
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536082"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core najlepsze rozwiązania w zakresie wydajności

Przez [Mike Rousos](https://github.com/mjrousos)

Ten temat zawiera wytyczne dotyczące wydajności, najlepsze rozwiązania z platformą ASP.NET Core.

<a name="hot"></a>

W tym dokumencie *ścieżka kodu gorąca* jest zdefiniowany jako ścieżka kodu, który jest często nazywany i gdzie większość czasu wykonywania występuje. Ścieżki kodu gorąca zwykle ograniczają skalowanie aplikacji oraz wydajności.

## <a name="cache-aggressively"></a>Agresywne pamięci podręcznej

Pamięć podręczna została omówiona w różnych części tego dokumentu. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.

## <a name="avoid-blocking-calls"></a>Unikanie zablokowania wywołania

Aplikacje platformy ASP.NET Core, powinny zostać tak zaprojektowane, aby przetwarzać wiele żądań jednocześnie. Asynchroniczne interfejsy API umożliwiają małych puli wątków, aby obsłużyć tysiące jednoczesnych żądań przez bez oczekiwania na blokuje wywołania. Bez czekania na długotrwałych synchroniczne zakończenie zadania, wątek może działać względem innego żądania.

Typowe problem z wydajnością w aplikacji platformy ASP.NET Core nie blokuje wywołania, które może być asynchroniczne. Wiele synchroniczne wywołania blokowania prowadzić do [wyczerpania puli wątków](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) i pogorszenie czasu odpowiedzi.

**Nie**:

* Blokuj wykonywanie asynchronicznych, wywołując [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) lub [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Uzyskania blokady w typowych ścieżek kodu. Aplikacje platformy ASP.NET Core to większość wydajne, gdy zaprojektowana do równoległego uruchamiania kodu.

**Do**:

* Wprowadź [hot ścieżek kodu](#hot) asynchronicznego.
* Asynchroniczne wywoływanie dostępu do danych i długotrwałych operacji interfejsów API.
* Należy kontrolera/Razor strony akcje asynchroniczne. Cały stos wywołań jest asynchroniczne, aby można było korzystać z [async/await](/dotnet/csharp/programming-guide/concepts/async/) wzorców.

A profiler takich jak [narzędzia PerfView](https://github.com/Microsoft/perfview), może służyć do wyszukiwania wątków, które często są dodawane do [puli wątków](/windows/desktop/procthread/thread-pools). `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` Zdarzeń wskazuje wątek, który został dodany do puli wątków. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Zminimalizować, alokacje dużego obiektu

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core -->
[Modułu zbierającego elementy bezużyteczne .NET Core](/dotnet/standard/garbage-collection/) zarządza alokacją i zwalnianiem pamięci automatycznie w aplikacji platformy ASP.NET Core. Automatyczne wyrzucanie elementów bezużytecznych zazwyczaj oznacza, że programistów nie potrzebuje już martwić się o sposób lub czas pamięć jest zwalniana. Jednak czyszczenie nieużywanych obiektów ma czas procesora CPU, dzięki czemu deweloperzy należy zminimalizować alokowania obiektów w [hot ścieżek kodu](#hot). Wyrzucanie elementów bezużytecznych jest szczególnie kosztowne w dużych obiektów (w bajtach > 85 K). Duże obiekty są przechowywane na [stertę dużego obiektu](/dotnet/standard/garbage-collection/large-object-heap) i wymagają pełnego (generacji 2) wyrzucanie elementów bezużytecznych, aby wyczyścić. W przeciwieństwie do generacji 0 i kolekcji generacji 1 kolekcji generacji 2 wymaga tymczasowego zawieszenie wykonywania aplikacji. Częste alokacji i dezalokacji dużych obiektów może spowodować niespójne wydajności.

Zalecenia:

* **Czy** należy wziąć pod uwagę buforowanie dużych obiektów, które są często używane. Buforowanie dużych obiektów zapobiega alokacje kosztowne.
* **Czy** puli buforów przy użyciu [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) do przechowywania dużych tablic.
* **Nie** przydzielić wiele, krótkotrwałych dużych obiektów na [hot ścieżek kodu](#hot).

Problemy z pamięcią, takich jak poprzedni, można zdiagnozować, przeglądając wyrzucania elementów bezużytecznych (GC) statystyki w [narzędzia PerfView](https://github.com/Microsoft/perfview) i badanie:

* Czas wstrzymania kolekcji wyrzucania elementów.
* Jaki procent czasu procesora poświęconego na w wyrzucania elementów bezużytecznych.
* Ile wyrzucania elementów bezużytecznych są generacji 0, 1 i 2.

Aby uzyskać więcej informacji, zobacz [wyrzucania elementów bezużytecznych i wydajność](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Optymalizowanie dostępu do danych

Interakcje z magazynem danych i innymi usługami zdalnego są często najwolniejsze części aplikacji ASP.NET Core. Odczytywanie i zapisywanie danych efektywnie jest krytyczny dla dobrą wydajność.

Zalecenia:

* **Czy** asynchroniczne wywoływanie dostępu do wszystkich danych interfejsów API.
* **Nie** pobieranie większej ilości danych niż jest to konieczne. Zapisywanie zapytań, aby zwrócić tylko dane, które są niezbędne dla bieżącego żądania HTTP.
* **Czy** należy wziąć pod uwagę buforowanie często uzyskać dostępu do danych pobranych z bazy danych lub usługi zdalnej, jeśli nieco nieaktualnych danych. W zależności od scenariusza, należy użyć [MemoryCache](xref:performance/caching/memory) lub [DistributedCache](xref:performance/caching/distributed). Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.
* **Czy** zminimalizować liczbę rund sieciowych. Celem jest zapewnienie pobrania niezbędnych danych w jednym wywołaniu, a nie kilka wywołań.
* **Czy** użyj [nie śledzenia zapytań](/ef/core/querying/tracking#no-tracking-queries) w Entity Framework Core przy uzyskiwaniu dostępu do danych dla celów tylko do odczytu. EF Core może zwrócić wyniki zapytania śledzenia nie bardziej efektywnie.
* **Czy** filtr i agregacji zapytań LINQ (przy użyciu `.Where`, `.Select`, lub `.Sum` instrukcji, na przykład), aby filtrowanie odbywa się przez bazę danych.
* **Czy** należy wziąć pod uwagę, że programu EF Core jest rozpoznawana jako niektórych operatorów zapytań, na komputerze klienckim, co może prowadzić do wykonywania zapytań nieefektywne. Aby uzyskać więcej informacji, zobacz [problemy z wydajnością oceny klienta](/ef/core/querying/client-eval#client-evaluation-performance-issues).
* **Nie** użyj kwerend projekcji w kolekcjach, które mogą skutkować wykonywania "N + 1" zapytania SQL. Aby uzyskać więcej informacji, zobacz [optymalizacji skorelowany podzapytań](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Zobacz [EF o wysokiej wydajności](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) w przypadku metod, które może poprawić wydajność w aplikacjach o dużej skali:

* [Buforowanie typu DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Zapytania skompilowane jawnie](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Firma Microsoft zaleca, mierzenie wpływu poprzednich metod o wysokiej wydajności przed zatwierdzeniem bazy kodu. Dodatkowe złożoność zapytania skompilowane nie może spowodować zwiększenie wydajności.

Zapytanie, może zostać wykryte problemy, przeglądając czas poświęcony na uzyskiwanie dostępu do danych za pomocą [usługi Application Insights](/azure/application-insights/app-insights-overview) lub za pomocą narzędzi profilowania. Większość baz danych także udostępnić dane statystyczne dotyczące często wykonywanych zapytań.

## <a name="pool-http-connections-with-httpclientfactory"></a>Połączenia HTTP puli HttpClientFactory

Mimo że [HttpClient](/dotnet/api/system.net.http.httpclient) implementuje `IDisposable` interfejsu, jest przeznaczona do ponownego wykorzystania. Zamknięte `HttpClient` wystąpień zamykaj gniazda w `TIME_WAIT` stanie krótkim czasie. Jeśli ścieżka kodu, które tworzy i usuwa `HttpClient` obiektów jest często używany, aplikacja może wyczerpać dostępne gniazda. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) wprowadzono w programie ASP.NET Core 2.1 jako rozwiązanie tego problemu. Obsługuje ona korzystanie z puli połączeń HTTP w celu zoptymalizowania wydajności i niezawodności.

Zalecenia:

* **Nie** tworzenie i usuwanie `HttpClient` wystąpień bezpośrednio.
* **Czy** użyj [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) można pobrać `HttpClient` wystąpień. Aby uzyskać więcej informacji, zobacz [HttpClientFactory Użyj, aby zaimplementować odporne na błędy żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Zachowaj typowe szybkie ścieżek kodu

Chcesz, że wszystkie swój kod, aby szybko, często volat jen ścieżek kodu są najbardziej krytycznych w celu zoptymalizowania:

* Składniki oprogramowania pośredniczącego w potoku przetwarzania żądań aplikacji, szczególnie oprogramowanie pośredniczące, uruchom na wczesnym etapie potoku. Te składniki mieć duży wpływ na wydajność.
* Kod, który jest wykonywany dla każdego żądania lub wiele razy na żądanie. Na przykład rejestrowanie niestandardowe, obsługi autoryzacji lub inicjowania usługi przejściowy.

Zalecenia:

* **Nie** składników oprogramowania pośredniczącego niestandardowych za pomocą długotrwałych zadań.
* **Czy** profilowania narzędzi, takich jak wydajność [Visual Studio, narzędzia diagnostyczne](/visualstudio/profiling/profiling-feature-tour) lub [narzędzia PerfView](https://github.com/Microsoft/perfview)), aby zidentyfikować [hot ścieżek kodu](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Wykonaj długotrwałych zadań poza żądania HTTP

Większość żądań do aplikacji ASP.NET Core może być obsługiwany przez kontroler lub modelu strony wywoływania niezbędne usługi i zwracania odpowiedzi HTTP. Dla niektórych żądań, które obejmują długotrwałych zadań lepiej jest zapewnienie asynchronicznej procesu całą odpowiedź na żądanie.

Zalecenia:

* **Nie** poczekaj, aż długotrwałych zadań do wykonania w ramach zwykłych przetwarzania żądania HTTP.
* **Czy** należy wziąć pod uwagę, obsługi długotrwałych żądań z [usługi w tle](xref:fundamentals/host/hosted-services) lub poza procesem, za pomocą [funkcji platformy Azure](/azure/azure-functions/). Kończenie pracy poza procesem jest szczególnie korzystne w przypadku zadań intensywnie wykorzystujące procesor CPU.
* **Czy** użycie opcji komunikacji w czasie rzeczywistym, takich jak [SignalR](xref:signalr/introduction)w celu komunikowania się z klientami asynchronicznie.

## <a name="minify-client-assets"></a>Zmniejszenie zasobów klienta

Aplikacje platformy ASP.NET Core za pomocą złożonych połączeń frontonach często służą wielu JavaScript, CSS lub pliki obrazów. Można zwiększyć wydajność żądań ładowania początkowego za:

* Tworzenie pakietów, które łączy wiele plików w jedno.
* Minifikacja, co zmniejsza rozmiar plików, usuwając białe znaki i komentarze.

Zalecenia:

* **Czy** korzystanie z platformy ASP.NET Core [wbudowaną obsługę](xref:client-side/bundling-and-minification) tworzenie pakietów i minifikacja zasobów klienta.
* **Czy** należy wziąć pod uwagę innych narzędzi innych firm, takich jak [Webpack](https://webpack.js.org/), dla klienta złożonego zarządzania zasobami.

## <a name="compress-responses"></a>Kompresji odpowiedzi

 Zazwyczaj zmniejszenie rozmiaru odpowiedzi często znacznie zwiększa szybkość reakcji aplikacji. Jednym ze sposobów, aby zmniejszyć rozmiar ładunku jest kompresji odpowiedzi aplikacji. Aby uzyskać więcej informacji, zobacz [kompresji odpowiedzi](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>Użyj najnowszej wersji platformy ASP.NET Core

Każdą nową wersją programu ASP.NET Core zawiera ulepszenia wydajności. Optymalizacje w .NET Core i ASP.NET Core oznacza, że nowszych wersji, której ogólnie starszych wersji. Na przykład, .NET Core 2.1 dodano obsługę dla skompilowanych wyrażeń regularnych i skorzystały z [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). Platforma ASP.NET Core 2.2 dodano obsługę protokołu HTTP/2. Jeśli wydajność ma najwyższy priorytet, należy rozważyć uaktualnienie do wersji bieżącej platformy ASP.NET Core.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Minimalizuj wyjątków

Wyjątki powinny być rzadko. Zgłaszania i przechwytywania wyjątków jest powolne względem innych wzorców przepływu kodu. W związku z tym wyjątki nie powinny służyć do programu Normalny przepływ sterowania.

Zalecenia:

* **Nie** Użyj zgłaszania i przechwytywania wyjątków jako środek przepływu normalnego programu, szczególnie w przypadku [hot ścieżek kodu](#hot).
* **Czy** obejmują logikę w aplikacji, aby wykrywać i obsługiwać sytuacje, które mogłyby spowodować wyjątek.
* **Czy** zgłosić lub przechwycić wyjątki dotyczące niezwykłych lub nieoczekiwanymi warunkami.

Narzędzia diagnostyczne aplikacji, takich jak usługi Application Insights może pomóc ustalić występujących wyjątków w aplikacji, która może mieć wpływ na wydajność.
