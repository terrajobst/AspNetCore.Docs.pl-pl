---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globalna Obsługa błędów w programie ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3e371760d2b34eb2be492e6ebbb33a5f9f7eff10
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577174"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globalna Obsługa błędów w programie ASP.NET Web API 2
====================
przez [David Matson](https://github.com/davidmatson), [Rick Anderson]((https://twitter.com/RickAndMSFT))

Obecnie nie istnieje łatwy sposób w interfejsie API sieci Web do logowania lub globalnie obsługi błędów. Niektóre nieobsługiwane wyjątki mogą być przetwarzane za pośrednictwem [filtry wyjątków](exception-handling.md), ale istnieje kilka przypadków, które nie obsługują filtry wyjątków. Na przykład:

1. Wyjątków zgłaszanych przez konstruktory kontrolera.
2. Wyjątków zgłaszanych przez programy obsługi komunikatów.
3. Wyjątki zgłoszone podczas routingu.
4. Wyjątki zgłoszone podczas serializacji treści odpowiedzi.

Chcemy zapewnić prosty i spójny sposób logowania i obsługiwać (tam, gdzie jest to możliwe) tych wyjątków. 

Istnieją dwa główne przypadki, dotyczące obsługi wyjątków, wtedy, gdy jesteśmy w stanie wysyłać odpowiedzi na błąd i przypadek, w którym wszystkie możliwości jest dziennik wyjątku. Na przykład na tym ostatnim przypadku sytuacja zgłaszany jest wyjątek w trakcie przesyłania strumieniowego zawartości odpowiedzi w takiej sytuacji jest za późno wysyłać nowy komunikat odpowiedzi, ponieważ kod stanu, nagłówki i częściowej zawartości już wykonano faktycznie, dzięki czemu możemy po prostu przerwać połączenie. Nawet jeśli wyjątek nie mogą być obsługiwane, który zwróci nowy komunikat odpowiedzi, firma Microsoft nadal obsługuje rejestrowania wyjątku. W przypadkach, gdy zostanie wykryte, błąd zostanie zwrócona odpowiedź odpowiedni komunikat o błędzie, jak pokazano w poniższym:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Istniejące opcje

Oprócz [filtry wyjątków](exception-handling.md), [programy obsługi komunikatów](../advanced/http-message-handlers.md) można dziś obserwować wszystkie odpowiedzi na poziomie 500, ale działają w odpowiedzi na te jest trudne, ponieważ mają kontekst dotyczący pierwotnego błędu. Programy obsługi komunikatów ma część te same ograniczenia jako filtry wyjątków dotycząca przypadków, w których one może obsłużyć. Podczas interfejsu API sieci Web jest Infrastruktura śledzenia, która przechwytuje warunki błędów Infrastruktura śledzenia jest na potrzeby diagnostyki i jest nie zaprojektowane lub odpowiednie do wykonywania w środowisku produkcyjnym. Globalne wyjątków, obsługa i rejestrowanie powinny być usług, które można uruchomić podczas produkcji i można podłączyć do istniejącego rozwiązania do monitorowania (na przykład [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Omówienie rozwiązania

 Firma Microsoft zapewnia dwie nowe usługi użytkownika replaceable [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) i IExceptionHandler, zaloguj się i obsługiwać nieobsługiwanych wyjątków. Usługi są bardzo podobne, z dwa główne różnice:

1. Firma Microsoft obsługuje rejestrowanie wielu rejestratorów wyjątków, ale tylko program obsługi wyjątku.
2. Rejestratory wyjątek zawsze wywoływana, nawet jeśli firma Microsoft zamierzasz przerwać połączenie. Tylko programy obsługi wyjątków zostać wywołana, gdy Będziemy nadal można wybrać, które komunikat odpowiedzi do odesłania.

Obie te usługi zapewniają dostęp do istotnych informacji od punktu, w których wykryto wyjątek zawierającą kontekstu wyjątku szczególnie [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), zgłoszony wyjątek i źródła wyjątku (szczegóły poniżej).

### <a name="design-principles"></a>Zasady projektowania

1. **Nie przełomowych zmianach** , ponieważ ta funkcja jest dodawany w wersji pomocniczej, jest ważne ograniczenia wpływu na rozwiązania, który istnieć żadnych istotnych zmian, albo wpisz kontraktów lub do zachowania. To ograniczenie wykluczyć niektóre oczyszczania, które chcemy wykonano pod względem istniejące bloki catch włączenie wyjątki w odpowiedzi 500. To dodatkowe oczyszczanie jest coś, co firma Microsoft może należy wziąć pod uwagę dla kolejnych wersji głównej. Jeśli jest to ważne, Zagłosuj na nim na [głos użytkownika ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Utrzymywanie spójności z interfejsu API sieci Web tworzy** potoku filtru internetowego interfejsu API jest to doskonały sposób, aby obsłużyć odciąż przekrojowe zagadnienia z możliwością stosowania logiki w zakresie określonych akcji, kontrolera lub globalnego. Filtry, w tym filtry wyjątków zawsze mieć akcji i kontrolera kontekstów, nawet wtedy, gdy zarejestrowana w zakresie globalnym. Istnieje kontraktu ma sens dla filtrów, że oznacza to, że filtry wyjątków, nawet o zakresie globalnym te nie są dobrym rozwiązaniem dla niektórych wyjątków, obsługa przypadkach, takich jak wyjątki od obsługi komunikatów, w przypadku, gdy kontekst nie akcji lub kontrolera. Jeśli chcemy użyć elastyczne zakresu udostępnianych przez filtry dla obsługi wyjątków, wciąż potrzebujemy filtry wyjątków. Ale jeśli musimy obsłużyć wyjątek poza kontekstem kontrolera, należy również oddzielne konstrukcji obsługi błędów pełnym globalnym (coś bez kontrolera kontekstu i działania kontekstu ograniczenia).

### <a name="when-to-use"></a>Kiedy należy używać

- Rejestratory wyjątek to rozwiązanie, aby wyświetlać wszystkie nieobsługiwany wyjątek zgłoszony przez interfejs API sieci Web.
- Programy obsługi wyjątków są rozwiązania dotyczące dostosowywania wszystkie możliwe odpowiedzi na nieobsługiwane wyjątki przechwytywane przez interfejs API sieci Web.
- Filtry wyjątków są najprostszym rozwiązaniem do przetwarzania podzbiorem unhandled wyjątki związane z określonej akcji lub kontrolera.

### <a name="service-details"></a>Szczegóły usługi

 Interfejsy usługi rejestrowania i obsługi wyjątków są również metody asynchroniczne proste podjęcia odpowiednich kontekstach: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Oferujemy również klas bazowych dla obu tych interfejsów. Nadpisywania metod core (synchronizacji lub asynchronicznego) to wszystko, co jest wymagane do logowania się lub obsługi w zalecanym razy. Do rejestrowania, `ExceptionLogger` klasy bazowej zapewni, że podstawowa metoda rejestrowania jest wywołany tylko raz dla każdego wyjątku (nawet jeśli później propaguje dalsze w górę stosu wywołań i zostanie przechwycony ponownie). `ExceptionHandler` Klasy bazowej, wywoła core obsługi metody tylko w przypadku wyjątków w górnej części stosu wywołań, ignorowanie starsza wersja zagnieżdżone bloki catch. (Uproszczone wersje tych klas bazowych są w poniższym załączniku). Zarówno `IExceptionLogger` i `IExceptionHandler` otrzymywać informacje o wyjątku za pośrednictwem `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Gdy struktura wywołuje moduł rejestrujący wyjątku lub obsługi wyjątków, zawsze będzie zapewniać `Exception` i `Request`. Z wyjątkiem testy jednostkowe, również są zawsze będzie ona `RequestContext`. Rzadko zapewni `ControllerContext` i `ActionContext` (tylko w przypadku wywoływania z bloku catch dla filtry wyjątków). Będzie ona bardzo rzadko `Response`(tylko w niektórych przypadkach usług IIS, po połowie próbu zapisu odpowiedzi). Należy pamiętać, że ponieważ niektóre z tych właściwości mogą być `null` zależy od konsumenta, aby wyszukać `null` przed uzyskaniem dostępu do elementów członkowskich klasy wyjątku.`CatchBlock` ciąg wskazuje blok catch, który wystąpił wyjątek. Ciągi bloku catch, są następujące:

- HttpServer (metoda SendAsync)
- HttpControllerDispatcher (metoda SendAsync)
- HttpBatchHandler (metoda SendAsync)
- IExceptionFilter (firmy klasy ApiController przetwarzania potoku filtru wyjątków w ExecuteAsync)
- Host OWIN.

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (w przypadku buforowania danych wyjściowych)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (do przesyłania strumieniowego danych wyjściowych)
- Host sieci Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (w przypadku buforowania danych wyjściowych)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (do przesyłania strumieniowego danych wyjściowych)
    - HttpControllerHandler.WriteErrorResponseContentAsync (pod kątem błędów odzyskiwanie po wystąpieniu błędu w trybie buforowanych wyników)

Lista ciągów bloku catch jest również dostępna za pośrednictwem właściwości statycznego tylko do odczytu. (Ciąg bloku catch core znajdują się na statyczne ExceptionCatchBlocks; resztę pojawiają się na jednej klasy statycznej każdego hosta OWIN i sieci web).`IsTopLevelCatchBlock` przydaje się do korzystania z zalecany wzorzec obsługi wyjątków, tylko w górnej części stosu wywołań. Zamiast włączania wyjątki w odpowiedzi 500, wszędzie tam, gdzie występuje zagnieżdżony blok catch, procedura obsługi wyjątków można pozwolić wyjątki dostawały aż do chwili informacje są widoczne dla hosta.

Oprócz `ExceptionContext`, Rejestrator pobiera jeszcze jeden fragment informacji za pośrednictwem pełnej `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Drugą właściwością `CanBeHandled`, umożliwia rejestratora do identyfikowania wyjątek, który nie może być obsługiwane. Gdy połączenie ma zostać przerwane i nie nowy komunikat odpowiedzi mogą być wysyłane, rejestratory, które zostaną wywołane, ale program obsługi będzie ***nie*** można wywołać i rejestratory, które można określić w tym scenariuszu z tej właściwości.

W dodatkowej `ExceptionContext`, klauzula obsługi przejmie jedną właściwość więcej można ustawić na pełną `ExceptionHandlerContext` do obsługi wyjątków:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Program obsługi wyjątku wskazuje, czy wyjątek jest obsługiwany dzięki ustawieniu `Result` właściwości wyniku akcji (na przykład [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), lub wynik niestandardowych). Jeśli `Result` właściwość ma wartość null, wyjątek jest nieobsługiwany i oryginalnym wyjątek zostanie zgłoszony ponownie.

Wyjątki w górnej części stosu wywołań Skorzystaliśmy dodatkowego kroku, aby upewnić się, że odpowiedź jest odpowiednia dla wywołań interfejsu API. Propaguje wyjątek do hosta, obiekt wywołujący będą w nim wyświetlane żółte ekran lub niektórych innych hostów podany odpowiedź, która ma zazwyczaj format HTML, a nie zawsze odpowiednią odpowiedź na błąd interfejsu API. W takich przypadkach uruchamia wynik inną niż null i tylko wtedy, gdy jawnie Ustawia program obsługi wyjątku niestandardowych z powrotem do `null` (nieobsługiwany) będzie wyjątek propagowany do hosta. Ustawienie `Result` do `null` w takich przypadkach może być przydatne w przypadku dwóch scenariuszy:

1. OWIN hostowane interfejsu API sieci Web, z wyjątkiem niestandardowych obsługi oprogramowania pośredniczącego zarejestrowane przed/poza interfejsu API sieci Web.
2. Lokalne debugowanie za pośrednictwem przeglądarki, gdzie żółty ekranu śmierci to naprawdę przydatne odpowiedzi dla nieobsługiwanego wyjątku.

Dla obu rejestratorów wyjątków i programy obsługi wyjątków firma Microsoft nie robią niczego do odzyskania, jeśli Rejestrator lub program obsługi zgłasza wyjątek. (Inne niż umożliwiając wyjątek Propagacja, przesłać opinię, u dołu tej strony, jeśli masz lepszym rozwiązaniem.) Kontrakt rejestratorów wyjątków i programy obsługi jest, czy należy zezwala wyjątki dostawały maksymalnie swoim klientom; w przeciwnym razie wyjątek po prostu rozpropaguje, często aż do hosta, zostaje wyświetlony błąd HTML (np. ASP. Ekran żółty firmy NET) są wysyłane z powrotem do klienta (zwykle nie jest preferowaną opcją dla wywołań interfejsu API, które oczekują JSON lub XML).

## <a name="examples"></a>Przykłady

### <a name="tracing-exception-logger"></a>Śledzenie Rejestrator wyjątku

Rejestrator wyjątku poniżej Wyślij dane o wyjątkach do skonfigurowanego źródła śledzenia (w tym oknie danych wyjściowych debugowania w programie Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Program obsługi wyjątków komunikat błędu niestandardowego

Następujące poniżej generuje odpowiedzi błędu niestandardowego do klientów, w tym adres e-mail kontaktu z działem pomocy technicznej.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Rejestrowanie filtry wyjątków

Jeśli używasz szablonu projektu "Aplikacja sieci Web 4 MVC ASP.NET" do tworzenia projektu, umieść kod konfiguracji interfejsu API sieci Web wewnątrz `WebApiConfig` klasy w *aplikacji/_uruchom* folderu:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Dodatku: Szczegóły klasy bazowej

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
