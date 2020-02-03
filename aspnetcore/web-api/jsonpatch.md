---
title: JsonPatch w interfejsie Web API ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obsługiwać żądania poprawek w formacie JSON w ASP.NET Core internetowym interfejsie API.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: e57556e4b3fba55c6c187092593ffab4e31ee2d9
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727024"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a>JsonPatch w interfejsie Web API ASP.NET Core

Autorzy [Dykstra](https://github.com/tdykstra) i [Kirka Larkin](https://github.com/serpent5)

::: moniker range=">= aspnetcore-3.0"

W tym artykule wyjaśniono, jak obsłużyć żądania poprawek w formacie JSON w ASP.NET Core interfejsie API sieci Web.

## <a name="package-installation"></a>Instalacja pakietu

Obsługa JsonPatch jest włączona przy użyciu pakietu `Microsoft.AspNetCore.Mvc.NewtonsoftJson`. Aby włączyć tę funkcję, aplikacje muszą:

* Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) .
* Zaktualizuj metodę `Startup.ConfigureServices` projektu w celu uwzględnienia wywołania `AddNewtonsoftJson`:

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

`AddNewtonsoftJson` jest zgodna z metodami rejestracji usługi MVC:

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a>JsonPatch, AddNewtonsoftJson i system. Text. JSON
  
`AddNewtonsoftJson` zastępuje oparte na `System.Text.Json` wejściowe i wyjściowe elementy formatujące używane do formatowania **całej** zawartości JSON. Aby dodać obsługę `JsonPatch` przy użyciu `Newtonsoft.Json`, pozostawiając inne elementy formatujące bez zmian, zaktualizuj `Startup.ConfigureServices` projektu w następujący sposób:

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

Poprzedzający kod wymaga odwołania do elementu [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) i następujących instrukcji using:

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a>Poprawka metody żądania HTTP

