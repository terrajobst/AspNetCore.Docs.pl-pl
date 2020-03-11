---
title: Testy jednostkowe Razor Pages w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzyć testy jednostkowe dla aplikacji Razor Pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 0e217b6b7f15519a3da44f5d074cf80fa96a3b3a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664410"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Testy jednostkowe Razor Pages w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core obsługuje testy jednostkowe Razor Pages aplikacji. Testy warstwy dostępu do danych (DAL) i modele stron pomagają zapewnić:

* Części aplikacji Razor Pages działają niezależnie i razem jako jednostka podczas konstruowania aplikacji.
* Klasy i metody mają ograniczone zakresy odpowiedzialności.
* Istnieje dodatkowa dokumentacja dotycząca zachowania aplikacji.
* Regresje, które są błędy związane z aktualizacjami kodu, są dostępne podczas zautomatyzowanego kompilowania i wdrażania.

W tym temacie założono, że masz podstawową wiedzę na temat Razor Pages aplikacji i testów jednostkowych. Jeśli nie znasz Razor Pages aplikacji lub testów, zapoznaj się z następującymi tematami:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Testowanie C# jednostkowe w programie .NET Core przy użyciu testu dotnet i xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowy projekt składa się z dwóch aplikacji:

| Aplikacja         | Folder projektu                     | Opis |
| ----------- | ---------------------------------- | ----------- |
| Aplikacja wiadomości | *SRC/RazorPagesTestSample*         | Zezwala użytkownikowi na dodawanie wiadomości, usuwanie jednego komunikatu, usuwanie wszystkich komunikatów i analizowanie komunikatów (Znajdowanie średniej liczby wyrazów na komunikat). |
| Aplikacja testowa    | *testy/RazorPagesTestSample. Tests* | Służy do przetestowania jednostki DAL i indeksu strony aplikacji wiadomości. |

