---
title: JsonPatch w interfejsie web API platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak obsługiwać żądania poprawki JSON w interfejsie web API platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: 14710e6431a2a7ce60fa7f190bef184da85281a0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901327"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a>JsonPatch w interfejsie web API platformy ASP.NET Core

przez [Tom Dykstra](https://github.com/tdykstra)

W tym artykule opisano sposób obsługi żądań poprawki JSON w interfejsie web API platformy ASP.NET Core.

## <a name="patch-http-request-method"></a>Metoda żądania PATCH HTTP

PUT i [PATCH](https://tools.ietf.org/html/rfc5789) metody są używane do aktualizowania istniejącego zasobu. Różnica między nimi polega na tym, PUT zamienia cały zasób, podczas gdy poprawka określa tylko zmiany.

## <a name="json-patch"></a>Poprawka JSON

[Poprawka JSON](https://tools.ietf.org/html/rfc6902) to format służącą do aktualizacji, które mają być stosowane do zasobu. Dokument poprawki JSON zawiera tablicę *operacji*. Każda operacja identyfikuje określonego typu zmiany, takie jak dodawanie elementu tablicy lub Zastąp wartość właściwości.

Na przykład następujące dokumenty JSON reprezentują zasobem, dokument poprawki JSON dla danego zasobu i wynik zastosowania operacji patch.

### <a name="resource-example"></a>Przykład zasobów

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>Przykład kodu JSON z poprawek

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

W poprzednim formacie JSON:

* `op` Właściwość wskazuje typ operacji.
* `path` Właściwość wskazuje element do zaktualizowania.
* `value` Dostarcza nową wartość.

### <a name="resource-after-patch"></a>Zasób po poprawki

Oto zasób, po zastosowaniu poprzedni dokument poprawki JSON:

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

Zmiany wprowadzone przez zastosowanie dokumentu poprawki JSON do zasobu są niepodzielne: w przypadku niepowodzenia żadnych operacji na liście jest stosowana żadna operacja na liście.

## <a name="path-syntax"></a>Składnia ścieżki

[Ścieżki](http://tools.ietf.org/html/rfc6901) właściwości obiektu operacja ma ukośników poziomów. Na przykład `"/address/zipCode"`.

Liczony od zera indeksy są używane do określenia elementów tablicy. Pierwszy element `addresses` tablicy byłoby w `/addresses/0`. Aby `add` do końca tablicy, należy użyć łącznika (-) zamiast numer indeksu: `/addresses/-`.

### <a name="operations"></a>Operacje

W poniższej tabeli przedstawiono obsługiwane operacje, zgodnie z definicją w [specyfikacji poprawki JSON](https://tools.ietf.org/html/rfc6902):

|Operacja  | Uwagi |
|-----------|--------------------------------|
| `add`     | Dodaj właściwość lub element tablicy. Dla istniejących właściwości: Ustaw wartość.|
| `remove`  | Usuń właściwość lub element tablicy. |
| `replace` | Taki sam jak `remove` następuje `add` w tej samej lokalizacji. |
| `move`    | Taki sam jak `remove` ze źródła, a następnie `add` do miejsca docelowego przy użyciu wartości ze źródła. |
| `copy`    | Taki sam jak `add` do miejsca docelowego przy użyciu wartości ze źródła. |
| `test`    | Zwraca kod stanu powodzenia, jeśli wartość `path` = pod warunkiem `value`.|

## <a name="jsonpatch-in-aspnet-core"></a>JsonPatch in ASP.NET Core

Implementacja programu ASP.NET Core poprawki JSON jest udostępniany w [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) pakietu NuGet. Pakiet znajduje się w [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) meta Microsoft.aspnetcore.all.

## <a name="action-method-code"></a>Kod metody akcji

W kontrolerze interfejsu API metody akcji poprawki JSON:

* Jest oznaczony za pomocą `HttpPatch` atrybutu.
* Akceptuje `JsonPatchDocument<T>`, zwykle z [FromBody].
* Wywołania `ApplyTo` na dokument poprawki, aby zastosować zmiany.

Oto przykład:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Ten kod z przykładowej aplikacji współdziała z następującymi `Customer` modelu.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Przykładowa metoda akcji:

* Konstruuje `Customer`.
* Stosuje poprawki.
* Zwraca wynik w treści odpowiedzi.

 W rzeczywistej aplikacji kod będzie pobierać dane z magazynu, takich jak bazy danych i aktualizują bazę danych po zastosowaniu poprawki.

### <a name="model-state"></a>Stan modelu

W poprzednim przykładzie metoda akcji wywołuje przeciążenia `ApplyTo` która pobiera stan modelu jako jeden z jego parametrów. Po wybraniu tej opcji można uzyskać komunikaty o błędach w odpowiedzi. W poniższym przykładzie pokazano treści 400 Niewłaściwe żądanie odpowiedzi dla `test` operacji:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Obiekty dynamiczne

W poniższym przykładzie metoda akcji pokazano, jak można zastosować poprawki do obiekt dynamiczny.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Operacji dodawania

* Jeśli `path` wskazuje elementu tablicy: Wstawia nowy element przed określonego przez `path`.
* Jeśli `path` wskazuje właściwość: ustawia wartości właściwości.
* Jeśli `path` nieistniejącej lokalizację:
  * Jeśli zasób zastosowania poprawki jest to obiekt dynamiczny: Dodaje właściwość typu.
  * Jeśli zasób do poprawki jest obiektem statyczne: żądanie nie powiedzie się.

Następującego przykładowego dokumentu poprawki ustawia wartość `CustomerName` i dodaje `Order` obiekt na koniec `Orders` tablicy.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Operacja usuwania

* Jeśli `path` wskazuje elementu tablicy: usuwa element.
* Jeśli `path` wskazuje właściwość:
  * Jeśli zasób zastosowania poprawki jest to obiekt dynamiczny: Usuwa właściwość.
  * Jeśli zasób do poprawki obiektu statycznego: 
    * Jeśli właściwość ma wartość null: ustawia ją na wartość null.
    * Jeśli właściwość ma wartość null, ustawia ją na `default<T>`.

Następujące przykładowe zestawy dokumentów poprawek `CustomerName` o wartości null i usuwa `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Operacja zamiany

Ta operacja jest funkcjonalnie taka sama jak `remove` następuje `add`.

Następującego przykładowego dokumentu poprawki ustawia wartość `CustomerName` i zastępuje `Orders[0]`dzięki nowemu `Order` obiektu.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Operacji przenoszenia

* Jeśli `path` wskazuje elementu tablicy: kopie `from` elementu do lokalizacji `path` elementu, następnie uruchamia `remove` operacja `from` elementu.
* Jeśli `path` wskazuje właściwość: kopiuje wartość `from` właściwości `path` właściwości, następnie uruchamia `remove` operacja `from` właściwości.
* Jeśli `path` wskazuje nieistniejącą właściwość:
  * Jeśli zasób do poprawki jest obiektem statyczne: żądanie nie powiedzie się.
  * Jeśli zasób zastosowania poprawki jest to obiekt dynamiczny: kopie `from` właściwość lokalizacji wskazanej przez `path`, następnie uruchamia `remove` operacja `from` właściwości.

Następującego przykładowego dokumentu poprawki:

* Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.
* Zestawy `Orders[0].OrderName` na wartość null.
* Przenosi `Orders[1]` przed `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Operacja kopiowania

Ta operacja jest funkcjonalnie taka sama jak `move` operację bez końcowe `remove` kroku.

Następującego przykładowego dokumentu poprawki:

* Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.
* Wstawia kopię `Orders[1]` przed `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Operacja testowania

Jeśli wartość w miejscu wskazywanym przez `path` różni się od podanej wartości w `value`, żądanie kończy się niepowodzeniem. W takim przypadku całego żądanie PATCH nie powiedzie się nawet wtedy, gdy wszystkie inne operacje w dokumencie poprawki, w przeciwnym razie powiedzie się.

`test` Operacji jest najczęściej używany do uniknięcia aktualizacji, gdy występuje konflikt współbieżności.

Następującego przykładowego dokumentu poprawki nie obowiązuje, jeśli wartość początkową `CustomerName` jest "John", ponieważ test zakończy się niepowodzeniem:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Pobierz kod

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([Sposobu pobierania](xref:index#how-to-download-a-sample)).

Aby przetestować próbki, należy uruchomić aplikację i wysyłać żądania HTTP z następującymi ustawieniami:

* ADRES URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* Metoda HTTP: `PATCH`
* Nagłówek: `Content-Type: application/json-patch+json`
* Body: Skopiuj i Wklej jeden z przykładów dokumentu poprawki JSON z *JSON* folderu projektu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Specyfikacja IETF RFC 5789 PATCH — metoda](https://tools.ietf.org/html/rfc5789)
* [Specyfikacja IETF RFC 6902 JSON poprawki](https://tools.ietf.org/html/rfc6902)
* [Specyfikacja formatu ścieżki poprawki JSON 6901 RFC organizacji IETF](http://tools.ietf.org/html/rfc6901)
* [Dokumentacja poprawki JSON](http://jsonpatch.com/). Zawiera łącza do zasobów dotyczących tworzenia dokumentów poprawki JSON.
* [Kod źródłowy poprawki JSON Core ASP.NET](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
