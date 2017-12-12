---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: "Globalne obsługi błędów w ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d2bdf04b4da2a099f3a2af100b16682c68f946f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globalne obsługi błędów w ASP.NET Web API 2
====================
przez [Matson Dominik](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Obecnie nie istnieje łatwy sposób w interfejsie API sieci Web do logowania się lub globalnie obsługi błędów. Niektórych z nieobsługiwanych wyjątków mogą być przetwarzane przez [filtry wyjątków](exception-handling.md), ale liczbę przypadków mogących filtry wyjątków nie może obsłużyć. Na przykład:

1. Wyjątki generowane z konstruktorami kontrolera.
2. Wyjątków zgłaszanych przez programy obsługi wiadomości.
3. Wyjątki generowane podczas routingu.
4. Wyjątki generowane podczas serializacji treści odpowiedzi.

Chcemy prosty i spójny sposób logowania i (jeśli będzie to możliwe) przekazać te wyjątki. 

Istnieją dwa główne przypadki do obsługi wyjątków, wtedy, gdy możemy wysłać odpowiedź o błędzie i dziennik wyjątku jest przypadek, w którym wszystkie możemy. Jest przykład drugim przypadku, gdy wyjątek w trakcie przesyłania strumieniowego zawartości odpowiedzi; w takim przypadku jest za późno wysłać nowy komunikat odpowiedzi, ponieważ kod stanu, nagłówki i zawartość częściowa już przeszły przez sieć, dlatego firma Microsoft po prostu przerwać połączenie. Mimo że nie mogą być obsługiwane wyjątek, który zwróci nowy komunikat odpowiedzi, nadal obsługuje się rejestrowania wyjątku. W przypadkach, gdy zostanie wykryte, błąd zostanie zwrócona odpowiedź o błędzie odpowiednie jak pokazano poniżej:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Istniejące opcje

Oprócz [filtry wyjątków](exception-handling.md), [programy obsługi komunikatów](../advanced/http-message-handlers.md) można obecnie przestrzegać wszystkich odpowiedzi na poziomie 500, ale działający na tych odpowiedzi jest trudne, ponieważ mają kontekstu o pierwotnego błędu. Programy obsługi wiadomości ma niektóre te same ograniczenia co filtry wyjątków dotyczących przypadków, które może obsłużyć ich. Podczas interfejsu API sieci Web jest Infrastruktura śledzenia, który przechwytuje warunki błędów Infrastruktura śledzenia w celach diagnostycznych i jest nie zaprojektowane lub przeznaczony do uruchamiania w środowisku produkcyjnym. Globalne wyjątków, obsługa i rejestrowanie powinna być usług, które można uruchomić w środowisku produkcyjnym i być podłączane do istniejących rozwiązań monitorowania (na przykład [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Omówienie rozwiązania

 Firma Microsoft udostępnia dwie nowe usługi użytkownik replaceable [interfejsu IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) i IExceptionHandler, aby zalogować się i obsługi nieobsługiwanych wyjątków. Usługi są bardzo podobne z dwóch głównych różnic:

1. Firma Microsoft obsługuje rejestracji wielu rejestratorów wyjątek, ale tylko jeden wyjątek obsługi.
2. Wyjątek rejestratorów zawsze uzyskać wywołana, nawet jeśli firma Microsoft zamierzasz przerwać połączenie. Tylko programy obsługi wyjątków uzyskać wywoływana, gdy będziemy mogli nadal wybierz które komunikat odpowiedzi do odesłania.

Obie te usługi zapewniają dostęp do kontekstu wyjątku zawierający informacje z punktu, w których wykryto wyjątek, szczególnie [HttpRequestMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/en-us/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), zgłoszony wyjątek i wyjątek źródła (szczegóły poniżej).

### <a name="design-principles"></a>Zasady projektowania

1. **Nie zmian, które psuły** , ponieważ ta funkcja jest dodawany w wersji pomocniczej, jest ważne ograniczenia wpływające na rozwiązanie, który nie istnieć żadne zmiany podziału, albo wpisz umów lub w celu zachowania. To ograniczenie wykluczyć pewne Oczyszczanie chcielibyśmy to zostało zrobione w istniejących bloki catch Włączanie wyjątków w odpowiedzi 500. To dodatkowe oczyszczanie jest coś, co możemy warto uwzględnić w kolejnych wersji głównej. Jeśli jest to ważne należy oddawać głosy na go w [głos użytkownika ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Zachowaniu spójności z interfejsu API sieci Web tworzy** składnika Web API potoku filtru jest doskonałym sposobem obsługi dotyczy kompleksowymi elastyczność stosowania logikę w zakresie określonych akcji, kontrolera specyficznych lub globalnego. Filtry, w tym filtry wyjątków zawsze mieć kontekstów akcji i kontrolera, nawet wtedy, gdy zarejestrowana w zakresie globalnym. Istnieje kontraktu ma sens dla filtrów, że oznacza to, że filtry wyjątków, nawet o zakresie globalnym te nie są dobrze dla niektórych wyjątków, obsługa przypadkach, takich jak wyjątki od obsługi komunikatów, których brak kontekstu akcji lub kontrolera. Jeśli chcemy użyć elastyczne zakres zapewnianej przez filtry dla obsługi wyjątków, potrzebujemy jeszcze filtry wyjątków. Ale jeśli trzeba obsłużyć wyjątek poza kontekstu kontrolera, również potrzebujemy konstrukcję oddzielne do obsługi błędów pełnej globalnej (coś bez kontrolera akcji i kontekstu kontekstu ograniczenia).

### <a name="when-to-use"></a>Kiedy używać

- Rejestratory wyjątek to rozwiązanie, aby wyświetlać wszystkie nieobsługiwany wyjątek zgłoszony przez interfejs API sieci Web.
- Programy obsługi wyjątków są w zakresie dostosowywania wszystkich możliwych odpowiedzi na nieobsługiwanych wyjątków zgłoszony przez interfejs API sieci Web.
- Filtry wyjątków są najlepszym rozwiązaniem dla przetwarzania podzbioru nieobsługiwany wyjątki związane z określonych akcji lub kontrolera.

### <a name="service-details"></a>Szczegóły usługi

 Interfejsy usługi rejestrowania i obsługi wyjątków są również metody asynchroniczne proste podjęcia odpowiednich kontekstach: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Firma Microsoft udostępnia również klasy podstawowe dla obu tych interfejsów. Zastępowanie metody core (synchronizacji lub async) jest wszystkie wymagane do logowania lub obsługiwać na zalecanej razy. Do rejestrowania, `ExceptionLogger` klasy podstawowej zapewni, że podstawowa metoda rejestrowania jest wywołany tylko raz dla każdego wyjątku (nawet jeśli później propaguje dodatkowo górę stosu wywołań i zostanie przechwycony ponownie). `ExceptionHandler` Klasy podstawowej wywoła podstawowej obsługi metody tylko dla wyjątków w górnej części stosu wywołań, ignorowanie starszych zagnieżdżone bloki catch. (Uproszczonej wersji tych klas podstawowych są w dodatku poniżej). Zarówno `IExceptionLogger` i `IExceptionHandler` otrzymywać informacje o wyjątku za pośrednictwem `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Gdy struktura wywołuje moduł rejestrujący wyjątku lub obsługi wyjątków, zawsze będą zapewniać `Exception` i `Request`. Z wyjątkiem testy jednostkowe, zawsze będzie ona `RequestContext`. Będzie ona rzadko `ControllerContext` i `ActionContext` (tylko w przypadku wywoływania z bloku catch dla filtry wyjątków). Bardzo rzadko zapewni `Response`(tylko w niektórych przypadkach usług IIS, gdy środku do zapisania w odpowiedzi). Należy pamiętać, że niektóre z tych właściwości może być `null` zależy od użytkownika, aby wyszukać `null` przed uzyskaniem dostępu do członków klasy wyjątku.`CatchBlock` jest ciąg wskazujący, który blok catch był wyświetlany wyjątek. Ciągi bloku catch są następujące:

- HttpServer (metoda SendAsync)
- HttpControllerDispatcher (metoda SendAsync)
- HttpBatchHandler (metoda SendAsync)
- IExceptionFilter (przetwarzanie klasy ApiController w potoku filtru wyjątków w ExecuteAsync)
- OWIN host:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (do buforowania danych wyjściowych)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (do strumienia wyjściowego)
- Host sieci Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (do buforowania danych wyjściowych)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (do strumienia wyjściowego)
    - HttpControllerHandler.WriteErrorResponseContentAsync (w przypadku awarii w błąd odzyskiwania w trybie buforowane dane wyjściowe)

Lista ciągów bloku catch jest również dostępny za pośrednictwem właściwości statycznego tylko do odczytu. (Ciągu bloku catch core znajdują się na statycznych ExceptionCatchBlocks; pozostałe pojawiają się na jednej klasy statycznej każdego hosta sieci web i OWIN).`IsTopLevelCatchBlock` jest przydatne w przypadku następujących zalecanych wzorzec obsługi wyjątków tylko u góry stosu wywołań. Zamiast włączania wyjątków w odpowiedzi 500, wszędzie tam, gdzie występuje zagnieżdżony blok catch, program obsługi wyjątku pozwolić wyjątki propagację aż do ich temat, aby była widoczna przez hosta.

Oprócz `ExceptionContext`, Rejestrator pobiera jednej więcej informacji za pomocą pełnego `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Drugą właściwością `CanBeHandled`, umożliwia rejestratora do identyfikowania wyjątek, który nie mogą być obsługiwane. Gdy połączenie ma być przerwane i mogą być wysyłane nie nowy komunikat odpowiedzi, rejestratorów zostanie wywołana, ale program obsługi zostanie ***nie*** można wywołać i rejestratorów może zidentyfikować ten scenariusz z tej właściwości.

W dodatkowej `ExceptionContext`, program obsługi pobiera jeden więcej właściwości można ustawić na pełny `ExceptionHandlerContext` do obsługi wyjątku:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Program obsługi wyjątku wskazuje, czy wyjątek jest obsługiwany dzięki ustawieniu `Result` wynik akcji dla właściwości (na przykład [ExceptionResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), lub niestandardowy wyników). Jeśli `Result` właściwość ma wartość null, jest nieobsługiwany wyjątek i pierwotny wyjątek zostanie zgłoszony ponownie.

Wyjątki w górnej części stosu wywołań Wybraliśmy dodatkowy krok, aby upewnić się, że odpowiedź jest odpowiedni dla wywołań interfejsu API. Jeśli wyjątek propaguje do hosta, wywołujący zobaczy żółty ekran lub niektórych innych hostów podane odpowiedzi, który jest zwykle HTML i zazwyczaj nie właściwą odpowiedź błąd interfejsu API. W takich przypadkach uruchamia wynik inną niż null i tylko wtedy, gdy jawnie Ustawia program obsługi wyjątku niestandardowych z powrotem do `null` (nieobsługiwany) zostanie wyjątek propagowane do hosta. Ustawienie `Result` do `null` w takich przypadkach może być przydatne w przypadku dwóch scenariuszy:

1. OWIN hostowana interfejsu API sieci Web, z wyjątkiem niestandardowych obsługi oprogramowania pośredniczącego w zarejestrowany przed/poza interfejsu API sieci Web.
2. Debugowanie lokalne za pośrednictwem przeglądarki, gdzie żółty ekran jest rzeczywiście przydatne odpowiedzi na nieobsługiwany wyjątek.

Rejestratory wyjątków i programy obsługi wyjątków firma Microsoft nie wykonuj żadnych czynności do odzyskania, jeśli Rejestrator lub obsługi samego zgłasza wyjątek. (Inne niż co wyjątek propagację, pozostaw opinii u dołu tej strony, jeśli masz lepszym rozwiązaniem.) Kontrakt dla rejestratorów wyjątków i programy obsługi jest czy powinna zezwala wyjątki propagacji do ich wywołań; w przeciwnym razie wyjątek właśnie rozpropaguje, często aż do hosta, co powoduje błąd HTML (na przykład ASP. Ekran żółty przez sieć) są wysyłane do klienta (która zazwyczaj nie jest opcją preferowaną dla wywołań interfejsu API, które JSON lub XML).

## <a name="examples"></a>Przykłady

### <a name="tracing-exception-logger"></a>Śledzenie Rejestrator wyjątku

Rejestrator wyjątku poniżej wysyłać dane wyjątek do skonfigurowana źródła śledzenia (w tym oknie danych wyjściowych debugowania w programie Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Obsługa wyjątków komunikat błędu niestandardowego

Następujące poniżej generuje odpowiedzi błędu niestandardowego do klientów, w tym adres e-mail kontaktu z działem pomocy technicznej.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Rejestrowanie filtry wyjątków

Użycie szablonu projektu "Platformy ASP.NET MVC 4 aplikacji sieci Web" Aby utworzyć projekt, umieść kod konfiguracji interfejsu API sieci Web wewnątrz `WebApiConfig` klasy w *aplikacji/_uruchom* folderu:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Dodatek: Szczegóły klasy podstawowej

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