Testy można uruchamiać przy użyciu wbudowanych funkcji testowych środowiska IDE, takich jak [Visual Studio](/visualstudio/test/unit-test-your-code) lub [Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). W przypadku używania [Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia wykonaj następujące polecenie w wierszu polecenia w folderze *Tests/RazorPagesTestSample. Tests* :

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>Organizacja aplikacji komunikatów

Aplikacja wiadomości jest systemem komunikatów Razor Pages o następujących cechach:

* Strona indeks aplikacji (*Pages/index. cshtml* i *Pages/index. cshtml. cs*) zawiera metody interfejsu użytkownika i modelu strony umożliwiające sterowanie dodawaniem, usuwaniem i analizą komunikatów (Znajdź średnią liczbę wyrazów na komunikat).
* Komunikat jest opisywany przez klasę `Message` (*Data/Message. cs*) z dwiema właściwościami: `Id` (Key) i `Text` (Message). Właściwość `Text` jest wymagana i jest ograniczona do 200 znaków.
* Komunikaty są przechowywane przy użyciu&#8224; [bazy danych znajdującej się w pamięci Entity Framework](/ef/core/providers/in-memory/).
* Aplikacja zawiera DAL w swojej klasie kontekstu bazy danych, `AppDbContext` (*Data/AppDbContext. cs*). Metody DAL są oznaczone `virtual`, co umożliwia imitację metod do użycia w testach.
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, magazyn komunikatów zostanie zainicjowany przy użyciu trzech komunikatów. Te *rozsiane komunikaty* są również używane w testach.

&#8224;W temacie EF [test z niepamięcią](/ef/core/miscellaneous/testing/in-memory), wyjaśniono, jak korzystać z bazy danych w pamięci dla testów z MSTest. W tym temacie jest stosowane środowisko testowe [xUnit](https://xunit.github.io/) . Koncepcje testowe i implementacje testów w różnych strukturach testów są podobne, ale nie są identyczne.

Chociaż Przykładowa aplikacja nie korzysta ze wzorca repozytorium i nie jest skutecznym przykładem [wzorca jednostki pracy](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages obsługuje te wzorce rozwoju. Aby uzyskać więcej informacji, zobacz [projektowanie warstwy trwałości infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) i <xref:mvc/controllers/testing> (przykład implementuje wzorzec repozytorium).

## <a name="test-app-organization"></a>Testuj organizację aplikacji

Aplikacja testowa jest aplikacją konsolową wewnątrz folderu *Tests/RazorPagesTestSample. Tests* .

| Testuj folder aplikacji | Opis |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* zawiera testy jednostkowe dla dal.</li><li>*IndexPageTests.cs* zawiera testy jednostkowe dla modelu strony indeksu.</li></ul> |
| *Uruchamiane*     | Zawiera metodę `TestDbContextOptions` używaną do tworzenia nowych opcji kontekstu bazy danych dla każdego testu jednostkowego DAL, aby baza danych została zresetowana do stanu linii bazowej dla każdego testu. |

Platforma testowa jest [xUnit](https://xunit.github.io/). Struktura imitacji obiektu jest [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Testy jednostkowe warstwy dostępu do danych (DAL)

Aplikacja wiadomości ma DAL z czterema metodami zawartymi w klasie `AppDbContext` (*src/RazorPagesTestSample/Data/AppDbContext. cs*). Każda metoda ma jeden lub dwa testy jednostkowe w aplikacji testowej.

| DAL, Metoda               | Funkcja                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Uzyskuje `List<Message>` z bazy danych posortowanej według właściwości `Text`. |
| `AddMessageAsync`        | Dodaje `Message` do bazy danych.                                          |
| `DeleteAllMessagesAsync` | Usuwa wszystkie wpisy `Message` z bazy danych.                           |
| `DeleteMessageAsync`     | Usuwa jeden `Message` z bazy danych przez `Id`.                      |

Testy jednostkowe DAL wymagają <xref:Microsoft.EntityFrameworkCore.DbContextOptions> podczas tworzenia nowego `AppDbContext` dla każdego testu. Jednym z metod tworzenia `DbContextOptions` dla każdego testu jest użycie <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Problem z tym podejściem polega na tym, że każdy test otrzymuje bazę danych w dowolnym stanie, w którym został pozostawiony poprzedni test. Może to być przyczyną problemów podczas próby zapisania niepodzielnych testów jednostkowych, które nie zakłócają siebie nawzajem. Aby wymusić użycie przez `AppDbContext` nowego kontekstu bazy danych dla każdego testu, podaj wystąpienie `DbContextOptions`, które jest oparte na nowym dostawcy usług. Aplikacja testowa pokazuje, jak to zrobić za pomocą metody klasy `Utilities` `TestDbContextOptions` (*Tests/RazorPagesTestSample. Tests/Utilities/Utilities. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Użycie `DbContextOptions` w testach jednostkowych DAL umożliwia niepodzielną uruchomienie każdego testu w przypadku wystąpienia nowej bazy danych:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Każda metoda testowa w klasie `DataAccessLayerTest` (*UnitTests/DataAccessLayerTest. cs*) postępuje zgodnie z podobnym wzorcem porządkowania:

1. Porządkowanie: baza danych jest skonfigurowana do testowania i/lub oczekiwany wynik jest zdefiniowany.
1. Działanie: test jest wykonywany.
1. Assert: potwierdzenia są wykonywane w celu ustalenia, czy wynik testu jest sukcesem.

Na przykład Metoda `DeleteMessageAsync` jest odpowiedzialna za usunięcie pojedynczego komunikatu identyfikowanego przez jego `Id` (*src/RazorPagesTestSample/Data/AppDbContext. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Istnieją dwa testy dla tej metody. Jeden test sprawdza, czy metoda usuwa komunikat, gdy komunikat jest obecny w bazie danych. Druga metoda testuje, że baza danych nie zmienia się, jeśli komunikat `Id` do usunięcia nie istnieje. Poniżej przedstawiono metodę `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`:

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Najpierw Metoda wykonuje krok rozmieszczania, w którym odbywa się przygotowanie do kroku działania. Wypełnianie komunikatów jest uzyskiwane i przechowywane w `seedMessages`. Wypełnianie komunikatów jest zapisywane w bazie danych. Komunikat z `Id` `1` jest ustawiany do usunięcia. Po wykonaniu metody `DeleteMessageAsync` oczekiwane komunikaty powinny zawierać wszystkie komunikaty z wyjątkiem tych, które mają `Id` `1`. Zmienna `expectedMessages` reprezentuje oczekiwany wynik.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Metoda działa: Metoda `DeleteMessageAsync` jest wykonywana w `recId` `1`:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Na koniec metoda uzyskuje `Messages` z kontekstu i porównuje ją z `expectedMessages` potwierdzenia, że dwa są równe:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

W celu porównania, że te dwa `List<Message>` są takie same:

* Komunikaty są uporządkowane według `Id`.
* Pary komunikatów są porównywane z właściwością `Text`.

Podobna metoda testowa, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` sprawdza wynik próby usunięcia komunikatu, który nie istnieje. W takim przypadku oczekiwane komunikaty w bazie danych powinny być równe rzeczywistym komunikatom po wykonaniu metody `DeleteMessageAsync`. Nie powinno się zmieniać zawartości bazy danych:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Testy jednostkowe metod modelu strony

Inny zestaw testów jednostkowych jest odpowiedzialny za testy metod modelu strony. W aplikacji wiadomości modele stron indeksu są dostępne w klasie `IndexModel` w *src/RazorPagesTestSample/Pages/index. cshtml. cs*.

| Metoda modelu strony | Funkcja |
| ----------------- | -------- |
| `OnGetAsync` | Uzyskuje komunikaty z DAL dla interfejsu użytkownika przy użyciu metody `GetMessagesAsync`. |
| `OnPostAddMessageAsync` | Jeśli [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) jest prawidłowy, program wywołuje `AddMessageAsync`, aby dodać komunikat do bazy danych. |
| `OnPostDeleteAllMessagesAsync` | Wywołuje `DeleteAllMessagesAsync`, aby usunąć wszystkie wiadomości w bazie danych. |
| `OnPostDeleteMessageAsync` | Wykonuje `DeleteMessageAsync`, aby usunąć komunikat z określonym `Id`. |
| `OnPostAnalyzeMessagesAsync` | Jeśli co najmniej jeden komunikat znajduje się w bazie danych, program oblicza średnią liczbę wyrazów na komunikat. |

Metody modelu strony są testowane przy użyciu siedmiu testów w klasie `IndexPageTests` (*Tests/RazorPagesTestSample. Tests/UnitTests/IndexPageTests. cs*). Testy używają znanego wzorca porządkowania i potwierdzeń. Te testy koncentrują się na:

* Określanie, czy metody są zgodne z prawidłowym zachowaniem, gdy [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) jest nieprawidłowe.
* Potwierdzenie metod daje poprawne <xref:Microsoft.AspNetCore.Mvc.IActionResult>.
* Sprawdzanie, czy przypisania wartości właściwości są wykonywane poprawnie.

Ta grupa testów często służy do zawzorowania metod DAL, aby generować oczekiwane dane dla kroku działania, w którym jest wykonywana metoda modelu strony. Na przykład Metoda `GetMessagesAsync` `AppDbContext` jest makietą w celu wygenerowania danych wyjściowych. Gdy metoda modelu strony wykonuje tę metodę, funkcja makieta zwraca wynik. Dane nie pochodzą z bazy danych. Powoduje to utworzenie przewidywalnych, niezawodnych warunków testowania dla korzystania z DAL w testach modelu strony.

Test `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` pokazuje, jak Metoda `GetMessagesAsync` jest makietą dla modelu strony:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Gdy metoda `OnGetAsync` jest wykonywana w kroku Act, wywołuje metodę `GetMessagesAsync` modelu strony.

Krok działania testów jednostkowych (*testy/RazorPagesTestSample. Tests/UnitTests/IndexPageTests. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

Metoda `OnGetAsync` `IndexPage` modelu strony (*src/RazorPagesTestSample/Pages/index. cshtml. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Metoda `GetMessagesAsync` w DAL nie zwraca wyniku dla tego wywołania metody. Makieta wersji metody zwraca wynik.

W kroku `Assert` rzeczywiste komunikaty (`actualMessages`) są przypisywane z właściwości `Messages` modelu strony. Sprawdzanie typu jest również wykonywane po przypisaniu komunikatów. Oczekiwane i rzeczywiste komunikaty są porównywane przez ich `Text` właściwości. Test potwierdza, że dwa wystąpienia `List<Message>` zawierają te same komunikaty.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Inne testy w tej grupie tworzą obiekty modelu strony, które obejmują <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, <xref:Microsoft.AspNetCore.Mvc.ActionContext> do ustanowienia `PageContext`, `ViewDataDictionary`i `PageContext`. Są one przydatne podczas przeprowadzania testów. Na przykład aplikacja wiadomości nawiązuje `ModelState` błąd z <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>, aby sprawdzić, czy podczas wykonywania `OnPostAddMessageAsync` jest zwracany prawidłowy <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Testowanie C# jednostkowe w programie .NET Core przy użyciu testu dotnet i xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Testowanie jednostkowe kodu](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Tworzenie kompletnego rozwiązania .NET Core w systemie macOS przy użyciu programu Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Wprowadzenie do usługi xUnit.net: używanie platformy .NET Core z wierszem polecenia zestawu .NET SDK](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [MOQ — Szybki Start](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core obsługuje testy jednostkowe Razor Pages aplikacji. Testy warstwy dostępu do danych (DAL) i modele stron pomagają zapewnić:

* Części aplikacji Razor Pages działają niezależnie i razem jako jednostka podczas konstruowania aplikacji.
* Klasy i metody mają ograniczone zakresy odpowiedzialności.
* Istnieje dodatkowa dokumentacja dotycząca zachowania aplikacji.
* Regresje, które są błędy związane z aktualizacjami kodu, są dostępne podczas zautomatyzowanego kompilowania i wdrażania.

W tym temacie założono, że masz podstawową wiedzę na temat Razor Pages aplikacji i testów jednostkowych. Jeśli nie znasz Razor Pages aplikacji lub testów, zapoznaj się z następującymi tematami:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Testowanie C# jednostkowe w programie .NET Core przy użyciu testu dotnet i xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowy projekt składa się z dwóch aplikacji:

| Aplikacja         | Folder projektu                     | Opis |
| ----------- | ---------------------------------- | ----------- |
| Aplikacja wiadomości | *SRC/RazorPagesTestSample*         | Zezwala użytkownikowi na dodawanie wiadomości, usuwanie jednego komunikatu, usuwanie wszystkich komunikatów i analizowanie komunikatów (Znajdowanie średniej liczby wyrazów na komunikat). |
| Aplikacja testowa    | *testy/RazorPagesTestSample. Tests* | Służy do przetestowania jednostki DAL i indeksu strony aplikacji wiadomości. |

Testy można uruchamiać przy użyciu wbudowanych funkcji testowych środowiska IDE, takich jak [Visual Studio](/visualstudio/test/unit-test-your-code) lub [Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). W przypadku używania [Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia wykonaj następujące polecenie w wierszu polecenia w folderze *Tests/RazorPagesTestSample. Tests* :

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>Organizacja aplikacji komunikatów

Aplikacja wiadomości jest systemem komunikatów Razor Pages o następujących cechach:

* Strona indeks aplikacji (*Pages/index. cshtml* i *Pages/index. cshtml. cs*) zawiera metody interfejsu użytkownika i modelu strony umożliwiające sterowanie dodawaniem, usuwaniem i analizą komunikatów (Znajdź średnią liczbę wyrazów na komunikat).
* Komunikat jest opisywany przez klasę `Message` (*Data/Message. cs*) z dwiema właściwościami: `Id` (Key) i `Text` (Message). Właściwość `Text` jest wymagana i jest ograniczona do 200 znaków.
* Komunikaty są przechowywane przy użyciu&#8224; [bazy danych znajdującej się w pamięci Entity Framework](/ef/core/providers/in-memory/).
* Aplikacja zawiera DAL w swojej klasie kontekstu bazy danych, `AppDbContext` (*Data/AppDbContext. cs*). Metody DAL są oznaczone `virtual`, co umożliwia imitację metod do użycia w testach.
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, magazyn komunikatów zostanie zainicjowany przy użyciu trzech komunikatów. Te *rozsiane komunikaty* są również używane w testach.

&#8224;W temacie EF [test z niepamięcią](/ef/core/miscellaneous/testing/in-memory), wyjaśniono, jak korzystać z bazy danych w pamięci dla testów z MSTest. W tym temacie jest stosowane środowisko testowe [xUnit](https://xunit.github.io/) . Koncepcje testowe i implementacje testów w różnych strukturach testów są podobne, ale nie są identyczne.

Chociaż Przykładowa aplikacja nie korzysta ze wzorca repozytorium i nie jest skutecznym przykładem [wzorca jednostki pracy](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages obsługuje te wzorce rozwoju. Aby uzyskać więcej informacji, zobacz [projektowanie warstwy trwałości infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) i <xref:mvc/controllers/testing> (przykład implementuje wzorzec repozytorium).

## <a name="test-app-organization"></a>Testuj organizację aplikacji

Aplikacja testowa jest aplikacją konsolową wewnątrz folderu *Tests/RazorPagesTestSample. Tests* .

| Testuj folder aplikacji | Opis |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* zawiera testy jednostkowe dla dal.</li><li>*IndexPageTests.cs* zawiera testy jednostkowe dla modelu strony indeksu.</li></ul> |
| *Uruchamiane*     | Zawiera metodę `TestDbContextOptions` używaną do tworzenia nowych opcji kontekstu bazy danych dla każdego testu jednostkowego DAL, aby baza danych została zresetowana do stanu linii bazowej dla każdego testu. |

Platforma testowa jest [xUnit](https://xunit.github.io/). Struktura imitacji obiektu jest [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Testy jednostkowe warstwy dostępu do danych (DAL)

Aplikacja wiadomości ma DAL z czterema metodami zawartymi w klasie `AppDbContext` (*src/RazorPagesTestSample/Data/AppDbContext. cs*). Każda metoda ma jeden lub dwa testy jednostkowe w aplikacji testowej.

| DAL, Metoda               | Funkcja                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Uzyskuje `List<Message>` z bazy danych posortowanej według właściwości `Text`. |
| `AddMessageAsync`        | Dodaje `Message` do bazy danych.                                          |
| `DeleteAllMessagesAsync` | Usuwa wszystkie wpisy `Message` z bazy danych.                           |
| `DeleteMessageAsync`     | Usuwa jeden `Message` z bazy danych przez `Id`.                      |

Testy jednostkowe DAL wymagają <xref:Microsoft.EntityFrameworkCore.DbContextOptions> podczas tworzenia nowego `AppDbContext` dla każdego testu. Jednym z metod tworzenia `DbContextOptions` dla każdego testu jest użycie <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Problem z tym podejściem polega na tym, że każdy test otrzymuje bazę danych w dowolnym stanie, w którym został pozostawiony poprzedni test. Może to być przyczyną problemów podczas próby zapisania niepodzielnych testów jednostkowych, które nie zakłócają siebie nawzajem. Aby wymusić użycie przez `AppDbContext` nowego kontekstu bazy danych dla każdego testu, podaj wystąpienie `DbContextOptions`, które jest oparte na nowym dostawcy usług. Aplikacja testowa pokazuje, jak to zrobić za pomocą metody klasy `Utilities` `TestDbContextOptions` (*Tests/RazorPagesTestSample. Tests/Utilities/Utilities. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Użycie `DbContextOptions` w testach jednostkowych DAL umożliwia niepodzielną uruchomienie każdego testu w przypadku wystąpienia nowej bazy danych:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Każda metoda testowa w klasie `DataAccessLayerTest` (*UnitTests/DataAccessLayerTest. cs*) postępuje zgodnie z podobnym wzorcem porządkowania:

1. Porządkowanie: baza danych jest skonfigurowana do testowania i/lub oczekiwany wynik jest zdefiniowany.
1. Działanie: test jest wykonywany.
1. Assert: potwierdzenia są wykonywane w celu ustalenia, czy wynik testu jest sukcesem.

Na przykład Metoda `DeleteMessageAsync` jest odpowiedzialna za usunięcie pojedynczego komunikatu identyfikowanego przez jego `Id` (*src/RazorPagesTestSample/Data/AppDbContext. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Istnieją dwa testy dla tej metody. Jeden test sprawdza, czy metoda usuwa komunikat, gdy komunikat jest obecny w bazie danych. Druga metoda testuje, że baza danych nie zmienia się, jeśli komunikat `Id` do usunięcia nie istnieje. Poniżej przedstawiono metodę `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Najpierw Metoda wykonuje krok rozmieszczania, w którym odbywa się przygotowanie do kroku działania. Wypełnianie komunikatów jest uzyskiwane i przechowywane w `seedMessages`. Wypełnianie komunikatów jest zapisywane w bazie danych. Komunikat z `Id` `1` jest ustawiany do usunięcia. Po wykonaniu metody `DeleteMessageAsync` oczekiwane komunikaty powinny zawierać wszystkie komunikaty z wyjątkiem tych, które mają `Id` `1`. Zmienna `expectedMessages` reprezentuje oczekiwany wynik.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Metoda działa: Metoda `DeleteMessageAsync` jest wykonywana w `recId` `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Na koniec metoda uzyskuje `Messages` z kontekstu i porównuje ją z `expectedMessages` potwierdzenia, że dwa są równe:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

W celu porównania, że te dwa `List<Message>` są takie same:

* Komunikaty są uporządkowane według `Id`.
* Pary komunikatów są porównywane z właściwością `Text`.

Podobna metoda testowa, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` sprawdza wynik próby usunięcia komunikatu, który nie istnieje. W takim przypadku oczekiwane komunikaty w bazie danych powinny być równe rzeczywistym komunikatom po wykonaniu metody `DeleteMessageAsync`. Nie powinno się zmieniać zawartości bazy danych:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Testy jednostkowe metod modelu strony

Inny zestaw testów jednostkowych jest odpowiedzialny za testy metod modelu strony. W aplikacji wiadomości modele stron indeksu są dostępne w klasie `IndexModel` w *src/RazorPagesTestSample/Pages/index. cshtml. cs*.

| Metoda modelu strony | Funkcja |
| ----------------- | -------- |
| `OnGetAsync` | Uzyskuje komunikaty z DAL dla interfejsu użytkownika przy użyciu metody `GetMessagesAsync`. |
| `OnPostAddMessageAsync` | Jeśli [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) jest prawidłowy, program wywołuje `AddMessageAsync`, aby dodać komunikat do bazy danych. |
| `OnPostDeleteAllMessagesAsync` | Wywołuje `DeleteAllMessagesAsync`, aby usunąć wszystkie wiadomości w bazie danych. |
| `OnPostDeleteMessageAsync` | Wykonuje `DeleteMessageAsync`, aby usunąć komunikat z określonym `Id`. |
| `OnPostAnalyzeMessagesAsync` | Jeśli co najmniej jeden komunikat znajduje się w bazie danych, program oblicza średnią liczbę wyrazów na komunikat. |

Metody modelu strony są testowane przy użyciu siedmiu testów w klasie `IndexPageTests` (*Tests/RazorPagesTestSample. Tests/UnitTests/IndexPageTests. cs*). Testy używają znanego wzorca porządkowania i potwierdzeń. Te testy koncentrują się na:

* Określanie, czy metody są zgodne z prawidłowym zachowaniem, gdy [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) jest nieprawidłowe.
* Potwierdzenie metod daje poprawne <xref:Microsoft.AspNetCore.Mvc.IActionResult>.
* Sprawdzanie, czy przypisania wartości właściwości są wykonywane poprawnie.

Ta grupa testów często służy do zawzorowania metod DAL, aby generować oczekiwane dane dla kroku działania, w którym jest wykonywana metoda modelu strony. Na przykład Metoda `GetMessagesAsync` `AppDbContext` jest makietą w celu wygenerowania danych wyjściowych. Gdy metoda modelu strony wykonuje tę metodę, funkcja makieta zwraca wynik. Dane nie pochodzą z bazy danych. Powoduje to utworzenie przewidywalnych, niezawodnych warunków testowania dla korzystania z DAL w testach modelu strony.

Test `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` pokazuje, jak Metoda `GetMessagesAsync` jest makietą dla modelu strony:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Gdy metoda `OnGetAsync` jest wykonywana w kroku Act, wywołuje metodę `GetMessagesAsync` modelu strony.

Krok działania testów jednostkowych (*testy/RazorPagesTestSample. Tests/UnitTests/IndexPageTests. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

Metoda `OnGetAsync` `IndexPage` modelu strony (*src/RazorPagesTestSample/Pages/index. cshtml. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Metoda `GetMessagesAsync` w DAL nie zwraca wyniku dla tego wywołania metody. Makieta wersji metody zwraca wynik.

W kroku `Assert` rzeczywiste komunikaty (`actualMessages`) są przypisywane z właściwości `Messages` modelu strony. Sprawdzanie typu jest również wykonywane po przypisaniu komunikatów. Oczekiwane i rzeczywiste komunikaty są porównywane przez ich `Text` właściwości. Test potwierdza, że dwa wystąpienia `List<Message>` zawierają te same komunikaty.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Inne testy w tej grupie tworzą obiekty modelu strony, które obejmują <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, <xref:Microsoft.AspNetCore.Mvc.ActionContext> do ustanowienia `PageContext`, `ViewDataDictionary`i `PageContext`. Są one przydatne podczas przeprowadzania testów. Na przykład aplikacja wiadomości nawiązuje `ModelState` błąd z <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>, aby sprawdzić, czy podczas wykonywania `OnPostAddMessageAsync` jest zwracany prawidłowy <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Testowanie C# jednostkowe w programie .NET Core przy użyciu testu dotnet i xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Testowanie jednostkowe kodu](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Tworzenie kompletnego rozwiązania .NET Core w systemie macOS przy użyciu programu Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Wprowadzenie do usługi xUnit.net: używanie platformy .NET Core z wierszem polecenia zestawu .NET SDK](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [MOQ — Szybki Start](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
