---
title: Logikę kontrolera testu w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak logikę kontrolera testu w programie ASP.NET Core za pomocą Moq i struktury xUnit.
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2018
uid: mvc/controllers/testing
ms.openlocfilehash: 7e27f30e35c2c6e9062c8321b8b8544a38a69605
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758144"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Logikę kontrolera testu w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

[Kontrolery](xref:mvc/controllers/actions) odgrywają główną rolę w dowolnej aplikacji platformy ASP.NET Core MVC. W efekcie powinien mieć pewność, że kontrolery zachowują się zgodnie z oczekiwaniami. Aplikacja jest wdrażana w środowisku produkcyjnym, zautomatyzowanych testów można wykrywać błędy.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Testy jednostkowe z logiką kontrolera

[Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) obejmują testowanie w ramach aplikacji w izolacji od jego infrastruktura i zależności. Gdy logiką kontrolera testów jednostki, tylko zawartość jednej akcji jest testowana nie zachowanie jego zależności lub samej strukturze.

Konfigurowanie testów jednostkowych dla akcji kontrolera, aby skoncentrować się na zachowanie kontrolera. Kontroler testu jednostkowego unika scenariuszy, takich jak [filtry](xref:mvc/controllers/filters), [routingu](xref:fundamentals/routing), i [wiązanie modelu](xref:mvc/models/model-binding). Testy, które obejmują interakcje między składnikami, zbiorczo odpowiada na żądanie, które są obsługiwane przez *testy integracji*. Aby uzyskać więcej informacji na temat testów integracji, zobacz <xref:test/integration-tests>.

Jeśli piszesz filtry niestandardowe i tras testów jednostkowych je oddzielnie, a nie jako część testy na określony kontroler akcji.

Aby zademonstrować kontrolera testów jednostkowych, przejrzyj następujący kontroler w przykładowej aplikacji. Kontrolera głównego Wyświetla listę Burza sesji i umożliwia tworzenie nowych sesji Diagram burzy za pomocą żądania POST:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

Poprzedni kontrolera:

