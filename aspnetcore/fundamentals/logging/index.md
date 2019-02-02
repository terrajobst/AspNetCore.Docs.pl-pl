---
title: Rejestrowanie w programie ASP.NET Core
author: tdykstra
description: Więcej informacji na temat struktury rejestrowania w programie ASP.NET Core. Odnajdywanie dostawcy wbudowane funkcje rejestrowania i Dowiedz się więcej na temat popularnych dostawców innych firm.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/14/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 81620f0c844f3dbb1a2da0e9f1c319f87d9790b6
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667703"
---
# <a name="logging-in-aspnet-core"></a>Rejestrowanie w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra)

Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm. W tym artykule przedstawiono sposób korzystania z interfejsu API rejestrowania za pomocą wbudowanych dostawców.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Dodawanie dostawcy

Dostawcy logowania Wyświetla lub są przechowywane dzienniki. Na przykład konsola dostawca Wyświetla dzienniki konsoli i dostawcy usługi Azure Application Insights przechowuje je w usłudze Azure Application Insights. Dzienniki mogą być wysyłane do wielu miejsc docelowych, dodając wielu dostawców.

::: moniker range=">= aspnetcore-2.0"

Aby dodać dostawcę, należy wywołać dostawcy `Add{provider name}` metody rozszerzenia w *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

Wywołania szablonu projektu domyślnego <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> metodę rozszerzenia, który dodaje następujących dostawców rejestrowania:

