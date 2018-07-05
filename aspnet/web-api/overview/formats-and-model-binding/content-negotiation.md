---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Zawartość negocjacji we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, jak ASP.NET Web API implementuje negocjacje zawartości HTTP.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: c4e7a0c2601ca60f081876e83757997a2e920298
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368932"
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negocjowanie zawartości we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym artykule opisano, jak ASP.NET Web API implementuje negocjacji zawartości.

Specyfikacja protokołu HTTP (RFC 2616) definiuje negocjacje zawartości jako "proces wybierania najlepszych reprezentację danej odpowiedzi, gdy istnieje wiele reprezentacji." Te nagłówki żądania są podstawowym mechanizmem negocjowanie zawartości w protokole HTTP:

- **Akceptuj:** typów nośników, które są obsługiwane w odpowiedzi, np. "application/json", "application/xml" lub typu niestandardowe, takie jak &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** zestawów znaków, które są dopuszczalne, takich jak UTF-8 lub ISO 8859-1.
- **Zaakceptuj-Encoding:** które kodowań zawartości są dopuszczalne, takich jak gzip.
- **Zaakceptuj — wartość pola język:** preferowanego języka naturalnego, takich jak "en-us".

Serwer można również przeglądać inne części żądania HTTP. Na przykład jeśli żądanie zawiera nagłówek X-Requested-With, wskazujący żądaniem AJAX serwer może domyślnie do formatu JSON w przypadku bez nagłówka Accept.

W tym artykule omówimy używaniu nagłówków Accept i Accept-Charset interfejsu API sieci Web. (W tej chwili nie jest brak wbudowanej obsługi Accept-Encoding lub Accept-Language).

## <a name="serialization"></a>Serializacja

Jeśli kontroler internetowego interfejsu API zwraca zasobu jako typ CLR, potok serializuje wartość zwracaną i zapisuje go w treści odpowiedzi HTTP.

Na przykład należy wziąć pod uwagę następujące akcji kontrolera:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Klient może wysłać to żądanie HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Serwer może wysłać w odpowiedzi:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

W tym przykładzie klient zażądał JSON, Javascript lub "nic" (\*/\*). Serwer odpowiada przy użyciu reprezentacji JSON `Product` obiektu. Należy zauważyć, że nagłówek Content-Type w odpowiedzi jest ustawiona na &quot;application/json&quot;.

Kontroler może również zwracać **obiektu HttpResponseMessage** obiektu. Aby określić obiekt CLR treść odpowiedzi, należy wywołać **CreateResponse** — metoda rozszerzenia:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Ta opcja zapewnia większą kontrolę nad szczegóły odpowiedzi. Można ustawić kod stanu, Dodaj nagłówki HTTP i tak dalej.

Obiekt, który serializuje zasobu jest nazywany *nośnika elementu formatującego*. Programy formatujące multimedia pochodzić od **element MediaTypeFormatter** klasy. Internetowy interfejs API udostępnia programy formatujące multimedia dla formatów XML i JSON. Ponadto można tworzyć niestandardowe elementy formatujące do obsługi innych typów nośników. Informacje na temat pisania niestandardowego elementu formatującego, zobacz [programy formatujące multimedia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Jak zawartości działa negocjowania

Po pierwsze, potok pobiera **IContentNegotiator** usługi z **HttpConfiguration** obiektu. Zapewnia również na liście programy formatujące multimedia z **HttpConfiguration.Formatters** kolekcji.

Następnie potok wywołuje **IContentNegotiatior.Negotiate**, przekazując:

- Typ obiektu do zserializowania
- Kolekcja programy formatujące multimedia
- Żądanie HTTP

**Negotiate** metoda zwraca dwóch rodzajów informacji:

- Które element formujący do używania
- Typ nośnika dla odpowiedzi

Jeśli element formatujący nie zostanie znaleziony, **Negotiate** metoda zwraca **null**i klienta błędu recevies HTTP 406 (niedozwolone).

Poniższy kod pokazuje, jak kontroler bezpośrednio wywołać negocjacje zawartości:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Ten kod jest równoważna do strony co potoku następuje automatyczne.

## <a name="default-content-negotiator"></a>Moduł negocjowania zawartości domyślny

**DefaultContentNegotiator** klasa udostępnia domyślną implementację elementu **IContentNegotiator**. Aby wybrać element formatujący używa kilku kryteriów.

Po pierwsze element formatujący musi mieć możliwość wykonywać serializację typu. To jest weryfikowana przez wywołanie metody **MediaTypeFormatter.CanWriteType**.

Następnie moduł negocjowania zawartości sprawdza każdy element formatujący i ocenia, jak dobrze pasuje żądania HTTP. Aby ocenić dopasowanie, moduł negocjowania zawartości sprawdza na element formatujący dwie rzeczy:

- **SupportedMediaTypes** kolekcji, która zawiera listę obsługiwanych typów nośnika. Moduł negocjowania zawartości próbuje dopasować tej listy przed żądaniem nagłówek Accept. Należy pamiętać, że nagłówek Accept mogą zawierać zakresy. Na przykład "text/plain" to pasuje do tekstu /\* lub \* / \*.
- **MediaTypeMappings** kolekcji, która zawiera listę **MediaTypeMapping** obiektów. **MediaTypeMapping** klasy zapewnia ogólny sposób zgodne z żądaniami HTTP przy użyciu typów nośników. Na przykład go zamapuj niestandardowego nagłówka HTTP, do określonego typu nośnika.

Jeśli dostępnych jest wiele jest zgodny, zgodna z wins współczynnik najwyższej jakości. Na przykład:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

W tym przykładzie application/json ma czynnikiem dorozumianych jakości w wersji 1.0, więc jest preferowana nad application/xml.

Jeśli nie znaleziono żadnych dopasowań, moduł negocjowania zawartości próbuje dopasować na typ nośnika treści żądania, jeśli istnieje. Na przykład jeśli żądanie zawiera dane JSON, moduł negocjowania zawartości szuka elementu formatującego JSON.

Jeśli nadal istnieją żadnych dopasowań, moduł negocjowania zawartości po prostu wybiera pierwszy element formatujący, który może wykonać serializację typu.

## <a name="selecting-a-character-encoding"></a>Wybranie kodowania znaków

Po wybraniu elementu formatującego moduł negocjowania zawartości wybiera optymalne kodowanie znaków, analizując **SupportedEncodings** właściwość elementu formatującego i dopasowywanie względem nagłówek Accept-Charset w żądaniu (jeśli istnieje).
