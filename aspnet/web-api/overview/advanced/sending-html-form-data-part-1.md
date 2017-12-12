---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Wysyłanie danych formularza HTML zakodowanych w składniku ASP.NET Web API: dane formularza urlencoded | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Wysyłanie danych formularza HTML zakodowanych w składniku ASP.NET Web API: urlencoded formularza danych
====================
przez [Wasson Jan](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Część 1: Dane formularza urlencoded

W tym artykule pokazano, jak dane formularza urlencoded do kontrolera interfejsu API sieci Web.

- [Przegląd formularzy HTML](#overview_of_html_forms)
- [Wysyłanie typów złożonych](#sending_complex_types)
- [Wysyłanie danych formularza za pomocą technologii AJAX](#sending_form_data_via_ajax)
- [Wysyłanie typy proste](#sending_simple_types)

> [!NOTE]
> [Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Przegląd formularzy HTML

Użyj formularzy HTML GET lub POST do wysyłania danych do serwera. **Metody** atrybutu **formularza** element udostępnia metodę HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Domyślną metodą jest GET. Jeśli formularz używa UZYSKAĆ, formularza, do którego dane są kodowane w identyfikatorze URI jako ciąg zapytania. Jeśli formularz używa POST, dane formularza jest umieszczona w treści żądania. W przypadku danych zaksięgowany **typ kodowania** atrybut określa format treści żądania:

| Typ kodowania | Opis |
| --- | --- |
| Application/x--www-form-urlencoded | Dane formularza są kodowane jako pary nazwa/wartość, podobny do ciągu zapytania identyfikatora URI. Jest to domyślny format dla żądania POST. |
| dane multipart/formularza | Dane formularza są kodowane jako wieloczęściowej wiadomości MIME. Użyj tego formatu, jeśli plik jest przekazywany do serwera. |

Część 1 w tym artykule analizuje format x--www-form-urlencoded. [Część 2](sending-html-form-data-part-2.md) opisuje wieloczęściowej wiadomości MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Wysyłanie typów złożonych

Zazwyczaj będzie wysyłać typu złożonego, składa się z wartości z kilku kontrolek formularza. Należy wziąć pod uwagę następujące modelu, który reprezentuje aktualizację stanu:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Oto kontrolera interfejsu API sieci Web, który akceptuje `Update` obiektu za pomocą POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Używa tego kontrolera [routingu opartego na akcję](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc jest szablon trasy &quot;interfejsu api / {controller} / {action} / {id}&quot;. Klient zostanie wysłany dane do &quot;/api/updates/complex&quot;.


Teraz napiszmy formularza HTML, aby umożliwić użytkownikom przesyłanie aktualizacji stanu.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Zwróć uwagę, że **akcji** atrybutu w formularzu jest identyfikatorem URI naszych akcji kontrolera. Oto formularza z niektórych wartości w:

![](sending-html-form-data-part-1/_static/image1.png)

Gdy użytkownik kliknie przycisk Prześlij, przeglądarka wysyła żądanie HTTP podobny do następującego:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Zwróć uwagę, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość. Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość w wystąpienie klasy `Update` klasy.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Wysyłanie danych formularza za pomocą technologii AJAX

Gdy użytkownik przesyła formularz, przeglądarka przechodzi od bieżącej strony i renderuje treści komunikatu odpowiedzi. Gdy odpowiedź jest strona HTML to normalne. Interfejs API sieci Web, jednak treść odpowiedzi jest zwykle albo pusta lub zawiera dane strukturalne, takie jak JSON. W takim przypadku warto więcej wysłanie żądania danych formularza za pomocą technologii AJAX, dzięki czemu strony może przetwarzać odpowiedzi.

Poniższy kod przedstawia sposób post formularza danych przy użyciu jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **przesłać** funkcja zastępuje akcji formularza z nowych funkcji. Zastępuje to domyślne zachowanie przycisku Prześlij. **Serializować** funkcja serializuje dane formularza do pary nazwa/wartość. Aby wysłać dane formularza do serwera, należy wywołać `$.post()`.

Po zakończeniu wykonywania żądania `.success()` lub `.error()` obsługi jest wyświetlany odpowiedni komunikat.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Wysyłanie typy proste

W poprzednich sekcjach wysłaliśmy typu złożonego interfejsu API sieci Web deserializacji do wystąpienia klasy modelu. Można również wysłać typów prostych, takiego jak ciąg.

> [!NOTE]
> Przed wysłaniem typu prostego, należy wziąć pod uwagę zawijania wartość zamiast tego w typie złożonym. Zapewnia korzyści wynikające z weryfikacją modelu po stronie serwera i ułatwia Rozszerzanie modelu, jeśli to konieczne.


Podstawowe kroki, aby wysłać typu prostego są takie same, ale istnieją dwie niewielkie różnice. Po pierwsze, w kontrolerze, musi dekoracji nazwę parametru z **FromBody** atrybutu.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Domyślnie interfejsu API sieci Web próbuje pobrać proste typy z identyfikatora URI żądania. **FromBody** atrybut informuje interfejsu API sieci Web można odczytać wartości z treści żądania.

> [!NOTE]
> Interfejs API sieci Web odczytuje treść odpowiedzi co najwyżej jeden raz, dlatego tylko jeden parametr akcji mogą pochodzić z treści żądania. Jeśli trzeba uzyskać wiele wartości z treści żądania, należy zdefiniować typu złożonego.


Po drugie klient musi wysłać wartość w następującym formacie:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

W szczególności część nazwy pary nazwa/wartość może być pusta dla typu prostego. Nie wszystkie przeglądarki obsługują to formularzy HTML, ale możesz utworzyć ten format w skrypt w następujący sposób:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Oto przykład formularza:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

I poniżej przedstawiono skrypt do przesyłania wartości formularza. Jedyną różnicą z poprzednich skryptu jest argument przekazany do **post** funkcji.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Można użyć tej samej metody, aby wysłać tablicę typów prostych:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME](sending-html-form-data-part-2.md)
