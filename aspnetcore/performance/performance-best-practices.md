---
title: ASP.NET Core najlepszych rozwiązań dotyczących wydajności
author: mjrousos
description: Wskazówki dotyczące zwiększania wydajności aplikacji ASP.NET Core i unikania typowych problemów z wydajnością.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/12/2019
no-loc:
- SignalR
uid: performance/performance-best-practices
ms.openlocfilehash: 279bf352580e5e45fc005e800ee536871210409b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963244"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core najlepszych rozwiązań dotyczących wydajności

Według [Jan Rousos](https://github.com/mjrousos)

Ten artykuł zawiera wskazówki dotyczące najlepszych rozwiązań dotyczących wydajności w ASP.NET Core.

## <a name="cache-aggressively"></a>Agresywne buforowanie

Buforowanie jest omówione w kilku częściach tego dokumentu. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.

## <a name="understand-hot-code-paths"></a>Informacje na temat ścieżek kodu gorącego

W tym dokumencie ścieżka do *kodu gorącego* jest definiowana jako ścieżka kodu, która jest często wywoływana i gdzie występuje większość czasu wykonywania. Ścieżki kodu gorącą zwykle ograniczają skalowalność i wydajność aplikacji oraz są omawiane w kilku częściach tego dokumentu.

## <a name="avoid-blocking-calls"></a>Unikaj blokowania wywołań

Aplikacje ASP.NET Core powinny być przeznaczone do przetwarzania wielu żądań jednocześnie. Asynchroniczne interfejsy API umożliwiają obsługę tysięcy współbieżnych żądań przez niewielką pulę wątków. Zamiast czekać na ukończenie długotrwałego zadania synchronicznego, wątek może działać na innym żądaniu.

Typowy problem z wydajnością w aplikacjach ASP.NET Core blokuje wywołania, które mogą być asynchroniczne. Wiele wywołań blokowania synchronicznego prowadzi do [przetrzymania puli wątków](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) i nieprawidłowej reakcji.

**Nie należy**:

* Zablokuj wykonywanie asynchroniczne przez wywołanie metody [Task. wait](/dotnet/api/system.threading.tasks.task.wait) lub [Task. Result](/dotnet/api/system.threading.tasks.task-1.result).
* Uzyskaj blokady w wspólnych ścieżkach kodu. Aplikacje ASP.NET Core są najbardziej wydajne, gdy jest to zaprojektowane w celu równoległego uruchamiania kodu.

**Do**:

* Utwórz asynchroniczne [ścieżki kodu na gorąco](#understand-hot-code-paths) .
* Asynchronicznie Wywołaj dostęp do danych i długotrwałe interfejsy API operacji.
* Akcja uczyń kontroler/stronę Razor asynchronicznie. Cały stos wywołań jest asynchroniczny, aby można było korzystać z wzorców [asynchronicznych/await](/dotnet/csharp/programming-guide/concepts/async/) .

Profiler, taki jak [Narzędzia PerfView](https://github.com/Microsoft/perfview), może służyć do znajdowania wątków często dodanych do [puli wątków](/windows/desktop/procthread/thread-pools). Zdarzenie `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` wskazuje wątek dodany do puli wątków. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a>Minimalizuj alokacje dużych obiektów

[Moduł wyrzucania elementów bezużytecznych programu .NET Core](/dotnet/standard/garbage-collection/) zarządza alokacją i zwalnianiem pamięci automatycznie w aplikacjach ASP.NET Core. Automatyczne wyrzucanie elementów bezużytecznych zazwyczaj oznacza, że deweloperzy nie muszą martwić się o to, jak i kiedy pamięć jest zwalniana. Jednak czyszczenie nieużywanych obiektów ma czas procesora, dlatego deweloperzy powinny zminimalizować przydzielanie obiektów w [ścieżkach kodu](#understand-hot-code-paths). Wyrzucanie elementów bezużytecznych jest szczególnie kosztowne w przypadku dużych obiektów (> 85 K b). Duże obiekty są przechowywane na [stercie dużego obiektu](/dotnet/standard/garbage-collection/large-object-heap) i wymagają oczyszczenia pełnej (generacji 2) wyrzucania elementów bezużytecznych. W przeciwieństwie do generacji 0 i generacji 1 kolekcja generacji 2 wymaga tymczasowego zawieszenia wykonywania aplikacji. Częste przydzielanie i cofanie alokacji dużych obiektów może spowodować niespójność wydajności.

Mając

* **Należy rozważyć buforowanie** dużych obiektów, które są często używane. Buforowanie dużych obiektów zapobiega kosztownym alokacjom.
* **Buforuj** bufory przy użyciu [`ArrayPool<T>`](/dotnet/api/system.buffers.arraypool-1) do przechowywania dużych tablic.
* **Nie** przydzielaj wielu, krótkoterminowych dużych obiektów w [ścieżkach kodu gorąca](#understand-hot-code-paths).

Problemy z pamięcią, takie jak poprzednia, można zdiagnozować, przeglądając statystyki dotyczące wyrzucania elementów bezużytecznych (GC) w [Narzędzia PerfView](https://github.com/Microsoft/perfview) i badając:

* Czas wstrzymania odzyskiwania pamięci.
* Jaki procent czasu procesora jest poświęcany na wyrzucanie elementów bezużytecznych.
* Ile kolekcji elementów bezużytecznych jest generacji 0, 1 i 2.

Aby uzyskać więcej informacji, zobacz [odzyskiwanie pamięci i wydajność](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Optymalizowanie dostępu do danych

Interakcje z magazynem danych i innymi usługami zdalnymi są często najwolniej częścią aplikacji ASP.NET Core. Wydajne odczytywanie i zapisywanie danych ma kluczowe znaczenie dla dobrej wydajności.

Mając

* Wywołuj asynchronicznie wszystkie interfejsy API **dostępu do danych** .
* **Nie** Pobieraj większej ilości danych niż jest to konieczne. Zapisz zapytania, aby zwrócić tylko dane niezbędne dla bieżącego żądania HTTP.
* **Należy rozważyć buforowanie** często używanych danych pobieranych z bazy danych lub usługi zdalnej w przypadku niewielkich nieaktualnych danych. W zależności od scenariusza użyj elementu [elemencie MemoryCache](xref:performance/caching/memory) lub [DistributedCache](xref:performance/caching/distributed). Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.
* **Minimalizacja** podróży sieci. Celem jest pobranie wymaganych danych w jednym wywołaniu, a nie w kilku wywołaniach.
* Podczas uzyskiwania dostępu do danych w celach tylko do **odczytu używaj** [zapytań bez śledzenia](/ef/core/querying/tracking#no-tracking-queries) w Entity Framework Core. EF Core może zwrócić wyniki niewydajnego śledzenia zapytań.
* **Wykonaj** filtrowanie i agregowanie zapytań LINQ (na przykład przy użyciu instrukcji `.Where`, `.Select`lub `.Sum`), aby filtrowanie było wykonywane przez bazę danych.
* **Należy wziąć pod** uwagę, że EF Core rozpoznaje niektórych operatorów zapytań na kliencie, co może prowadzić do niewydajnego wykonywania zapytań. Aby uzyskać więcej informacji, zobacz [problemy z wydajnością oceny klienta](/ef/core/querying/client-eval#client-evaluation-performance-issues).
* **Nie** używaj zapytań projekcji w kolekcjach, co może spowodować wykonanie zapytań SQL "N + 1". Aby uzyskać więcej informacji, zobacz [Optymalizacja skorelowanych podzapytań](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Zobacz [Dr wysoka wydajność](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) , aby dowiedzieć się, co może poprawić wydajność w aplikacjach o dużej skali:

* [Buforowanie kontekstu DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Jawne skompilowane zapytania](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Zalecamy zmierzenie wpływu powyższych metod o wysokiej wydajności przed zatwierdzeniem bazy kodu. Dodatkowa złożoność skompilowanych zapytań może nie uzasadniać poprawy wydajności.

Problemy z kwerendami można wykryć, przeglądając czas spędzony na dostępie do danych za pomocą [Application Insights](/azure/application-insights/app-insights-overview) lub narzędzi profilowania. Większość baz danych udostępnia również statystyki dotyczące często wykonywanych zapytań.

## <a name="pool-http-connections-with-httpclientfactory"></a>Połączenia HTTP puli z HttpClientFactory

Chociaż [HttpClient](/dotnet/api/system.net.http.httpclient) implementuje interfejs `IDisposable`, jest przeznaczony do ponownego użycia. Zamknięte wystąpienia `HttpClient` pozostawiają gniazda otwarte w stanie `TIME_WAIT` przez krótki czas. Jeśli ścieżka kodu, która tworzy i usuwa `HttpClient` obiektów jest często używana, aplikacja może wyczerpać dostępne gniazda. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) został wprowadzony w ASP.NET Core 2,1 jako rozwiązanie tego problemu. Obsługuje buforowanie połączeń HTTP w celu zoptymalizowania wydajności i niezawodności.

Mając

* **Nie** Twórz ani nie usuwaj wystąpień `HttpClient` bezpośrednio.
* **Użyj** [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) , aby pobrać wystąpienia `HttpClient`. Aby uzyskać więcej informacji, zobacz [Używanie HttpClientFactory do implementowania odpornych żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Szybkie śledzenie wspólnych ścieżek kodu

Chcesz, aby cały kod był szybki, często nazywane ścieżki kodu są najbardziej krytyczne do zoptymalizowania:

* Składniki pośredniczące w potoku przetwarzania żądań aplikacji, szczególnie oprogramowanie pośredniczące działające wczesnie w potoku. Te składniki mają duży wpływ na wydajność.
* Kod wykonywany dla każdego żądania lub wiele razy na żądanie. Na przykład niestandardowe rejestrowanie, programy obsługi autoryzacji lub inicjowanie usług przejściowych.

Mając

* **Nie** należy używać niestandardowych składników pośredniczących z długotrwałymi zadaniami.
* **Użyj narzędzi** profilowania wydajności, takich jak [Visual Studio narzędzia diagnostyczne](/visualstudio/profiling/profiling-feature-tour) lub [Narzędzia PerfView](https://github.com/Microsoft/perfview)), aby zidentyfikować [ścieżki kodu gorąca](#understand-hot-code-paths).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Wykonywanie długotrwałych zadań poza żądaniami HTTP

Większość żądań do aplikacji ASP.NET Core może być obsługiwany przez kontroler lub model strony wywołujący wymagane usługi i zwracają odpowiedź HTTP. W przypadku niektórych żądań, które obejmują długotrwałe zadania, lepszym rozwiązaniem jest wykonanie całego procesu żądania odpowiedzi.

Mając

* **Nie** czekaj na ukończenie długotrwałych zadań w ramach zwykłego przetwarzania żądań HTTP.
* **Rozważ obsługę** długotrwałych żądań z usługami w [tle](xref:fundamentals/host/hosted-services) lub poza procesem przy użyciu [funkcji platformy Azure](/azure/azure-functions/). Zakończenie pracy poza procesem jest szczególnie przydatne w przypadku zadań intensywnie korzystających z procesora CPU.
* **Użyj opcji** komunikacji w czasie rzeczywistym, takich jak [SignalR](xref:signalr/introduction), aby komunikować się z klientami asynchronicznie.

## <a name="minify-client-assets"></a>Zminifikować zasoby klienta

ASP.NET Core aplikacje z złożonymi frontonami często mają wiele plików JavaScript, CSS lub obrazów. Wydajność początkowych żądań ładowania można ulepszyć, wykonując następujące działania:

* Zgrupowanie, które łączy wiele plików w jeden.
* Minifikacja, co zmniejsza rozmiar plików przez usunięcie odstępów i komentarzy.

Mając

* **Użyj** [wbudowanej obsługi](xref:client-side/bundling-and-minification) ASP.NET Core do tworzenia i minifikacja zasobów klienta.
* Zapoznaj się z innymi narzędziami innych firm, takimi jak [WebPack](https://webpack.js.org/), na potrzeby złożonego zarządzania zasobami klientów.

## <a name="compress-responses"></a>Kompresuj odpowiedzi

 Zmniejszenie rozmiaru odpowiedzi zwykle zwiększa czas odpowiedzi aplikacji, często znacząco. Jednym ze sposobów zmniejszenia rozmiaru ładunku jest kompresowanie odpowiedzi aplikacji. Aby uzyskać więcej informacji, zobacz [kompresja odpowiedzi](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>Użyj najnowszej wersji ASP.NET Core

Każda nowa wersja ASP.NET Core obejmuje ulepszenia wydajności. Optymalizacje w oprogramowaniu .NET Core i ASP.NET Core oznaczają, że nowsze wersje zwykle outperform starsze wersje. Na przykład program .NET Core 2,1 dodaliśmy obsługę skompilowanych wyrażeń regularnych i benefitted z [`Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx). ASP.NET Core 2,2 dodano obsługę protokołu HTTP/2. [ASP.NET Core 3,0 dodaje wiele ulepszeń](xref:aspnetcore-3.0) , które zmniejszają wykorzystanie pamięci i zwiększają przepływność. Jeśli wydajność jest priorytetem, rozważ uaktualnienie do bieżącej wersji ASP.NET Core.

## <a name="minimize-exceptions"></a>Minimalizuj wyjątki

Wyjątki powinny być rzadkie. Generowanie i przechwytywanie wyjątków jest powolne względem innych wzorców przepływu kodu. Z tego powodu wyjątki nie powinny być używane do sterowania normalnym przepływem programu.

Mając

* **Nie** należy używać zgłaszania ani przechwytywania wyjątków jako metody normalnego przepływu programu, szczególnie w przypadku [ścieżek kodu gorąca](#understand-hot-code-paths).
* **Dodaj** logikę w aplikacji, aby wykryć i obsłużyć warunki, które spowodują wystąpienie wyjątku.
* **Należy** zgłosić lub przechwycić wyjątki dla nietypowych lub nieoczekiwanych warunków.

Narzędzia do diagnostyki aplikacji, takie jak Application Insights, mogą pomóc w zidentyfikowaniu typowych wyjątków w aplikacji, która może mieć wpływ na wydajność.

## <a name="performance-and-reliability"></a>Wydajność i niezawodność

Poniższe sekcje zawierają wskazówki dotyczące wydajności oraz znane problemy z niezawodnością i rozwiązania.

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a>Unikaj synchronicznego odczytu lub zapisu w treści HttpRequest/HttpResponse

Wszystkie operacje we/wy w ASP.NET Core są asynchroniczne. Serwery implementują interfejs `Stream`, który ma przeciążenia synchroniczne i asynchroniczne. Asynchronicznie powinny być preferowane, aby uniknąć blokowania wątków puli wątków. Blokowanie wątków może prowadzić do zablokowania puli wątków.

**Nie wykonuj tej czynności:** Poniższy przykład używa <xref:System.IO.StreamReader.ReadToEnd*>. Blokuje bieżący wątek, aby oczekiwać na wynik. Jest to przykład [synchronizacji przez asynchroniczne](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
).

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

W poprzednim kodzie `Get` synchronicznie odczytuje całą treść żądania HTTP do pamięci. Jeśli klient działa powoli, aplikacja wykonuje synchronizację przez asynchroniczny. Aplikacja synchronizuje się za pośrednictwem Async, ponieważ Kestrel **nie obsługuje operacji** odczytu synchronicznego.

**Wykonaj następujące czynności:** Poniższy przykład używa <xref:System.IO.StreamReader.ReadToEndAsync*> i nie blokuje wątku podczas odczytywania.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

Powyższy kod asynchronicznie odczytuje całą treść żądania HTTP do pamięci.

> [!WARNING]
> Jeśli żądanie jest duże, odczytywanie całej treści żądania HTTP do pamięci może prowadzić do niewystarczającej ilości pamięci (OOM). OOM może spowodować odmowę usługi.  Aby uzyskać więcej informacji, zobacz [unikanie odczytywania dużych treści żądań lub treści odpowiedzi do pamięci](#arlb) w tym dokumencie.

**Wykonaj następujące czynności:** Poniższy przykład jest w pełni asynchroniczny przy użyciu niebuforowanej treści żądania:

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

Poprzedni kod asynchronicznie deserializacji treści żądania do C# obiektu.

## <a name="prefer-readformasync-over-requestform"></a>Preferuj ReadFormAsync przez żądanie. formularz

Użyj `HttpContext.Request.ReadFormAsync`, a nie `HttpContext.Request.Form`.
`HttpContext.Request.Form` mogą być bezpiecznie odczytywane tylko z następującymi warunkami:

* Formularz został odczytany przez wywołanie `ReadFormAsync`i
* Odczytywanie wartości formularza w pamięci podręcznej przy użyciu `HttpContext.Request.Form`

**Nie wykonuj tej czynności:** Poniższy przykład używa `HttpContext.Request.Form`.  `HttpContext.Request.Form` używa [synchronizacji przez asynchroniczne](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
) i może prowadzić do zablokowania puli wątków.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

**Wykonaj następujące czynności:** Poniższy przykład używa `HttpContext.Request.ReadFormAsync` do odczytywania treści formularza asynchronicznie.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a>Unikanie odczytywania dużych treści żądań lub treści odpowiedzi do pamięci

W programie .NET każda alokacja obiektu większa niż 85 KB zostaje zakończona w stosie dużych obiektów ([LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)). Duże obiekty są kosztowne na dwa sposoby:

* Koszt alokacji jest wysoki, ponieważ pamięć dla nowo przydzielonego dużego obiektu musi zostać wyczyszczona. Środowisko CLR gwarantuje, że pamięć dla wszystkich nowo przyznanych obiektów jest wyczyszczona.
* LOH jest zbierany z resztą sterty. LOH wymaga pełnego [wyrzucania elementów bezużytecznych](/dotnet/standard/garbage-collection/fundamentals) lub [kolekcji Gen2](/dotnet/standard/garbage-collection/fundamentals#generations).

Ten [wpis w blogu](https://adamsitnik.com/Array-Pool/#the-problem) opisuje problem zwięzłie:

> Po przydzieleniu dużego obiektu jest on oznaczony jako obiekt generacji 2. Nie generacji 0 jako dla małych obiektów. W przypadku braku pamięci w LOH, system GC czyści całe zarządzane sterty, a nie tylko LOH. W związku z tym czyści generacji gen 0, Gen 1 i Gen 2, w tym LOH. Jest to nazywane pełnym odzyskiwaniem pamięci i jest największym czasochłonnym wyrzucaniem elementów bezużytecznych. W przypadku wielu aplikacji może to być możliwe do zaakceptowania. Ale nie tylko w przypadku serwerów sieci Web o wysokiej wydajności, w których do obsługi średniego żądania sieci Web jest wymagana część pamięci podręcznej, która jest niezbędna do obsłużenia średniej, dekompresowania, dekodowania JSON & więcej).

Naively przechowywanie dużej treści żądania lub odpowiedzi w jeden `byte[]` lub `string`:

* Może to spowodować, że w LOH szybko kończy się miejsce.
* Może powodować problemy z wydajnością aplikacji ze względu na pełny operacje odzyskiwania pamięci uruchomiony.

## <a name="working-with-a-synchronous-data-processing-api"></a>Praca z synchronicznym interfejsem API przetwarzania danych

W przypadku korzystania z serializatora/deserializatora, który obsługuje tylko odczyty synchroniczne i zapisy (na przykład [JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)):

* Przebuforuj dane do pamięci asynchronicznie przed przekazaniem ich do serializatora/deserializatora.

> [!WARNING]
> Jeśli żądanie jest duże, może prowadzić do niewystarczającej ilości pamięci (OOM). OOM może spowodować odmowę usługi.  Aby uzyskać więcej informacji, zobacz [unikanie odczytywania dużych treści żądań lub treści odpowiedzi do pamięci](#arlb) w tym dokumencie.

ASP.NET Core 3,0 używa <xref:System.Text.Json> domyślnie dla serializacji JSON. <xref:System.Text.Json>:

* Odczytuje i zapisuje dane JSON asynchronicznie.
* Jest zoptymalizowany pod kątem tekstu w formacie UTF-8.
* Zwykle wyższa wydajność niż `Newtonsoft.Json`.

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a>Nie należy przechowywać IHttpContextAccessor. HttpContext w polu

[IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext) zwraca `HttpContext` aktywnego żądania, gdy uzyskuje dostęp z wątku żądania. `IHttpContextAccessor.HttpContext` **nie** powinna być przechowywana w polu ani zmiennej.

**Nie wykonuj tej czynności:** Poniższy przykład zapisuje `HttpContext` w polu, a następnie próbuje użyć go później.

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

Poprzedzający kod często przechwytuje wartość null lub nieprawidłowy `HttpContext` w konstruktorze.

**Wykonaj następujące czynności:** Poniższy przykład:

* Przechowuje <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> w polu.
* Używa pola `HttpContext` w prawidłowym czasie i sprawdza, czy `null`.

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a>Nie uzyskuj dostępu do obiektu HttpContext z wielu wątków

`HttpContext` *nie* jest bezpieczna wątkowo. Dostęp do `HttpContext` z wielu wątków równolegle może spowodować niezdefiniowane zachowanie, takie jak zawieszenie, awarie i uszkodzenie danych.

**Nie wykonuj tej czynności:** Poniższy przykład tworzy trzy żądania równoległe i rejestruje ścieżkę żądania przychodzącego przed i po wychodzącym żądaniu HTTP. Ścieżka żądania jest dostępna z wielu wątków, potencjalnie równolegle.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

**Wykonaj następujące czynności:** Poniższy przykład kopiuje wszystkie dane z żądania przychodzącego przed wykonaniem trzech żądań równoległych.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a>Nie używaj obiektu HttpContext po zakończeniu żądania

`HttpContext` jest prawidłowy tylko pod warunkiem, że w potoku ASP.NET Core istnieje aktywne żądanie HTTP. Cały potok ASP.NET Core jest asynchronicznym łańcuchem delegatów, które wykonują każde żądanie. Po zakończeniu `Task` zwrócone z tego łańcucha `HttpContext` zostanie odtworzony.

**Nie wykonuj tej czynności:** W poniższym przykładzie zastosowano `async void`, które powoduje, że żądanie HTTP kończy się po osiągnięciu pierwszego `await`:

* Jest to **zawsze** złe rozwiązanie w aplikacjach ASP.NET Core.
* Uzyskuje dostęp do `HttpResponse` po zakończeniu żądania HTTP.
* Powoduje awarię procesu.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

**Wykonaj następujące czynności:** Poniższy przykład zwraca `Task` do struktury, aby żądanie HTTP nie zostało ukończone do momentu zakończenia akcji.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a>Nie Przechwytuj obiektu HttpContext w wątkach w tle

**Nie wykonuj tej czynności:** Poniższy przykład pokazuje, że zamknięcie przechwytuje `HttpContext` z właściwości `Controller`. Jest to niewłaściwe rozwiązanie, ponieważ element roboczy może:

* Uruchom poza zakresem żądania.
* Podjęto próbę odczytu niewłaściwego `HttpContext`.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

**Wykonaj następujące czynności:** Poniższy przykład:

* Kopiuje dane wymagane w zadaniu w tle podczas żądania.
* Nie odwołuje się do żadnego z kontrolerów.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

Zadania w tle należy zaimplementować jako usługi hostowane. Aby uzyskać więcej informacji, zobacz [zadania w tle z usługami hostowanymi](xref:fundamentals/host/hosted-services).

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a>Nie Przechwytuj usług wprowadzonych do kontrolerów w wątkach w tle

**Nie wykonuj tej czynności:** Poniższy przykład pokazuje, że zamknięcie przechwytuje `DbContext` z parametru akcji `Controller`. Jest to niewłaściwe rozwiązanie.  Element roboczy można uruchomić poza zakresem żądania. `ContosoDbContext` jest objęty zakresem żądania, co skutkuje `ObjectDisposedException`.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

**Wykonaj następujące czynności:** Poniższy przykład:

* Wprowadza <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> w celu utworzenia zakresu w tle elementu pracy. `IServiceScopeFactory` jest klasą pojedynczą.
* Tworzy nowy zakres iniekcji zależności w wątku w tle.
* Nie odwołuje się do żadnego z kontrolerów.
* Nie przechwytuje `ContosoDbContext` z żądania przychodzącego.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

Następujący wyróżniony kod:

* Tworzy zakres czasu istnienia operacji w tle i rozwiązuje z niego usługi.
* Używa `ContosoDbContext` z poprawnego zakresu.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a>Nie należy modyfikować kodu stanu ani nagłówków po rozpoczęciu treści odpowiedzi

ASP.NET Core nie buforuje treści odpowiedzi HTTP. Podczas pierwszego zapisywania odpowiedzi:

* Nagłówki są wysyłane wraz z tym fragmentem treści do klienta programu.
* Nie można już zmieniać nagłówków odpowiedzi.

**Nie wykonuj tej czynności:** Poniższy kod próbuje dodać nagłówki odpowiedzi po rozpoczęciu odpowiedzi:

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

W poprzednim kodzie, `context.Response.Headers["test"] = "test value";` zgłosi wyjątek, jeśli `next()` zapisał w odpowiedzi.

**Wykonaj następujące czynności:** Poniższy przykład sprawdza, czy odpowiedź HTTP została uruchomiona przed zmodyfikowaniem nagłówków.

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

**Wykonaj następujące czynności:** Poniższy przykład używa `HttpResponse.OnStarting`, aby ustawić nagłówki przed opróżnieniem nagłówków odpowiedzi do klienta.

Sprawdzanie, czy odpowiedź nie została rozpoczęta, umożliwia rejestrowanie wywołania zwrotnego, które zostanie wywołane tuż przed zapisaniem nagłówków odpowiedzi. Sprawdzanie, czy nie uruchomiono odpowiedzi:

* Umożliwia dołączanie lub zastępowanie nagłówków tylko w czasie.
* Nie wymaga znajomości następnego oprogramowania pośredniczącego w potoku.

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a>Nie wywołuj Next (), jeśli już rozpoczęto zapisywanie do treści odpowiedzi

Składniki należy wywołać tylko wtedy, gdy jest to możliwe, aby obsługiwać i manipulować odpowiedzią.
