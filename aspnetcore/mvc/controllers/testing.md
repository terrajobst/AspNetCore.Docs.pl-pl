---
title: Logika kontrolera testów w ASP.NET Core
author: ardalis
description: Dowiedz się, jak testować logikę kontrolera w ASP.NET Core z MOQ i xUnit.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: mvc/controllers/testing
ms.openlocfilehash: 7f4fcb1a5d6e9959c751ebe24e41b39ee05a5819
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799500"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Logika kontrolera testów w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

::: moniker range=">= aspnetcore-3.0"

[Kontrolery](xref:mvc/controllers/actions) odgrywają centralną rolę w dowolnej aplikacji ASP.NET Core MVC. W związku z tym należy mieć pewność, że kontrolery zachowują się zgodnie z oczekiwaniami. Testy automatyczne mogą wykrywać błędy, zanim aplikacja zostanie wdrożona w środowisku produkcyjnym.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/testing/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Testy jednostkowe logiki kontrolera

[Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) obejmują testowanie części aplikacji w izolacji z jej infrastruktury i zależności. Podczas logiki kontrolera testów jednostkowych tylko zawartość pojedynczej akcji jest testowana, a nie zachowanie jej zależności lub samego środowiska.

Skonfiguruj testy jednostkowe akcji kontrolera, aby skoncentrować się na zachowaniu kontrolera. Test jednostkowy kontrolera pozwala uniknąć scenariuszy, takich jak [filtry](xref:mvc/controllers/filters), [Routing](xref:fundamentals/routing)i [powiązania modelu](xref:mvc/models/model-binding). Testy, które obejmują interakcje między składnikami, które zbiorczo odpowiadają na żądanie, są obsługiwane przez *testy integracji*. Aby uzyskać więcej informacji na temat testów integracji, zobacz <xref:test/integration-tests>.

Jeśli piszesz filtry niestandardowe i trasy, przetestuj je jednostkowo, a nie jako część testów dla konkretnej akcji kontrolera.

Aby zademonstrować testy jednostkowe kontrolera, przejrzyj następujący kontroler w przykładowej aplikacji. Kontroler Home wyświetla listę sesji burzy mózgów i umożliwia tworzenie nowych sesji burzy mózgów przy użyciu żądania POST:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

Poprzedni kontroler:

