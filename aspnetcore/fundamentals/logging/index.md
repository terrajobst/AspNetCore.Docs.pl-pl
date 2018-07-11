---
title: Rejestrowanie w programie ASP.NET Core
author: ardalis
description: Więcej informacji na temat struktury rejestrowania w programie ASP.NET Core. Odnajdywanie dostawcy wbudowane funkcje rejestrowania i Dowiedz się więcej na temat popularnych dostawców innych firm.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: dde01129bb7ea29544c4c416dfe9b5522a738d01
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938488"
---
# <a name="logging-in-aspnet-core"></a>Rejestrowanie w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra)

Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców logowania. Wbudowani dostawcy umożliwiają wysyłanie dzienników do jednego lub więcej miejsc docelowych, a w ramach rejestrowania innych firm, możesz podłączyć. W tym artykule pokazano, jak korzystać z interfejsu API wbudowane funkcje rejestrowania i dostawców w kodzie.

::: moniker range=">= aspnetcore-2.0"

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

Instrukcje dotyczące rejestrowania strumienia wyjściowego stdout w przypadku hostowania za pomocą programu IIS, zobacz <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Aby uzyskać informacji na temat rejestrowania strumienia wyjściowego stdout w usłudze Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

## <a name="how-to-create-logs"></a>Jak utworzyć dzienników

Aby utworzyć dzienników, należy zaimplementować [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) obiektu z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Następnie wywołań metod rejestrowania dla tego obiektu rejestratora:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