Metody PUT i [patch](https://tools.ietf.org/html/rfc5789) są używane do aktualizowania istniejącego zasobu. Różnica między nimi polega na tym, że zastępuje cały zasób, podczas gdy poprawka określa tylko te zmiany.

## <a name="json-patch"></a>Poprawka JSON

[Poprawka JSON](https://tools.ietf.org/html/rfc6902) to format służący do określania aktualizacji, które mają zostać zastosowane do zasobu. Dokument poprawki JSON zawiera tablicę *operacji*. Każda operacja identyfikuje określony typ zmiany, na przykład Dodaj element tablicy lub Zastąp wartość właściwości.

Na przykład następujące dokumenty JSON reprezentują zasób, dokument poprawki JSON dla zasobu oraz wynik zastosowania operacji patch.

### <a name="resource-example"></a>Przykład zasobu

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>Przykład poprawki JSON

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

W powyższym formacie JSON:

* Właściwość `op` wskazuje typ operacji.
* Właściwość `path` wskazuje element do zaktualizowania.
* Właściwość `value` udostępnia nową wartość.

### <a name="resource-after-patch"></a>Zasób po zastosowaniu poprawki

Poniżej znajduje się zasób po zastosowaniu poprzedniego dokumentu poprawki JSON:

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

Zmiany wprowadzone przez zastosowanie dokumentu poprawek JSON do zasobu są niepodzielne: Jeśli jakakolwiek operacja na liście nie powiedzie się, nie zostanie zastosowana żadna operacja na liście.

## <a name="path-syntax"></a>Składnia ścieżki

Właściwość [Path](https://tools.ietf.org/html/rfc6901) obiektu operacji ma ukośniki między poziomami. Na przykład `"/address/zipCode"`.

W celu określenia elementów tablicy są używane indeksy oparte na wartości zero. Pierwszy element tablicy `addresses` będzie miał `/addresses/0`. Aby `add` do końca tablicy, użyj łącznika (-), a nie numeru indeksu: `/addresses/-`.

### <a name="operations"></a>Operacje

W poniższej tabeli przedstawiono obsługiwane operacje zgodnie z definicją w [specyfikacji poprawek JSON](https://tools.ietf.org/html/rfc6902):

|Operacja  | Uwagi |
|-----------|--------------------------------|
| `add`     | Dodaj właściwość lub element tablicy. Dla istniejącej właściwości: Ustaw wartość.|
| `remove`  | Usuń właściwość lub element tablicy. |
| `replace` | Taka sama jak `remove`, a następnie `add` w tej samej lokalizacji. |
| `move`    | Taka sama jak `remove` ze źródła, po której następuje `add` do miejsca docelowego przy użyciu wartości ze źródła. |
| `copy`    | Analogicznie jak `add` do miejsca docelowego przy użyciu wartości ze źródła. |
| `test`    | Zwróć kod stanu sukcesu, jeśli wartość w `path` = dostarczona `value`.|

## <a name="jsonpatch-in-aspnet-core"></a>JsonPatch w ASP.NET Core

ASP.NET Core implementacja poprawki JSON jest dostępna w pakiecie NuGet [Microsoft. AspNetCore. JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) .

## <a name="action-method-code"></a>Kod metody akcji

W kontrolerze interfejsu API Metoda akcji dla poprawki JSON:

* Ma adnotację z atrybutem `HttpPatch`.
* Akceptuje `JsonPatchDocument<T>`, zazwyczaj z `[FromBody]`.
* Wywołuje `ApplyTo` w dokumencie poprawki, aby zastosować zmiany.

Oto przykład:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Ten kod z przykładowej aplikacji współdziała z poniższym modelem `Customer`.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Przykładowa Metoda akcji:

* Konstruuje `Customer`.
* Stosuje poprawkę.
* Zwraca wynik w treści odpowiedzi.

 W rzeczywistej aplikacji kod pobiera dane z magazynu, takiego jak baza danych, i aktualizuje bazę danych po zastosowaniu poprawki.

### <a name="model-state"></a>Stan modelu

Poprzednia metoda działania przykład wywołuje Przeciążenie `ApplyTo`, które pobiera stan modelu jako jeden z jego parametrów. W przypadku tej opcji można odbierać komunikaty o błędach w odpowiedziach. Poniższy przykład przedstawia treść nieprawidłowej odpowiedzi na żądanie 400 dla operacji `test`:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Obiekty dynamiczne

W poniższym przykładzie metody akcji pokazano, jak zastosować poprawkę do obiektu dynamicznego.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Operacja dodawania

* Jeśli `path` wskazuje element tablicy: wstawia nowy element przed określony przez `path`.
* Jeśli `path` wskazuje Właściwość: ustawia wartość właściwości.
* Jeśli `path` wskazuje nieistniejącą lokalizację:
  * Jeśli zasób do poprawki jest obiektem dynamicznym: dodaje właściwość.
  * Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.

Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i dodaje obiekt `Order` do końca tablicy `Orders`.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Operacja usuwania

* Jeśli `path` wskazuje element tablicy: usuwa element.
* Jeśli `path` wskazuje Właściwość:
  * Jeśli zasób do poprawki jest obiektem dynamicznym: usuwa właściwość.
  * Jeśli zasób do poprawki jest obiektem statycznym:
    * Jeśli właściwość dopuszcza wartość null: ustawia ją na wartość null.
    * Jeśli właściwość nie dopuszcza wartości null, ustawia ją na `default<T>`.

Następujący przykładowy dokument poprawek ustawia `CustomerName` na null i usuwa `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Operacja zamiany

Ta operacja jest funkcjonalnie taka sama jak `remove` i `add`.

Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i zastępuje `Orders[0]`nowym obiektem `Order`.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Operacja przenoszenia

* Jeśli `path` wskazuje element tablicy: kopiuje `from` element do lokalizacji `path` elementu, a następnie uruchamia operację `remove` na `from` elemencie.
* Jeśli `path` wskazuje Właściwość: kopiuje wartość właściwości `from` do właściwości `path`, a następnie uruchamia operację `remove` na właściwości `from`.
* Jeśli `path` wskazuje nieistniejącą Właściwość:
  * Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.
  * Jeśli zasób do naprawienia jest obiektem dynamicznym: kopiuje Właściwość `from` do lokalizacji wskazywanej przez `path`, a następnie uruchamia operację `remove` na właściwości `from`.

Następujący przykładowy dokument poprawek:

* Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.
* Ustawia `Orders[0].OrderName` na wartość null.
* Przenosi `Orders[1]` do przed `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Operacja kopiowania

Ta operacja jest funkcjonalnie taka sama jak operacja `move` bez kroku `remove` końcowego.

Następujący przykładowy dokument poprawek:

* Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.
* Wstawia kopię `Orders[1]` przed `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Operacja testowa

Jeśli wartość w lokalizacji wskazywanej przez `path` różni się od wartości podanej w `value`, żądanie kończy się niepowodzeniem. W takim przypadku całe żądanie PATCH kończy się niepowodzeniem, nawet jeśli wszystkie inne operacje w dokumencie poprawki zakończyły się powodzeniem.

Operacja `test` jest często używana do zapobiegania aktualizacji, gdy występuje konflikt współbieżności.

Następujący przykładowy dokument poprawek nie ma wpływu, jeśli początkowa wartość `CustomerName` to "Jan", ponieważ test kończy się niepowodzeniem:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([Jak pobrać](xref:index#how-to-download-a-sample)).

Aby przetestować przykład, uruchom aplikację i Wyślij żądania HTTP z następującymi ustawieniami:

* Adres URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* Metoda HTTP: `PATCH`
* Nagłówek: `Content-Type: application/json-patch+json`
* Treść: Skopiuj i wklej jeden z przykładów dokumentu poprawki JSON z folderu projektu *JSON* .

## <a name="additional-resources"></a>Dodatkowe zasoby

* [IETF RFC 5789 Specyfikacja metody poprawek](https://tools.ietf.org/html/rfc5789)
* [IETF RFC 6902 — Specyfikacja poprawek JSON](https://tools.ietf.org/html/rfc6902)
* [IETF RFC 6901 JSON Format ścieżki poprawek](https://tools.ietf.org/html/rfc6901)
* [Dokumentacja poprawek JSON](https://jsonpatch.com/). Zawiera linki do zasobów do tworzenia dokumentów poprawek JSON.
* [ASP.NET Core kod źródłowy poprawki JSON](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym artykule wyjaśniono, jak obsłużyć żądania poprawek w formacie JSON w ASP.NET Core interfejsie API sieci Web.

## <a name="patch-http-request-method"></a>Poprawka metody żądania HTTP

Metody PUT i [patch](https://tools.ietf.org/html/rfc5789) są używane do aktualizowania istniejącego zasobu. Różnica między nimi polega na tym, że zastępuje cały zasób, podczas gdy poprawka określa tylko te zmiany.

## <a name="json-patch"></a>Poprawka JSON

[Poprawka JSON](https://tools.ietf.org/html/rfc6902) to format służący do określania aktualizacji, które mają zostać zastosowane do zasobu. Dokument poprawki JSON zawiera tablicę *operacji*. Każda operacja identyfikuje określony typ zmiany, na przykład Dodaj element tablicy lub Zastąp wartość właściwości.

Na przykład następujące dokumenty JSON reprezentują zasób, dokument poprawki JSON dla zasobu oraz wynik zastosowania operacji patch.

### <a name="resource-example"></a>Przykład zasobu

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>Przykład poprawki JSON

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

W powyższym formacie JSON:

* Właściwość `op` wskazuje typ operacji.
* Właściwość `path` wskazuje element do zaktualizowania.
* Właściwość `value` udostępnia nową wartość.

### <a name="resource-after-patch"></a>Zasób po zastosowaniu poprawki

Poniżej znajduje się zasób po zastosowaniu poprzedniego dokumentu poprawki JSON:

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

Zmiany wprowadzone przez zastosowanie dokumentu poprawek JSON do zasobu są niepodzielne: Jeśli jakakolwiek operacja na liście nie powiedzie się, nie zostanie zastosowana żadna operacja na liście.

## <a name="path-syntax"></a>Składnia ścieżki

Właściwość [Path](https://tools.ietf.org/html/rfc6901) obiektu operacji ma ukośniki między poziomami. Na przykład `"/address/zipCode"`.

W celu określenia elementów tablicy są używane indeksy oparte na wartości zero. Pierwszy element tablicy `addresses` będzie miał `/addresses/0`. Aby `add` do końca tablicy, użyj łącznika (-), a nie numeru indeksu: `/addresses/-`.

### <a name="operations"></a>Operacje

W poniższej tabeli przedstawiono obsługiwane operacje zgodnie z definicją w [specyfikacji poprawek JSON](https://tools.ietf.org/html/rfc6902):

|Operacja  | Uwagi |
|-----------|--------------------------------|
| `add`     | Dodaj właściwość lub element tablicy. Dla istniejącej właściwości: Ustaw wartość.|
| `remove`  | Usuń właściwość lub element tablicy. |
| `replace` | Taka sama jak `remove`, a następnie `add` w tej samej lokalizacji. |
| `move`    | Taka sama jak `remove` ze źródła, po której następuje `add` do miejsca docelowego przy użyciu wartości ze źródła. |
| `copy`    | Analogicznie jak `add` do miejsca docelowego przy użyciu wartości ze źródła. |
| `test`    | Zwróć kod stanu sukcesu, jeśli wartość w `path` = dostarczona `value`.|

## <a name="jsonpatch-in-aspnet-core"></a>JsonPatch w ASP.NET Core

ASP.NET Core implementacja poprawki JSON jest dostępna w pakiecie NuGet [Microsoft. AspNetCore. JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) . Pakiet jest zawarty w pakiecie [Microsoft. AspnetCore. app](xref:fundamentals/metapackage-app) .

## <a name="action-method-code"></a>Kod metody akcji

W kontrolerze interfejsu API Metoda akcji dla poprawki JSON:

* Ma adnotację z atrybutem `HttpPatch`.
* Akceptuje `JsonPatchDocument<T>`, zazwyczaj z `[FromBody]`.
* Wywołuje `ApplyTo` w dokumencie poprawki, aby zastosować zmiany.

Oto przykład:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Ten kod z przykładowej aplikacji współdziała z poniższym modelem `Customer`.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Przykładowa Metoda akcji:

* Konstruuje `Customer`.
* Stosuje poprawkę.
* Zwraca wynik w treści odpowiedzi.

 W rzeczywistej aplikacji kod pobiera dane z magazynu, takiego jak baza danych, i aktualizuje bazę danych po zastosowaniu poprawki.

### <a name="model-state"></a>Stan modelu

Poprzednia metoda działania przykład wywołuje Przeciążenie `ApplyTo`, które pobiera stan modelu jako jeden z jego parametrów. W przypadku tej opcji można odbierać komunikaty o błędach w odpowiedziach. Poniższy przykład przedstawia treść nieprawidłowej odpowiedzi na żądanie 400 dla operacji `test`:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Obiekty dynamiczne

W poniższym przykładzie metody akcji pokazano, jak zastosować poprawkę do obiektu dynamicznego.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Operacja dodawania

* Jeśli `path` wskazuje element tablicy: wstawia nowy element przed określony przez `path`.
* Jeśli `path` wskazuje Właściwość: ustawia wartość właściwości.
* Jeśli `path` wskazuje nieistniejącą lokalizację:
  * Jeśli zasób do poprawki jest obiektem dynamicznym: dodaje właściwość.
  * Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.

Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i dodaje obiekt `Order` do końca tablicy `Orders`.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Operacja usuwania

* Jeśli `path` wskazuje element tablicy: usuwa element.
* Jeśli `path` wskazuje Właściwość:
  * Jeśli zasób do poprawki jest obiektem dynamicznym: usuwa właściwość.
  * Jeśli zasób do poprawki jest obiektem statycznym:
    * Jeśli właściwość dopuszcza wartość null: ustawia ją na wartość null.
    * Jeśli właściwość nie dopuszcza wartości null, ustawia ją na `default<T>`.

Następujący przykładowy dokument poprawek ustawia `CustomerName` na null i usuwa `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Operacja zamiany

Ta operacja jest funkcjonalnie taka sama jak `remove` i `add`.

Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i zastępuje `Orders[0]`nowym obiektem `Order`.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Operacja przenoszenia

* Jeśli `path` wskazuje element tablicy: kopiuje `from` element do lokalizacji `path` elementu, a następnie uruchamia operację `remove` na `from` elemencie.
* Jeśli `path` wskazuje Właściwość: kopiuje wartość właściwości `from` do właściwości `path`, a następnie uruchamia operację `remove` na właściwości `from`.
* Jeśli `path` wskazuje nieistniejącą Właściwość:
  * Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.
  * Jeśli zasób do naprawienia jest obiektem dynamicznym: kopiuje Właściwość `from` do lokalizacji wskazywanej przez `path`, a następnie uruchamia operację `remove` na właściwości `from`.

Następujący przykładowy dokument poprawek:

* Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.
* Ustawia `Orders[0].OrderName` na wartość null.
* Przenosi `Orders[1]` do przed `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Operacja kopiowania

Ta operacja jest funkcjonalnie taka sama jak operacja `move` bez kroku `remove` końcowego.

Następujący przykładowy dokument poprawek:

* Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.
* Wstawia kopię `Orders[1]` przed `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Operacja testowa

Jeśli wartość w lokalizacji wskazywanej przez `path` różni się od wartości podanej w `value`, żądanie kończy się niepowodzeniem. W takim przypadku całe żądanie PATCH kończy się niepowodzeniem, nawet jeśli wszystkie inne operacje w dokumencie poprawki zakończyły się powodzeniem.

Operacja `test` jest często używana do zapobiegania aktualizacji, gdy występuje konflikt współbieżności.

Następujący przykładowy dokument poprawek nie ma wpływu, jeśli początkowa wartość `CustomerName` to "Jan", ponieważ test kończy się niepowodzeniem:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([Jak pobrać](xref:index#how-to-download-a-sample)).

Aby przetestować przykład, uruchom aplikację i Wyślij żądania HTTP z następującymi ustawieniami:

* Adres URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* Metoda HTTP: `PATCH`
* Nagłówek: `Content-Type: application/json-patch+json`
* Treść: Skopiuj i wklej jeden z przykładów dokumentu poprawki JSON z folderu projektu *JSON* .

## <a name="additional-resources"></a>Dodatkowe zasoby

* [IETF RFC 5789 Specyfikacja metody poprawek](https://tools.ietf.org/html/rfc5789)
* [IETF RFC 6902 — Specyfikacja poprawek JSON](https://tools.ietf.org/html/rfc6902)
* [IETF RFC 6901 JSON Format ścieżki poprawek](https://tools.ietf.org/html/rfc6901)
* [Dokumentacja poprawek JSON](https://jsonpatch.com/). Zawiera linki do zasobów do tworzenia dokumentów poprawek JSON.
* [ASP.NET Core kod źródłowy poprawki JSON](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
