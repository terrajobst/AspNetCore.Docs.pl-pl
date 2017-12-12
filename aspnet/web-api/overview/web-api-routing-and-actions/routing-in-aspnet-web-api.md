---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: "Routing w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>Routing w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym artykule opisano, jak składnika ASP.NET Web API kieruje żądania HTTP do kontrolerów.

> [!NOTE]
> Jeśli znasz platformy ASP.NET MVC, Web API routingu jest bardzo podobny do routingu MVC. Główna różnica polega na tym, że interfejsu API sieci Web używa metody HTTP, nie ścieżka identyfikatora URI, aby wybrać akcję. Umożliwia również MVC stylu routingu w składniku Web API. W tym artykule nie przyjmuje żadnych wiedzy na temat platformy ASP.NET MVC.


## <a name="routing-tables"></a>Tabele routingu

W interfejsie API sieci Web ASP.NET *kontrolera* jest klasa, która obsługuje żądania HTTP. Metody publiczne kontrolera są nazywane *metod akcji* lub po prostu *akcje*. Gdy strukturę interfejsu API sieci Web odbiera żądanie, kieruje żądanie do akcji.

Aby ustalić, jakie działania należy wywołać, używa platformę *tabeli routingu*. Szablon projektu Visual Studio dla interfejsu API sieci Web tworzy trasę domyślną:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Ta trasa jest zdefiniowana w pliku WebApiConfig.cs, który znajduje się w aplikacji\_Start katalogu:

![](routing-in-aspnet-web-api/_static/image1.png)