* Następuje [zasady jawne zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).
* Oczekuje, że [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) zapewnienie wystąpienie `IBrainstormSessionRepository`.
* Można przetestować za pomocą pozorowane `IBrainstormSessionRepository` usługi przy użyciu framework makiety obiektu, takie jak [Moq](https://www.nuget.org/packages/Moq/). A *pozorowane obiektu* jest obiektem metalowych z zestawem wstępnie zdefiniowanych zachowań właściwości i metody używane do testowania. Aby uzyskać więcej informacji, zobacz [Introduction to testy integracji](xref:test/integration-tests#introduction-to-integration-tests).

`HTTP GET Index` Metoda nie ma pętli lub rozgałęziania i tylko wywołania jednej metody. Test jednostkowy dla tej akcji:

* Mocks `IBrainstormSessionRepository` usługi przy użyciu `GetTestSessions` metody. `GetTestSessions` tworzy dwie sesje makiety mózgów z datami i nazwy sesji.
* Wykonuje `Index` metody.
* Sprawia, że potwierdzenia wynik zwracany przez metodę:
  * Element <xref:Microsoft.AspNetCore.Mvc.ViewResult> jest zwracana.
  * [ViewDataDictionary.Model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) jest `StormSessionViewModel`.
  * Dostępne są dwie sesje Diagram burzy przechowywane w `ViewDataDictionary.Model`.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

Kontrolera głównego `HTTP POST Index` metoda badania sprawdza, czy:

* Gdy [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) jest `false`, metoda akcji zwraca *400 Niewłaściwe żądanie* <xref:Microsoft.AspNetCore.Mvc.ViewResult> przy użyciu odpowiednich danych.
* Gdy `ModelState.IsValid` jest `true`:
  * `Add` Wywoływana jest metoda w repozytorium.
  * Element <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> jest zwracany za pomocą poprawne argumenty.

Nieprawidłowy stan modelu jest testowany, dodając błędów za pomocą <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> zgodnie z pierwszego testu poniżej:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

Gdy [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) nie jest prawidłowa, taka sama `ViewResult` jest zwracany w przypadku żądania GET. Test nie próbuje przekazać nieprawidłowy model. Przekazywanie nieprawidłowy model nie jest prawidłową podejście, ponieważ powiązanie modelu nie jest uruchomiona (mimo że [test integracji](xref:test/integration-tests) Użyj wiązania modelu). W tym przypadku nie jest testowana wiązania modelu. Te testy jednostkowe tylko testowany kod w metodzie akcji.

Drugi test weryfikuje, że w przypadku `ModelState` jest prawidłowy:

* Nowy `BrainstormSession` jest dodawana (repozytorium).
* Metoda ta zwraca `RedirectToActionResult` z oczekiwanych właściwości.

Pozorowane wywołania, które nie są wywoływane są zwykle zignorowane, ale wywoływania `Verifiable` na końcu instalacji wywołanie umożliwia makiety walidacji w teście. Jest to wykonywane przy użyciu wywołania do `mockRepo.Verify`, który kończy się niepowodzeniem testu, jeśli oczekiwany metoda nie została wywołana.

> [!NOTE]
> Biblioteka Moq używane w tym przykładzie sprawia, że można mieszać mocks weryfikowalny lub "strict" z-weryfikowalny mocks (nazywanych również "luźne" mocks wycinków). Dowiedz się więcej o [Dostosowywanie zachowania pozorny przy użyciu Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

[SessionController](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/controllers/testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) w przykładzie aplikacja wyświetla informacje dotyczące sesji określonego Diagram burzy. Kontroler zawiera logikę do czynienia z nieprawidłowy `id` wartości (istnieją dwa `return` scenariuszy, w poniższym przykładzie, aby uwzględnić te scenariusze). Końcowe `return` instrukcja zwraca nową `StormSessionViewModel` do widoku (*Controllers/SessionController.cs*):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

Testy jednostkowe zawierają jeden test dla każdego `return` scenariusza w kontrolerze sesji `Index` akcji:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

Przenoszenie do kontrolera pomysły, ujawniania funkcji przez aplikację jako interfejs API sieci web na `api/ideas` trasy:

* Listę pomysłów (`IdeaDTO`) skojarzonego z Diagram burzy sesji jest zwracany przez `ForSession` metody.
* `Create` Metoda dodaje nowe pomysły z sesją.

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

Należy unikać zwracanie jednostki domeny biznesowej bezpośrednio za pomocą wywołań interfejsu API. Jednostki domeny:

* Często zawierają więcej danych niż wymaganej przez klienta.
* Niepotrzebnie Połącz modelu domeny wewnętrznej aplikacji przy użyciu publicznie ujawnionych interfejsu API.

Mapowanie między domeny jednostek i typy zwracane do klienta mogą być wykonywane:

* Ręcznie za pomocą LINQ `Select`, jak przykładowa aplikacja używa. Aby uzyskać więcej informacji, zobacz [LINQ (Language Integrated Query)](/dotnet/standard/using-linq).
* Automatycznie za pomocą biblioteki takie jak [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Następnie Przykładowa aplikacja pokazuje testów jednostkowych dla `Create` i `ForSession` metody interfejsu API kontrolera pomysły.

Przykładowa aplikacja zawiera dwa `ForSession` testów. Określa pierwszy test, jeśli `ForSession` zwraca <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (nie znaleziono HTTP) na nieprawidłową sesję:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

Drugi `ForSession` testu określa, czy `ForSession` zwraca listę pomysłów sesji (`<List<IdeaDTO>>`) dla prawidłowej sesji. Kontrole również przyjrzeć się pierwszy pomysł, aby potwierdzić jego `Name` właściwość jest prawidłowa:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

Aby przetestować działanie `Create` metody podczas `ModelState` jest nieprawidłowa i przykładowa aplikacja dodaje błąd modelu do kontrolera jako część testu. Nie należy próbować testów sprawdzania poprawności modelu lub model powiązania w testach jednostkowych&mdash;testować zachowanie metody akcji, gdy do czynienia z nieprawidłowym `ModelState`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

Drugi test `Create` zależy od repozytorium, zwracając `null`, więc makiety repozytorium jest skonfigurowany do zwrócenia `null`. Nie ma potrzeby tworzenia bazy danych testowych (w pamięci lub w inny sposób) i konstruowania zapytania, które zwraca wynik. Test może się odbywać w pojedynczej instrukcji, tak jak pokazano w przykładowym kodzie:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

Trzeci `Create` przetestować, `Create_ReturnsNewlyCreatedIdeaForSession`, sprawdza, czy z repozytorium `UpdateAsync` metoda jest wywoływana. Projekt jest wywoływana z `Verifiable`, a repozytorium, pozorowane `Verify` metoda jest wywoływana, aby upewnić się, możliwe do zweryfikowania metoda jest wykonywana. Nie jest obowiązkiem testu jednostkowego, upewnij się, że `UpdateAsync` metody zapisane dane&mdash;mogą być wykonywane przy użyciu wiąże się test integracji.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

::: moniker range=">= aspnetcore-2.1"

## <a name="test-actionresultlttgt"></a>Testowanie ActionResult&lt;T&gt;

W programie ASP.NET Core 2.1 lub nowszej [ActionResult&lt;T&gt; ](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult`1>) pozwala na zwracany typ pochodząca od `ActionResult` lub je zwracają określonego typu.

Przykładowa aplikacja zawiera metodę, która zwraca `List<IdeaDTO>` dla danej sesji `id`. Jeśli sesja `id` nie istnieje, ten kontroler zwraca <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

Testuje dwa z `ForSessionActionResult` są objęte kontrolerze `ApiIdeasControllerTests`.

Pierwszy test potwierdza, że ten kontroler zwraca `ActionResult` , ale nie nieistniejącej listę pomysłów dla nieistniejącego sesji `id`:

* `ActionResult` Typ jest `ActionResult<List<IdeaDTO>>`.
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> Jest <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

Aby uzyskać prawidłową sesję `id`, drugi test potwierdza, że metoda zwraca:

* `ActionResult` z `List<IdeaDTO>` typu.
* [ActionResult&lt;T&gt;. Wartość](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) jest `List<IdeaDTO>` typu.
* Pierwszy element na liście jest prawidłowy dopasowania pomysł przechowywanych w sesji makiety (można uzyskać przez wywołanie `GetTestSession`).

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

Przykładowa aplikacja zawiera również metodę, aby utworzyć nową `Idea` dla danej sesji. Ten kontroler zwraca:

* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> nieprawidłowa w przypadku modelu.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> Jeśli sesja nie istnieje.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> gdy sesja jest aktualizowana nowy pomysł.

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

Trzy testy z `CreateActionResult` znajdują się w `ApiIdeasControllerTests`.

Pierwszy tekst potwierdza, że <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> zwracany jest nieprawidłowy w przypadku modelu.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

Drugi test sprawdza, czy <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> jest zwracany, jeśli sesja nie istnieje.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

Aby uzyskać prawidłową sesję `id`, test końcowy sprawdzający potwierdza, że:

* Metoda ta zwraca `ActionResult` z `BrainstormSession` typu.
* [ActionResult&lt;T&gt;. Wynik](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*) jest <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult` jest odpowiednikiem *201 utworzono* odpowiedzi z `Location` nagłówka.
* [ActionResult&lt;T&gt;. Wartość](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) jest `BrainstormSession` typu.
* Makiety wywołania do zaktualizowania sesji, `UpdateAsync(testSession)`, została wywołana. `Verifiable` Wywołania metody jest sprawdzana przez wykonywanie `mockRepo.Verify()` w potwierdzenia.
* Dwa `Idea` obiekty są zwracane w sesji.
* Ostatni element ( `Idea` dodawany przez wywołanie makiety `UpdateAsync`) dopasowuje `newIdea` dodane do sesji w teście.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/integration-tests>
* [Tworzenie i Uruchamianie testów jednostkowych za pomocą programu Visual Studio](/visualstudio/test/unit-test-your-code).
* [Zasada jawne zależności](https://deviq.com/explicit-dependencies-principle/)
