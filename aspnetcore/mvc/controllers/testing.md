---
title: Logikę kontrolera testu w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak logikę kontrolera testu w programie ASP.NET Core za pomocą Moq i struktury xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2018
ms.locfileid: "41754511"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Logikę kontrolera testu w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Kontrolery są centralnym elementem dowolnej aplikacji platformy ASP.NET Core MVC. W efekcie powinien mieć pewność, której działają zgodnie z przeznaczeniem dla swojej aplikacji. Testy automatyczne udostępnić Ci ten zaufania i umożliwić wykrycie błędów, zanim dotrą produkcji. Należy unikać umieszczania niepotrzebne obowiązki w kontrolerach i upewnij się, zespół testy tylko na obowiązki kontrolera.

Logiką kontrolera powinny być minimalne i nie koncentrować się na logikę lub infrastruktury potencjalne problemy biznesowe (na przykład dostęp do danych). Przetestuj logiką kontrolera, nie framework. Test jak kontroler *zachowuje się* oparte na prawidłowe lub nieprawidłowe dane wejściowe. Testowanie odpowiedzi kontroler, na podstawie wyniku operacji biznesowych, który wykonuje.

Obowiązki typowe kontrolera:

* Sprawdź `ModelState.IsValid`.
* Zwraca odpowiedź na błąd, jeśli `ModelState` jest nieprawidłowy.
* Pobieranie jednostki biznesowej z trwałości.
* Wykonaj akcję na jednostkach biznesowych.
* Zapisz stan trwały jednostkach biznesowych.
* Zwraca preferowany `IActionResult`.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Testy jednostkowe z logiką kontrolera

[Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) obejmują testowanie w ramach aplikacji w izolacji od jego infrastruktura i zależności. Gdy logiką kontrolera, tylko zawartość jednej akcji testów jednostkowych jest testowany, nie zachowanie jego zależności lub samej strukturze. Jako jednostki możesz przetestować akcji kontrolera, upewnij się, że koncentrują się na jego zachowanie. Kontroler testu jednostkowego pozwala uniknąć elementów, takich jak [filtry](xref:mvc/controllers/filters), [routingu](xref:fundamentals/routing), lub [wiązanie modelu](xref:mvc/models/model-binding). Skupienie się na testowanie tylko jedną z rzeczy, testy jednostkowe są zwykle łatwe do zapisania i szybkie uruchamianie. Dobrze napisane zestaw testów jednostkowych mogą być uruchamiane często bez koszty ogólne. Jednak testy jednostkowe Nie wykrywaj problemy w interakcji między składnikami, który jest celem [testy integracji](xref:test/integration-tests).

Jeśli piszesz filtry niestandardowe i tras, wykonaj następujące czynności jednostki przetestuj je oddzielnie, a nie jako część testy na określony kontroler akcji.

> [!TIP]
> [Tworzenie i Uruchamianie testów jednostkowych za pomocą programu Visual Studio](/visualstudio/test/unit-test-your-code).