* Zgodnie z [zasadą jawnych zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).
* Oczekuje [iniekcji zależności (di)](xref:fundamentals/dependency-injection) , aby zapewnić wystąpienie `IBrainstormSessionRepository`.
* Program może być testowany `IBrainstormSessionRepository` z [ZaMoqą](https://www.nuget.org/packages/Moq/)usługą za pomocą struktury obiektów makiety, na przykład na przykład. *Obiekt imitacji* jest obiektem prefabrykowanym ze wstępnie określonym zestawem zachowań właściwości i metod używanych do testowania. Aby uzyskać więcej informacji, zobacz [wprowadzenie do testów integracji](xref:test/integration-tests#introduction-to-integration-tests).

Metoda `HTTP GET Index` nie ma pętli ani rozgałęziania i wywołuje tylko jedną metodę. Test jednostkowy dla tej akcji:

* Program umożliwia imitację usługi `IBrainstormSessionRepository` przy użyciu metody `GetTestSessions`. `GetTestSessions` tworzy dwie sesje z burzą mózgów z datami i nazwami sesji.
* Wykonuje metodę `Index`.
* Wykonuje potwierdzenia w wyniku zwróconym przez metodę:
  * Zostanie zwrócona <xref:Microsoft.AspNetCore.Mvc.ViewResult>.
  * [ViewDataDictionary. model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) to `StormSessionViewModel`.
  * W `ViewDataDictionary.Model`są przechowywane dwie sesje burzy mózgów.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

Testy metody `HTTP POST Index` kontrolera macierzystego sprawdzają, czy:

* Gdy [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) jest `false`, Metoda akcji zwraca *400 Nieprawidłowe żądanie* <xref:Microsoft.AspNetCore.Mvc.ViewResult> z odpowiednimi danymi.
* Gdy `ModelState.IsValid` jest `true`:
  * Metoda `Add` w repozytorium jest wywoływana.
  * <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> jest zwracana z prawidłowymi argumentami.

Nieprawidłowy stan modelu jest testowany przez dodanie błędów przy użyciu <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>, jak pokazano w pierwszym teście poniżej:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

Gdy [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) jest nieprawidłowy, ten sam `ViewResult` jest zwracany jako dla żądania GET. Test nie próbuje przekazać nieprawidłowego modelu. Przekazywanie nieprawidłowego modelu nie jest prawidłowym podejściem, ponieważ powiązanie modelu nie jest uruchomione (mimo że [test integracji](xref:test/integration-tests) używa powiązania modelu). W takim przypadku powiązanie modelu nie jest testowane. Te testy jednostkowe służą wyłącznie do testowania kodu w metodzie akcji.

Drugi test weryfikuje, czy `ModelState` jest prawidłowy:

* Zostanie dodany nowy `BrainstormSession` (za pośrednictwem repozytorium).
* Metoda zwraca `RedirectToActionResult` z oczekiwanymi właściwościami.

Wywołane wywołania, które nie są wywoływane, są zwykle ignorowane, ale wywołanie `Verifiable` na końcu wywołania Instalatora pozwala na weryfikację makiety w teście. Jest to wykonywane z wywołaniem do `mockRepo.Verify`, które kończy się niepowodzeniem, jeśli oczekiwana Metoda nie została wywołana.

> [!NOTE]
> Biblioteka MOQ używana w tym przykładzie umożliwia mieszanie możliwej do zweryfikowania lub "ścisłych", imitacje z niemożliwymi do zweryfikowania makietami (nazywanymi również "luźnymi" fragmentami lub wycinkami). Dowiedz się więcej o [dostosowywaniu zachowań makiety za pomocą MOQ](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

[SessionController](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) w aplikacji przykładowej wyświetla informacje powiązane z określoną sesją burzy mózgów. Kontroler zawiera logikę do postępowania z nieprawidłowymi wartościami `id` (Istnieją dwa `return` scenariusze w poniższym przykładzie, aby pokryć te scenariusze). Końcowa instrukcja `return` zwraca nowy `StormSessionViewModel` do widoku (*controllers/SessionController. cs*):

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

Testy jednostkowe obejmują jeden test dla każdego scenariusza `return` w `Index` akcji kontrolera sesji:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

Po przeniesieniu do kontrolera pomysłów aplikacja udostępnia funkcje jako interfejs API sieci Web na trasie `api/ideas`:

* Lista pomysłów (`IdeaDTO`) skojarzonych z sesją burzy mózgów jest zwracana przez metodę `ForSession`.
* Metoda `Create` dodaje nowe pomysły do sesji.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

Unikaj zwracania jednostek domeny biznesowej bezpośrednio za pośrednictwem wywołań interfejsu API. Jednostki domeny:

* Często zawierają więcej danych niż jest to wymagane przez klienta.
* Niepotrzebne połączenie wewnętrznego modelu domeny aplikacji z publicznie uwidocznionym interfejsem API.

Można wykonać mapowanie między jednostkami domeny a typami zwracanymi do klienta:

* Ręczne korzystanie z programu LINQ `Select`jako aplikacji przykładowej. Aby uzyskać więcej informacji, zobacz [LINQ (Language Integrated Query)](/dotnet/standard/using-linq).
* Automatycznie z biblioteką, taką jak [automapowania](https://github.com/AutoMapper/AutoMapper).

Następnie w przykładowej aplikacji przedstawiono testy jednostkowe dla `Create` i `ForSession` metod interfejsu API kontrolera pomysłów.

Przykładowa aplikacja zawiera dwa `ForSession` testy. Pierwszy test określa, czy `ForSession` zwraca <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (nie znaleziono protokołu HTTP) dla nieprawidłowej sesji:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

Drugi test `ForSession` określa, czy `ForSession` zwraca listę pomysłów sesji (`<List<IdeaDTO>>`) dla prawidłowej sesji. Sprawdzenia również sprawdzają pierwszy pomysł, aby potwierdzić, że jego właściwość `Name` jest poprawna:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

Aby przetestować zachowanie metody `Create`, gdy `ModelState` jest nieprawidłowa, przykładowa aplikacja dodaje błąd modelu do kontrolera w ramach testu. Nie próbuj testować walidacji modelu lub powiązania modelu w testach jednostkowych,&mdash;tylko przetestować zachowanie metody akcji, gdy wystąpił nieprawidłowy `ModelState`:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

Drugi test `Create` zależy od repozytorium zwracającego `null`, więc repozytorium makiety jest skonfigurowane do zwracania `null`. Nie ma potrzeby tworzenia testowej bazy danych (w pamięci lub w inny sposób) i konstruowania zapytania zwracającego ten wynik. Test można wykonać w pojedynczej instrukcji, jak przykładowy kod ilustruje:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

Trzeci `Create` test, `Create_ReturnsNewlyCreatedIdeaForSession`, sprawdza, czy metoda `UpdateAsync` repozytorium jest wywoływana. Makieta jest wywoływana z `Verifiable`, a metoda `Verify`nego repozytorium jest wywoływana w celu potwierdzenia, że metoda możliwa do zweryfikowania jest wykonywana. To nie jest odpowiedzialność za testy jednostkowe, aby upewnić się, że metoda `UpdateAsync` zapisała&mdash;danych, które można wykonać przy użyciu testu integracji.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

## <a name="test-actionresultt"></a>Test ActionResult\<T >

W ASP.NET Core 2,1 lub nowszej [ActionResult\<t >](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult%601>) umożliwia zwrócenie typu pochodnego od `ActionResult` lub zwrócenie określonego typu.

Przykładowa aplikacja zawiera metodę, która zwraca `List<IdeaDTO>` dla danej sesji `id`. Jeśli sesja `id` nie istnieje, kontroler zwraca <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

W `ApiIdeasControllerTests`znajdują się dwa testy kontrolera `ForSessionActionResult`.

Pierwszy test potwierdza, że kontroler zwraca `ActionResult` ale nie nieistniejącą listę pomysłów dla nieistniejącej sesji `id`:

* Typ `ActionResult` jest `ActionResult<List<IdeaDTO>>`.
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> jest <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

W przypadku prawidłowej sesji `id`drugi test potwierdza, że metoda zwraca:

* `ActionResult` z typem `List<IdeaDTO>`.
* [ActionResult\<t >. Wartość](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) jest typu `List<IdeaDTO>`.
* Pierwszy element na liście jest prawidłowym pomysłem pasującym do pomysłu przechowywanego w sesji makiety (uzyskany przez wywołanie `GetTestSession`).

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

Przykładowa aplikacja zawiera również metodę tworzenia nowego `Idea` dla danej sesji. Kontroler zwraca:

* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> nieprawidłowy model.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>, jeśli sesja nie istnieje.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>, gdy sesja zostanie zaktualizowana przy użyciu nowego pomysłu.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

W `ApiIdeasControllerTests`znajdują się trzy testy `CreateActionResult`.

Pierwszy tekst potwierdza, że dla nieprawidłowego modelu jest zwracany <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

Drugi test sprawdza, czy <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> jest zwracana, jeśli sesja nie istnieje.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

W przypadku prawidłowej sesji `id`test końcowy potwierdza, że:

* Metoda zwraca `ActionResult` z typem `BrainstormSession`.
* [ActionResult\<t >. Wynikiem](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Result*) jest <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult` jest analogiczny do *201 utworzonej* odpowiedzi z nagłówkiem `Location`.
* [ActionResult\<t >. Wartość](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) jest typu `BrainstormSession`.
* Wywołano wywołanie makiety w celu zaktualizowania sesji, `UpdateAsync(testSession)`. Wywołanie metody `Verifiable` jest sprawdzane przez wykonywanie `mockRepo.Verify()` w potwierdzeniach.
* Dla sesji są zwracane dwa obiekty `Idea`.
* Ostatni element (`Idea` dodany przez wywołanie makiety do `UpdateAsync`) dopasowuje `newIdea` dodany do sesji w teście.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Kontrolery](xref:mvc/controllers/actions) odgrywają centralną rolę w dowolnej aplikacji ASP.NET Core MVC. W związku z tym należy mieć pewność, że kontrolery zachowują się zgodnie z oczekiwaniami. Testy automatyczne mogą wykrywać błędy, zanim aplikacja zostanie wdrożona w środowisku produkcyjnym.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/testing/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Testy jednostkowe logiki kontrolera

[Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) obejmują testowanie części aplikacji w izolacji z jej infrastruktury i zależności. Podczas logiki kontrolera testów jednostkowych tylko zawartość pojedynczej akcji jest testowana, a nie zachowanie jej zależności lub samego środowiska.

Skonfiguruj testy jednostkowe akcji kontrolera, aby skoncentrować się na zachowaniu kontrolera. Test jednostkowy kontrolera pozwala uniknąć scenariuszy, takich jak [filtry](xref:mvc/controllers/filters), [Routing](xref:fundamentals/routing)i [powiązania modelu](xref:mvc/models/model-binding). Testy, które obejmują interakcje między składnikami, które zbiorczo odpowiadają na żądanie, są obsługiwane przez *testy integracji*. Aby uzyskać więcej informacji na temat testów integracji, zobacz <xref:test/integration-tests>.

Jeśli piszesz filtry niestandardowe i trasy, przetestuj je jednostkowo, a nie jako część testów dla konkretnej akcji kontrolera.

Aby zademonstrować testy jednostkowe kontrolera, przejrzyj następujący kontroler w przykładowej aplikacji. Kontroler Home wyświetla listę sesji burzy mózgów i umożliwia tworzenie nowych sesji burzy mózgów przy użyciu żądania POST:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

Poprzedni kontroler:

* Zgodnie z [zasadą jawnych zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).
* Oczekuje [iniekcji zależności (di)](xref:fundamentals/dependency-injection) , aby zapewnić wystąpienie `IBrainstormSessionRepository`.
* Program może być testowany `IBrainstormSessionRepository` z [ZaMoqą](https://www.nuget.org/packages/Moq/)usługą za pomocą struktury obiektów makiety, na przykład na przykład. *Obiekt imitacji* jest obiektem prefabrykowanym ze wstępnie określonym zestawem zachowań właściwości i metod używanych do testowania. Aby uzyskać więcej informacji, zobacz [wprowadzenie do testów integracji](xref:test/integration-tests#introduction-to-integration-tests).

Metoda `HTTP GET Index` nie ma pętli ani rozgałęziania i wywołuje tylko jedną metodę. Test jednostkowy dla tej akcji:

* Program umożliwia imitację usługi `IBrainstormSessionRepository` przy użyciu metody `GetTestSessions`. `GetTestSessions` tworzy dwie sesje z burzą mózgów z datami i nazwami sesji.
* Wykonuje metodę `Index`.
* Wykonuje potwierdzenia w wyniku zwróconym przez metodę:
  * Zostanie zwrócona <xref:Microsoft.AspNetCore.Mvc.ViewResult>.
  * [ViewDataDictionary. model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) to `StormSessionViewModel`.
  * W `ViewDataDictionary.Model`są przechowywane dwie sesje burzy mózgów.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

Testy metody `HTTP POST Index` kontrolera macierzystego sprawdzają, czy:

* Gdy [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) jest `false`, Metoda akcji zwraca *400 Nieprawidłowe żądanie* <xref:Microsoft.AspNetCore.Mvc.ViewResult> z odpowiednimi danymi.
* Gdy `ModelState.IsValid` jest `true`:
  * Metoda `Add` w repozytorium jest wywoływana.
  * <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> jest zwracana z prawidłowymi argumentami.

Nieprawidłowy stan modelu jest testowany przez dodanie błędów przy użyciu <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>, jak pokazano w pierwszym teście poniżej:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

Gdy [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) jest nieprawidłowy, ten sam `ViewResult` jest zwracany jako dla żądania GET. Test nie próbuje przekazać nieprawidłowego modelu. Przekazywanie nieprawidłowego modelu nie jest prawidłowym podejściem, ponieważ powiązanie modelu nie jest uruchomione (mimo że [test integracji](xref:test/integration-tests) używa powiązania modelu). W takim przypadku powiązanie modelu nie jest testowane. Te testy jednostkowe służą wyłącznie do testowania kodu w metodzie akcji.

Drugi test weryfikuje, czy `ModelState` jest prawidłowy:

* Zostanie dodany nowy `BrainstormSession` (za pośrednictwem repozytorium).
* Metoda zwraca `RedirectToActionResult` z oczekiwanymi właściwościami.

Wywołane wywołania, które nie są wywoływane, są zwykle ignorowane, ale wywołanie `Verifiable` na końcu wywołania Instalatora pozwala na weryfikację makiety w teście. Jest to wykonywane z wywołaniem do `mockRepo.Verify`, które kończy się niepowodzeniem, jeśli oczekiwana Metoda nie została wywołana.

> [!NOTE]
> Biblioteka MOQ używana w tym przykładzie umożliwia mieszanie możliwej do zweryfikowania lub "ścisłych", imitacje z niemożliwymi do zweryfikowania makietami (nazywanymi również "luźnymi" fragmentami lub wycinkami). Dowiedz się więcej o [dostosowywaniu zachowań makiety za pomocą MOQ](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

[SessionController](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) w aplikacji przykładowej wyświetla informacje powiązane z określoną sesją burzy mózgów. Kontroler zawiera logikę do postępowania z nieprawidłowymi wartościami `id` (Istnieją dwa `return` scenariusze w poniższym przykładzie, aby pokryć te scenariusze). Końcowa instrukcja `return` zwraca nowy `StormSessionViewModel` do widoku (*controllers/SessionController. cs*):

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

Testy jednostkowe obejmują jeden test dla każdego scenariusza `return` w `Index` akcji kontrolera sesji:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

Po przeniesieniu do kontrolera pomysłów aplikacja udostępnia funkcje jako interfejs API sieci Web na trasie `api/ideas`:

* Lista pomysłów (`IdeaDTO`) skojarzonych z sesją burzy mózgów jest zwracana przez metodę `ForSession`.
* Metoda `Create` dodaje nowe pomysły do sesji.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

Unikaj zwracania jednostek domeny biznesowej bezpośrednio za pośrednictwem wywołań interfejsu API. Jednostki domeny:

* Często zawierają więcej danych niż jest to wymagane przez klienta.
* Niepotrzebne połączenie wewnętrznego modelu domeny aplikacji z publicznie uwidocznionym interfejsem API.

Można wykonać mapowanie między jednostkami domeny a typami zwracanymi do klienta:

* Ręczne korzystanie z programu LINQ `Select`jako aplikacji przykładowej. Aby uzyskać więcej informacji, zobacz [LINQ (Language Integrated Query)](/dotnet/standard/using-linq).
* Automatycznie z biblioteką, taką jak [automapowania](https://github.com/AutoMapper/AutoMapper).

Następnie w przykładowej aplikacji przedstawiono testy jednostkowe dla `Create` i `ForSession` metod interfejsu API kontrolera pomysłów.

Przykładowa aplikacja zawiera dwa `ForSession` testy. Pierwszy test określa, czy `ForSession` zwraca <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (nie znaleziono protokołu HTTP) dla nieprawidłowej sesji:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

Drugi test `ForSession` określa, czy `ForSession` zwraca listę pomysłów sesji (`<List<IdeaDTO>>`) dla prawidłowej sesji. Sprawdzenia również sprawdzają pierwszy pomysł, aby potwierdzić, że jego właściwość `Name` jest poprawna:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

Aby przetestować zachowanie metody `Create`, gdy `ModelState` jest nieprawidłowa, przykładowa aplikacja dodaje błąd modelu do kontrolera w ramach testu. Nie próbuj testować walidacji modelu lub powiązania modelu w testach jednostkowych,&mdash;tylko przetestować zachowanie metody akcji, gdy wystąpił nieprawidłowy `ModelState`:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

Drugi test `Create` zależy od repozytorium zwracającego `null`, więc repozytorium makiety jest skonfigurowane do zwracania `null`. Nie ma potrzeby tworzenia testowej bazy danych (w pamięci lub w inny sposób) i konstruowania zapytania zwracającego ten wynik. Test można wykonać w pojedynczej instrukcji, jak przykładowy kod ilustruje:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

Trzeci `Create` test, `Create_ReturnsNewlyCreatedIdeaForSession`, sprawdza, czy metoda `UpdateAsync` repozytorium jest wywoływana. Makieta jest wywoływana z `Verifiable`, a metoda `Verify`nego repozytorium jest wywoływana w celu potwierdzenia, że metoda możliwa do zweryfikowania jest wykonywana. To nie jest odpowiedzialność za testy jednostkowe, aby upewnić się, że metoda `UpdateAsync` zapisała&mdash;danych, które można wykonać przy użyciu testu integracji.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

## <a name="test-actionresultt"></a>Test ActionResult\<T >

W ASP.NET Core 2,1 lub nowszej [ActionResult\<t >](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult%601>) umożliwia zwrócenie typu pochodnego od `ActionResult` lub zwrócenie określonego typu.

Przykładowa aplikacja zawiera metodę, która zwraca `List<IdeaDTO>` dla danej sesji `id`. Jeśli sesja `id` nie istnieje, kontroler zwraca <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

W `ApiIdeasControllerTests`znajdują się dwa testy kontrolera `ForSessionActionResult`.

Pierwszy test potwierdza, że kontroler zwraca `ActionResult` ale nie nieistniejącą listę pomysłów dla nieistniejącej sesji `id`:

* Typ `ActionResult` jest `ActionResult<List<IdeaDTO>>`.
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> jest <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

W przypadku prawidłowej sesji `id`drugi test potwierdza, że metoda zwraca:

* `ActionResult` z typem `List<IdeaDTO>`.
* [ActionResult\<t >. Wartość](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) jest typu `List<IdeaDTO>`.
* Pierwszy element na liście jest prawidłowym pomysłem pasującym do pomysłu przechowywanego w sesji makiety (uzyskany przez wywołanie `GetTestSession`).

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

Przykładowa aplikacja zawiera również metodę tworzenia nowego `Idea` dla danej sesji. Kontroler zwraca:

* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> nieprawidłowy model.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>, jeśli sesja nie istnieje.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>, gdy sesja zostanie zaktualizowana przy użyciu nowego pomysłu.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

W `ApiIdeasControllerTests`znajdują się trzy testy `CreateActionResult`.

Pierwszy tekst potwierdza, że dla nieprawidłowego modelu jest zwracany <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

Drugi test sprawdza, czy <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> jest zwracana, jeśli sesja nie istnieje.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

W przypadku prawidłowej sesji `id`test końcowy potwierdza, że:

* Metoda zwraca `ActionResult` z typem `BrainstormSession`.
* [ActionResult\<t >. Wynikiem](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Result*) jest <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult` jest analogiczny do *201 utworzonej* odpowiedzi z nagłówkiem `Location`.
* [ActionResult\<t >. Wartość](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) jest typu `BrainstormSession`.
* Wywołano wywołanie makiety w celu zaktualizowania sesji, `UpdateAsync(testSession)`. Wywołanie metody `Verifiable` jest sprawdzane przez wykonywanie `mockRepo.Verify()` w potwierdzeniach.
* Dla sesji są zwracane dwa obiekty `Idea`.
* Ostatni element (`Idea` dodany przez wywołanie makiety do `UpdateAsync`) dopasowuje `newIdea` dodany do sesji w teście.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/integration-tests>
* [Tworzenie i uruchamianie testów jednostkowych za pomocą programu Visual Studio](/visualstudio/test/unit-test-your-code)
* [Przetestowana Biblioteka testowania AspNetCore. MVC-Fluent dla ASP.NET Core Mvc](https://github.com/ivaylokenov/MyTested.AspNetCore.Mvc) &ndash; Biblioteka testów jednostkowych z silną typem, zapewniająca interfejs Fluent do testowania aplikacji MVC i Web API. (*Niekonserwowane lub obsługiwane przez firmę Microsoft).*

