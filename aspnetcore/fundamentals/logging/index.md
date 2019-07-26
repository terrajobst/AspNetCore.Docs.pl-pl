---
title: Logowanie ASP.NET Core
author: tdykstra
description: Dowiedz się więcej na temat struktury rejestrowania w ASP.NET Core. Odkryj wbudowanych dostawców rejestrowania i Dowiedz się więcej o popularnych dostawcach innych firm.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 4fe677e69478284db2ccab655c35b5744b6f63f9
ms.sourcegitcommit: 059ab380744fa3be3b69aa90d431b563c57092cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68410910"
---
# <a name="logging-in-aspnet-core"></a>Logowanie ASP.NET Core

[Steve Kowalski](https://ardalis.com/) i [Tomasz Dykstra](https://github.com/tdykstra)

ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm. W tym artykule pokazano, jak używać interfejsu API rejestrowania z wbudowanymi dostawcami.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Dodaj dostawców

Dostawca rejestrowania wyświetla lub przechowuje dzienniki. Na przykład dostawca konsoli wyświetla dzienniki w konsoli programu, a Dostawca usługi Azure Application Insights przechowuje je na platformie Azure Application Insights. Dzienniki mogą być wysyłane do wielu miejsc docelowych przez dodanie wielu dostawców.

::: moniker range=">= aspnetcore-2.0"

Aby dodać dostawcę, wywołaj metodę `Add{provider name}` rozszerzenia dostawcy w *program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

Poprzedzający kod wymaga odwołania do `Microsoft.Extensions.Logging` i `Microsoft.Extensions.Configuration`.

Domyślne wywołania <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>szablonu projektu, które dodaje następujących dostawców rejestrowania:

* Konsola
* Debugowanie
* EventSource (rozpoczęcie w ASP.NET Core 2,2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Jeśli używasz `CreateDefaultBuilder`programu, możesz zastąpić domyślnych dostawców własnymi opcjami. Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>i Dodaj żądanych dostawców.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aby użyć dostawcy, zainstaluj jego pakiet NuGet i Wywołaj metodę rozszerzenia dostawcy w wystąpieniu <xref:Microsoft.Extensions.Logging.ILoggerFactory>:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [iniekcja zależności (di)](xref:fundamentals/dependency-injection) udostępnia `ILoggerFactory` wystąpienie. Metody rozszerzenia `AddDebug`isą zdefiniowane w pakietach [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft. Extensions. Logging. Debug.](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) `AddConsole` Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` metodę, przekazując do wystąpienia dostawcy.

> [!NOTE]
> [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) dodaje dostawców rejestrowania w `Startup.Configure` metodzie. Aby uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, Dodaj dostawców `Startup` rejestrowania w konstruktorze klas.

::: moniker-end

Więcej informacji na temat [wbudowanych dostawców rejestrowania](#built-in-logging-providers) i [dostawców rejestrowania innych](#third-party-logging-providers) firm znajduje się w dalszej części artykułu.

## <a name="create-logs"></a>Tworzenie dzienników

Pobierz obiekt <xref:Microsoft.Extensions.Logging.ILogger%601> z elementu di.

::: moniker range=">= aspnetcore-2.0"

Poniższy przykład kontrolera tworzy `Information` i `Warning` rejestruje. *Kategoria* to `TodoApiSample.Controllers.TodoController` (w pełni `TodoController` kwalifikowana nazwa klasy w aplikacji przykładowej):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Poniższy Razor Pages przykład tworzy dzienniki z użyciem `Information` jako *poziomu* i `TodoApiSample.Pages.AboutModel` jako *kategorii*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Poprzedni przykład tworzy dzienniki z `Information` i `Warning` jako  `TodoController` *Kategoria*. 

::: moniker-end

*Poziom* dziennika wskazuje ważność rejestrowanego zdarzenia. *Kategoria* dziennika jest ciągiem, który jest skojarzony z każdym dziennikiem. Wystąpienie tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` jako kategorię. `ILogger<T>` [Poziomy](#log-level) i [Kategorie](#log-category) zostały wyjaśnione bardziej szczegółowo w dalszej części tego artykułu. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Tworzenie dzienników w programie startowym

Aby napisać dzienniki w `Startup` klasie, `ILogger` Dołącz parametr do sygnatury konstruktora:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>Tworzenie dzienników w programie

Aby napisać dzienniki w `Program` klasie, Pobierz wystąpienie z elementu `ILogger` di:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Brak metod rejestratora asynchronicznego

Rejestracja powinna być tak szybka, że nie jest to koszt wydajności kodu asynchronicznego. Jeśli magazyn danych rejestrowania jest wolny, nie zapisuj go bezpośrednio. Najpierw Rozważ zapisanie komunikatów dziennika do szybkiego sklepu, a następnie przeniesienie ich do wolnego magazynu później. Na przykład jeśli rejestrujesz się do SQL Server, nie chcesz tego robić bezpośrednio w `Log` metodzie, `Log` ponieważ metody są synchroniczne. Zamiast tego można synchronicznie dodawać komunikaty dziennika do kolejki w pamięci, a proces roboczy w tle ściągał komunikaty z kolejki, aby wykonać asynchroniczne działanie wypychania danych do SQL Server.

## <a name="configuration"></a>Konfiguracja

Konfiguracja dostawcy rejestrowania jest świadczona przez co najmniej jednego dostawcę konfiguracji:

* Formaty plików (INI, JSON i XML).
* Argumenty wiersza polecenia.
* Zmienne środowiskowe.
* Obiekty platformy .NET w pamięci.
* Magazyn niezaszyfrowanego [klucza tajnego](xref:security/app-secrets) .
* Zaszyfrowany magazyn użytkowników, taki jak [Azure Key Vault](xref:security/key-vault-configuration).
* Dostawcy niestandardowi (instalowani lub utworzony).

Na przykład konfiguracja rejestrowania jest zwykle dostarczana przez `Logging` sekcję plików ustawień aplikacji. Poniższy przykład pokazuje zawartość typowej wartości *appSettings. Plik Development. JSON* :

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

Właściwość może mieć `LogLevel` właściwości dostawcy dzienników i. `Logging`

Właściwość w obszarze `Logging` określa minimalny poziom rejestrowania wybranych kategorii. [](#log-level) `LogLevel` W przykładzie, `System` i `Microsoft` kategorie są rejestrowane na `Information` poziomie, a wszystkie inne logowania na `Debug` poziomie.

Inne właściwości w `Logging` obszarze Określ dostawców rejestrowania. Przykład dotyczy dostawcy konsoli. Jeśli dostawca obsługuje [zakresy rejestrowania](#log-scopes), wskazuje `IncludeScopes` , czy są one włączone. Właściwość dostawcy (taka jak `Console` w przykładzie) może także `LogLevel` określić właściwość. `LogLevel`w obszarze dostawca Określa poziomy do rejestrowania dla tego dostawcy.

Jeśli w programie `Logging.{providername}.LogLevel`są określone poziomy, zastępują one wszystko `Logging.LogLevel`ustawione w.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel`klucze reprezentują nazwy dzienników. `Default` Klucz ma zastosowanie do dzienników, które nie są jawnie wymienione. Wartość reprezentuje [poziom dziennika](#log-level) zastosowany do danego dziennika.

::: moniker-end

Informacje o implementowaniu dostawców konfiguracji znajdują <xref:fundamentals/configuration/index>się w temacie.

## <a name="sample-logging-output"></a>Przykładowe dane wyjściowe rejestrowania

W przypadku przykładowego kodu podanego w poprzedniej sekcji Dzienniki są wyświetlane w konsoli programu, gdy aplikacja jest uruchamiana z wiersza polecenia. Oto przykład danych wyjściowych konsoli:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Poprzednie dzienniki zostały wygenerowane przez utworzenie żądania HTTP GET do przykładowej aplikacji w lokalizacji `http://localhost:5000/api/todo/0`.

Oto przykład tych samych dzienników, które są wyświetlane w oknie debugowania podczas uruchamiania przykładowej aplikacji w programie Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Dzienniki utworzone przez `ILogger` wywołania pokazane w poprzedniej sekcji zaczynają się od "TodoApi. controllers. TodoController". Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework. ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.

W pozostałej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.

## <a name="nuget-packages"></a>Pakiety NuGet

Interfejsy `ILogger` i`ILoggerFactory` są w [Microsoft. Extensions. Logging. Abstracts](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)i domyślne implementacje dla nich są w [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Kategoria dziennika

Po utworzeniu obiektu jest dla niego określona *Kategoria.* `ILogger` Ta kategoria jest dołączona do każdego komunikatu dziennika utworzonego przez to wystąpienie `ILogger`. Kategoria może być dowolnym ciągiem, ale Konwencja ma używać nazwy klasy, takiej jak "TodoApi. controllers. TodoController".

Użyj `ILogger<T>` , aby `ILogger` uzyskać wystąpienie używające w `T` pełni kwalifikowanej nazwy typu jako kategorii:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Aby jawnie określić kategorię, wywołaj `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>`jest odpowiednikiem wywołania `CreateLogger` z w pełni kwalifikowaną `T`nazwą typu.

## <a name="log-level"></a>Poziom dziennika

Każdy dziennik Określa <xref:Microsoft.Extensions.Logging.LogLevel> wartość. Poziom dziennika wskazuje ważność lub ważność. Przykładowo można napisać `Information` dziennik, gdy metoda jest zwykle zakończona `Warning` , a dziennik, gdy metoda zwraca 404, *nie znaleziono* kodu stanu.

Poniższy kod tworzy `Information` i `Warning` rejestruje:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

W powyższym kodzie pierwszy parametr jest IDENTYFIKATORem [zdarzenia dziennika](#log-event-id). Drugi parametr jest szablonem wiadomości z symbolami zastępczymi dla wartości argumentów dostarczonych przez pozostałe parametry metody. Parametry metody zostały wyjaśnione w [sekcji szablon komunikatu](#log-message-template) w dalszej części tego artykułu.

Metody rejestrowania, które obejmują poziom w nazwie metody (na przykład `LogInformation` i `LogWarning`) są metodami [rozszerzającymi dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Te metody wywołują `Log` metodę, która `LogLevel` pobiera parametr. `Log` Metodę można wywołać bezpośrednio zamiast jednej z tych metod rozszerzających, ale składnia jest stosunkowo skomplikowana. Aby uzyskać więcej informacji, <xref:Microsoft.Extensions.Logging.ILogger> Zobacz i [kod źródłowy rozszerzeń rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core definiuje następujące poziomy dziennika uporządkowane w tym miejscu od najniższej do najwyższej wagi.

* Ślad = 0

  Aby uzyskać informacje, które są zazwyczaj cenne tylko dla debugowania. Komunikaty te mogą zawierać poufne dane aplikacji, dlatego nie powinny być włączone w środowisku produkcyjnym. *Domyślnie wyłączona.*

* Debuguj = 1

  Informacje, które mogą być przydatne podczas tworzenia i debugowania. Przykład: `Entering method Configure with flag set to true.`Włącz `Debug` dzienniki poziomów w środowisku produkcyjnym tylko w przypadku rozwiązywania problemów, ze względu na dużą ilość dzienników.

* Informacje = 2

  Do śledzenia ogólnego przepływu aplikacji. Te dzienniki zwykle mają pewną wartość długoterminową. Przykład: `Request received for path /api/todo`

* Ostrzeżenie = 3

  Dla nietypowych lub nieoczekiwanych zdarzeń w przepływie aplikacji. Mogą to być błędy lub inne warunki, które nie powodują zatrzymania aplikacji, ale konieczne może być zbadanie. Obsłużone wyjątki są typowym miejscem do korzystania `Warning` z poziomu dziennika. Przykład: `FileNotFoundException for file quotes.txt.`

* Błąd = 4

  W przypadku błędów i wyjątków, których nie można obsłużyć. Te komunikaty wskazują niepowodzenie w bieżącym działaniu lub operacji (np. bieżące żądanie HTTP), a nie awaria całej aplikacji. Przykładowy komunikat dziennika:`Cannot insert record due to duplicate key violation.`

* Krytyczne = 5

  Dla niepowodzeń, które wymagają natychmiastowej uwagi. Przykłady: scenariusze utraty danych, brak miejsca na dysku.

Poziom dziennika służy do kontrolowania, ile danych wyjściowych dziennika jest zapisywana w określonym nośniku lub oknie wyświetlania. Na przykład:

* W obszarze produkcja, `Trace` Wyślij `Information` przez poziom do magazynu danych woluminu. Wyślij `Warning`domagazynudanych wartości.`Critical`
* `Warning` Podczas tworzenia, wysyłaj `Critical` do `Trace` konsoli i dodawaj podczas rozwiązywania problemów. `Information`

W sekcji [filtrowanie dzienników](#log-filtering) w dalszej części tego artykułu wyjaśniono, jak kontrolować poziomy dzienników obsługiwane przez dostawcę.

ASP.NET Core zapisuje dzienniki dla zdarzeń struktury. Przykłady dzienników znajdujące się wcześniej w tym artykule nie `Information` wykluczają dzienników `Debug` poniżej `Trace` , dlatego nie zostały utworzone dzienniki. Oto przykład dzienników konsoli utworzonych przez uruchomienie przykładowej aplikacji skonfigurowanej do wyświetlania `Debug` dzienników:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>Identyfikator zdarzenia dziennika

Każdy dziennik może określać *Identyfikator zdarzenia*. Aplikacja Przykładowa wykonuje to przy użyciu lokalnie zdefiniowanej `LoggingEvents` klasy:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Identyfikator zdarzenia kojarzy zestaw zdarzeń. Na przykład wszystkie dzienniki związane z wyświetlaniem listy elementów na stronie mogą być 1001.

Dostawca rejestrowania może przechowywać identyfikator zdarzenia w polu identyfikatora, w komunikacie rejestrowania lub wcale. Dostawca debugowania nie pokazuje identyfikatorów zdarzeń. Dostawca konsoli pokazuje identyfikatory zdarzeń w nawiasach po kategorii:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Szablon komunikatu dziennika

Każdy dziennik Określa szablon wiadomości. Szablon wiadomości może zawierać symbole zastępcze, dla których podano argumenty. Użyj nazw dla symboli zastępczych, a nie liczby.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Kolejność symboli zastępczych, nie ich nazw, określa, które parametry są używane do dostarczania ich wartości. W poniższym kodzie Zwróć uwagę, że nazwy parametrów są poza kolejnością w szablonie wiadomości:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Ten kod tworzy komunikat dziennika z wartościami parametrów w kolejności:

```
Parameter values: parm1, parm2
```

Struktura rejestrowania działa w ten sposób, aby dostawcy rejestrowania mogli zaimplementować [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Same argumenty są przesyłane do systemu rejestrowania, a nie tylko dla sformatowanego szablonu wiadomości. Te informacje umożliwiają dostawcom rejestrowania przechowywanie wartości parametrów jako pól. Na przykład załóżmy, że wywołania metod rejestratora wyglądają następująco:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

W przypadku wysyłania dzienników do usługi Azure Table Storage każda jednostka tabeli platformy Azure może mieć `ID` właściwości i `RequestTime` , które upraszczają zapytania dotyczące danych dziennika. Zapytanie może znaleźć wszystkie dzienniki w określonym `RequestTime` zakresie bez analizowania limitu czasu wiadomości tekstowej.

## <a name="logging-exceptions"></a>Wyjątki rejestrowania

Metody rejestratora mają przeciążenia umożliwiające przekazanie wyjątku, jak w poniższym przykładzie:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Różni dostawcy obsługują informacje o wyjątkach na różne sposoby. Oto przykład danych wyjściowych dostawcy debugowania z kodu pokazanego powyżej.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrowanie dzienników

::: moniker range=">= aspnetcore-2.0"

Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii. Wszystkie dzienniki poniżej minimalnego poziomu nie są przesyłane do tego dostawcy, więc nie są wyświetlane ani przechowywane.

Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom dziennika. Wartość `LogLevel.None` całkowita wynosi 6, która jest większa niż `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Utwórz reguły filtru w konfiguracji

Kod szablonu projektu wywołuje `CreateDefaultBuilder` w celu skonfigurowania rejestrowania dla dostawców konsoli i debugowania. Metoda konfiguruje również rejestrowanie, aby wyszukać konfigurację `Logging` w sekcji przy użyciu kodu jak poniżej: `CreateDefaultBuilder`

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17)]

Dane konfiguracyjne określają minimalne poziomy dziennika według dostawcy i kategorii, jak w poniższym przykładzie:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Ten kod JSON tworzy sześć reguł filtrowania: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców. Dla każdego dostawcy wybierana jest pojedyncza reguła, `ILogger` gdy tworzony jest obiekt.

### <a name="filter-rules-in-code"></a>Filtrowanie reguł w kodzie

Poniższy przykład pokazuje, jak zarejestrować reguły filtru w kodzie:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Drugi `AddFilter` określa dostawcę debugowania za pomocą nazwy typu. Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typu dostawcy.

### <a name="how-filtering-rules-are-applied"></a>Jak są stosowane reguły filtrowania

Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach tworzą reguły pokazane w poniższej tabeli. Pierwsze sześć pochodzi z przykładu konfiguracji, a ostatnie dwa pochodzą z przykładu kodu.

| Wartość liczbowa | Dostawca      | Kategorie zaczynające się od...          | Minimalny poziom rejestrowania |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debugowanie         | Wszystkie kategorie                          | Informacje       |
| 2      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Ostrzeżenie           |
| 3      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debugowanie             |
| 4      | Konsola       | Microsoft.AspNetCore.Mvc.Razor          | Błąd             |
| 5      | Konsola       | Wszystkie kategorie                          | Informacje       |
| 6      | Wszyscy dostawcy | Wszystkie kategorie                          | Debugowanie             |
| 7      | Wszyscy dostawcy | System                                  | Debugowanie             |
| 8      | Debugowanie         | Microsoft                               | Szuka             |

Po utworzeniu `ILogger` `ILoggerFactory` obiektu obiekt wybiera jedną regułę dla każdego dostawcy, która ma zostać zastosowana do tego rejestratora. Wszystkie komunikaty zapisywane przez `ILogger` wystąpienie są filtrowane na podstawie wybranych reguł. Najbardziej konkretną regułą można wybrać dla każdego dostawcy i pary kategorii z dostępnych reguł.

Następujący algorytm jest używany dla każdego dostawcy, `ILogger` gdy jest tworzony dla danej kategorii:

* Wybierz wszystkie reguły, które pasują do dostawcy lub jego aliasu. Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły z pustym dostawcą.
* W wyniku poprzedniego kroku wybierz pozycję reguły z najdłuższym prefiksem kategorii. Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.
* Jeśli wybrano wiele reguł, zrób to **ostatnie** .
* Jeśli nie wybrano żadnych reguł, `MinimumLevel`Użyj.

Na powyższej liście reguł Załóżmy, że `ILogger` tworzysz obiekt dla kategorii "Microsoft. AspNetCore. MVC. Razor. RazorViewEngine":

* Dla dostawcy debugowania obowiązują reguły 1, 6 i 8. Reguła 8 jest najbardziej specyficzna, więc jest to jedna wybrana.
* W przypadku dostawcy konsoli obowiązują reguły 3, 4, 5 i 6. Reguła 3 jest najbardziej specyficzna.

Wystąpienie wyników `ILogger` wysyła `Trace` dzienniki poziomu i powyżej do dostawcy debugowania. `Debug` Dzienniki poziomów i powyżej są wysyłane do dostawcy konsoli.

### <a name="provider-aliases"></a>Aliasy dostawców

Każdy dostawca definiuje *alias* , który może być używany w konfiguracji zamiast w pełni kwalifikowanej nazwy typu.  W przypadku dostawców wbudowanych Użyj następujących aliasów:

* Konsola
* Debugowanie
* EventSource
* Elemencie
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>Domyślny poziom minimalny

Istnieje ustawienie minimalnego poziomu, które działa tylko wtedy, gdy nie mają zastosowania żadne reguły z konfiguracji lub kodu dla danego dostawcy i kategorii. Poniższy przykład pokazuje, jak ustawić poziom minimalny:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Jeśli poziom minimalny nie został jawnie ustawiony, wartość domyślna to `Information`, co oznacza, że `Trace` dzienniki i `Debug` są ignorowane.

### <a name="filter-functions"></a>Funkcje filtrowania

Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorii, które nie mają przypisanych do nich reguł przez konfigurację lub kod. Kod w funkcji ma dostęp do typu dostawcy, kategorii i poziomu dziennika. Przykład:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy dzienniki mają być zapisywane na nośniku magazynu, lub ignorowane na podstawie poziomu dziennika i kategorii.

Metody rozszerzające `AddDebug`izapewniają przeciążenia, które akceptują kryteria filtrowania. `AddConsole` Następujący przykładowy kod powoduje, że dostawca konsoli ignoruje dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzone przez strukturę.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Metoda ma Przeciążenie, które `EventLogSettings` przyjmuje wystąpienie, które może zawierać funkcję filtrowania w swojej `Filter` właściwości. `AddEventLog` Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jego poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.

Aby ustawić reguły filtrowania dla wszystkich dostawców zarejestrowanych w `ILoggerFactory` wystąpieniu, `WithFilter` Użyj metody rozszerzenia. Poniższy przykład ogranicza dzienniki struktury (kategoria zaczyna się od "Microsoft" lub "system") do ostrzeżeń podczas rejestrowania na poziomie debugowania dla dzienników utworzonych przez kod aplikacji.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Aby uniemożliwić zapisywanie jakichkolwiek dzienników, określ `LogLevel.None` jako minimalny poziom dziennika. Wartość `LogLevel.None` całkowita wynosi 6, która jest większa niż `LogLevel.Critical` (5).

Metoda rozszerzenia jest dostarczana przez pakiet NuGet [Microsoft. Extensions. Logging. Filter.](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) `WithFilter` Metoda zwraca nowe `ILoggerFactory` wystąpienie, które będzie odfiltrować komunikaty dziennika przesłane do wszystkich dostawców rejestratorów zarejestrowanych z nim. Nie ma to wpływu na `ILoggerFactory` żadne inne wystąpienia, łącznie `ILoggerFactory` z oryginalnym wystąpieniem.

::: moniker-end

## <a name="system-categories-and-levels"></a>Kategorie i poziomy systemu

Poniżej przedstawiono niektóre kategorie używane przez ASP.NET Core i Entity Framework Core, z informacjami o dziennikach, od których należy się spodziewać:

| Kategoria                            | Uwagi |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Ogólna Diagnostyka ASP.NET Core. |
| Microsoft.AspNetCore.DataProtection | Które klucze zostały wzięte pod uwagę, znaleziono i użyte. |
| Microsoft.AspNetCore.HostFiltering  | Dozwolone hosty. |
| Microsoft.AspNetCore.Hosting        | Jak długo trwa wykonywanie żądań HTTP i czas ich uruchomienia. Które hostowanie zestawów uruchamiania zostało załadowane. |
| Microsoft.AspNetCore.Mvc            | Diagnostyka MVC i Razor. Powiązanie modelu, wykonywanie filtru, kompilacja widoku, wybór akcji. |
| Microsoft.AspNetCore.Routing        | Informacje o trasie. |
| Microsoft.AspNetCore.Server         | Reagowanie na uruchamianie, zatrzymywanie i utrzymywanie aktywności. Informacje o certyfikacie HTTPS. |
| Microsoft.AspNetCore.StaticFiles    | Obsługiwane pliki. |
| Microsoft.EntityFrameworkCore       | Ogólna Diagnostyka Entity Framework Core. Aktywność i Konfiguracja bazy danych, wykrywanie zmian, migracje. |

## <a name="log-scopes"></a>Zakresy dziennika

 *Zakres* może grupować zestaw operacji logicznych. Takie grupowanie może służyć do dołączania tych samych danych do każdego dziennika, który został utworzony jako część zestawu. Na przykład każdy dziennik utworzony w ramach przetwarzania transakcji może zawierać identyfikator transakcji.

Zakres jest `IDisposable` typem zwracanym <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> przez metodę i obowiązuje do momentu jego usunięcia. Użyj zakresu przez Zawijanie wywołań rejestratora w `using` bloku:

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Poniższy kod włącza zakresy dla dostawcy konsoli:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie opcji rejestratora konsoli jest wymagane do włączenia rejestrowania na podstawie zakresu. `IncludeScopes`
>
> Informacje o konfiguracji znajdują się w sekcji [Konfiguracja](#configuration) .

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie opcji rejestratora konsoli jest wymagane do włączenia rejestrowania na podstawie zakresu. `IncludeScopes`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Każdy komunikat dziennika zawiera informacje o zakresie:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Wbudowani dostawcy rejestrowania

ASP.NET Core dostarcza następujących dostawców:

* [Console](#console-provider)
* [Debugowanie](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

Aby uzyskać informacje na temat strumienia stdout i rejestrowania debugowania za pomocą modułu <xref:test/troubleshoot-azure-iis> ASP.NET Core <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>, zobacz i.

### <a name="console-provider"></a>Dostawca konsoli

Pakiet [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) Provider wysyła dane wyjściowe dziennika do konsoli programu. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) Funkcja addoverloads umożliwia przekazywanie na minimalnym poziomie dziennika, funkcji filtru i wartości logicznej, która wskazuje, czy zakresy są obsługiwane. Kolejną opcją jest przekazanie `IConfiguration` obiektu, który może określać obsługę zakresów i poziomy rejestrowania.

Aby zapoznać się z opcjami <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>dostawcy konsoli, zobacz.

Dostawca konsoli ma znaczny wpływ na wydajność i zwykle nie jest odpowiedni do użycia w środowisku produkcyjnym.

Podczas tworzenia nowego projektu w programie Visual Studio `AddConsole` Metoda wygląda następująco:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Ten kod odnosi się `Logging` do sekcji pliku *appSettings. JSON* :

[!code-json[](index/samples/1.x/TodoApiSample/appsettings.json)]

Ustawienia pokazują, że można ograniczyć dzienniki struktury do ostrzeżeń, jednocześnie umożliwiając aplikacji rejestrowanie na poziomie debugowania, zgodnie z opisem w sekcji [filtrowanie dzienników](#log-filtering) . Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

::: moniker-end

Aby wyświetlić dane wyjściowe rejestrowania konsoli, Otwórz wiersz polecenia w folderze projektu i uruchom następujące polecenie:

```console
dotnet run
```

### <a name="debug-provider"></a>Dostawca debugowania

Pakiet [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) Provider zapisuje dane wyjściowe dziennika przy użyciu klasy [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) (`Debug.WriteLine` wywołania metod).

W systemie Linux ten dostawca zapisuje dzienniki do */var/log/Message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) Funkcja adddebug overloads pozwala przekazać minimalny poziom rejestrowania lub funkcję filtru.

::: moniker-end

### <a name="eventsource-provider"></a>Dostawca EventSource

W przypadku aplikacji przeznaczonych do ASP.NET Core 1.1.0 lub nowszych pakiet dostawcy [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) może zaimplementować śledzenie zdarzeń. W systemie Windows używa [funkcji ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Dostawca jest międzyplatformowy, ale nie ma jeszcze kolekcji zdarzeń i narzędzi do wyświetlania dla systemu Linux lub macOS.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [Narzędzia Narzędzia PerfView](https://github.com/Microsoft/perfview). Istnieją inne narzędzia do wyświetlania dzienników ETW, ale narzędzia PerfView zapewnia najlepsze środowisko pracy z zdarzeniami ETW emitowanymi przez ASP.NET.

Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj ciąg `*Microsoft-Extensions-Logging` do listy **dodatkowych dostawców** . (Nie przegap gwiazdki na początku ciągu znaków).

![Narzędzia PerfView dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Dostawca dziennika zdarzeń systemu Windows

Pakiet dostawcy [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) wysyła dane wyjściowe dziennika do dziennika zdarzeń systemu Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[Przeciążenia addeventlog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazywanie `EventLogSettings` lub minimalny poziom dziennika.

::: moniker-end

### <a name="tracesource-provider"></a>Dostawca TraceSource

Pakiet dostawcy [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa <xref:System.Diagnostics.TraceSource> bibliotek i dostawców.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) umożliwiają przekazywanie danych w przełączniku źródłowym i odbiorniku śledzenia.

Aby można było korzystać z tego dostawcy, aplikacja musi działać na .NET Framework (a nie na platformie .NET Core). Dostawca może kierować komunikaty do różnych odbiorników, [](/dotnet/framework/debug-trace-profile/trace-listeners)takich jak <xref:System.Diagnostics.TextWriterTraceListener> używane w przykładowej aplikacji.

::: moniker range="< aspnetcore-2.0"

Poniższy przykład konfiguruje `TraceSource` dostawcę, który rejestruje `Warning` i wyższych komunikatów do okna konsoli.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a>Dostawca Azure App Service

Pakiet [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) Provider zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji Azure App Service i w usłudze [BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage. Pakiet dostawcy jest dostępny dla aplikacji przeznaczonych dla platformy .NET Core 1,1 lub nowszej.

::: moniker range=">= aspnetcore-2.0"

W przypadku określania wartości .NET Core należy zwrócić uwagę na następujące kwestie:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Pakiet dostawcy jest zawarty w ASP.NET Core [Microsoft. AspNetCore. All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Pakiet dostawcy nie jest uwzględniony w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Aby użyć dostawcy, zainstaluj pakiet.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Jeśli element docelowy .NET Framework lub odwołuje się `Microsoft.AspNetCore.App` do pakietu, Dodaj pakiet dostawcy do projektu. Wywołaj `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> Przeciążenie umożliwia <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>przekazywanie. Obiekt Settings może przesłonić ustawienia domyślne, takie jak szablon danych wyjściowych rejestrowania, nazwa obiektu BLOB i limit rozmiaru pliku. (*Szablon danych wyjściowych* to szablon wiadomości, który jest stosowany do wszystkich dzienników oprócz tego, co jest dostępne `ILogger` w wywołaniu metody).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

Po wdrożeniu w aplikacji App Service, aplikacja będzie przestrzegać ustawień w sekcji [dzienniki App Service](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) na stronie **App Service** Azure Portal. Po zaktualizowaniu następujących ustawień zmiany zaczynają obowiązywać natychmiast, bez konieczności ponownego uruchomienia lub ponownej wdrożenia aplikacji.

* **Rejestrowanie aplikacji (system plików)**
* **Rejestrowanie aplikacji (BLOB)**

Domyślną lokalizacją plików dziennika jest folder *D\\:\\Home\\LogFiles* , a domyślna nazwa pliku to *Diagnostics-YYYYMMDD. txt*. Domyślny limit rozmiaru pliku wynosi 10 MB, a domyślna maksymalna liczba zachowywanych plików to 2. Domyślną nazwą obiektu BLOB jest *{App-Name} {timestamp}/yyyy/mm/dd/hh/{GUID}-applicationLog.txt*.

Dostawca działa tylko wtedy, gdy projekt jest uruchomiony w środowisku platformy Azure. Nie ma ono wpływu, gdy projekt jest uruchamiany lokalnie&mdash;, nie zapisuje w plikach lokalnych ani w lokalnym magazynie programistycznym dla obiektów BLOB.

#### <a name="azure-log-streaming"></a>Przesyłanie strumieniowe dzienników Azure

Usługa przesyłania strumieniowego w usłudze Azure log umożliwia wyświetlanie dziennika aktywności w czasie rzeczywistym z:

* Serwer aplikacji
* Serwer sieci Web
* Śledzenie nieudanych żądań

Aby skonfigurować przesyłanie strumieniowe dzienników Azure:

* Przejdź do strony **dzienników App Service** ze strony portalu aplikacji.
* Ustaw **Rejestrowanie aplikacji (system plików)** na **włączone**.
* Wybierz **poziom**dziennika.

Przejdź do strony **strumień dziennika** , aby wyświetlić komunikaty aplikacji. Są one rejestrowane przez aplikację za pomocą `ILogger` interfejsu.

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Rejestrowanie śledzenia Application Insights platformy Azure

Pakiet [Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) Provider zapisuje dzienniki na platformie Azure Application Insights. Application Insights to usługa, która monitoruje aplikację internetową i udostępnia narzędzia do wykonywania zapytań i analizowania danych telemetrycznych. Jeśli używasz tego dostawcy, możesz wysyłać zapytania i analizować dzienniki przy użyciu narzędzi Application Insights.

Dostawca rejestrowania jest dołączony jako zależność [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), który jest pakietem, który zapewnia wszystkie dostępne dane telemetryczne dla ASP.NET Core. Jeśli używasz tego pakietu, nie musisz instalować pakietu dostawcy.

Nie używaj&mdash;pakietu [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) , który jest przeznaczony dla ASP.NET 4. x.

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Przegląd Application Insights](/azure/application-insights/app-insights-overview)
* [Application Insights dla ASP.NET Core aplikacji](/azure/azure-monitor/app/asp-net-core) — Zacznij tutaj, jeśli chcesz zaimplementować cały zakres Application Insights telemetrii wraz z rejestrowaniem.
* [ApplicationInsightsLoggerProvider for .NET Core ILogger](/azure/azure-monitor/app/ilogger) — Rozpocznij tutaj, jeśli chcesz zaimplementować dostawcę rejestrowania bez pozostałej części telemetrii Application Insights.
* [Karty rejestrowania Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).
* [Zainstaluj, skonfiguruj i zainicjuj samouczek Application INSIGHTS SDK](/learn/modules/instrument-web-app-code-with-application-insights) — interaktywny w witrynie Microsoft Learn.
::: moniker-end

## <a name="third-party-logging-providers"></a>Dostawcy rejestrowania innych firm

Platformy rejestrowania innych firm, które współpracują z ASP.NET Core:

* [ELMAH.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([Repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([Repozytorium GitHub](https://github.com/mperdeck/jsnlog))
* [KissLog.NET](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](https://loggr.net/) ([Repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLOG](https://nlog-project.org/) ([Repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([Repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([Repozytorium GitHub](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Repozytorium GitHub](https://github.com/googleapis/google-cloud-dotnet))

Niektóre struktury innych firm mogą wykonywać [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Korzystanie z struktury innej firmy jest podobne do korzystania z jednego z wbudowanych dostawców:

1. Dodaj pakiet NuGet do projektu.
1. Wywołaj `ILoggerFactory`metodę.

Aby uzyskać więcej informacji, zobacz dokumentację każdego dostawcy. Dostawcy rejestrowania innych firm nie są obsługiwani przez firmę Microsoft.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/logging/loggermessage>