Aby zademonstrować testy jednostkowe, przejrzyj następujący kontroler. Jego Wyświetla listę Burza sesji i zezwala na nowe Burza sesje są tworzone z wpisu:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Obserwowanych kontrolera [zasady jawne zależności](http://deviq.com/explicit-dependencies-principle/), oczekiwano wstrzykiwanie zależności, aby udostępnić wystąpienia `IBrainstormSessionRepository`. Dzięki temu dość łatwe testowanie za pomocą platformy makiety obiektu, takie jak [Moq](https://www.nuget.org/packages/Moq/). `HTTP GET Index` Metoda nie ma pętli lub rozgałęziania i tylko wywołania jednej metody. Testować tę aplikację `Index` metody, należy sprawdzić, czy `ViewResult` jest zwracany za pomocą `ViewModel` z repozytorium `List` metody.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` Należy sprawdzić, metoda (jak pokazano powyżej):

* Metoda akcji zwraca nieprawidłowe żądanie `ViewResult` odpowiednimi danymi podczas `ModelState.IsValid` jest `false`.

* `Add` Wywoływana jest metoda repozytorium i `RedirectToActionResult` jest zwracany za pomocą poprawne argumenty po `ModelState.IsValid` ma wartość true.

Nieprawidłowy stan modelu mogą być testowane przez dodanie błędów za pomocą `AddModelError` pokazany poniżej pierwszego testu.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Pierwsze badanie potwierdza, kiedy `ModelState` nie jest prawidłowa, taka sama `ViewResult` jest zwracana jako dla `GET` żądania. Pamiętaj, że test nie próbuje przekazać nieprawidłowy model. To pomoże w takich sytuacjach przydałaby mimo to, ponieważ powiązanie modelu nie jest uruchomiona (chociaż [test integracji](xref:test/integration-tests) użyje wiązania modelu wykonywania). W tym przypadku nie jest poddawana testom wiązania modelu. Te testy jednostkowe są tylko testowanie, jak działa kod w metodzie akcji.

Drugi test weryfikuje, że w przypadku `ModelState` jest prawidłowy, nowy `BrainstormSession` jest dodawana (repozytorium), a metoda zwraca `RedirectToActionResult` z oczekiwanych właściwości. Pozorowane wywołania, które nie są wywoływane są zwykle zignorowane, ale wywoływania `Verifiable` na końcu instalacji wywołanie umożliwia weryfikowana w teście. Odbywa się przy użyciu wywołania do `mockRepo.Verify`, który zakończy się niepowodzeniem testu, jeśli oczekiwany metoda nie została wywołana.

> [!NOTE]
> Biblioteka Moq używane w tym przykładzie ułatwia mieszać mocks weryfikowalny lub "strict" z-weryfikowalny mocks (nazywanych również "luźne" mocks wycinków). Dowiedz się więcej o [Dostosowywanie zachowania pozorny przy użyciu Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Inny kontroler w aplikacji Wyświetla informacje związane z sesją określonego Diagram burzy. Zawiera logikę do czynienia z wartościami nieprawidłowy identyfikator:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Akcja kontrolera ma trzy przypadki, aby przetestować, jednej dla każdego `return` instrukcji:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Aplikacja ujawnia funkcjonalność w postaci internetowego interfejsu API (Lista pomysły skojarzony z sesją Diagram burzy a metoda dodawania nowych pomysłów do sesji):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` Metoda zwraca listę `IdeaDTO` typów. Należy unikać cofania jednostek biznesowych domeny bezpośrednio za pomocą wywołań interfejsu API, ponieważ często zawierają więcej danych niż klienta interfejsu API wymaga, a ich niepotrzebnie Połącz modelu domeny wewnętrznej aplikacji przy użyciu interfejsu API, należy udostępnić zewnętrznie. Mapowanie między domeny jednostek i typy będzie zwracać przewodowo może odbywać się ręcznie (przy użyciu LINQ `Select` jak pokazano poniżej) lub za pomocą biblioteki, takich jak [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Testowanie jednostkowe dla `Create` i `ForSession` metody interfejsu API:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Jak wspomniano wcześniej, aby sprawdzić zachowanie metody podczas `ModelState` jest nieprawidłowy, dodać błąd modelu do kontrolera jako część testu. Nie należy próbować testów sprawdzania poprawności lub modelu wiązania modelu do testów jednostkowych — po prostu testowanie zachowania metodzie akcji, gdy do czynienia z określonym `ModelState` wartość.

Drugi test jest zależny od repozytorium, zwracając wartość null, aby zasymulować repozytorium jest skonfigurowany do zwrócenia wartości null. Nie ma potrzeby tworzenia bazy danych testowych (w pamięci lub w inny sposób) i utworzyć zapytanie, które zwróci wynik — może to nastąpić w pojedynczej instrukcji jak pokazano.

Ostatni test sprawdza, czy z repozytorium `Update` metoda jest wywoływana. Ile My mieliśmy wcześniej, pozorny jest wywoływana z `Verifiable` i następnie pozorowane z repozytorium `Verify` metoda jest wywoływana, aby upewnić się, możliwe do zweryfikowania metoda została wykonana. Nie jest obowiązkiem testu jednostki, upewnij się, że `Update` metody zapisane dane; można to zrobić za pomocą wiąże się test integracji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/integration-tests>
