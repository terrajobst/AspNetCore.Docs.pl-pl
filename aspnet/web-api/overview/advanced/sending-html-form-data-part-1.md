---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: dane zakodowane | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755067"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: dane zakodowane
====================
przez [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Część 1: Dane zakodowane

W tym artykule pokazano, jak publikować dane zakodowane w kontrolerze interfejsu API sieci Web.

- [Przegląd formularzy HTML](#overview_of_html_forms)
- [Wysyłanie typy złożone](#sending_complex_types)
- [Wysyłanie danych formularza za pomocą technologii AJAX](#sending_form_data_via_ajax)
- [Wysyłanie typy proste](#sending_simple_types)

> [!NOTE]
> [Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Przegląd formularzy HTML

Użyj formularzy HTML Uzyskaj lub POST do wysyłania danych do serwera. **Metoda** atrybutu **formularza** elementu udostępnia metodę HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Domyślną metodą jest metoda GET. Formularz używa otrzymują, formularza, do którego dane są kodowane w identyfikatorze URI jako ciąg zapytania. Jeśli formularz korzysta z wpisu, dane formularza jest umieszczany w treści żądania. W przypadku danych zaksięgowana **typ kodowania** atrybut określa format treści żądania:

| Typ kodowania | Opis |
| --- | --- |
| application/x-www-form-urlencoded | Dane formularza jest zakodowane jako pary nazwa/wartość, podobnie jak ciąg zapytania URI. Jest to domyślny format wpisu. |
| multipart/formularza data | Dane formularza jest zakodowane jako wieloczęściowej wiadomości MIME. Użyj tego formatu, jeśli plik jest przekazywany do serwera. |

Część 1 w tym artykule patrzy na format x--www-form-urlencoded. [Część 2](sending-html-form-data-part-2.md) opisuje wieloczęściowej wiadomości MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Wysyłanie typy złożone

Zazwyczaj będzie wysyłać typ złożony, zawierający wartości z kilku kontrolek formularza. Należy wziąć pod uwagę następujące modelu, który reprezentuje aktualizację stanu:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Poniżej przedstawiono kontroler internetowego interfejsu API, który akceptuje `Update` obiektu za pomocą wpisu.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Korzysta z tego kontrolera [routing oparty na akcję](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc szablon trasy jest &quot;interfejsu api / {controller} / {action} / {id}&quot;. Klient będzie wysyłania danych do &quot;/api/updates/complex&quot;.


Teraz napiszmy formularza HTML służący do przesyłania aktualizacji stanu.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Należy zauważyć, że **akcji** w formie znajduje się atrybut identyfikatora URI o naszej akcji kontrolera. Poniżej przedstawiono formularz z niektórych wartościami podanymi w:

![](sending-html-form-data-part-1/_static/image1.png)

Gdy użytkownik kliknie przycisk Prześlij, przeglądarce wysyła żądanie HTTP podobny do następującego:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Należy zauważyć, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość. Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość na wystąpienie `Update` klasy.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Wysyłanie danych formularza za pomocą technologii AJAX

Gdy użytkownik przesyła formularz, przeglądarka przechodzi od bieżącej strony i renderuje treści komunikatu odpowiedzi. To normalne, gdy odpowiedź jest stroną HTML. Za pomocą internetowego interfejsu API, jednak treść odpowiedzi jest zwykle albo pusta lub zawiera dane strukturalnych, takich jak JSON. W takim przypadku warto więcej wysyłania żądania danych formularza za pomocą interfejsu AJAX, dzięki czemu strony może przetwarzać odpowiedzi.

Poniższy kod pokazuje, jak publikować dane formularza przy użyciu jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **przesłać** funkcja zastępuje akcji formularza za pomocą nowych funkcji. Zastępuje to domyślne zachowanie przycisk Prześlij. **Serializacji** funkcja serializuje dane formularza do pary nazwa/wartość. Aby wysłać dane formularza do serwera, należy wywołać `$.post()`.

Po ukończeniu żądania `.success()` lub `.error()` obsługi wyświetla odpowiedni komunikat dla użytkownika.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Wysyłanie typy proste

W poprzednich sekcjach wysłaliśmy typ złożony, który internetowego interfejsu API została zdeserializowana do wystąpienia klasy modelu. Można również wysyłać typów prostych, takie jak ciąg.

> [!NOTE]
> Przed wysłaniem typu prostego, należy wziąć pod uwagę zamiast zawijania wartość w typie złożonym. Zapewnia korzyści wynikające z weryfikacją modelu po stronie serwera i ułatwia Rozszerzanie modelu, jeśli to konieczne.


Podstawowe kroki, aby wysłać typu prostego są takie same, ale istnieją dwa drobne różnice. Najpierw na kontrolerze musi dekorowania nazwy parametru z **FromBody** atrybutu.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Domyślnie interfejs API sieci Web próbuje uzyskać proste typy z identyfikatora URI żądania. **FromBody** atrybut informuje interfejsu API sieci Web ma zostać odczytana wartość z treści żądania.

> [!NOTE]
> Interfejs API sieci Web odczytuje treść odpowiedzi co najwyżej jeden raz, dlatego tylko jeden parametr akcji mogą pochodzić z treści żądania. Jeśli musisz pobrać wiele wartości z treści żądania, należy zdefiniować typ złożony.


Po drugie klient musi wysyłać wartość w następującym formacie:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

W szczególności część nazwy pary nazwa/wartość musi być pusty dla typu prostego. Nie wszystkie przeglądarki są obsługiwane przez to formularzy HTML, ale utworzysz ten format w skrypcie w następujący sposób:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Oto przykład formularza:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

A Oto skrypt, aby przesłać wartości formularza. Jedyną różnicą między poprzednim skrypcie jest argument przekazany do **wpis** funkcji.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

To samo podejście umożliwia wysyłanie tablicę typów prostych:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME](sending-html-form-data-part-2.md)
