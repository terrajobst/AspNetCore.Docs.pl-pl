---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Akcja wyniki w interfejsie Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808930"
---
<a name="action-results-in-web-api-2"></a>Wyniki akcji w interfejsie Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym temacie opisano, jak ASP.NET Web API konwertuje wartość zwracaną akcji kontrolera, do komunikatu odpowiedzi HTTP.

Akcja kontrolera interfejsu API sieci Web może zwracać dowolne z następujących czynności:

1. void
2. **Element HttpResponseMessage**
3. **IHttpActionResult**
4. Innego typu

W zależności od tego, który z nich jest zwracany, interfejsu API sieci Web używa innego mechanizmu do tworzenia odpowiedzi HTTP.

| Zwracany typ | Jak interfejs API sieci Web tworzy odpowiedź |
| --- | --- |
| void | Zwraca pusty 204 (Brak zawartości) |
| **Element HttpResponseMessage** | Konwertuj bezpośrednio do komunikatu odpowiedzi HTTP. |
| **IHttpActionResult** | Wywołaj **ExecuteAsync** utworzyć **obiektu HttpResponseMessage**, następnie przekonwertować komunikatu odpowiedzi HTTP. |
| Inny typ | Zapisz wartość zwracaną serializacji do treści odpowiedzi; Zwraca 200 (OK). |

Pozostała część tego tematu opisano wszystkie opcje, które bardziej szczegółowo.

## <a name="void"></a>void

Jeśli typ zwracany jest `void`, interfejs API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (Brak zawartości).

Przykład kontrolera:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Odpowiedź HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>Element HttpResponseMessage

Jeśli działanie zwróci [obiektu HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), internetowy interfejs API konwertuje wartość zwracaną bezpośrednio na komunikat odpowiedzi HTTP przy użyciu właściwości **obiektu HttpResponseMessage** obiekt do wypełnienia odpowiedź.

Ta opcja zapewnia wysoki poziom kontroli nad komunikat odpowiedzi. Na przykład następującej akcji kontrolera ustawia nagłówek Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Odpowiedź:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

W przypadku przekazania model domeny, aby **CreateResponse** metody, interfejs API sieci Web używa [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do zapisania Zserializowany model na treść odpowiedzi.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Internetowy interfejs API używa nagłówek Accept w żądaniu, aby wybrać element formatujący. Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** interfejsu została wprowadzona w sieci Web API 2. Zasadniczo definiuje **obiektu HttpResponseMessage** fabryki. Oto niektóre korzyści wynikające z używania **IHttpActionResult** interfejsu:

- Upraszcza [testy jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerach.
- Przenosi wspólnej logiki do tworzenia odpowiedzi HTTP do osobnych klas.
- Sprawia, że celem akcji kontrolera, bardziej zrozumiały, ukrywając niskopoziomowych szczegółów konstruowanie odpowiedzi.

**IHttpActionResult** zawiera jedną metodę **ExecuteAsync**, które asynchronicznie tworzy **obiektu HttpResponseMessage** wystąpienia.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Jeśli akcja kontrolera zwraca **IHttpActionResult**, wywołań interfejsu API sieci Web **ExecuteAsync** metodę w celu utworzenia **obiektu HttpResponseMessage**. A następnie konwertuje **obiektu HttpResponseMessage** do komunikatu odpowiedzi HTTP.

Poniżej przedstawiono prosty przewidujące z **IHttpActionResult** tworząca odpowiedź jako zwykły tekst:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Przykład akcji kontrolera:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Odpowiedź:

[!code-console[Main](action-results/samples/sample9.cmd)]

Częściej, użyjesz **IHttpActionResult** zdefiniowane w implementacji **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** przestrzeni nazw. **Klasy ApiController** klasa definiuje metody pomocnika, które zwracają wyniki tych wbudowanych akcji.

W poniższym przykładzie, jeśli żądanie nie jest zgodny z istniejący identyfikator produktu, kontroler wywołuje [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) do tworzenia odpowiedzi 404 (nie znaleziono). W przeciwnym razie kontroler wywołuje [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), która tworzy odpowiedź 200 (OK), zawiera produktu.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Inne typy zwracane

Dla wszystkich innych typów zwrotu, korzysta z interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) serializować wartość zwracaną. Interfejs API sieci Web zapisuje wartość zserializowana do treści odpowiedzi. Kod stanu odpowiedzi to 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Wadą tego podejścia jest, że bezpośrednio nie może zwrócić kod błędu, takie jak 404. Jednak może zgłosić **HttpResponseException** dla kodów błędów. Aby uzyskać więcej informacji, zobacz [Exception Handling in Web API platformy ASP.NET](../error-handling/exception-handling.md).

Internetowy interfejs API używa nagłówek Accept w żądaniu, aby wybrać element formatujący. Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).

Przykładowe żądanie

[!code-console[Main](action-results/samples/sample12.cmd)]

Przykładowa odpowiedź:

[!code-console[Main](action-results/samples/sample13.cmd)]
