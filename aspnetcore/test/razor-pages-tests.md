---
title: Testy jednostkowe stron razor w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak utworzyć testy jednostkowe dla aplikacji stron Razor.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: 364c4c0fd75954f1c7e0bfbc221938afe5332bde
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755901"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Testy jednostkowe stron razor w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Platforma ASP.NET Core obsługuje testy jednostkowe aplikacji stron Razor. Testy danych dostęp do warstwy (DAL) i strony modelom upewnij się:

* Części aplikacji stron Razor działać niezależnie i ze sobą jako jednostka podczas konstruowania aplikacji.
* Klasy i metody mają ograniczoną zakresów odpowiedzialności.
* Dodatkową dokumentację istnieje na zachowanie aplikacji.
* Regresji, które błędy wynikające z aktualizacji w kodzie, zostały znalezione w czasie zautomatyzowanego kompilowania i wdrażania.

W tym temacie założono, że masz podstawową wiedzę na temat stron Razor aplikacji i testów jednostkowych. Jeśli jesteś zaznajomiony z aplikacji stron Razor i pojęcia, testów, zobacz następujące tematy:

* [Wprowadzenie do stron Razor](xref:razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testowanie jednostek języka C# w .NET Core za pomocą polecenia dotnet test i struktury xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowy projekt składa się z dwóch aplikacji:

| Aplikacja         | Folder projektu                        | Opis |
| ----------- | ------------------------------------- | ----------- |
| Komunikat o aplikacji | *src/RazorPagesTestSample*            | Umożliwia użytkownikowi Dodaj, Usuń jedno, Usuń wszystkie i analizowania komunikatów. |
| Testowanie aplikacji    | *tests/RazorPagesTestSample.Tests*    | Używany do testów jednostkowych aplikacji wiadomości: warstwy (DAL) i indeks strony modelu dostępu do danych. |

Testy mogą być uruchamiane przy użyciu funkcji wbudowanych testu środowisko IDE, takich jak [programu Visual Studio](https://www.visualstudio.com/vs/). Jeśli przy użyciu [programu Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia, uruchom następujące polecenie w wierszu polecenia w *tests/RazorPagesTestSample.Tests* folderu:

```console
dotnet test
```

## <a name="message-app-organization"></a>Komunikat aplikacji organizacji

Aplikacja komunikat jest prosty system komunikatów stron Razor o następującej charakterystyce:

* Strony indeksu aplikacji (*Pages/Index.cshtml* i *Pages/Index.cshtml.cs*) udostępnia interfejs użytkownika i strony metody modelu w celu kontrolowania dodawania, usuwania i analizy wiadomości (średni słowa na komunikat) .
* Komunikat jest opisana przez `Message` klasy (*Data/Message.cs*) za pomocą dwie właściwości: `Id` (klucz) i `Text` (komunikat). `Text` Właściwość jest wymagana i maksymalnie 200 znaków.
* Komunikaty są przechowywane przy użyciu [bazy danych w pamięci programu Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Ta aplikacja zawiera warstwy dostępu do danych (DAL) w swojej klasie kontekst bazy danych `AppDbContext` (*Data/AppDbContext.cs*). Metody DAL są oznaczone `virtual`, co pozwala pozorowanie metody do użycia w testach.
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, Magazyn komunikatu jest inicjowany z trzech wiadomości. Te *zasilany wiadomości* są również używane w testach.

&#8224;Temat EF [testu za pomocą InMemory](/ef/core/miscellaneous/testing/in-memory), wyjaśnia, jak użyć bazy danych w pamięci dla testów w narzędziu MSTest. W tym temacie używany [xUnit](https://xunit.github.io/) struktury testowej. Test pojęć i badanie implementacji różnych środowisk testowych różnych są podobne, ale nie są identyczne.

Mimo że aplikacja nie używa [wzorca repozytorium](xref:fundamentals/repository-pattern) i nie jest skuteczne przykładem [wzorzec jednostki pracy (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), stron Razor obsługuje te wzorce programowania. Aby uzyskać więcej informacji, zobacz [projektowanie warstwy trwałości infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), <xref:fundamentals/repository-pattern>, i [logikę kontrolera testu](/aspnet/core/mvc/controllers/testing) (przykład implementuje wzorzec repozytorium).

## <a name="test-app-organization"></a>Testowanie aplikacji organizacji

Aplikacja testowa jest aplikacją konsoli wewnątrz *tests/RazorPagesTestSample.Tests* folderu.

| Folder aplikacji testowego | Opis |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* zawiera testy jednostkowe dla warstwy DAL.</li><li>*IndexPageTests.cs* zawiera testy jednostkowe dla modelu strony indeksu.</li></ul> |
| *Narzędzia*     | Zawiera `TestingDbContextOptions` metodę używaną do tworzenia nowej bazy danych opcji kontekst dla każdego testu jednostkowego DAL tak, aby baza danych jest resetowany do stanu punktu odniesienia dla każdego testu. |

Framework testów jest [xUnit](https://xunit.github.io/). Obiekt pozorowanie framework jest [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Testy jednostkowe danych dostęp do warstwy (DAL)

Aplikacja komunikatu ma warstwy DAL z czterech metod zawarte w `AppDbContext` klasy (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Każda metoda charakteryzuje się co najmniej dwóch testów jednostkowych w aplikacji testowych.

| Warstwa DAL — metoda               | Funkcja                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Uzyskuje `List<Message>` z bazy danych, posortowane według `Text` właściwości. |
| `AddMessageAsync`        | Dodaje `Message` w bazie danych.                                          |
| `DeleteAllMessagesAsync` | Usuwa wszystkie `Message` wpisy z bazy danych.                           |
| `DeleteMessageAsync`     | Usuwa pojedynczy `Message` z bazy danych przez `Id`.                      |

Testy jednostkowe warstwy dal wymagają [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) podczas tworzenia nowego `AppDbContext` dla każdego testu. Jedno z podejść do tworzenia `DbContextOptions` dla każdego testu jest użycie [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Problem w przypadku tej metody polega na tym, że każdy test odbiera bazy danych w niezależnie od stanu poprzedni test, że pozostała. Może to być problematyczne, przy próbie zapisania testy pojedynczej Atomowej jednostki, które nie kolidują ze sobą. Aby wymusić `AppDbContext` używać nowy kontekst bazy danych dla każdego testu, należy podać `DbContextOptions` wystąpienia, która jest oparta na nowego dostawcę usługi. Aplikacja testowa pokazuje, jak to zrobić za pomocą jego `Utilities` metody klasy `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Za pomocą `DbContextOptions` w jednostce warstwa DAL testów umożliwia każdy test do uruchomienia niepodzielne przy użyciu wystąpienia nową bazą danych:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Każdej metody testowej w `DataAccessLayerTest` klasy (*UnitTests/DataAccessLayerTest.cs*) jest zgodna ze wzorcem podobne Assert Act Rozmieść:

1. Rozmieść: Baza danych jest skonfigurowany do testu i/lub oczekiwany wynik jest zdefiniowana.
1. Ustawa: Test jest wykonywany.
1. Asercja: Potwierdzenia są wprowadzane do ustalenia, czy wynik testu jest sukcesu.

Na przykład `DeleteMessageAsync` metoda jest odpowiedzialna za jeden komunikat o identyfikowane przez usunięcie jego `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Istnieją dwa testy dla tej metody. Jeden test sprawdza, czy metoda wiadomość znajduje się w bazie danych, usuwa komunikat. Inne testy metody, które nie powoduje zmiany bazy danych, jeśli komunikat `Id` do usunięcia nie istnieje. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Metoda znajdują się poniżej:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Po pierwsze metoda wykonuje krok Rozmieść gdzie odbywa się przygotowanie do kroku Act. Uzyskane i przechowywane w rozmieszczania komunikatów `seedMessages`. Rozmieszczania komunikaty są zapisywane w bazie danych. Wiadomość o `Id` z `1` jest ustawiony do usunięcia. Gdy `DeleteMessageAsync` metoda jest wykonywana, oczekiwane komunikaty powinny mieć wszystkie komunikaty z wyjątkiem z `Id` z `1`. `expectedMessages` Zmienna reprezentuje to oczekiwany wynik.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Metoda działa: `DeleteMessageAsync` metoda jest wykonywana, przekazując `recId` z `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Na koniec metody uzyskuje `Messages` z kontekstu i porównuje go do `expectedMessages` potwierdzające, że dwa są takie same:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Aby można było porównać dwa `List<Message>` są takie same:

* Komunikaty są uporządkowane według `Id`.
* Pary komunikatów są porównywane w `Text` właściwości.

Podobne metody testowej, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` sprawdza wynik próby usunięcia komunikatu, który nie istnieje. W tym przypadku oczekiwane komunikaty w bazie danych powinna być równa rzeczywiste wiadomości po `DeleteMessageAsync` metoda jest wykonywana. Powinien istnieć bez zmian do bazy danych zawartości:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Testy jednostkowe metod strony modelu

Inny zestaw testów jednostkowych jest odpowiedzialny za testy metody modelu strony. W aplikacji wiadomość modeli strony indeksu znajdują się w `IndexModel` klasy w *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Metoda korzystająca ze strony modelu | Funkcja |
| ----------------- | -------- |
| `OnGetAsync` | Pobiera komunikaty z warstwy DAL za pomocą interfejsu użytkownika `GetMessagesAsync` metody. |
| `OnPostAddMessageAsync` | Jeśli `ModelState` jest prawidłowy, wywołuje `AddMessageAsync` do dodania komunikatu do bazy danych. |
| `OnPostDeleteAllMessagesAsync` | Wywołania `DeleteAllMessagesAsync` można usunąć wszystkie komunikaty w bazie danych. |
| `OnPostDeleteMessageAsync` | Wykonuje `DeleteMessageAsync` można usunąć wiadomości z `Id` określony. |
| `OnPostAnalyzeMessagesAsync` | Jeśli co najmniej jeden komunikat znajdują się w bazie danych, oblicza średnią liczbę słów na komunikat. |

Metody modelu strony jest testowana przy użyciu siedem testy w `IndexPageTests` klasy (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Testy używać dobrze znanych wzorca Rozmieść-Assert-czynność. Te testy skupić się na:

* Określanie, jeśli te metody postępuj zgodnie z poprawnego zachowania podczas `ModelState` jest nieprawidłowy.
* Potwierdzanie metody utworzenia prawidłowego `IActionResult`.
* Sprawdzanie, czy przypisania wartości właściwości zostały poprawnie wprowadzone.

Ta grupa testy często testowanie metody warstwy DAL do produkcji oczekiwanych danych kroku Act, gdzie jest wykonywana metoda modelu strony. Na przykład `GetMessagesAsync` metody `AppDbContext` jest w postaci makiet do generowania danych wyjściowych. Gdy metoda modelu strony wykonuje tę metodę, pozorny zwraca wynik. Dane nie pochodzi z bazy danych. Spowoduje to utworzenie warunki badania przewidywalne i niezawodne używający warstwy DAL w testach modelu strony.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Testu pokazuje sposób, w jaki `GetMessagesAsync` metoda jest w postaci makiet modelu strony:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Gdy `OnGetAsync` metoda jest wykonywana w kroku Act, wywoływanych przez nią modelu strony `GetMessagesAsync` metody.

Act krok testu jednostkowego (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` model strony `OnGetAsync` — metoda (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` In warstwy DAL metoda nie zwraca wyniku dla tego wywołania metody. Wersja pozorowane metody zwraca wynik.

W `Assert` krok, rzeczywiste wiadomości (`actualMessages`) są przypisywane z `Messages` właściwości modelu strony. Sprawdzanie typu odbywa się również, gdy komunikaty są przypisane. Komunikaty o przewidywanych i rzeczywistych są porównywane według ich `Text` właściwości. Badanie potwierdza, że dwa `List<Message>` wystąpienia zawierają tymi samymi komunikatami.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Inne testy w tej grupie, Utwórz stronę obiekty modelu, które obejmują `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` nawiązać `PageContext`, `ViewDataDictionary`, a `PageContext`. Są one przydatne podczas przeprowadzania testów. Na przykład aplikacja komunikat ustanawia `ModelState` błąd `AddModelError` Aby sprawdzić, czy prawidłowy `PageResult` jest zwracana, gdy `OnPostAddMessageAsync` jest wykonywana:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Testowanie jednostek języka C# w .NET Core za pomocą polecenia dotnet test i struktury xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Kontrolery testów](xref:mvc/controllers/testing)
* [Kod testu jednostkowego](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Testy integracji](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Wprowadzenie do xUnit.net (Core/ASP.NET .NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Przewodnik Szybki Start Moq](https://github.com/Moq/moq4/wiki/Quickstart)
