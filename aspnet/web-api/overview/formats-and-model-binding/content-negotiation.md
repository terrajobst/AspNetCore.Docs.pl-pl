---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "Zawartość negocjacji w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym artykule opisano, jak składnika ASP.NET Web API zaimplementowano negocjacje zawartości HTTP."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negocjowanie zawartości w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym artykule opisano, jak składnika ASP.NET Web API zaimplementowano negocjacji zawartości.

Specyfikacja protokołu HTTP (RFC 2616) definiuje negocjacje zawartości jako "proces polegający na wyborze najlepsze reprezentację danej odpowiedzi, gdy wiele reprezentacje są dostępne." Podstawowy mechanizm negocjacje zawartości HTTP czy te nagłówki żądań:

- **Akceptuj:** typów nośników, które są dozwolone dla odpowiedzi, takie jak "application/json", "application/xml" lub typu niestandardowe, takie jak &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** zestawów znaków, które są dozwolone, takich jak UTF-8 lub ISO 8859-1.
- **Zaakceptuj kodowania:** które kodowań zawartości są dozwolone, takich jak gzip.
- **Zaakceptuj język:** preferowanego języka naturalnego, takich jak "en-us".

Serwer można również sprawdzić innych części żądania HTTP. Na przykład jeśli żądanie zawiera nagłówek X-Requested-With, wskazującą żądaniem AJAX serwer może być domyślnie JSON Jeśli nagłówek Accept nie występuje.

W tym artykule wyjaśniono, jak interfejsu API sieci Web używa nagłówków Accept i Accept-Charset. (W tej chwili nie jest brak wbudowaną obsługę Accept-Encoding lub Accept-Language).

## <a name="serialization"></a>Serializacja

Jeśli Kontroler interfejsu API sieci Web zwraca zasobu jako typ CLR, potoku serializuje zwracana wartość i zapisuje go w treści odpowiedzi HTTP.

Na przykład wziąć pod uwagę następujące akcji kontrolera:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Klient może wysyłać żądania HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

W odpowiedzi serwer może wysłać:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

W tym przykładzie klient żądał JSON, Javascript lub "wszystko" (\*/\*). Odpowiedź zawierała reprezentacja JSON serwera `Product` obiektu. Należy zauważyć, że ma ustawioną wartość nagłówka Content-Type w odpowiedzi &quot;application/json&quot;.

Kontroler może również zwrócić **HttpResponseMessage** obiektu. Aby określić obiektu CLR dla treści odpowiedzi, należy wywołać **CreateResponse** — metoda rozszerzenia:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Opcja ta zapewnia większą kontrolę nad treść odpowiedzi. Można ustawić kod stanu, Dodaj nagłówków HTTP i tak dalej.

Obiekt, który serializuje zasobu jest nazywany *nośnika elementu formatującego*. Programy formatujące multimedia pochodzi od **MediaTypeFormatter** klasy. Interfejs API sieci Web udostępnia programy formatujące multimedia XML i JSON, a można tworzyć niestandardowe elementy formatujące do obsługi innych typów nośników. Informacje na temat pisania niestandardowego elementu formatującego, zobacz [programy formatujące multimedia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Jak zawartości działa negocjacji

Najpierw pobiera potoku **IContentNegotiator** usługi **HttpConfiguration** obiektu. Zapewnia również na liście programy formatujące multimedia z **HttpConfiguration.Formatters** kolekcji.

Następnie wywołuje potok **IContentNegotiatior.Negotiate**, przekazując:

- Typ obiektu do serializacji
- Kolekcja programy formatujące multimedia
- Żądania HTTP

**Negotiate** metoda zwraca informacje:

- Które element formatujący do użycia
- Typ nośnika dla odpowiedzi

Jeśli element formatujący nie zostanie znaleziony, **Negotiate** metoda zwraca **null**, a błąd recevies HTTP klienta 406 (niedopuszczalne).

Poniższy kod przedstawia sposób kontrolera można bezpośrednio wywołać negocjacje zawartości:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Ten kod jest równoważna do potoku jest automatycznie.

## <a name="default-content-negotiator"></a>Moduł negocjowania zawartości domyślny

**DefaultContentNegotiator** klasa udostępnia domyślną implementację elementu **IContentNegotiator**. Aby wybrać element formatujący używa kilku kryteriów.

Po pierwsze element formatujący musi mieć możliwość wykonywać serializację typu. To jest zweryfikowany przez wywołanie metody **MediaTypeFormatter.CanWriteType**.

Następnie moduł negocjowania zawartości sprawdza elementu formatującego i ocenia skuteczność odpowiada żądania HTTP. Aby ocenić dopasowania, moduł negocjowania zawartości sprawdza na program formatujący dwie czynności:

- **SupportedMediaTypes** kolekcji, która zawiera listę obsługiwanych typów nośnika. Moduł negocjowania zawartości próbuje odpowiada liście dla żądania nagłówek Accept. Należy pamiętać, że nagłówek Accept mogą obejmować zakresów. Na przykład "text/plain" ma dopasowania dla tekstu /\* lub \* / \*.
- **MediaTypeMappings** kolekcji, który zawiera listę **MediaTypeMapping** obiektów. **MediaTypeMapping** klasa umożliwia ogólne pasuje żądania HTTP do typów nośników. Na przykład można go mapować niestandardowy nagłówek HTTP do określonego typu nośnika.

Jeśli dostępnych jest wiele zgodne, niezgodne z wins współczynnik najwyższej jakości. Na przykład:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

W tym przykładzie application/json ma współczynnik jakości domniemanych 1.0, więc jest preferowana przez kod xml i aplikacji.

W przypadku nieodnalezienia żadnych dopasowań moduł negocjowania zawartości próbuje dopasować typ nośnika treści żądania, jeśli istnieje. Na przykład jeśli żądanie zawiera dane JSON, moduł negocjowania zawartości szuka elementu formatującego JSON.

Jeśli nadal nie mają odpowiedników, moduł negocjowania zawartości po prostu wybiera pierwszy element formatujący, który może wykonać serializację typu.

## <a name="selecting-a-character-encoding"></a>Wybranie kodowania znaków

Po wybraniu elementu formatującego, moduł negocjowania zawartości wybierze optymalne kodowanie znaków na podstawie **SupportedEncodings** właściwość element formatujący, a odpowiadające mu względem nagłówka Accept-Charset w żądaniu (jeśli istnieje).