Aby uzyskać więcej informacji na temat **WebApiConfig** , zobacz [konfigurowania składnika ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Jeśli samodzielną interfejsu API sieci Web, musisz ustawić tabeli routingu bezpośrednio na **HttpSelfHostConfiguration** obiektu. Aby uzyskać więcej informacji, zobacz [interfejs API sieci Web hosta samodzielnego](../older-versions/self-host-a-web-api.md).

Każdy wpis w tabeli routingu zawiera *szablon trasy*. Domyślny szablon trasy dla interfejsu API sieci Web jest &quot;interfejsu api / {controller} / {id}&quot;. W tym szablonie &quot;interfejsu api&quot; jest segment ścieżki literału i {controller} i {id} są zmiennymi symbolu zastępczego.

Gdy strukturę interfejsu API sieci Web otrzymuje żądanie HTTP, próbuje pasuje do identyfikatora URI na jednym z szablonów tras w tabeli routingu. Jeśli żadna trasa nie pasuje, klient odbierze błąd 404. Na przykład następujące identyfikatory URI dopasowania trasy domyślnej:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Jednak następujący identyfikator URI nie jest zgodny, ponieważ brakuje w nim &quot;interfejsu api&quot; segmentu:

- / Kontakty/1

> [!NOTE]
> Powodem przy użyciu "interfejsu api" w trasie jest uniknąć kolizji z routing na platformie ASP.NET MVC. Dzięki temu można mieć &quot;/kontaktuje się&quot; przejdź do kontrolera MVC i &quot;/api/contacts&quot; przejdź do kontrolera interfejsu API sieci Web. Oczywiście jeśli ta Konwencja nie podoba, można zmienić tabeli trasy domyślnej.

Po znalezieniu pasującej trasy interfejsu API sieci Web wybiera kontroler i akcję:

- Aby odnaleźć kontrolera, interfejsu API sieci Web dodaje &quot;kontrolera&quot; wartość *{controller}* zmiennej.
- Można znaleźć akcji, interfejsu API sieci Web analizuje metodę HTTP, a następnie szuka akcji, których nazwa rozpoczyna się z tą nazwą metody HTTP. Na przykład z żądania GET, interfejsu API sieci Web szuka akcję, która rozpoczyna się od &quot;uzyskać... &quot;, takich jak &quot;GetContact&quot; lub &quot;GetAllContacts&quot;. Konwencja dotyczy tylko GET, POST, PUT i DELETE metody. Inne metody HTTP można włączyć za pomocą atrybutów kontrolera. Firma Microsoft będzie Zobacz przykład tego później.
- Inne zmienne symbolu zastępczego w szablonie trasy, takich jak *{id}* są mapowane na parametry akcji.

Oto przykład. Załóżmy, że zdefiniujesz następujący kontroler:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Poniżej przedstawiono niektóre możliwe żądania HTTP wraz z akcji, które pobiera wywoływane dla każdego:

| Metoda HTTP | Ścieżka identyfikatora URI | Akcja | Parametr |
| --- | --- | --- | --- |
| POBIERZ | Interfejs API/produktów | GetAllProducts | *(Brak)* |
| POBIERZ | 4-API/produktów | GetProductById | 4 |
| DELETE | 4-API/produktów | DeleteProduct | 4 |
| POST | Interfejs API/produktów | *(Brak dopasowań)* |  |

Zwróć uwagę, że *{id}* segmencie identyfikatora URI, jeśli istnieje, jest mapowany na *identyfikator* parametru akcji. W tym przykładzie kontrolera definiuje dwie metody GET, jeden z *identyfikator* parametr i bez parametrów.

Ponadto Pamiętaj, że żądanie POST zakończy się niepowodzeniem, ponieważ kontroler nie definiuje &quot;Post... &quot; metody.

## <a name="routing-variations"></a>Zmiany routingu

Poprzedniej sekcji opisano podstawowego mechanizmu routingu dla interfejsu API sieci Web platformy ASP.NET. W tej sekcji opisano różne wersje.

### <a name="http-methods"></a>Metody HTTP

Zamiast używać konwencji nazewnictwa dla metod HTTP, zostaną jawnie określone metody HTTP dla akcji przez metodę akcji za pomocą dekoracji **HttpGet**, **HttpPut**, **HttpPost** , lub **HttpDelete** atrybutu.

W poniższym przykładzie metoda FindProduct jest mapowany na żądania GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Zezwalaj na wiele metod HTTP dla akcji lub Zezwalaj metod HTTP innych niż GET, PUT, POST i DELETE, należy użyć **AcceptVerbs** atrybut, który przyjmuje listę metod HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing według nazwy akcji

Z szablon routingu domyślnego interfejsu API sieci Web używa metody HTTP, aby wybrać akcję. Można jednak również utworzyć trasy, gdzie nazwa akcji jest dołączona do identyfikatora URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

W tym szablonie trasy *{action}* nazwy parametrów metody akcji kontrolera. Z tym stylem routingu umożliwia określenie dopuszczalnych metod HTTP atrybutów. Na przykład załóżmy, że dany kontroler ma następującą metodę:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

W takim przypadku żądania GET "interfejsu api/produkty/szczegóły/1" spowoduje mapy do metody szczegóły. Ten styl routingu jest podobny do platformy ASP.NET MVC i może być odpowiednie dla interfejsu API w stylu wywołania RPC.

Nazwa akcji można zastąpić przy użyciu **Nazwa akcji** atrybutu. W poniższym przykładzie są dwie akcje, które mapują &quot;interfejsu api/produkty/miniatur/*identyfikator*. Obsługuje jeden GET, a druga POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Akcje z systemem innym niż

Aby zapobiec metodę pobierania wywołany jako akcję, należy użyć **NonAction** atrybutu. To sygnały do struktury czy metoda nie jest akcją, nawet jeśli w przeciwnym razie dopasuje reguły routingu.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Dalsze informacje

W tym temacie podano Widok ogólny routingu. Aby uzyskać więcej szczegółów, zobacz [routingu i wybór akcji](routing-and-action-selection.md), która opisuje dokładnie sposób platformę pasuje identyfikator URI do trasy, wybiera kontrolera, a następnie wybiera akcję do wywołania.
