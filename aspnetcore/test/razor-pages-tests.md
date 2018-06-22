---
title: Testy jednostkowe stron razor w ASP.NET Core
author: guardrex
description: Informacje o sposobie tworzenia testów jednostkowych dla aplikacji stron Razor.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: bde1bef78fcc7ac1d570057d54636ea0f5490de8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274410"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Testy jednostkowe stron razor w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Platformy ASP.NET Core obsługuje testów jednostkowych aplikacji stron Razor. Testy danych uzyskać dostępu do warstwy (DAL) i zapewnienia modeli strony:

* Części aplikacji stron Razor działają niezależnie i jednocześnie jako jednostka podczas konstruowania aplikacji.
* Klasy i metody mają ograniczony zakres odpowiedzialności.
* Dodatkową dokumentację istnieje na zachowanie aplikacji.
* Znaleziono regresji, które błędów spowodowanych aktualizacji do kodu podczas automatycznego tworzenia i wdrażania.

W tym temacie założono, że masz podstawową wiedzę na temat stron Razor aplikacji i testów jednostkowych. Jeśli znasz stron Razor aplikacji lub badanie koncepcji, zobacz następujące tematy:

* [Wprowadzenie do stron Razor](xref:razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Badanie C# .NET Core za pomocą testu dotnet i xUnit jednostki](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowy projekt składa się z dwóch aplikacji:

| Aplikacji         | Folder projektu                        | Opis |
| ----------- | ------------------------------------- | ----------- |
| Komunikat aplikacji | *src/RazorPagesTestSample*            | Zezwala użytkownikowi na dodawanie, Usuń jedno, Usuń wszystkie i analizowania wiadomości. |
| Przetestuj aplikację    | *tests/RazorPagesTestSample.Tests*    | Używane do testu jednostkowego aplikacji wiadomości: warstwy (DAL) i indeksu strony modelu dostępu do danych. |

Testy można uruchomić przy użyciu funkcji wbudowanych testów IDE, takich jak [programu Visual Studio](https://www.visualstudio.com/vs/). Jeśli przy użyciu [Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia, uruchom następujące polecenie w wierszu polecenia *tests/RazorPagesTestSample.Tests* folderu:

```console
dotnet test
```

## <a name="message-app-organization"></a>Komunikat aplikacji organizacji

Aplikacja wiadomości jest prosty system komunikatów stron Razor o następującej charakterystyce:

* Strona indeksu aplikacji (*Pages/Index.cshtml* i *Pages/Index.cshtml.cs*) zapewnia interfejsu użytkownika i strony metody modelu kontroli Dodawanie, usuwanie i analizy wiadomości (średni słów na wiadomości) .
* Komunikat jest opisane przez `Message` klasy (*Data/Message.cs*) z dwóch właściwości: `Id` (klucz) i `Text` (komunikat). `Text` Właściwość jest wymagana i maksymalnie 200 znaków.
* Wiadomości są przechowywane przy użyciu [bazy danych programu Entity Framework w pamięci](/ef/core/providers/in-memory/)&#8224;.
* Ta aplikacja zawiera Warstwa dostępu do danych (DAL) w swojej klasie kontekst bazy danych `AppDbContext` (*Data/AppDbContext.cs*). Metody DAL są oznaczone `virtual`, dzięki czemu mocking metod do użycia w testach.
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, Magazyn komunikatu jest inicjowany z trzech wiadomości. Te *rozpoczęta wiadomości* są również używane w testach.

&#8224;Temat EF [testu z InMemory](/ef/core/miscellaneous/testing/in-memory), opisano sposób korzystania z bazy danych w pamięci dla testów z użyciem MSTest. W tym temacie używa [xUnit](https://xunit.github.io/) struktury testowej. Badanie koncepcji i implementacje testu w innym testem struktury są podobne, lecz nie są identyczne.

Mimo że nie korzysta z aplikacji [wzorca repozytorium](http://martinfowler.com/eaaCatalog/repository.html) i nie jest skuteczne przykład [wzorzec jednostki pracy (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), stron Razor obsługuje te wzorce programowania. Aby uzyskać więcej informacji, zobacz [projektowania infrastruktury warstwę trwałości](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [wdrażanie repozytorium i jednostki pracy wzorce w aplikacji platformy ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), i [kontrolera testów Logika](/aspnet/core/mvc/controllers/testing) (próbki implementuje wzorca repozytorium).

## <a name="test-app-organization"></a>Testowanie aplikacji organizacji

Test jest to aplikacja konsoli wewnątrz *tests/RazorPagesTestSample.Tests* folderu.

| Folder aplikacji testu | Opis |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* zawiera testy jednostkowe dla warstwy DAL.</li><li>*IndexPageTests.cs* zawiera testy jednostkowe dla modelu strony indeksu.</li></ul> |
| *Narzędzia*     | Zawiera `TestingDbContextOptions` metodę używaną do tworzenia nowej bazy danych opcji kontekstu dla każdego testu jednostkowego DAL tak, aby bazy danych zostanie zresetowana do stanu linii bazowej dla każdego z testów. |

Struktury testowej jest [xUnit](https://xunit.github.io/). Obiekt mocking framework [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Testy jednostkowe danych uzyskać dostępu do warstwy (DAL)

Aplikacja wiadomości ma warstwy DAL z czterech metod zawartych w `AppDbContext` klasy (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Każda metoda ma co najmniej dwóch testów jednostkowych w aplikacji testu.

| Warstwa DAL — metoda               | Funkcja                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Uzyskuje `List<Message>` z bazy danych, posortowane według `Text` właściwości. |
| `AddMessageAsync`        | Dodaje `Message` do bazy danych.                                          |
| `DeleteAllMessagesAsync` | Usuwa wszystkie `Message` wpisy z bazy danych.                           |
| `DeleteMessageAsync`     | Usuwa pojedynczy `Message` z bazy danych przez `Id`.                      |

Testy jednostkowe warstwy dal wymagają [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) podczas tworzenia nowego `AppDbContext` dla każdego z testów. Jeden ze sposobów tworzenia `DbContextOptions` dla każdego z testów jest użycie [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Problem z tym podejściem jest, że każdy test odbiera bazy danych w dowolnie wybrany stan poprzedni test, że pozostała. Może to być problemem, podczas próby zapisania atomic testów, które nie koliduje ze sobą. Aby wymusić `AppDbContext` do użycia nowego kontekstu bazy danych dla każdego z testów, należy podać `DbContextOptions` wystąpienia, która jest oparta na nowego dostawcę usługi. Przetestuj aplikację pokazano, jak to zrobić przy użyciu jego `Utilities` metoda klasy `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Przy użyciu `DbContextOptions` w jednostce DAL testy umożliwia każdego z testów do uruchomienia automatycznie przy użyciu wystąpienia Nowa baza danych:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

W każdej metody `DataAccessLayerTest` klasy (*UnitTests/DataAccessLayerTest.cs*) jest zgodna ze wzorcem podobne Assert Act Rozmieść:

1. Rozmieść: Baza danych jest skonfigurowana dla testu i/lub oczekiwany wynik jest zdefiniowany.
1. Działanie: Test jest wykonywany.
1. Assert: Potwierdzenia odnoszą się do ustalenia, czy wynik testu jest sukcesu.

Na przykład `DeleteMessageAsync` metoda jest odpowiedzialna za pojedynczym komunikacie identyfikowane przez usunięcie jego `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Istnieją dwa testów dla tej metody. Jeden test sprawdza, czy metoda usuwa komunikat, gdy komunikat jest obecna w bazie danych. Inne testy metody, które bazy danych nie powoduje zmiany komunikat `Id` do usunięcia nie istnieje. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Metody jest pokazany poniżej:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Najpierw metoda wykonuje kroku Rozmieść których odbywa się przygotowania kroku Act. Rozmieszczania wiadomości są uzyskiwane i przechowywać w formacie `seedMessages`. Rozmieszczania komunikaty są zapisywane w bazie danych. Wiadomość o `Id` z `1` jest ustawiony do usunięcia. Gdy `DeleteMessageAsync` metoda jest wykonywana, oczekiwane komunikaty powinny mieć wszystkie komunikaty z wyjątkiem z `Id` z `1`. `expectedMessages` To oczekiwany wynik reprezentuje zmienną.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Metoda pełni: `DeleteMessageAsync` metoda jest wykonywana, przekazując `recId` z `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Ponadto metoda uzyskuje `Messages` z kontekstu i porównuje go do `expectedMessages` potwierdzające, że dwa są równe:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Aby porównać dwa `List<Message>` są takie same:

* Komunikaty są uporządkowane według `Id`.
* Pary komunikatów są porównywane `Text` właściwości.

Podobne metody testowej, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` sprawdza wynik próby usunięcia komunikat, który nie istnieje. W takim przypadku oczekiwane komunikaty w bazie danych powinna być równa rzeczywiste wiadomości po `DeleteMessageAsync` metoda jest wykonywana. Nie powinny istnieć nie zmiany do bazy danych zawartości:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Testy jednostkowe metod modelu strony

Inny zestaw testów jednostkowych jest odpowiedzialny za testy metod modelu strony. W aplikacji komunikat modeli strony indeksu znajdują się w `IndexModel` klasy w *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Metoda modelu strony | Funkcja |
| ----------------- | -------- |
| `OnGetAsync` | Pobiera komunikaty z warstwy DAL dla interfejsu użytkownika za pomocą `GetMessagesAsync` metody. |
| `OnPostAddMessageAsync` | Jeśli `ModelState` jest prawidłowy, wywołuje `AddMessageAsync` do dodania komunikatu do bazy danych. |
| `OnPostDeleteAllMessagesAsync` | Wywołania `DeleteAllMessagesAsync` można usunąć wszystkie wiadomości w bazie danych. |
| `OnPostDeleteMessageAsync` | Wykonuje `DeleteMessageAsync` można usunąć wiadomości z `Id` określony. |
| `OnPostAnalyzeMessagesAsync` | Jeśli jeden lub więcej komunikatów znajdują się w bazie danych, oblicza średnią liczbę słów na wiadomość. |

Strona metody modelu są sprawdzane pod siedmiu testów w `IndexPageTests` klasy (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Testy używać znanych wzorca Rozmieść-Assert-Act. Te testy skupić się na:

* Określanie metody wykonaj poprawne zachowanie podczas `ModelState` jest nieprawidłowy.
* Potwierdzenie metod tworzenia poprawny `IActionResult`.
* Sprawdzanie, czy przypisania wartości właściwości są wprowadzone prawidłowo.

Ta grupa testów mock często metody warstwy DAL do uzyskania oczekiwanych danych kroku Act, którym jest przeprowadzana metody modelu strony. Na przykład `GetMessagesAsync` metoda `AppDbContext` jest mocked do generowania danych wyjściowych. Podczas tej metody, metoda modelu strony makiety zwraca wynik. Dane nie pochodzi z bazy danych. Spowoduje to utworzenie warunki przewidywalne, niezawodne testu używający warstwy DAL w testach modelu strony.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Test pokazuje sposób `GetMessagesAsync` metody jest mocked modelu strony:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Gdy `OnGetAsync` metoda jest wykonywana w kroku Act, wywołuje modelu strony `GetMessagesAsync` metody.

Działanie krok testu jednostkowego (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` Strona modelu `OnGetAsync` — metoda (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Metoda w warstwy DAL nie zwraca wyników dla tego wywołania metody. Wersja mocked metody zwraca wynik.

W `Assert` krok, rzeczywista wiadomości (`actualMessages`) są przypisywane z `Messages` właściwości modelu strony. Sprawdzanie typu również jest wykonywane, gdy komunikaty są przypisane. Komunikaty oczekiwanymi i rzeczywistymi są porównywane przez ich `Text` właściwości. Test potwierdza, że dwa `List<Message>` wystąpienia zawierają tej wiadomości.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Inne testy w tej grupie modelu obiektów, które obejmują tworzenie strony `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` ustanowienie `PageContext`, `ViewDataDictionary`, a `PageContext`. Są one przydatne podczas przeprowadzania testów. Na przykład ustanawia aplikacji komunikat `ModelState` błąd `AddModelError` Aby sprawdzić, czy prawidłowy `PageResult` jest zwracany, gdy `OnPostAddMessageAsync` jest wykonywana:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Badanie C# .NET Core za pomocą testu dotnet i xUnit jednostki](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Kontrolery testów](xref:mvc/controllers/testing)
* [Kod testu jednostkowego](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Testy integracji](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Wprowadzenie do korzystania z xUnit.net (Core/ASP.NET .NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq Szybki Start](https://github.com/Moq/moq4/wiki/Quickstart)