* Konsola
* Debugowanie
* EventSource (począwszy od programu ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Jeśli używasz `CreateDefaultBuilder`, domyślnych dostawców można zastąpić własnymi wartościami. Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, i dostawców, o których chcesz dodać.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Można użyć dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy w wystąpieniu <xref:Microsoft.Extensions.Logging.ILoggerFactory>:

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

Platforma ASP.NET Core [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) zapewnia `ILoggerFactory` wystąpienia. `AddConsole` i `AddDebug` metody rozszerzenia są zdefiniowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów. Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` jest metoda w wystąpieniu dostawcy.

> [!NOTE]
> [Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) dodaje rejestrowania dostawców w `Startup.Configure` metody. Aby uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, należy dodać rejestrowania dostawców w `Startup` konstruktora klasy.

::: moniker-end

Dowiedz się więcej o [wbudowane funkcje rejestrowania dostawców](#built-in-logging-providers) i [rejestrowania innych dostawców](#third-party-logging-providers) w dalszej części artykułu.

## <a name="create-logs"></a>Twórz dzienniki

Pobierz <xref:Microsoft.Extensions.Logging.ILogger`1> obiekt z DI.

::: moniker range=">= aspnetcore-2.0"

Poniższy przykład kontrolera tworzy `Information` i `Warning` dzienniki. *Kategorii* jest `TodoApiSample.Controllers.TodoController` (w pełni kwalifikowaną nazwę klasy z `TodoController` w przykładowej aplikacji):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

W poniższym przykładzie stron Razor utworzy dzienników za pomocą `Information` jako *poziom* i `TodoApiSample.Pages.AboutModel` jako *kategorii*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Poprzedni przykład tworzy dzienniki za pomocą `Information` i `Warning` jako *poziom* i `TodoController` klasy *kategorii*. 

::: moniker-end

Dziennik *poziom* wskazuje ważność rejestrowane zdarzenia. Dziennik *kategorii* jest ciągiem, który jest skojarzony z każdym dzienniku. `ILogger<T>` Wystąpienie tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` kategorię. [Poziomy](#log-level) i [kategorie](#log-category) omówiona bardziej szczegółowo w dalszej części tego artykułu. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Twórz dzienniki przy uruchamianiu

Zapisywanie dzienników `Startup` klasy, należy umieścić `ILogger` parametru w sygnatury konstruktora:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,19,26)]

### <a name="create-logs-in-program"></a>Twórz dzienniki w programie

Zapisywanie dzienników `Program` klasy, Uzyskaj `ILogger` wystąpienia DI:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Nie metody asynchronicznej rejestratora

Rejestrowanie powinno być tak szybko, że nie jest wart spadek wydajności kodu asynchronicznego. Jeśli magazyn danych rejestrowania jest powolne, nie zapisanie w nim bezpośrednio. Należy wziąć pod uwagę początkowo zapisywanie komunikatów dziennika do szybkiego magazynu, a następnie przenieść je do magazynu powolne później. Na przykład Zaloguj się do kolejki komunikatów, która ma odczytu utrwalana w magazynie powolne przez inny proces.

## <a name="configuration"></a>Konfiguracja

Rejestrowanie dostawcy konfiguracji jest dostarczane przez przynajmniej jednego dostawcy konfiguracji:

* Formaty plików (INI, JSON i XML).
* Argumenty wiersza polecenia.
* Zmienne środowiskowe.
* Obiekty .NET w pamięci.
* Niezaszyfrowane [Menedżera klucz tajny](xref:security/app-secrets) magazynu.
* Użytkownik zaszyfrowanych przechowywanych informacji, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).
* Dostawcy niestandardowi (zainstalowane lub utworzone).

Na przykład konfiguracja rejestrowania są często dostarczane przez `Logging` części plików ustawień aplikacji. W poniższym przykładzie pokazano zawartość typowej *appsettings. Development.JSON* pliku:

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

`Logging` Właściwość może mieć `LogLevel` dziennika właściwości dostawcy (konsoli znajduje się) i innych źródeł.

`LogLevel` Właściwości `Logging` Określa minimalny [poziom](#log-level) logowania dla wybranych kategorii. W tym przykładzie `System` i `Microsoft` kategorie dziennika na `Information` poziom, a wszystkie pozostałe rejestrowania `Debug` poziom.

Inne właściwości w obszarze `Logging` Określ rejestrowania dostawców. W przykładzie występuje dla dostawcy konsoli. Jeśli dostawca obsługuje [dziennika zakresy](#log-scopes), `IncludeScopes` wskazuje, czy są włączone. Właściwość dostawcy (takich jak `Console` w przykładzie) mogą także określić `LogLevel` właściwości. `LogLevel` w obszarze dostawcy określa poziomy dziennika dla tego dostawcy.

Jeśli nie określono poziomy w `Logging.{providername}.LogLevel`, że zastępują one nic w `Logging.LogLevel`.

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

`LogLevel` klucze reprezentują nazwy dziennika. `Default` Klucz ma zastosowanie do dzienników, które nie zostały jawnie wymienione. Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danego dziennika.

::: moniker-end

Aby uzyskać informacje dotyczące implementowania dostawcy konfiguracji, zobacz <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Przykładowe dane wyjściowe z rejestrowania

Za pomocą przykładowego kodu pokazano w poprzedniej sekcji dzienniki są wyświetlane w konsoli, gdy aplikacja jest uruchamiana z poziomu wiersza polecenia. Oto przykład danych wyjściowych konsoli:

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

Poprzedni dzienniki wygenerowane przez wysłał żądanie HTTP Get do przykładowej aplikacji pod `http://localhost:5000/api/todo/0`.

Oto przykład tego samego dzienników, w jakiej występują w oknie Debugowanie Uruchom przykładową aplikację w programie Visual Studio:


```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Dzienniki, które są tworzone przez `ILogger` wywołania pokazano w poprzedniej sekcji zaczyna się od "TodoApi.Controllers.TodoController". Dzienniki, które zaczynają się od "Microsoft" kategorie pochodzą z kodu struktury programu ASP.NET Core. Platforma ASP.NET Core i kodu aplikacji korzystają z tego samego interfejsu API rejestrowania i dostawców.

W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.

## <a name="nuget-packages"></a>Pakiety NuGet

`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), i znajdują się w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Kategoria dziennika

Gdy `ILogger` obiekt zostanie utworzony, *kategorii* określono dla niej. Tej kategorii jest uwzględnione w każdej wiadomości dziennika utworzone przez to wystąpienie `Ilogger`. Kategoria może być dowolnym ciągiem, ale Konwencji jest użycie nazwy klasy, takie jak "TodoApi.Controllers.TodoController".

Użyj `ILogger<T>` można pobrać `ILogger` wystąpienie, które korzysta z w pełni kwalifikowana nazwa typu z `T` kategorię:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Aby jawnie określić kategorię, należy wywołać `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` jest równoważne z wywoływaniem `CreateLogger` z w pełni kwalifikowana nazwa typu z `T`.

## <a name="log-level"></a>Poziom dziennika