W tym przykładzie tworzy dzienniki za pomocą `TodoController` klasy *kategorii*. Kategorie są wyjaśnione [w dalszej części tego artykułu](#log-category).

Platformy ASP.NET Core nie zapewnia z asynchronicznej metody rejestratora ponieważ rejestrowanie powinny być rozważnie, czy nie jest warte użycia async. Jeśli jesteś w sytuacji, w przypadku, gdy nie jest spełniony, należy rozważyć zmianę sposobu logowania. Jeśli magazyn danych jest powolne, najpierw zapisać komunikaty dziennika do szybkiego magazynu, a następnie później przenieść je do magazynu powolne. Na przykład Zaloguj się do kolejki komunikatów, która ma odczytu utrwalana w magazynie powolne przez inny proces.

## <a name="how-to-add-providers"></a>Jak dodać dostawców

::: moniker range=">= aspnetcore-2.0"

Dostawcy logowania przyjmuje komunikaty, utworzonych za pomocą `ILogger` obiektów, wyświetla i przechowuje je. Na przykład konsola dostawca wyświetla komunikaty na konsoli i dostawcy usługi Azure App Service można przechowywać w usłudze Azure blob storage.

Aby korzystać z dostawcy, należy wywołać dostawcy `Add<ProviderName>` metody rozszerzenia w *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Domyślny szablon projektu umożliwia rejestrowanie za pomocą [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metody:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dostawcy logowania przyjmuje komunikaty, utworzonych za pomocą `ILogger` obiektów, wyświetla i przechowuje je. Na przykład konsola dostawca wyświetla komunikaty na konsoli i dostawcy usługi Azure App Service można przechowywać w usłudze Azure blob storage.

Można użyć dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy w wystąpieniu [element ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak pokazano w poniższym przykładzie:

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

Platforma ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) zapewnia `ILoggerFactory` wystąpienia. `AddConsole` i `AddDebug` metody rozszerzenia są zdefiniowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów. Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` jest metoda w wystąpieniu dostawcy. 

> [!NOTE]
> [Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) dodaje rejestrowania dostawców w `Startup.Configure` metody. Jeśli chcesz uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, Dodaj rejestrowania dostawców w `Startup` konstruktora klasy.

::: moniker-end

Znajdziesz informacje o poszczególnych [wbudowane funkcje rejestrowania dostawcy](#built-in-logging-providers) opinii i linkami do [rejestrowania innych dostawców](#third-party-logging-providers) w dalszej części artykułu.

## <a name="settings-file-configuration"></a>Ustawienia pliku konfiguracji

Każdego z powyższych przykładach w [sposobu dodawania dostawcy](#how-to-add-providers) sekcji ładuje rejestrowania dostawcy konfiguracji z `Logging` części plików ustawień aplikacji. W poniższym przykładzie pokazano zawartość typowej *appsettings. Development.JSON* pliku:

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
      "IncludeScopes": "true"
    }
  }
}
```

`LogLevel` klucze reprezentują nazwy dziennika. `Default` Klucz ma zastosowanie do dzienników, które nie zostały jawnie wymienione. Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danego dziennika. Dziennik klucze tego zestawu `IncludeScopes` (`Console` w przykładzie), określ, jeśli [dziennika zakresy](#log-scopes) są włączone dla wskazanych dziennika.

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

## <a name="sample-logging-output"></a>Przykładowe dane wyjściowe z rejestrowania

Za pomocą przykładowego kodu pokazano w poprzedniej sekcji zobaczysz dzienniki w konsoli po uruchomieniu z wiersza polecenia. Oto przykład danych wyjściowych konsoli:

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

Te dzienniki zostały utworzone, przechodząc do `http://localhost:5000/api/todo/0`, która wyzwala wykonanie obu `ILogger` wywołania pokazano w poprzedniej sekcji.

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

Dzienniki, które zostały utworzone przez `ILogger` wywołania pokazano w poprzedniej sekcji zaczyna się od "TodoApi.Controllers.TodoController". Dzienniki, które zaczynają się od "Microsoft" kategorie pochodzą z platformą ASP.NET Core. ASP.NET Core, sama i kodu aplikacji korzystają z tego samego interfejsu API rejestrowania i tego samego dostawcy rejestrowania.

W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.

## <a name="nuget-packages"></a>Pakiety NuGet

`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), i znajdują się w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Kategoria dziennika

A *kategorii* jest dołączany do każdego dziennika, które tworzysz. Określ kategorię, po utworzeniu `ILogger` obiektu. Kategoria może być dowolnym ciągiem, ale Konwencję polega na użyciu w pełni kwalifikowana nazwa klasy, w którym są zapisywane dzienniki. Na przykład: "TodoApi.Controllers.TodoController".

Można określić kategorię, która jako ciąg lub użyć metody rozszerzenia pochodzący z tej kategorii z typu. Aby określić kategorię, która jako ciąg znaków, należy wywołać `CreateLogger` na `ILoggerFactory` wystąpienia, jak pokazano poniżej.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

W większości przypadków, będzie łatwiejszy w obsłudze `ILogger<T>`, jak w poniższym przykładzie.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Jest to równoważne z wywoływaniem `CreateLogger` z w pełni kwalifikowana nazwa typu z `T`.

## <a name="log-level"></a>Poziom dziennika

Zawsze możesz pisać dziennika, należy określić jego [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Poziom dziennika wskazuje stopień ważności lub ważności. Na przykład można napisać `Information` logowania, jeśli metoda zakończy się normalnie, `Warning` Zaloguj się, gdy metoda zwróci kod zwrotny 404, a moduł `Error` rejestrowania podczas catch nieoczekiwany wyjątek.

W poniższym przykładzie kodu nazwy metod (na przykład `LogWarning`) określ poziom dziennika. Pierwszy parametr jest [identyfikator zdarzenia dziennika](#log-event-id). Drugi parametr jest [szablon wiadomości z](#log-message-template) przy użyciu symboli zastępczych dla wartości argumentów dostarczone przez pozostałe parametry metody. Parametry metody zostały omówione bardziej szczegółowo w dalszej części tego artykułu.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Metody dziennika, które obejmują poziom w nazwie metody są [metody rozszerzenia dla ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). W tle wywołania tych metod `Log` metody, która przyjmuje `LogLevel` parametru. Możesz wywołać `Log` bezpośrednio zamiast jednej z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane. Aby uzyskać więcej informacji, zobacz [interfejsu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definiuje następujące [poziomy dziennika](/dotnet/api/microsoft.extensions.logging.loglevel), uporządkowanych w tym miejscu, z najmniejszą ważność do najwyższego.

* Śledzenie = 0

  Aby uzyskać informacje, które jest przydatna tylko dla deweloperów, debugowanie problemu. Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym. *Domyślnie wyłączone.* Przykład: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debugowanie = 1

  Aby uzyskać informacje, który ma krótkoterminowej przydatność podczas opracowywania i debugowania. Przykład: `Entering method Configure with flag set to true.` zwykle nie włączysz `Debug` poziom dzienniki w środowisku produkcyjnym, chyba że występuje problem z powodu dużej liczby dzienników.

* Informacje o = 2

  Do śledzenia ogólnego przepływu w aplikacji. Te dzienniki są zazwyczaj mają niektóre wartości długoterminowe. Przykład: `Request received for path /api/todo`

* Ostrzeżenie = 3

  Nietypowe lub nieoczekiwanych zdarzeń w usłudze flow aplikacji. Mogą one zawierać błędy lub inne warunki, które nie powodują przerwania aplikacji, ale która może być konieczne należy zbadać. Obsługiwane wyjątki są spójne użyj `Warning` poziom dziennika. Przykład: `FileNotFoundException for file quotes.txt.`

* Błąd = 4

  Błędy i wyjątki, które nie mogą być obsługiwane. Te komunikaty wskazują wystąpił błąd podczas bieżącego działania lub operacji (takie jak bieżące żądanie HTTP), nie wystąpił błąd całej aplikacji. Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`

* Krytyczne = 5

  Dla błędów, które wymagają natychmiastowej uwagi. Przykłady: utratą danych, brak miejsca na dysku.

Poziom dziennika można użyć do kontrolowania, jak dużo danych wyjściowych dziennika są zapisywane na nośniku określonego lub wyświetlić okno. Na przykład w środowisku produkcyjnym możesz zechcieć wszystkie dzienniki z `Information` poziomu i niższe, aby przejść do magazynu danych woluminu oraz wszystkie dzienniki z `Warning` poziomu lub nowszej, aby przejść do magazynu danych wartości. Podczas tworzenia aplikacji, zwykle może wysyłać dzienniki `Warning` lub wyższej ważności do konsoli. Jeśli potrzebujesz rozwiązać problem, można dodać, a następnie `Debug` poziom. [Filtrowanie dziennika](#log-filtering) sekcję w dalszej części tego artykułu opisano sposób kontrolowania poziomy dziennika, który dostawca obsługuje.

Zapisuje w ramach platformy ASP.NET Core `Debug` poziom dzienniki zdarzeń framework. Przykłady dzienników wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomie, więc nie `Debug` poziom dzienniki były wyświetlane. Oto przykład dzienniki konsoli po uruchomieniu aplikacji przykładowej, skonfigurowana do wyświetlania `Debug` i wyższych dzienniki dostawcy konsoli.

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

Zawsze możesz pisać dziennika, możesz określić *identyfikator zdarzenia*. Przykładowa aplikacja robi to przy użyciu zdefiniowanej lokalnie `LoggingEvents` klasy:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Identyfikator zdarzenia jest wartość całkowitą, która umożliwia kojarzenie zestaw zarejestrowanych zdarzeń ze sobą. Na przykład dziennika, aby dodać element do koszyka, może być identyfikator 1000 zdarzeń i dziennik ukończenia zakup może być identyfikator zdarzenia 1001.

W danych wyjściowych rejestrowania identyfikator zdarzenia może być przechowywane w polu lub zawarte w wiadomości SMS, w zależności od dostawcy. Dostawca debugowania nie Wyświetla identyfikatory zdarzeń, ale dostawca konsoli Pokazuje je w nawiasach kwadratowych po kategorii:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Szablon wiadomości dziennika

Każdorazowo, gdy zapisu komunikatu dziennika, należy podać szablon wiadomości. Szablon wiadomości może być ciągiem lub może zawierać nazwanego symboli zastępczych, w której argument wartości są umieszczone. Szablon nie jest ciągiem formatu, a symbole zastępcze powinno się nazywać, nie są numerowane.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Kolejność symboli zastępczych, nie ich nazwy, określa parametry, które służą do zapewniania ich wartości. Jeśli masz następujący kod:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Wynikowy komunikatu dziennika wygląda następująco:

```
Parameter values: parm1, parm2
```

Struktury rejestrowania komunikatów formatowania w ten sposób, aby umożliwić rejestrowanie dostawców, aby zaimplementować [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Ponieważ same argumenty są przekazywane do systemu rejestrowania, a nie tylko szablon sformatowany komunikat rejestrowania dostawców można przechowywać wartości parametrów jako pola oprócz szablon wiadomości. Jeśli masz kierowanie dziennika danych wyjściowych do usługi Azure Table Storage i rejestratora metody wywołania wygląda następująco:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Każda jednostka usługi Azure Table może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytań dotyczących danych dziennika. Możesz znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakresu bez konieczności przeanalizować limit czasu wiadomości SMS.

## <a name="logging-exceptions"></a>Rejestrowania wyjątków

Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątek, jak w poniższym przykładzie:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Różnych dostawców obsługiwać informacje o wyjątku na różne sposoby. Oto przykład danych wyjściowych debugowania dostawcy w kodzie pokazanym powyżej.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrowanie dziennika

::: moniker range=">= aspnetcore-2.0"

Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii. Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, dzięki czemu nie uzyskać wyświetlane lub przechowywane. 

Jeśli chcesz pominąć wszystkie dzienniki, można określić `LogLevel.None` jako minimalny poziom rejestrowania. Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).

**Tworzenie reguły filtrów w konfiguracji**

Szablony projektów tworzenie kodu, który wywołuje `CreateDefaultBuilder` skonfigurować rejestrowanie dla dostawców konsoli i debugowania. `CreateDefaultBuilder` Metoda konfiguruje również rejestrowania do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu, podobnie do poniższego:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Dane konfiguracji Określa poziomy minimalna dziennika przez dostawcę i kategorii, jak w poniższym przykładzie:

[!code-json[](index/sample2/appsettings.json)]

Ten kod JSON tworzy sześć reguły filtrowania, jeden dla dostawcy debugowania, cztery dla dostawcy konsoli oraz jedną, która ma zastosowanie do wszystkich dostawców. Zostanie wyświetlone, pokażemy, jak tylko jeden z tych reguł wybrano dla każdego dostawcy podczas `ILogger` obiekt zostanie utworzony.

**Reguły filtrowania w kodzie**

Można zarejestrować reguły filtrów w kodzie, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Drugi `AddFilter` określa dostawcę debugowania przy użyciu jego nazwy. Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typ dostawcy.

**Jak reguły filtrowania zostaną zastosowane.**

Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach Tworzenie reguły pokazano w poniższej tabeli. Pierwsze sześć pochodzą przykład konfiguracji i ostatnie dwa pochodzą w przykładzie kodu.

| Wartość liczbowa | Dostawcy      | Kategorie, które zaczynają się od...          | Minimalny poziom rejestrowania |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debugowanie         | Wszystkie kategorie                          | Informacje       |
| 2      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Ostrzeżenie           |
| 3      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debugowanie             |
| 4      | Konsola       | Microsoft.AspNetCore.Mvc.Razor          | Błąd             |
| 5      | Konsola       | Wszystkie kategorie                          | Informacje       |
| 6      | Wszyscy dostawcy | Wszystkie kategorie                          | Debugowanie             |
| 7      | Wszyscy dostawcy | System                                  | Debugowanie             |
| 8      | Debugowanie         | Microsoft                               | Śledzenia             |

Po utworzeniu `ILogger` obiekt do zapisywania dzienników, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora. Wszystkie komunikaty napisane przez to `ILogger` obiektu są filtrowane w zależności od wybranej reguły. Najbardziej określonej reguły dla każdego dostawcy i pary kategoria możliwe jest wybrana w zaufanym dostępne reguły.

Następujące algorytm jest używany dla każdego dostawcy podczas `ILogger` jest tworzona dla danej kategorii:

* Wybierz wszystkie reguły, które odpowiadają przez dostawcę lub jego alias. Jeśli nie zostaną znalezione, zaznacz wszystkie reguły przy użyciu dostawcy puste.
* W wyniku poprzedniego kroku wybierz reguły z najdłużej dopasowania prefiksu kategorii. Jeśli nie zostaną znalezione, zaznacz wszystkie reguły, które nie określają kategorii.
* Jeśli wybrano wiele reguł **ostatniego** jeden.
* Jeśli nie zaznaczono żadnych reguł, użyj `MinimumLevel`.

Na przykład, załóżmy, że masz powyższej listy reguł i tworzenia `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* W przypadku dostawcy debugowania mają zastosowanie reguły 1, 6 i 8. Reguła 8 jest najbardziej specyficzną, więc to jest zaznaczony.
* W przypadku dostawcy konsoli mają zastosowanie reguły, 3, 4, 5 i 6. Reguła 3 jest bardziej konkretny od pozostałych.

Po utworzeniu dzienników za pomocą `ILogger` dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" dzienniki z `Trace` poziomu i powyżej zostanie umieszczona dostawcy debugowania i dzienniki `Debug` poziomu i powyżej zostanie umieszczona na dostawcy konsoli.

**Aliasy dostawcy**

Nazwa typu można użyć, aby określić dostawcę konfiguracji, ale każdy dostawca definiuje krótszy *alias* jest łatwiejszy w użyciu. W przypadku dostawców wbudowanych należy stosować następujące aliasy:

- Konsola
- Debugowanie
- Dziennik zdarzeń
- AzureAppServices
- TraceSource
- EventSource

**Minimalny poziom domyślny**

Istnieje minimalne ustawienie poziomie staje się skuteczny tylko wtedy, gdy nie z konfiguracji czy kodu reguły dla danego dostawcy i kategorii. Poniższy przykład pokazuje, jak ustawić minimalny poziom:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Jeśli nie zostanie jawnie ustawiona minimalny poziom, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.

**Funkcje filtrowania**

Można napisać kod w funkcji filtru, aby zastosować reguły filtrowania. Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorie, które nie mają zasady przypisane do nich przez konfiguracji czy kodu. Kod w funkcji ma dostęp do typ dostawcy, kategorii i poziom dziennika, aby zdecydować, czy komunikat powinny być rejestrowane. Na przykład:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy zapisany na nośniku lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.

`AddConsole` i `AddDebug` metody rozszerzenia, zapewnienia przeciążenia, które umożliwiają przekazywanie w kryteriach filtrowania. Następujący przykładowy kod powoduje, że Konsola dostawca ignorowanie dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzonych w ramach.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, co może zawierać funkcji filtrowania w jego `Filter` właściwości. Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.

Możesz ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia przy użyciu `WithFilter` — metoda rozszerzenia. W poniższym przykładzie ogranicza dzienniki framework (kategoria zaczyna się od "Microsoft" lub "System") z ostrzeżeniami, a w dzienniku aplikacji na poziomie debugowania.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Jeśli chcesz użyć filtrowania, aby uniemożliwić wszystkie dzienniki są zapisywane dla danej kategorii, można określić `LogLevel.None` jako minimalny poziom rejestrowania dla tej kategorii. Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).

`WithFilter` Metody rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet. Metoda ta zwraca nowy `ILoggerFactory` zarejestrowano wystąpienia, która będzie filtrować komunikaty dziennika, przekazywane do wszystkich dostawców rejestratora. Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalny `ILoggerFactory` wystąpienia.

::: moniker-end

## <a name="log-scopes"></a>Zakresy dziennika

Można grupować zestaw operacji logicznej w ramach *zakres* Aby dołączyć te same dane do każdego dziennika, który jest tworzony jako część tego zbioru. Na przykład możesz zechcieć każdy dziennik utworzony jako część przetwarzania transakcji, aby uwzględnić ten identyfikator.

Zakres jest `IDisposable` typu, który jest zwracany przez [ILogger.BeginScope&lt;stanu dławienia&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) metody i trwa do momentu jego usunięcia. Możesz użyć zakresu, zawijania z Rejestratora wywołania w `using` zablokować, jak pokazano poniżej:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Poniższy kod umożliwia zakresy dla dostawcy konsoli:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.
>
> `IncludeScopes` mogą być konfigurowane przez *appsettings* plików konfiguracyjnych. Aby uzyskać więcej informacji, zobacz [ustawień pliku konfiguracji](#settings-file-configuration) sekcji.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

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
* [Usługa Azure App Service](#azure-app-service-provider)

### <a name="console-provider"></a>Konsola dostawcy

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli. 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

[Przeciążenia AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) umożliwiają przekazanej minimalny poziom rejestrowania, funkcję filtru i atrybut typu wartość logiczna, która wskazuje, czy zakresy są obsługiwane. Innym rozwiązaniem jest przekazywanie `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomów rejestrowania. 

Jeśli rozważasz Konsola dostawca do użycia w środowisku produkcyjnym, należy pamiętać, ma znaczący wpływ na wydajność.

Podczas tworzenia nowego projektu w programie Visual Studio, `AddConsole` metoda wygląda następująco:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Ten kod, który odwołuje się do `Logging` części *appSettings.json* pliku:

[!code-json[](index/sample//appsettings.json)]

Ustawienia pokazano limit framework dzienniki, aby ostrzeżenia zezwalając aplikacji do logowania na poziomie debugowania, jak wyjaśniono w [filtrowanie dziennika](#log-filtering) sekcji. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

::: moniker-end

### <a name="debug-provider"></a>Debugowanie dostawcy

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika za pomocą [system.Diagnostics.Debug —](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).

W systemie Linux, tego dostawcy zapisuje dzienniki na */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

[Przeciążenia AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) pozwalają przekazać minimalny poziom rejestrowania lub funkcję filtru.

::: moniker-end

### <a name="eventsource-provider"></a>Dostawca źródła zdarzeń

W przypadku aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszej, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń. W Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Dostawca jest dla wielu platform, ale nie są dostępne żadne zdarzenie gromadzenia i wyświetlania narzędzia jeszcze dla systemu Linux lub macOS. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [narzędzia PerfView](https://github.com/Microsoft/perfview). Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET. 

Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj parametry `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy. (Nie przegap gwiazdki na początku ciągu).

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Dostawca dziennika zdarzeń Windows

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

[Przeciążenia AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) umożliwiają przekazanej `EventLogSettings` lub minimalny poziom rejestrowania.

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource dostawcy

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) bibliotek i dostawców.

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

[Przeciążenia AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) pozwalają przekazać przełącznik źródła i odbiornika śledzenia.

Aby użyć tego dostawcy, aplikacja ma do uruchamiania na .NET Framework (a nie .NET Core). Umożliwia dostawcy kierowanie komunikatów w postaci z szeroką gamą [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) używane w przykładowej aplikacji.

Poniższy przykład umożliwia skonfigurowanie `TraceSource` dostawcy, która rejestruje `Warning` i wyższych komunikaty w oknie konsoli.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a>Dostawca usługi Azure App Service

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage. Dostawca jest dostępna wyłącznie dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.

::: moniker range=">= aspnetcore-2.0"

Jeśli przeznaczonych dla platformy .NET Core, nie należy zainstalować pakiet dostawcy lub jawnie wywołać [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics). Dostawca jest automatycznie dostępne dla aplikacji, gdy aplikacja jest wdrożona w usłudze Azure App Service.

Jeśli przeznaczony dla .NET Framework, Dodaj pakiet dostawcy do projektu i wywoływać `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) przeciążenia umożliwia przekazywanie w [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) za pomocą którego można zastąpić ustawienia domyślne, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i plików limit rozmiaru. (*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników na elemencie, który podajesz podczas wywoływania `ILogger` metody.)

::: moniker-end

Podczas wdrażania aplikacji usługi App Service aplikacja honoruje ustawienia w [dzienniki diagnostyczne](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) części **usługi App Service** strony w witrynie Azure Portal. Jeśli te ustawienia zostaną zaktualizowane, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchomienia lub ponownego wdrażania aplikacji.

![Ustawienia rejestrowania platformy Azure](index/_static/azure-logging-settings.png)

Domyślną lokalizacją dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folder i nazwę pliku domyślny jest *yyyymmdd.txt diagnostyki*. Domyślnego limitu rozmiaru pliku to 10 MB, a maksymalną domyślną liczbę plików, które przechowywane jest 2. Domyślna nazwa obiektu blob to *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Aby uzyskać więcej informacji na temat zachowania domyślnego, zobacz [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

Dostawca działa tylko w przypadku, gdy projekt jest uruchamiany w środowisku platformy Azure. Nie ma wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisywać pliki lokalne lub lokalnym magazynem projektowym dla obiektów blob.

## <a name="third-party-logging-providers"></a>Rejestrowanie innych dostawców

Struktury rejestrowania innych firm, które działają z platformą ASP.NET Core:

* [elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))
* [Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-extensions-logging))

Niektóre środowiska innych producentów mogą wykonywać [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Za pomocą środowiska innych producentów są podobne do przy użyciu jednej z wbudowanych dostawców:

1. Dodaj pakiet NuGet do projektu.
1. Wywoływanie metody rozszerzenia na `ILoggerFactory`.

Aby uzyskać więcej informacji można znaleźć w temacie każdego szablonu dokumentacji.

## <a name="azure-log-streaming"></a>Przesyłanie strumieniowe dzienników platformy Azure

Przesyłanie strumieniowe dzienników platformy Azure umożliwia wyświetlenie dziennika aktywności w czasie rzeczywistym z: 

* Serwer aplikacji
* Serwer sieci web
* Śledzenie żądań zakończonych niepowodzeniem

Aby skonfigurować, przesyłanie strumieniowe dzienników platformy Azure:

* Przejdź do **dzienniki diagnostyczne** strony na stronie portalu aplikacji
* Ustaw **rejestrowanie aplikacji (system plików)** pozycji włączone.

![Strona portalu dzienniki diagnostyczne platformy Azure](index/_static/azure-diagnostic-logs.png)

Przejdź do **przesyłanie strumieniowe dzienników** strony w celu wyświetlenia komunikatów aplikacji. Zalogowani przez aplikację za pośrednictwem `ILogger` interfejsu.

![Przesyłanie strumieniowe dzienników aplikacji z portalu Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Rejestrowanie śledzenia w usłudze Azure Application Insights

[Usługi Application Insights](https://azure.microsoft.com/services/application-insights/) zestawu SDK jest zdolny do zbierania danych telemetrycznych śledzenia z dzienniki wygenerowane za pomocą infrastruktury rejestrowania platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz [Wiki dotycząca usługi Application Insights/Microsoft-aspnetcore: rejestrowanie](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).

## <a name="additional-resources"></a>Dodatkowe zasoby

[Rejestrowanie o wysokiej wydajności, za pomocą funkcji LoggerMessage](xref:fundamentals/logging/loggermessage)
