---
title: Logowanie do platformy ASP.NET Core
author: ardalis
description: "Więcej informacji na temat struktury rejestrowania w ASP.NET Core. Odnajdywanie dostawców wbudowanych rejestrowania i Dowiedz się więcej o innych popularnych dostawców."
keywords: Platformy ASP.NET Core, rejestrowanie, providers,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,scopes rejestrowania
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 3eb167c961b8d089d508ef5622db6ae1cdd99088
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Wprowadzenie do rejestrowania w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra)

Platformy ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnych dostawców rejestrowania. Dostawców wbudowanych umożliwiają wysyłanie dzienników do jednego lub więcej miejsc docelowych, a można podłączyć w ramach rejestrowania innych firm. W tym artykule pokazano, jak korzystać z interfejsu API wbudowanych rejestrowania i dostawców w kodzie.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>Jak utworzyć dzienników

Aby utworzyć dzienników, Pobierz `ILogger` obiekt z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Następnie wywołania metody rejestrowania dla tego obiektu rejestratora:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

W tym przykładzie tworzy dzienniki z `TodoController` klasy *kategorii*. Kategorie są objaśnione [dalszej części tego artykułu](#log-category).

Platformy ASP.NET Core nie udostępniają asynchronicznej metody rejestratora, ponieważ rejestrowania należy rozważnie nie warto kosztów asynchronicznego. Jeśli pracujesz w sytuacji, w przypadku, gdy nie jest prawdziwe, należy rozważyć zmianę sposobu logowania. Jeśli w magazynie danych przebiega powoli, najpierw zapisać komunikaty dziennika do szybkiego magazynu, a następnie przenieść je do magazynu powolne później. Na przykład Zaloguj się do kolejki komunikatów, która jest do odczytu i utrwalone w magazynie powolne przez inny proces.

## <a name="how-to-add-providers"></a>Jak dodać dostawców

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Dostawcy logowania ma utworzonych za pomocą wiadomości `ILogger` obiektów, wyświetla i przechowuje je. Na przykład konsola dostawca wyświetla komunikaty w konsoli i dostawcy usługi Azure App Service można przechowywać w magazynie obiektów blob Azure.

Aby użyć dostawcy, należy wywołać dostawcy `Add<ProviderName>` — metoda rozszerzenia w *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Domyślny szablon projektu umożliwia rejestrowanie z [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metody:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Dostawcy logowania ma utworzonych za pomocą wiadomości `ILogger` obiektów, wyświetla i przechowuje je. Na przykład konsola dostawca wyświetla komunikaty w konsoli i dostawcy usługi Azure App Service można przechowywać w magazynie obiektów blob Azure.

Do korzystania z dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy na wystąpienie `ILoggerFactory`, jak pokazano w poniższym przykładzie.

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

Platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane) zapewnia `ILoggerFactory` wystąpienia. `AddConsole` i `AddDebug` metody rozszerzenia są definiowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów. Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` metody, przekazując wystąpienie dostawcy. 

> [!NOTE]
> Przykładowa aplikacja dla tego artykułu dodaje rejestrowanie dostawców w `Configure` metody `Startup` klasy. Jeśli chcesz pobrać dane wyjściowe dziennika z kodu, która wykonuje wcześniej, Dodaj rejestrowanie dostawców w `Startup` klasy zamiast tego konstruktora. 

---

Znajdują się informacje o poszczególnych [rejestrowania wbudowanego dostawcy](#built-in-logging-providers) i łącza do [dostawców innych firm rejestrowania](#third-party-logging-providers) dalszej części tego artykułu.

## <a name="sample-logging-output"></a>Przykładowe dane wyjściowe rejestrowania

Z przykładowy kod wyświetlany w poprzedniej sekcji zobaczysz dzienniki w konsoli po uruchomieniu z wiersza polecenia. Oto przykładowe dane wyjściowe konsoli:

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
 
Te dzienniki zostały utworzone, przechodząc do `http://localhost:5000/api/todo/0`, który wyzwala wykonanie obu `ILogger` wywołania wyświetlone w poprzedniej sekcji.

Oto przykład tego samego dzienniki znajdujące się w oknie Debugowanie po uruchomieniu przykładowej aplikacji w programie Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Dzienniki, które zostały utworzone przez `ILogger` wywołania wyświetlone w poprzedniej sekcji rozpoczynać się od "TodoApi.Controllers.TodoController". Dzienniki, które zaczynają się od "Microsoft" kategorie są z platformy ASP.NET Core. Kod aplikacji i ASP.NET Core, sama korzystają z tego samego interfejsu API rejestrowania i tego samego dostawcy rejestrowania.

W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.

## <a name="nuget-packages"></a>Pakiety NuGet

`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Kategoria dziennika

A *kategorii* znajduje się w każdym dzienniku utworzony. Określ kategorię, po utworzeniu `ILogger` obiektu. Kategoria może być dowolnym ciągiem, ale Konwencji jest użycie w pełni kwalifikowana nazwa klasy, z którego dzienniki są zapisywane. Na przykład: "TodoApi.Controllers.TodoController".

Można określić kategorię jako ciąg lub użyj metody rozszerzenia, która pochodzi z typu kategorii. Aby określić kategorię jako ciąg, należy wywołać `CreateLogger` na `ILoggerFactory` wystąpienie, jak pokazano poniżej.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

W większości przypadków, będzie łatwiejszy do używania `ILogger<T>`, jak w poniższym przykładzie.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Jest to odpowiednik wywołania `CreateLogger` z w pełni kwalifikowaną nazwę typu `T`.

## <a name="log-level"></a>Poziom dziennika

Zawsze możesz zapisać dziennik, należy określić jego [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Poziom dziennika wskazuje poziom ważności lub ważność. Na przykład można napisać `Information` Rejestruj, gdy metoda kończy się zwykle `Warning` Zaloguj się przy metoda zwraca kod powrotu 404 oraz `Error` rejestrowania podczas catch wystąpił nieoczekiwany wyjątek.

W poniższym przykładzie kodu, nazwy metody (na przykład `LogWarning`) określ poziom dziennika. Pierwszym parametrem jest [identyfikator zdarzenia dziennika](#log-event-id). Drugi parametr jest [szablon wiadomości](#log-message-template) z symbole zastępcze wartości argumentów dostarczonych przez pozostałe parametry metody. Parametry metody są szczegółowo w dalszej części tego artykułu.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Metody dziennika, które obejmują poziom w nazwie metody są [metody rozszerzenia dla ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). W tle wywołania tych metod `Log` metody pobierającej `LogLevel` parametru. Możesz wywołać `Log` bezpośrednio zamiast jeden z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane. Aby uzyskać więcej informacji, zobacz [interfejsu ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

Platformy ASP.NET Core definiuje następujące [dziennika poziomy](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), uporządkowanych w tym miejscu z co najmniej do najwyższej ważności.

* Śledzenia = 0

  Aby uzyskać informacje, których jest przydatna tylko dla deweloperów, debugowanie problemu. Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie powinna być włączona w środowisku produkcyjnym. *Domyślnie wyłączone.* Przykład:`Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debugowanie = 1

  Aby uzyskać informacje, których ma krótkoterminowej przydatność podczas projektowania i debugowania. Przykład: `Entering method Configure with flag set to true.` można zwykle nie umożliwia `Debug` poziomu rejestruje w środowisku produkcyjnym, chyba że występuje problem z powodu dużej liczby dzienników.

* Informacje o = 2

  Pozwala to na śledzenie ogólny przebieg aplikacji. Te dzienniki zwykle mają niektóre wartości długoterminowej. Przykład:`Request received for path /api/todo`

* Ostrzeżenie = 3

  Dla nieprawidłowego lub nieoczekiwanego zdarzenia w procesie aplikacji. Mogą one zawierać błędy lub inne warunki nie powodują aplikacji do zatrzymania, ale które mogą wymagać należy zbadać. Obsługiwany wyjątki są spójne użyj `Warning` poziom dziennika. Przykład:`FileNotFoundException for file quotes.txt.`

* Błąd = 4

  Błędy i wyjątki, które nie mogą być obsługiwane. Te komunikaty wskazują błędu bieżącego działania lub działania (takie jak bieżące żądanie HTTP), nie awarii całej aplikacji. Przykładowy komunikat dziennika:`Cannot insert record due to duplicate key violation.`

* Krytyczne = 5

  Niepowodzeń, które wymagają natychmiastowej uwagi. Przykłady: danych scenariuszy utraty, brak miejsca na dysku.

Poziom dziennika służy do kontrolowania, jak dużo danych wyjściowych dziennika są zapisywane na nośniku magazynujące lub Wyświetl okno. Na przykład w środowisku produkcyjnym może być wszystkie dzienniki `Information` poziomu i zmniejszyć, aby przejść do magazynu danych woluminu, a wszystkie dzienniki `Warning` poziomu lub nowszej, aby przejść do magazynu danych wartości. Podczas tworzenia, zwykle może wysyłać dzienniki `Warning` lub wyższej ważności do konsoli. Następnie podczas należy rozwiązać, można dodać `Debug` poziom. [Filtrowania dziennika](#log-filtering) później w tym artykule wyjaśniono, jak kontrolować poziomy rejestrowania, które obsługuje dostawcę.

Zapisuje w ramach platformy ASP.NET Core `Debug` poziomu dzienników zdarzeń framework. Przykłady dziennika wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomu, więc nie ma `Debug` poziomu dzienniki były wyświetlane. Oto przykład dzienniki konsoli po uruchomieniu aplikacji przykładowej, jest skonfigurowana do wyświetlania `Debug` i wyższe dzienniki dla dostawcy konsoli.

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

Zawsze możesz zapisać dziennik, można określić *identyfikator zdarzenia*. Przykładowa aplikacja robi to przy użyciu zdefiniowane lokalnie `LoggingEvents` klasy:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Identyfikator zdarzenia jest wartość całkowita używanego do kojarzenia zestaw zarejestrowane zdarzenia ze sobą. Na przykład dziennika dodania elementu do koszyk może być zdarzenie o identyfikatorze 1000 oraz dziennika kończenia zakupu można identyfikator zdarzenia 1001.

W danych wyjściowych rejestrowania identyfikator zdarzenia może przechowywanych w polu lub zawarte w wiadomości tekstowej, w zależności od dostawcy. Dostawca debugowania nie Wyświetla identyfikatory zdarzeń, ale dostawca konsoli Pokazuje je w nawiasach po kategorii:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Szablon wiadomości dziennika

Zawsze, gdy zapisu komunikatu dziennika, musisz podać szablon wiadomości. Szablon wiadomości może być ciągiem lub może zawierać nazwanego symbole zastępcze w argumentu, które są umieszczone wartości. Ciąg formatu nie jest szablon i symbole zastępcze powinno być nazwanym, nie.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Kolejność symbole zastępcze, a nie ich nazw, określa, które parametry są używane do zapewnienia ich wartości. Jeśli masz następujący kod:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Wynikowa komunikatu dziennika wygląda następująco:

```
Parameter values: parm1, parm2
```

Struktury rejestrowania komunikatów formatowania w ten sposób, aby umożliwić rejestrowanie dostawców do zaimplementowania [semantycznego rejestrowania, nazywany również strukturalnych rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Ponieważ same argumenty są przekazywane do rejestrowania systemu, nie tylko szablon sformatowany komunikat rejestrowania dostawców można przechowywać wartości parametrów za pomocą pól oprócz szablon wiadomości. Jeśli jest kierowanie dziennika dane wyjściowe do magazynu tabel platformy Azure i wywołania metody Rejestrator wygląda następująco:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Każda jednostka tabel Azure może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytania na danych dziennika. Możesz znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakres bez konieczności przeanalizować limit czasu wiadomości SMS.

## <a name="logging-exceptions"></a>Rejestrowaniem wyjątków

Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątku, jak w poniższym przykładzie:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Różnych dostawców obsługi informacji o wyjątkach na różne sposoby. Oto przykład danych wyjściowych debugowania dostawcy z kodem przedstawionym powyżej.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrowanie dziennika

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Można określić poziom dziennika minimalną dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii. Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, tak, aby nie pobrać wyświetlane ani przechowywane. 

Jeśli chcesz pominąć wszystkie dzienniki, można określić `LogLevel.None` jako poziom dziennika minimalnej. Wartość całkowita `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).

**Tworzenie reguły filtrów w konfiguracji**

Szablony projektów utworzyć kod, który wywołuje `CreateDefaultBuilder` Aby skonfigurować rejestrowanie dla dostawców konsoli i debugowania. `CreateDefaultBuilder` Metody konfiguruje również rejestrowanie do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu podobnie do następującej:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Dane konfiguracji Określa poziomy dziennika minimalna przez dostawcę i kategorii, jak w poniższym przykładzie:

[!code-json[](index/sample2/appsettings.json)]

Ten kod JSON tworzy sześciu reguły filtrowania, jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jedną, która ma zastosowanie do wszystkich dostawców. Pojawi się później, jak tylko jeden z tych zasad jest wybierany dla każdego dostawcy podczas `ILogger` tworzony jest obiekt.

**Reguły filtrowania w kodzie**

Można zarejestrować reguły filtrów w kodzie, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Drugi `AddFilter` określa dostawcę, który debugowania za pomocą jego nazwy. Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określono typu dostawcy.

**Sposób filtrowania zasady są stosowane.**

Dane konfiguracji i `AddFilter` kodem przedstawionym w powyższych przykładach tworzenia reguł pokazano w poniższej tabeli. Pierwszy sześciu pochodzą z przykład konfiguracji i ostatnie dwa pochodzi z przykładów kodu.

| Wartość liczbowa | Dostawcy      | Kategorie, które zaczynają się od...          | Poziom dziennika minimalna |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debugowanie         | Wszystkie kategorie                          | Informacje       |
| 2      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Ostrzeżenie           |
| 3      | Konsola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debugowanie             |
| 4      | Konsola       | Microsoft.AspNetCore.Mvc.Razor          | Błąd             |
| 5      | Konsola       | Wszystkie kategorie                          | Informacje       |
| 6      | Wszystkich dostawców | Wszystkie kategorie                          | Debugowanie             |
| 7      | Wszystkich dostawców | System                                  | Debugowanie             |
| 8      | Debugowanie         | Microsoft                               | śledzenia             |

Po utworzeniu `ILogger` obiektu do zapisania dzienniki, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora. Wszystkie komunikaty napisane przez to `ILogger` obiektu są filtrowane w zależności od wybranej reguły. Reguła specyficzny dla każdej pary kategorii i dostawcy możliwe jest wybierana z dostępne reguły.

Następujący algorytm jest używana dla każdego dostawcy podczas `ILogger` jest tworzony dla danej kategorii:

* Wybierz wszystkie reguły zgodne dostawca lub jego alias. Jeśli żaden nie zostaną znalezione, wybierz wszystkie reguły z dostawcą puste.
* W wyniku poprzedniego kroku wybierz reguły z najdłuższy dopasowanie prefiksu kategorii. Jeśli żaden nie zostaną znalezione, wybierz wszystkie reguły, które nie określają kategorii.
* Jeśli wybrano wiele reguł **ostatniego** jeden.
* Jeśli nie wybrano żadnych reguł, użyj `MinimumLevel`.
 
Na przykład, załóżmy, że powyższej listy reguł i utworzysz `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* W przypadku dostawcy usług debugowania mają zastosowanie zasady 1, 6, 8. Reguła 8 jest najbardziej konkretny, więc jest zaznaczony.
* Dostawca konsoli zastosować zasady 3, 4, 5 i 6. Reguła 3 jest specyficzny.

Po utworzeniu dzienniki z `ILogger` dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" dzienniki z `Trace` poziomu i nowszych nastąpi przejście do debugowania dostawcy i dzienniki `Debug` poziomu i nowszych nastąpi przejście do dostawcy konsoli.

**Aliasy dostawcy**

Nazwa typu można używać, aby określić dostawcę konfiguracji, ale każdy dostawca definiuje krótszą *alias* łatwiej do użycia. W przypadku dostawców wbudowanych, użyj następujących aliasów:

- Konsola
- Debugowanie
- Dziennik zdarzeń
- AzureAppServices
- TraceSource
- EventSource

**Minimalny poziom domyślny**

Brak minimalnej ustawienie poziomu, które obowiązuje tylko wtedy, gdy żadne reguły z konfiguracji lub kod odnoszą się do danego dostawcy oraz kategorii. Poniższy przykład pokazuje, jak ustawić minimalny poziom:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Jeśli nie zostanie jawnie ustawiona na poziomie minimalnym, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.

**Funkcje filtrowania**

W funkcji Filtr można zastosować reguł filtrowania, można napisać kod. Funkcja filtru jest wywoływany dla wszystkich dostawców i kategorie, które nie ma przypisanego przez konfiguracji lub kod reguł. Kod w funkcji ma dostęp do typ dostawcy, kategorii i poziom dziennika, aby zdecydować, czy wiadomość powinny być rejestrowane. Na przykład:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Niektórzy dostawcy rejestrowania pozwalają określić, gdy zapisywane na nośniku magazynowania lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.

`AddConsole` i `AddDebug` metody rozszerzenia udostępniają przeciążenia, które umożliwiają przekazywanie w kryteriach filtrowania. Następujący przykładowy kod powoduje, że dostawca konsoli do ignorowania dzienniki poniżej `Warning` poziom, gdy dostawca debugowania ignoruje dzienników tworzone w ramach.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, która może zawierać funkcji filtrowania w jego `Filter` właściwości. Dostawca TraceSource nie ma żadnych z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` go używa.

Można ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia przy użyciu `WithFilter` — metoda rozszerzenia. W poniższym przykładzie ogranicza dzienniki framework (kategoria rozpoczyna się od "Microsoft" lub "System") z ostrzeżeniami, a w dzienniku aplikacji na poziomie debugowania.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Jeśli chcesz użyć filtrowania, aby uniemożliwić wszystkie dzienniki zapisywane dla określonej kategorii, można określić `LogLevel.None` jako poziom dziennika minimalną dla tej kategorii. Wartość całkowita `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).

`WithFilter` — Metoda rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet. Metoda zwraca nową `ILoggerFactory` wystąpienia, która będzie filtrować wiadomości dziennika przekazany do wszystkich dostawców rejestratora zarejestrowany z nim. Nie dotyczy ono innych `ILoggerFactory` wystąpienia, w tym oryginalnej `ILoggerFactory` wystąpienia.

---

## <a name="log-scopes"></a>Zakresy dziennika

Można grupować zestaw operacji logicznych w *zakres* aby można było dołączyć tych samych danych do każdego dziennika, który został utworzony jako część tego zbioru. Na przykład może być co dziennika utworzona w ramach przetwarzania transakcji, aby uwzględnić identyfikator transakcji.

Zakres jest `IDisposable` typu, który jest zwracany przez `ILogger.BeginScope<TState>` — metoda i trwa do momentu jego usunięcia. Użyj zakresu przez zawijania odwołuje Twoje rejestratora `using` zablokować, jak pokazano poniżej:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Poniższy kod umożliwia zakresy dla dostawcy konsoli:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

W *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu. Konfiguracja `IncludeScopes` przy użyciu *appsettings* pliki konfiguracji będą dostępne w wersji platformy ASP.NET Core 2.1.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

W *Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

Każdy komunikat dziennika zawiera informacje o zakresie:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Rejestrowanie wbudowanych dostawców

Platformy ASP.NET Core dostarczany następujących dostawców:

* [Console](#console)
* [Debugowania](#debug)
* [Źródła zdarzeń](#eventsource)
* [Dziennik zdarzeń](#eventlog)
* [TraceSource](#tracesource)
* [Usługa aplikacji Azure](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>Dostawca konsoli

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[Przeciążenia AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let przekazywane w poziom dziennika minimalne, funkcji filtru i wartość logiczną wskazującą, czy zakresy są obsługiwane. Innym rozwiązaniem jest przekazanie w `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomy rejestrowania. 

Planując Konsola dostawca do użycia w środowisku produkcyjnym, należy pamiętać, ma znaczący wpływ na wydajność.

Podczas tworzenia nowego projektu programu Visual Studio `AddConsole` metody wygląda następująco:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Ten kod odwołuje się do `Logging` sekcji *appSettings.json* pliku:

[!code-json[](index/sample//appsettings.json)]

Ustawienia wyświetlane limit framework dzienniki, aby ostrzeżenia, umożliwiając aplikacji do logowania się na poziomie debugowania, zgodnie z objaśnieniem w [filtrowania dziennika](#log-filtering) sekcji. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>Dostawca debugowania

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika przy użyciu [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) klasy (`Debug.WriteLine` wywołania metody).

W systemie Linux, tego dostawcy zapisuje dzienniki, aby */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Przeciążenia AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) umożliwiają przekazywanie poziom dziennika minimalną lub funkcji filtru.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>Dostawca źródła zdarzeń

Dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszej, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń. W systemie Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Dostawca jest między platformami, ale żadne zdarzenie kolekcji i wyświetlanie narzędzi jeszcze dla systemu Linux lub macOS. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Dobrym sposobem na zbieranie i przeglądanie dzienników jest użycie [narzędzie narzędzia PerfView](https://www.microsoft.com/download/details.aspx?id=28567). Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET. 

Aby skonfigurować narzędzia PerfView zbierania zdarzenia zarejestrowane przez tego dostawcę, Dodaj ciąg `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy. (Nie zostały pominięte gwiazdki na początku ciąg.)

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

Przechwytywanie zdarzeń na serwerze Nano wymaga dodatkowej konfiguracji:

* Nawiąż połączenie z serwerem Nano obsługę zdalną środowiska PowerShell:

  ```powershell
  Enter-PSSession [name]
  ```

* Utwórz sesję ETW:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Dodawanie dostawcy ETW dla [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), platformy ASP.NET Core, a inne zgodnie z potrzebami. Dostawcy platformy ASP.NET Core identyfikator GUID jest `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Uruchom witrynę i czy niezależnie od akcje chcesz informacji śledzenia dla.

* Zatrzymaj sesję śledzenia, po zakończeniu:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

Powstałe w ten sposób *C:\trace.etl* plik może być analizowane za pomocą narzędzia PerfView w innych wersjach systemu Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Dostawca dziennika zdarzeń systemu Windows

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń systemu Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Przeciążenia AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let przekazywane w `EventLogSettings` lub poziom dziennika minimalnej.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>Dostawca TraceSource

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) bibliotek i dostawców.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Przeciążenia AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) umożliwiają przekazywanie przełącznik źródła i nasłuchującego śledzenia.

Aby użyć tego dostawcy, aplikacja musi działać .NET Framework (zamiast .NET Core). Umożliwia dostawcy rozesłać komunikaty do różnych [odbiorników](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), takich jak [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) używane w przykładowej aplikacji.

Poniższy przykład konfiguruje `TraceSource` dostawcy, który rejestruje `Warning` i wyższe wiadomości dla okna konsoli.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Dostawca usługi Azure App Service

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie magazynu Azure. Dostawca jest dostępna tylko dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszej. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli przeznaczonych dla platformy .NET Core, nie trzeba zainstalować pakiet dostawcy lub jawnie wywołać `AddAzureWebAppDiagnostics`. Dostawca jest automatycznie dostępne dla aplikacji, podczas wdrażania aplikacji w usłudze Azure App Service.

Jeśli przeznaczonych dla platformy .NET Framework, Dodaj pakiet dostawcy do projektu i wywołać `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` Przeciążenia umożliwia przekazywanie w [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) z którym mogą zastąpić ustawienia domyślne, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i limit rozmiaru pliku. (*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników na elemencie podane podczas wywoływania `ILogger` metody.)

---

Podczas wdrażania aplikacji usługi app Service, aplikacja będzie honorować ustawień w [dzienników diagnostycznych](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sekcji **usługi aplikacji** strony portalu Azure. Zmiana tych ustawień, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchomienia aplikacji, lub Wdróż ponownie kod, aby go. 

![Ustawienia rejestrowania usługi Azure](index/_static/azure-logging-settings.png)

Domyślna lokalizacja dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folderu, a domyślna nazwa pliku jest *yyyymmdd.txt diagnostyki*. Domyślnego limitu rozmiaru pliku to 10 MB, a domyślna maksymalna liczba pliki przechowywane wynosi 2. Domyślna nazwa obiektu blob jest *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Aby uzyskać więcej informacji na temat domyślnego zachowania, zobacz [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

Dostawca działa tylko w przypadku, gdy projekt jest uruchamiana w środowisku platformy Azure. Po uruchomieniu lokalnie nie ma wpływu &mdash; nie zapisu plików lokalnych lub rozwoju lokalnego magazynu obiektów blob.

## <a name="third-party-logging-providers"></a>Rejestrowanie innych dostawców

Poniżej przedstawiono niektóre platform rejestrowania innych firm, które współpracują z platformy ASP.NET Core:

* [elmah.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) — dostawca usługi Elmah.Io

* [JSNLog](http://jsnlog.com) — rejestruje w dzienniku po stronie serwera wyjątków JavaScript oraz inne zdarzenia po stronie klienta.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) — dostawca usługi Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -dostawca dla biblioteki NLog

* [Serilog](https://github.com/serilog/serilog-extensions-logging) -dostawca dla biblioteki Serilog

Możliwość niektórych struktur innych firm [semantycznego rejestrowania, nazywany również strukturalnych rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Przy użyciu platformy innej firmy jest podobny do przy użyciu jednego z dostawców wbudowanych: Dodaj pakiet NuGet do projektu i wywołanie metody rozszerzenia na `ILoggerFactory`. Aby uzyskać więcej informacji zobacz dokumentację każdego framework.

Możesz utworzyć własnych dostawców niestandardowych, również do obsługi innych platform rejestrowania lub wymagań rejestrowania.

## <a name="azure-log-streaming"></a>Strumieniowe przesyłanie dzienników Azure

Strumieniowe przesyłanie dzienników Azure umożliwia wyświetlenie dziennika aktywności w czasie rzeczywistym z: 

* Serwer aplikacji 
* Serwer sieci web
* Śledzenie nieudanych żądań 

Aby skonfigurować przesyłania strumieniowego dzienników Azure: 

* Przejdź do **dzienników diagnostycznych** strony ze strony portalu aplikacji
* Ustaw **rejestrowanie aplikacji (systemu plików)** do włączenia. 

![Strona Azure portalu dzienników diagnostycznych](index/_static/azure-diagnostic-logs.png)

Przejdź do **dzienników przesyłania strumieniowego** strony w celu wyświetlenia komunikatów aplikacji. Są one rejestrowane przez aplikację za pomocą `ILogger` interfejsu. 

![Przesyłanie strumieniowe dziennika aplikacji z portalu Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>Zobacz także

[Rejestrowanie wysokiej wydajności z LoggerMessage](xref:fundamentals/logging/loggermessage)