Każdy dziennik Określa <xref:Microsoft.Extensions.Logging.LogLevel> wartość. Poziom dziennika wskazuje ważność lub ważność. Na przykład można napisać `Information` logowania, jeśli metoda zakończy się normalnie, a jeśli tak, to a `Warning` Zaloguj się, gdy metoda zwróci wartość *404 Nie znaleziono* kod stanu.

Poniższy kod tworzy `Information` i `Warning` dzienników:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

W poprzednim kodzie pierwszy parametr jest [identyfikator zdarzenia dziennika](#log-event-id). Drugi parametr jest szablon wiadomości z symboli zastępczych dla wartości argumentów dostarczone przez pozostałe parametry metody. Parametry metody są wyjaśnione w [komunikatu sekcji szablonu](#log-message-template) w dalszej części tego artykułu.

Metody, które obejmują poziom w nazwie metody logowania (na przykład `LogInformation` i `LogWarning`) są [metody rozszerzenia dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Wywoływanie tych metod `Log` metody, która przyjmuje `LogLevel` parametru. Możesz wywołać `Log` bezpośrednio zamiast jednej z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane. Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.Logging.ILogger> i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

Platforma ASP.NET Core definiuje następujące poziomy dziennika, w tym miejscu uporządkowane od najniższej do najwyższej ważności.

* Śledzenie = 0

  Aby uzyskać informacje, które są zazwyczaj przydatne tylko w przypadku debugowania. Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym. *Domyślnie wyłączone.*

* Debugowanie = 1

  Aby uzyskać informacje, które mogą być użyteczne podczas programowania i debugowania. Przykład: `Entering method Configure with flag set to true.` Włącz `Debug` poziom dzienniki w środowisku produkcyjnym, tylko wtedy, gdy rozwiązywania problemów, z powodu dużej liczby dzienników.

* Informacje o = 2

  Do śledzenia ogólny przebieg aplikacji. Te dzienniki są zazwyczaj mają niektóre wartości długoterminowe. Przykład: `Request received for path /api/todo`

* Ostrzeżenie = 3

  Nietypowe lub nieoczekiwanych zdarzeń w usłudze flow aplikacji. Mogą one zawierać błędy lub inne warunki, które nie powodują aplikacji zatrzymać, ale może być konieczne należy zbadać. Obsługiwane wyjątki są spójne użyj `Warning` poziom dziennika. Przykład: `FileNotFoundException for file quotes.txt.`

* Błąd = 4

  Błędy i wyjątki, które nie mogą być obsługiwane. Te komunikaty wskazują wystąpił błąd podczas bieżącego działania lub operacji (takie jak bieżące żądanie HTTP), nie wystąpił błąd całej aplikacji. Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`

* Krytyczne = 5

  Dla błędów, które wymagają natychmiastowej uwagi. Przykłady: utratą danych, brak miejsca na dysku.

Umożliwia kontrolowanie, ile dane wyjściowe dziennika są zapisywane na nośniku określonego poziomu dziennika lub wyświetlić okno. Na przykład:

* W środowisku produkcyjnym, należy wysłać `Trace` za pośrednictwem `Information` poziomu do woluminu magazynu danych. Wyślij `Warning` za pośrednictwem `Critical` danych wartość przechowywania.
* Podczas tworzenia aplikacji, Wyślij `Warning` za pośrednictwem `Critical` do konsoli i Dodaj `Trace` za pośrednictwem `Information` podczas rozwiązywania problemów.

[Filtrowanie dziennika](#log-filtering) sekcję w dalszej części tego artykułu opisano sposób kontrolowania poziomy dziennika, który dostawca obsługuje.

Platforma ASP.NET Core zapisuje dzienniki zdarzeń framework. Przykłady dzienników wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomie, więc nie `Debug` lub `Trace` poziom Dzienniki zostały utworzone. Oto przykład dzienniki konsoli produkowanych przez przykładową aplikację skonfigurowana do wyświetlania `Debug` dzienników:

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

## <a name="log-event-id"></a>Identyfikator zdarzenia logowania

Można określić w każdym dzienniku *identyfikator zdarzenia*. Przykładowa aplikacja robi to przy użyciu zdefiniowanej lokalnie `LoggingEvents` klasy:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Identyfikator zdarzenia kojarzy zestaw zdarzeń. Na przykład wszystkie dzienniki związane z wyświetlanie listy elementów na stronie może być 1001.

Dostawcy logowania mogą być przechowywane identyfikator zdarzenia w polu Identyfikator w komunikacie rejestrowania lub wcale. Dostawca debugowania nie Wyświetla identyfikatory zdarzeń. Konsola dostawca przedstawia identyfikatory zdarzeń w nawiasach kwadratowych po kategorii:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Szablon wiadomości dziennika

Każdy dziennik Określa szablon wiadomości. Szablon wiadomości mogą zawierać symbole zastępcze, dla których są podane argumenty. Użyj nazw symboli zastępczych, nie liczby.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Kolejność symboli zastępczych, nie ich nazwy, określa parametry, które służą do zapewniania ich wartości. W poniższym kodzie Zauważ, że nazwy parametrów są poza kolejnością w szablonie wiadomości:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Ten kod tworzy komunikat dziennika przy użyciu wartości parametrów w sekwencji:

```
Parameter values: parm1, parm2
```

Struktury rejestrowania działa w ten sposób, aby zaimplementować rejestrowania dostawców [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Same argumenty są przekazywane do systemu rejestrowania, a nie tylko szablon sformatowany komunikat. Te informacje umożliwiają dostawcom rejestrowania do przechowywania wartości parametrów jako pola. Załóżmy na przykład, rejestratora metody wywołania się następująco:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Jeśli dzienniki jest wysyłana do usługi Azure Table Storage, każda jednostka usługi Azure Table może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytań dotyczących danych dziennika. Zapytania można znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakresu bez analizowania limit czasu wiadomości SMS.

## <a name="logging-exceptions"></a>Rejestrowania wyjątków

Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątek, jak w poniższym przykładzie:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Różnych dostawców obsługiwać informacje o wyjątku na różne sposoby. Oto przykład danych wyjściowych debugowania dostawcy w kodzie pokazanym powyżej.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrowanie dziennika

::: moniker range=">= aspnetcore-2.0"

Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii. Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, dzięki czemu nie uzyskać wyświetlane lub przechowywane.

Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom rejestrowania. Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Tworzenie reguły filtrów w konfiguracji

Kod wywołuje szablon projektu `CreateDefaultBuilder` skonfigurować rejestrowanie dla dostawców konsoli i debugowania. `CreateDefaultBuilder` Metoda konfiguruje również rejestrowania do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu, podobnie do poniższego:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

Dane konfiguracji Określa poziomy minimalna dziennika przez dostawcę i kategorii, jak w poniższym przykładzie:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Ten kod JSON tworzy sześć reguły filtru: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców. Wybrano jedną regułę dla każdego dostawcy podczas `ILogger` obiekt zostanie utworzony.

### <a name="filter-rules-in-code"></a>Reguły filtrowania w kodzie

Poniższy przykład pokazuje, jak zarejestrować reguły filtrów w kodzie:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Drugi `AddFilter` określa dostawcę debugowania przy użyciu jego nazwy. Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typ dostawcy.

### <a name="how-filtering-rules-are-applied"></a>Jak reguły filtrowania zostaną zastosowane.

Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach Tworzenie reguły pokazano w poniższej tabeli. Pierwsze sześć pochodzą przykład konfiguracji i ostatnie dwa pochodzą w przykładzie kodu.

| Wartość liczbowa | Dostawca      | Kategorie, które zaczynają się od...          | Minimalny poziom rejestrowania |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debugowanie         | Wszystkie kategorie                          | Informacje       |
| 2      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Ostrzeżenie           |
| 3      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debugowanie             |
| 4      | Konsola       | Microsoft.AspNetCore.Mvc.Razor          | Błąd             |
| 5      | Konsola       | Wszystkie kategorie                          | Informacje       |
| 6      | Wszyscy dostawcy | Wszystkie kategorie                          | Debugowanie             |
| 7      | Wszyscy dostawcy | System                                  | Debugowanie             |
| 8      | Debugowanie         | Microsoft                               | Śledzenia             |

Gdy `ILogger` obiekt zostanie utworzony, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora. Wszystkie komunikaty napisane przez `ILogger` wystąpienia są filtrowane w zależności od wybranej reguły. Najbardziej określonej reguły dla każdego dostawcy i pary kategoria możliwe jest wybrana w zaufanym dostępne reguły.

Następujące algorytm jest używany dla każdego dostawcy podczas `ILogger` jest tworzona dla danej kategorii:

* Wybierz wszystkie reguły, które odpowiadają przez dostawcę lub jego alias. Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły przy użyciu dostawcy puste.
* W wyniku poprzedniego kroku wybierz reguły z najdłużej dopasowania prefiksu kategorii. Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.
* Jeśli wybrano wiele reguł, **ostatniego** jeden.
* Jeśli nie zaznaczono żadnych reguł, użyj `MinimumLevel`.

Przy użyciu poprzednich listę reguł, załóżmy, że możesz utworzyć `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* W przypadku dostawcy debugowania mają zastosowanie reguły 1, 6 i 8. Reguła 8 jest najbardziej specyficzną, więc to jest zaznaczony.
* W przypadku dostawcy konsoli mają zastosowanie reguły, 3, 4, 5 i 6. Reguła 3 jest bardziej konkretny od pozostałych.

Wynikowy `ILogger` wystąpienia wysyła dzienniki `Trace` poziom i nowsze wersje do debugowania dostawcy. Dzienniki programu `Debug` poziomu i nowszych są wysyłane do dostawcy konsoli.

### <a name="provider-aliases"></a>Aliasy dostawcy

Każdy dostawca definiuje *alias* które mogą być używane w konfiguracji zamiast w pełni kwalifikowana nazwa typu.  W przypadku dostawców wbudowanych należy stosować następujące aliasy:

* Konsola
* Debugowanie
* Dziennik zdarzeń
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Minimalny poziom domyślny

Istnieje minimalne ustawienie poziomie staje się skuteczny tylko wtedy, gdy nie z konfiguracji czy kodu reguły dla danego dostawcy i kategorii. Poniższy przykład pokazuje, jak ustawić minimalny poziom:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Jeśli nie zostanie jawnie ustawiona minimalny poziom, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.

### <a name="filter-functions"></a>Funkcje filtrowania

Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorie, które nie mają zasady przypisane do nich przez konfiguracji czy kodu. Kod w funkcji ma dostęp do typu Dostawca, kategoria i poziom dziennika. Na przykład:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy zapisany na nośniku lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.

`AddConsole` i `AddDebug` metody rozszerzenia, zapewnienia przeciążenia, które akceptują kryteria filtrowania. Następujący przykładowy kod powoduje, że Konsola dostawca ignorowanie dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzonych w ramach.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, co może zawierać funkcji filtrowania w jego `Filter` właściwości. Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.

Aby ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia, należy użyć `WithFilter` — metoda rozszerzenia. Poniższy przykład ogranicza framework dzienniki (kategoria zaczyna się od "Microsoft" lub "System") ostrzeżeń podczas logowania się na poziomie debugowania dla dzienników tworzona przez kod aplikacji.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Aby zapobiec sytuacji, w której wszelkie dzienniki zapisywana, podaj `LogLevel.None` jako minimalny poziom rejestrowania. Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).

`WithFilter` Metody rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet. Metoda ta zwraca nowy `ILoggerFactory` zarejestrowano wystąpienia, która będzie filtrować komunikaty dziennika, przekazywane do wszystkich dostawców rejestratora. Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalny `ILoggerFactory` wystąpienia.

::: moniker-end

## <a name="system-categories-and-levels"></a>Kategorie systemu i poziomy

Poniżej przedstawiono niektóre kategorie używane przez platformy ASP.NET Core i Entity Framework Core, z uwagi na temat co dzienniki można oczekiwać od nich:

| Kategoria                            | Uwagi |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Diagnostyka ogólnego ASP.NET Core. |
| Microsoft.AspNetCore.DataProtection | Klucze, które zostały uznane za znaleziono i używane. |
| Microsoft.AspNetCore.HostFiltering  | Hosty dozwolone. |
| Microsoft.AspNetCore.Hosting        | Ile żądań HTTP potrzebny do ukończenia, a także czas rozpoczęcia pracy. Zestawy startowe, które hostingu zostały załadowane. |
| Microsoft.AspNetCore.Mvc            | Diagnostyka MVC i Razor. Powiązanie modelu, wykonywanie filtru, kompilacja widoku wybór akcji. |
| Microsoft.AspNetCore.Routing        | Trasę odpowiednią dla informacji. |
| Microsoft.AspNetCore.Server         | Rozpoczęcie połączenia zatrzymać i zachować aktywności odpowiedzi. Informacje o certyfikacie protokołu HTTPS. |
| Microsoft.AspNetCore.StaticFiles    | Obsługiwane pliki. |
| Microsoft.EntityFrameworkCore       | Ogólne Diagnostyka platformy Entity Framework Core. Działanie i konfiguracji wykrywania zmian migracje baz danych. |

## <a name="log-scopes"></a>Zakresy dziennika

 A *zakres* można grupować zestaw operacji logicznej. Ta metoda grupowania może służyć do dołączenia do tych samych danych do każdego dziennika, który jest tworzony jako część zestawu. Na przykład każdy dziennik utworzony jako część przetwarzania transakcji może zawierać identyfikator transakcji.

Zakres jest `IDisposable` typu, który jest zwracany przez <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> metody i trwa do momentu jego usunięcia. Użyj zakresu opakowując wywołania rejestratora w `using` bloku:

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Poniższy kod umożliwia zakresy dla dostawcy konsoli:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.
>
> Aby uzyskać informacji na temat konfigurowania, zobacz [konfiguracji](#configuration) sekcji.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Każdy komunikat dziennika zawiera informacje o określonym zakresie:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Wbudowane funkcje rejestrowania dostawców

ASP.NET Core jest dostarczana w następujących dostawców:

* [Console](#console-provider)
* [Debugowanie](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)

Opcje dla [rejestrowania na platformie Azure](#logging-in-azure) zostały omówione w dalszej części tego artykułu.

Informacje dotyczące rejestrowania strumienia wyjściowego stdout, zobacz <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> i <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

### <a name="console-provider"></a>Konsola dostawcy

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[Przeciążenia AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) pozwalają przekazać minimalny poziom rejestrowania, funkcję filtru i atrybut typu wartość logiczna, która wskazuje, czy zakresy są obsługiwane. Innym rozwiązaniem jest przekazywanie `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomów rejestrowania.

Dostawca konsola ma znaczący wpływ na wydajność i zwykle nie jest przeznaczone do użycia w środowisku produkcyjnym.

Podczas tworzenia nowego projektu w programie Visual Studio, `AddConsole` metoda wygląda następująco:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Ten kod, który odwołuje się do `Logging` części *appSettings.json* pliku:

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

Ustawienia pokazano limit framework dzienniki, aby ostrzeżenia zezwalając aplikacji do logowania na poziomie debugowania, jak wyjaśniono w [filtrowanie dziennika](#log-filtering) sekcji. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

::: moniker-end

Aby wyświetlić rejestrowania danych wyjściowych konsoli, otwórz wiersz polecenia w folderze projektu, a następnie uruchom następujące polecenie:

```console
dotnet run
```

### <a name="debug-provider"></a>Debugowanie dostawcy

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika za pomocą [system.Diagnostics.Debug —](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).

W systemie Linux, tego dostawcy zapisuje dzienniki na */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[Przeciążenia AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) pozwalają przekazać minimalny poziom rejestrowania lub funkcję filtru.

::: moniker-end

### <a name="eventsource-provider"></a>Dostawca źródła zdarzeń

W przypadku aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszym, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń. W Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Dostawca jest dla wielu platform, ale nie są dostępne żadne zdarzenie gromadzenia i wyświetlania narzędzia jeszcze dla systemu Linux lub macOS.

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

Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [narzędzia PerfView](https://github.com/Microsoft/perfview). Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET.

Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj parametry `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy. (Nie przegap gwiazdki na początku ciągu).

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Dostawca dziennika zdarzeń Windows

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[Przeciążenia AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazanej `EventLogSettings` lub minimalny poziom rejestrowania.

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource dostawcy

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu <xref:System.Diagnostics.TraceSource> bibliotek i dostawców.

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

[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) pozwalają przekazać przełącznik źródła i odbiornika śledzenia.

Aby użyć tego dostawcy, aplikacja ma do uruchamiania na .NET Framework (a nie .NET Core). Dostawca komunikaty można kierować do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takie jak <xref:System.Diagnostics.TextWriterTraceListener> używane w przykładowej aplikacji.

::: moniker range="< aspnetcore-2.0"

Poniższy przykład umożliwia skonfigurowanie `TraceSource` dostawcy, która rejestruje `Warning` i wyższych komunikaty w oknie konsoli.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a>Rejestrowanie na platformie Azure

Aby uzyskać informacji na temat rejestrowania na platformie Azure zobacz następujące sekcje:

* [Dostawca usługi Azure App Service](#azure-app-service-provider)
* [Przesyłanie strumieniowe dzienników platformy Azure](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [Rejestrowanie śledzenia w usłudze Azure Application Insights](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a>Dostawca usługi Azure App Service

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage. Pakiet dostawcy jest dostępna dla aplikacji przeznaczonych dla platformy .NET Core 1.1 lub nowszej.

::: moniker range=">= aspnetcore-2.0"

Jeśli przeznaczony dla platformy .NET Core, pamiętaj o następujących kwestiach:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Pakiet dostawcy znajduje się w programie ASP.NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Pakiet dostawcy nie jest zawarty w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Aby użyć dostawcy, zainstaluj pakiet.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* Nie jawnie wywołać <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>. Dostawca automatycznie staje się dostępny do aplikacji, gdy aplikacja jest wdrożona w usłudze Azure App Service.

Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj pakiet dostawcy do projektu. Wywoływanie `AddAzureWebAppDiagnostics` na <xref:Microsoft.Extensions.Logging.ILoggerFactory> wystąpienie:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> Przeciążenia umożliwia przekazywanie w <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. Obiekt ustawień można zastąpić domyślne ustawienia, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i limit rozmiaru pliku. (*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników oprócz co to jest dostarczana z `ILogger` wywołania metody.)

Podczas wdrażania aplikacji usługi App Service aplikacja honoruje ustawienia w [dzienniki diagnostyczne](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) części **usługi App Service** strony w witrynie Azure Portal. Jeśli te ustawienia zostaną zaktualizowane, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchomienia lub ponownego wdrażania aplikacji.

![Ustawienia rejestrowania platformy Azure](index/_static/azure-logging-settings.png)

Domyślną lokalizacją dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folder i nazwę pliku domyślny jest *yyyymmdd.txt diagnostyki*. Domyślnego limitu rozmiaru pliku to 10 MB, a maksymalną domyślną liczbę plików, które przechowywane jest 2. Domyślna nazwa obiektu blob to *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Aby uzyskać więcej informacji na temat zachowania domyślnego, zobacz <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.

Dostawca działa tylko w przypadku, gdy projekt jest uruchamiany w środowisku platformy Azure. Nie ma wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisywać pliki lokalne lub lokalnym magazynem projektowym dla obiektów blob.

::: moniker-end

### <a name="azure-log-streaming"></a>Przesyłanie strumieniowe dzienników platformy Azure

Przesyłanie strumieniowe dzienników platformy Azure umożliwia wyświetlanie dzienników aktywności w czasie rzeczywistym z:

* Serwer aplikacji
* Serwer sieci web
* Śledzenie żądań zakończonych niepowodzeniem

Aby skonfigurować, przesyłanie strumieniowe dzienników platformy Azure:

* Przejdź do **dzienniki diagnostyczne** strony na stronie portalu swojej aplikacji.
* Ustaw **rejestrowanie aplikacji (system plików)** do **na**.

![Strona portalu dzienniki diagnostyczne platformy Azure](index/_static/azure-diagnostic-logs.png)

Przejdź do **przesyłanie strumieniowe dzienników** strony, aby wyświetlić komunikaty aplikacji. Logowanie przeprowadzono za pomocą aplikacji za pośrednictwem `ILogger` interfejsu.

![Przesyłanie strumieniowe dzienników aplikacji z portalu Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Rejestrowanie śledzenia w usłudze Azure Application Insights

Zestaw SDK usługi Application Insights można zbierać i zgłaszać dzienniki generowane przez infrastrukturę rejestrowania platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Omówienie usługi Application Insights](/azure/application-insights/app-insights-overview)
* [Usługa Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [Rejestrowanie kart w usłudze Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).

::: moniker-end

## <a name="third-party-logging-providers"></a>Rejestrowanie innych dostawców

Struktury rejestrowania innych firm, które działają z platformą ASP.NET Core:

* [elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-extensions-logging))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repozytorium Github](https://github.com/googleapis/google-cloud-dotnet))

Niektóre środowiska innych producentów mogą wykonywać [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Za pomocą środowiska innych producentów są podobne do przy użyciu jednej z wbudowanych dostawców:

1. Dodaj pakiet NuGet do projektu.
1. Wywołaj `ILoggerFactory`.

Aby uzyskać więcej informacji można znaleźć w temacie każdy dostawca dokumentacji. Rejestrowanie innych firm nie są obsługiwani przez firmę Microsoft.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/logging/loggermessage>
