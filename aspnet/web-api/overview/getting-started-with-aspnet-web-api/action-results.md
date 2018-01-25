---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Akcja powoduje składnika Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="action-results-in-web-api-2"></a>Wyniki akcji w składniku Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym temacie opisano, jak składnika ASP.NET Web API konwertuje wartość zwracaną akcji kontrolera do komunikatu odpowiedzi HTTP.

Akcja kontrolera interfejsu API sieci Web może zwracać jedną z następujących czynności:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Innego typu

W zależności od tego, który z nich jest zwracany, interfejsu API sieci Web używa inny mechanizm do tworzenia odpowiedzi HTTP.

| Zwracany typ | Jak interfejsu API sieci Web tworzy odpowiedzi |
| --- | --- |
| void | Zwraca pusty 204 (bez zawartości) |
| **HttpResponseMessage** | Konwertuj bezpośrednio do komunikatu odpowiedzi HTTP. |
| **IHttpActionResult** | Wywołanie **ExecuteAsync** utworzyć **HttpResponseMessage**, następnie Konwertuj na komunikat odpowiedzi HTTP. |
| Innego typu | Zapisywanie serializacji wartości zwracanej w treści odpowiedzi; Zwróć 200 (OK). |

Pozostała część tego tematu opisano poszczególne opcje więcej szczegółów.

## <a name="void"></a>void

Jeśli typem zwracanym jest `void`, interfejsu API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (bez zawartości).

Przykład controller:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Odpowiedź HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Jeśli akcja zwraca [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), interfejsu API sieci Web konwertuje wartości zwracanej bezpośrednio na komunikat odpowiedzi HTTP przy użyciu właściwości **HttpResponseMessage** obiektu, aby wypełnić odpowiedź.

Ta opcja umożliwia szczegółową kontrolę nad komunikat odpowiedzi. Na przykład następująca Akcja kontrolera ustawia nagłówek Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Odpowiedź:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

W przypadku przekazania modelu domeny do **CreateResponse** metoda, używa interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do zapisania modelem serializacji w treści odpowiedzi.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Interfejsu API sieci Web używa nagłówek Accept w żądaniu, aby wybrać element formatujący. Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** interfejs został wprowadzony w sieci Web API 2. Zasadniczo definiuje **HttpResponseMessage** fabryki. Oto niektóre korzyści wynikające z używania **IHttpActionResult** interfejsu:

- Upraszcza [testy jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerów.
- Przenosi wspólnej logiki do tworzenia odpowiedzi HTTP do osobnych klas.
- Sprawia, że celem akcji kontrolera jaśniejszy, ukrywając szczegółów niskiego poziomu konstruowanie odpowiedzi.

**IHttpActionResult** zawiera jedną metodę **ExecuteAsync**, które asynchronicznie tworzy **HttpResponseMessage** wystąpienia.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Jeśli akcja kontrolera zwraca **IHttpActionResult**, wywołania interfejsu API sieci Web **ExecuteAsync** metodę w celu utworzenia **HttpResponseMessage**. Następnie konwertuje **HttpResponseMessage** do komunikatu odpowiedzi HTTP.

Oto prosty przewidujące z **IHttpActionResult** tworzącą odpowiedź jako zwykły tekst:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Przykład akcji kontrolera:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Odpowiedź:

[!code-console[Main](action-results/samples/sample9.cmd)]

Częściej, którego użyjesz **IHttpActionResult** zdefiniowane w implementacji  **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)**  przestrzeni nazw. **Klasy ApiController** klasa definiuje metody pomocnicze, które zwracają wyniki tych działań wbudowanych.

W poniższym przykładzie, jeśli żądanie nie pasuje do istniejącego Identyfikatora produktu, kontrolera wywołuje [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) do tworzenia odpowiedzi 404 (nie znaleziono). W przeciwnym razie wywołuje kontrolera [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), który utworzy odpowiedź 200 (OK) która zawiera produktu.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Inne typy zwracane

Dla wszystkich innych typów zwracanych, używa interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do serializacji wartości zwracanej. Interfejs API sieci Web zapisuje wartość zserializowana do treści odpowiedzi. Kod stanu odpowiedzi jest 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Wadą tego podejścia to, że bezpośrednio nie może zwrócić kodu błędu, takie jak 404. Jednak może zgłosić **HttpResponseException** kodów błędów. Aby uzyskać więcej informacji, zobacz [obsługi wyjątków w ASP.NET Web API](../error-handling/exception-handling.md).

Interfejsu API sieci Web używa nagłówek Accept w żądaniu, aby wybrać element formatujący. Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).

Przykładowe żądanie

[!code-console[Main](action-results/samples/sample12.cmd)]

Przykład odpowiedzi:

[!code-console[Main](action-results/samples/sample13.cmd)]
