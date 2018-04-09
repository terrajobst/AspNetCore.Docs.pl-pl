---
title: Logikę kontrolera testu w ASP.NET Core
author: ardalis
description: Dowiedz się, jak logikę kontrolera testu w ASP.NET Core z Moq i xUnit.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: b80f92b815439796693528b314b521c1484ba661
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="test-controller-logic-in-aspnet-core"></a>Logikę kontrolera testu w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Kontrolery w aplikacjach ASP.NET MVC powinna być mała i skupiają się na problemy z interfejsem użytkownika. Dużych kontrolerów, które zajmują się problemów bez interfejsu użytkownika są trudne do testowania i obsługi.

[Przykładowy widok lub pobrania z witryny GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Kontrolery testów

Kontrolery są centralnej częścią żadnej aplikacji ASP.NET Core MVC. Tak powinien mieć pewność, które działają zgodnie z przeznaczeniem dla aplikacji. Testy automatyczne może udostępnić tym zaufania i może szybko wykrywać błędy przed dotarciem produkcji. Należy unikać umieszczania niepotrzebnych obowiązki w kontrolerach i upewnij się, zespół testy tylko na obowiązki kontrolera.

Logika kontroler powinien być minimalny i nie ukierunkowanych na logiki lub infrastruktury problemy biznesowe (na przykład dostęp do danych). Przetestuj logiką kontrolera, nie framework. Test jak kontroler *zachowuje się* oparte na prawidłowe lub nieprawidłowe dane wejściowe. Test na podstawie wyniku prowadzonej działalności, którą wykonuje odpowiedzi kontrolera.

Typowy kontroler obowiązki:

* Sprawdź `ModelState.IsValid`.
* Zwraca odpowiedź o błędzie, jeśli `ModelState` jest nieprawidłowy.
* Jednostek biznesowych należy pobrać z magazynu trwałego.
* Wykonaj akcję w jednostce biznesowej.
* Zapisz jednostki biznesowe trwałości.
* Zwraca preferowany `IActionResult`.

## <a name="unit-testing"></a>Testowanie jednostek

[Testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) obejmuje testowania części aplikacji w izolacji od jego infrastruktury i zależności. Podczas testowania logiką kontrolera, zawartości jednej akcji jednostek jest testowany, nie zachowanie z jego zależności lub struktury sam. Jako jednostki można przetestować akcji kontrolera, upewnij się, że można skupić się tylko na jego zachowania. Kontroler testu jednostkowego pozwala uniknąć elementów, jak [filtry](filters.md), [routingu](../../fundamentals/routing.md), lub [modelu powiązania](../models/model-binding.md). Istotą testowania tylko jeden element, testy jednostkowe są zwykle proste do zapisu i szybkie uruchamianie. Dobrze napisane zbiór testy jednostkowe mogą być uruchamiane często bez znacznie obciążenie. Jednak testów jednostkowych nie wykrywaj problemy w interakcji między składnikami, który jest celem [testy integracji](xref:mvc/controllers/testing#integration-testing).

Jeśli piszesz filtry niestandardowe, tras, itp., należy testu jednostkowego ich, ale nie jako część testy na akcję określony kontroler. Powinny one być testowane w izolacji.

> [!TIP]
> [Tworzenie i Uruchamianie testów jednostkowych z programem Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).

Aby zademonstrować testy jednostkowe, przejrzyj następujący kontroler. Wyświetla listę Burza sesji, a zezwala na nowe Burza sesji ma zostać utworzony wpis:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Oto kontrolera [zasady jawne zależności](http://deviq.com/explicit-dependencies-principle/), oczekiwano iniekcji zależności do tego celu z wystąpieniem `IBrainstormSessionRepository`. Ułatwia to dość łatwe do testowania przy użyciu platformy zasymulować obiektu, takie jak [Moq](https://www.nuget.org/packages/Moq/). `HTTP GET Index` — Metoda nie ma pętli lub gałęzi i tylko wywołania jednej metody. Aby to sprawdzić `Index` metody, należy sprawdzić, czy `ViewResult` jest zwracany, z `ViewModel` w repozytorium `List` metody.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` Sprawdzić — metoda (pokazanym powyżej):

* Metoda akcji zwraca nieprawidłowe żądanie `ViewResult` odpowiednimi danymi podczas `ModelState.IsValid` jest `false`

* `Add` Wywoływana jest metoda repozytorium i `RedirectToActionResult` jest zwracany za poprawne argumenty podczas `ModelState.IsValid` ma wartość true.

Nieprawidłowy stan modelu można sprawdzić przez dodanie błędów za pomocą `AddModelError` jak pokazano poniżej pierwszego testu.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Potwierdza pierwszego testu, gdy `ModelState` nie jest prawidłowa, taka sama `ViewResult` jest zwracane jako `GET` żądania. Należy pamiętać, że test nie próba przekazania w nieprawidłowy model. To zadziała nie mimo to ponieważ wiązania modelu nie jest uruchomiony (chociaż [testu integracji](xref:mvc/controllers/testing#integration-testing) użyje wiązania modelu wykonywania). W takim przypadku nie jest poddawana testom wiązania modelu. Te testy jednostkowe tylko testowania jest kod w metodzie akcji.

Drugi test sprawdza, że w przypadku `ModelState` jest prawidłowy, nowy `BrainstormSession` jest dodawana (repozytorium), a metoda zwraca `RedirectToActionResult` z oczekiwanym właściwości. Mocked wywołania, które nie są nazywane są zwykle została zignorowana, ale wywołanie `Verifiable` na końcu instalacji wywołania umożliwi można sprawdzić w teście. Jest to zrobić za pomocą wywołania `mockRepo.Verify`, który zakończy się niepowodzeniem testu, jeśli nie został wywołany oczekiwanej metody.

> [!NOTE]
> Biblioteka Moq używane w tym przykładzie ułatwia mieszać mocks weryfikowalny lub "strict" z-weryfikowalny mocks (zwaną również "utracić" mocks lub klas zastępczych). Dowiedz się więcej o [Dostosowywanie zachowania makiety z Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Inny kontroler w aplikacji Wyświetla informacje związane z sesją określonego Diagram burzy. Zawiera logikę radzenia sobie z wartości nieprawidłowy identyfikator:

[!code-csharp[](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Akcja kontrolera ma trzech przypadkach do testowania, po jednej dla każdego `return` instrukcji:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Aplikacja udostępnia funkcje jako składnika web API (Lista koncepcji związanych z sesji Diagram burzy i metody do dodawania nowych pomysłów do sesji):

<a name="ideas-controller"></a>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` Metoda zwraca listę `IdeaDTO` typów. Należy unikać zwracania jednostek biznesowych domeny bezpośrednio za pomocą interfejsu API, ponieważ często zawierają więcej danych niż wymaga klienta interfejsu API, a ich połączenie niepotrzebnie modelu domeny wewnętrznej aplikacji przy użyciu interfejsu API można ujawnić zewnętrznie. Mapowanie między domeny jednostek i typy zwróci się przez sieć można przeprowadzić ręcznie (za pomocą LINQ `Select` w sposób pokazany poniżej) lub za pomocą biblioteki, takich jak [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Testów jednostkowych dla `Create` i `ForSession` metody interfejsu API:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Jak wspomniano wcześniej, aby przetestować działanie metody podczas `ModelState` jest nieprawidłowy, Dodaj błąd modelu do kontrolera jako część testu. Nie należy próbować testów sprawdzania poprawności lub modelu wiązania modelu w testy jednostkowe — po prostu przetestować zachowania metodzie akcji, gdy do czynienia z określonego `ModelState` wartość.

Drugi test zależy od repozytorium zwracanie wartości null, aby zasymulować repozytorium jest skonfigurowany do zwrócenia wartości null. Nie istnieje potrzeba można utworzyć testowej bazy danych (w pamięci lub w inny sposób) i utworzyć kwerendę, która zwraca wynik — mogą to robić w jednej instrukcji jak pokazano.

Ostatni test sprawdza, czy z repozytorium `Update` metoda jest wywoływana. Jak opisano wcześniej, makiety jest wywoływana z `Verifiable` , a następnie mocked z repozytorium `Verify` metoda jest wywoływana, aby upewnić się, możliwe do zweryfikowania metody zostało wykonane. Nie jest odpowiedzialność testów jednostkowych upewnij się, że `Update` metody zapisane dane; który można zrobić za pomocą testu integracji.

## <a name="integration-testing"></a>Testowanie integracji

[Testy integracji](../../testing/integration-testing.md) poprawnie ze sobą zapewnia oddzielnych modułów w pracy aplikacji. Ogólnie rzecz biorąc coś, co można przetestować z testu jednostkowego, można również sprawdzić z testem integracji, ale odwrotnej nie jest spełniony. Jednak testy integracji zazwyczaj można znacznie mniejsza niż testy jednostkowe. W związku z tym najlepiej do testowania, niezależnie od można przy użyciu testów jednostkowych i użyć testów integracji dla scenariuszy obejmujących wiele współpracowników.

Mimo że nadal mogą być użyteczne, zasymulować obiekty są rzadko używane w testach integracji. W przypadku przeprowadzania testów jednostkowych zasymulować obiekty są efektywny sposób kontrolować sposób zachowania przypadku współpracownicy poza jednostki testowane na potrzeby testu. W teście integracji rzeczywistych współpracownicy są używane do upewnij się, że podsystem całego razem działa prawidłowo.

### <a name="application-state"></a>Stan aplikacji

Ważnym zagadnieniem podczas przeprowadzania testów integracji jest sposób ustawiania stanu aplikacji. Testy muszą działać od siebie niezależne, a więc każdy test powinien zaczynać się od aplikacji w znanego stanu. Jeśli dana aplikacja nie korzystania z bazy danych lub mieć żadnych trwałości, nie można tego problemu. Jednak większość aplikacji rzeczywistych utrwalić stanu do określonego rodzaju magazynu danych, więc wszelkie zmiany dokonane przez jeden test może mieć wpływ na innego testu, chyba że zostanie zresetowana do magazynu danych. Za pomocą wbudowanych `TestServer`, jest bardzo prosta do aplikacji platformy ASP.NET Core hosta w ramach naszych testów integracji, ale który nie musi udzielić dostępu do danych, zostanie użyty. Jeśli używasz istniejącej bazy danych, jednym z podejść jest aplikacjami nawiązać połączenia z bazą danych testów, które testy mogą uzyskać dostęp i upewnij się, zostanie zresetowana do znanego stanu przed wykonaniem każdego z testów.

W tej przykładowej aplikacji używam programu Entity Framework Core InMemoryDatabase pomocy technicznej, tak właśnie nie mogę połączyć do niej w projekcie testowym. Zamiast tego należy udostępnić `InitializeDatabase` metody z poziomu aplikacji `Startup` klasy, które można wywołać podczas uruchamiania aplikacji, jeśli jest w `Development` środowiska. Moje testy integracji automatycznie korzystać z to tak długo, jak będą one ustawienia środowiska na `Development`. Nie mam martwić się o zresetowanie bazy danych, ponieważ InMemoryDatabase jest resetowany każdym ponownym uruchomieniu aplikacji.

`Startup` Klasy:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Zobaczysz `GetTestSession` metody często używane w testach integracji poniżej.

### <a name="accessing-views"></a>Uzyskiwanie dostępu do widoków

Umożliwia skonfigurowanie każdej klasy testowej integracji `TestServer` który uruchomi aplikacji platformy ASP.NET Core. Domyślnie `TestServer` hostem aplikacji sieci web w folderze, w którym jest uruchomiona — w takim przypadku folderu projektu testowego. W związku z tym podczas próby test akcji kontrolera, które zwracają `ViewResult`, może zostać wyświetlony ten błąd:

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

Aby rozwiązać ten problem, należy skonfigurować serwer zawartości głównego, aby mogły zlokalizować widoki dla projektu testowane. Jest to realizowane przez wywołanie `UseContentRoot` w `TestFixture` klasy, pokazano poniżej:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture` Klasy jest odpowiedzialny za konfigurowanie i tworzenie `TestServer`, ustawieniu `HttpClient` do komunikowania się z `TestServer`. Każdy integrację testów używa `Client` właściwość, aby połączyć się z serwerem testu i wysłać żądanie.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

W pierwszym badaniu powyżej `responseString` przechowuje rzeczywiste renderowania kodu HTML z widoku, który można przeprowadzić inspekcji, aby potwierdzić zawiera oczekiwanych rezultatów.

Drugi test tworzy POST formularza z nazwą sesji unikatowy i zapisuje go w aplikacji, a następnie sprawdza, czy oczekiwanego przekierowania jest zwracany.

### <a name="api-methods"></a>Metody interfejsu API

Jeśli aplikacja udostępnia web API, jego warto mieć testów automatycznych upewnij się, są one wykonywane zgodnie z oczekiwaniami. Wbudowane `TestServer` ułatwia testowanie interfejsów API sieci web. Jeśli Twoje metody interfejsu API z wiązania modelu, należy zawsze sprawdzić `ModelState.IsValid`, oraz testy integracji są odpowiednim miejscu, aby potwierdzić z weryfikacją modelu działa prawidłowo.

Następujący zestaw docelowy testy `Create` metody w [IdeasController](xref:mvc/controllers/testing#ideas-controller) klasy pokazanym powyżej:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

W przeciwieństwie do integracji testy akcji, które zwraca widoków HTML metody interfejsu API sieci web, które zwracają wyniki zwykle można zdeserializowany jako silnie typizowanych obiektów, jak pokazano powyżej ostatniego testu. W takim przypadku testu deserializuje wynik, który ma `BrainstormSession` wystąpienia i potwierdza, że pomysł poprawnie została dodana do jego kolekcja pomysłów.

W tym artykule znajdziesz więcej przykładów testów integracji [przykładowy projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).
