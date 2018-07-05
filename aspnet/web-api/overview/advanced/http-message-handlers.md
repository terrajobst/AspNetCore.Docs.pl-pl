---
uid: web-api/overview/advanced/http-message-handlers
title: Programy obsługi komunikatów HTTP we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: daf5bac8203beca3be0036b798188e373b02212a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394524"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Programy obsługi komunikatów HTTP we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

A *obsługi wiadomości* to klasa, która odbierze żądanie HTTP, a następnie zwraca odpowiedź HTTP. Programy obsługi komunikatów dziedziczyć abstrakcyjnej **klasa HttpMessageHandler** klasy.

Zazwyczaj szereg programy obsługi komunikatów są połączone. Pierwszego programu obsługi odebrania żądania HTTP, przetwarza je i zapewnia żądania do następnej procedury obsługi. W pewnym momencie odpowiedzi jest tworzony i powraca łańcuch. Ten wzorzec jest nazywany *delegowanie* programu obsługi.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Programy obsługi komunikatów po stronie serwera

Po stronie serwera potok składnika Web API używa obsługi niektórych wbudowanych wiadomości:

- **HttpServer** pobiera żądania od hosta.
- **HttpRoutingDispatcher** wysyła żądania na podstawie trasy.
- **HttpControllerDispatcher** wysyła żądanie do kontrolera internetowego interfejsu API.

Niestandardowe programy obsługi można dodać do potoku. Programy obsługi komunikatów dla zastosowań dobre są odciąż przekrojowe zagadnienia, które działają na poziomie HTTP wiadomości (zamiast akcji kontrolera). Może na przykład program obsługi komunikatów:

- Odczytywać lub modyfikować nagłówki żądania.
- Dodaj nagłówek odpowiedzi do odpowiedzi.
- Sprawdź poprawność żądania, zanim dotrą kontrolera.

Ten diagram przedstawia dwa niestandardowe programy obsługi dodaje do potoku:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Po stronie klienta HttpClient używa także procedury obsługi komunikatów. Aby uzyskać więcej informacji, zobacz [programy obsługi komunikatów HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Programy obsługi komunikatów niestandardowych

Aby napisać program obsługi komunikatów niestandardowych, pochodzi od **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody. Ta metoda ma następujący podpis:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Ta metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **obiektu HttpResponseMessage**. Typowa implementacja wykonuje następujące czynności:

1. Przetwarzanie komunikatu żądania.
2. Wywołaj `base.SendAsync` wysyłać żądania do wewnętrznego programu obsługi.
3. Wewnętrzny program obsługi zwraca komunikat odpowiedzi. (Ten krok jest asynchroniczne).
4. Przetworzenie odpowiedzi i zwraca go do obiektu wywołującego.

Oto przykład prosta:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Wywołanie `base.SendAsync` jest asynchroniczna. Jeśli program obsługi ma wszelkie prace po tym wywołaniu, użyj **await** — słowo kluczowe, jak pokazano.


Obsługi delegowania można również pominąć wewnętrznym programem obsługi i bezpośrednio tworzyć odpowiedzi:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Jeśli delegowanie obsługi tworzy odpowiedź bez wywoływania `base.SendAsync`, żądanie pomija pozostałego potoku. Może to być przydatne w przypadku obsługi, która weryfikuje żądanie (Tworzenie odpowiedź o błędzie).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Dodawanie obsługi do potoku

Aby dodać program obsługi komunikatów po stronie serwera, należy dodać program obsługi **HttpConfiguration.MessageHandlers** kolekcji. Jeśli szablon "Platformy ASP.NET MVC 4 aplikacja sieci Web" są używane do tworzenia projektu, możesz to zrobić to wewnątrz **WebApiConfig** klasy:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Programy obsługi komunikatów są wywoływane w tej samej kolejności, w jakiej występują w **MessageHandlers** kolekcji. Z powodu zagnieżdżenia, komunikat odpowiedzi jest wysyłane w drugą stronę. Ostatnią procedurę obsługi jest pierwszy komunikat odpowiedzi.

Należy zauważyć, że nie musisz ustawić wewnętrzny obsługi; w ramach interfejsu API sieci Web automatycznie nawiązuje połączenie obsługi komunikatów.

Jeśli jesteś [hostingu samodzielnego](../older-versions/self-host-a-web-api.md), Utwórz wystąpienie obiektu **HttpSelfHostConfiguration** klasy i dodawanie obsługi do **MessageHandlers** kolekcji.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Teraz Przyjrzyjmy się kilka przykładów programy obsługi komunikatów niestandardowych.

## <a name="example-x-http-method-override"></a>Przykład: X-HTTP-Method-Override

X-HTTP-Method-Override jest niestandardowy nagłówek HTTP. Jest ona przeznaczona dla klientów, którzy nie mogą wysyłać pewne typy żądań HTTP, takich jak PUT lub DELETE. Zamiast tego klient wysyła żądanie POST i ustawia dla nagłówka X-HTTP-Method-Override żądanej metody. Na przykład:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Poniżej przedstawiono program obsługi komunikatów, który dodaje obsługę X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

W **SendAsync** metody obsługi sprawdza, czy komunikat żądania jest żądaniem POST i czy zawiera nagłówek X-HTTP-Method-Override. Jeśli tak, sprawdza poprawność wartości nagłówka, a następnie modyfikuje metoda żądania. Ponadto program obsługi wyjątku wywołuje `base.SendAsync` do przekazywania wiadomości do następnej procedury obsługi.

Gdy żądanie dotrze **HttpControllerDispatcher** klasy **HttpControllerDispatcher** będzie kierować żądania oparte na metodzie zaktualizowane żądanie.

## <a name="example-adding-a-custom-response-header"></a>Przykład: Dodanie nagłówka odpowiedzi niestandardowych

Poniżej przedstawiono program obsługi komunikatów, umożliwiający dodawanie niestandardowego nagłówka do każdego komunikatu odpowiedzi:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Po pierwsze, program obsługi wyjątku wywołuje `base.SendAsync` do przekazania żądania do obsługi wiadomości wewnętrznego. Wewnętrzny program obsługi zwraca komunikat odpowiedzi, ale robi tak asynchronicznie przy użyciu **zadań&lt;T&gt;**  obiektu. Komunikat odpowiedzi nie jest dostępna tylko do `base.SendAsync` kończy asynchronicznie.

W tym przykładzie użyto **await** — słowo kluczowe do wykonywania pracy asynchronicznie po `SendAsync` kończy. Jeśli masz na celu platformy .NET Framework 4.0, użyj **zadań**&lt;T&gt;**. ContinueWith** metody:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Przykład: Wyszukiwanie klucza interfejsu API

Niektóre usługi sieci web wymagają klientów uwzględnić klucza interfejsu API w ich żądania. Poniższy przykład pokazuje, jak program obsługi komunikatów można sprawdzić żądania, aby uzyskać prawidłowy klucz interfejsu API:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Ten program obsługi szuka klucza interfejsu API w ciągu zapytania identyfikatora URI. (W tym przykładzie przyjęto założenie, że klucz jest ciąg statyczny. Rzeczywistej implementacji będzie prawdopodobnie użyć bardziej złożoną walidację). Jeśli ciąg zapytania zawiera klucz, program obsługi przekazuje żądanie do wewnętrznym programem obsługi.

Jeśli żądanie nie ma prawidłowego klucza, program obsługi tworzy komunikat odpowiedzi ze stanem 403, dostęp zabroniony. W tym przypadku nie wywołuje program obsługi `base.SendAsync`, więc wewnętrznym programem obsługi nigdy nie odbiera żądanie, ani nie kontrolera. W związku z tym kontrolera można założyć, że wszystkie żądania przychodzące ma prawidłowy klucz interfejsu API.

> [!NOTE]
> Jeśli klucz interfejsu API ma zastosowanie tylko do określonych akcji kontrolera, należy wziąć pod uwagę przy użyciu filtru akcji zamiast programu obsługi komunikatów. Filtry akcji uruchamiania routingu identyfikator URI zostanie wykonane.


## <a name="per-route-message-handlers"></a>Programy obsługi komunikatów Route

Programy obsługi w **HttpConfiguration.MessageHandlers** kolekcji są stosowane globalnie.

Alternatywnie można dodać program obsługi komunikatów do określonej trasy, podczas definiowania trasy:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

W tym przykładzie, jeśli pasuje do identyfikatora URI żądania "Route2", żądanie jest wysyłane do `MessageHandler2`. Na poniższym diagramie przedstawiono potok dla tych na dwa sposoby:

![](http-message-handlers/_static/image4.png)

Należy zauważyć, że `MessageHandler2` zastępuje domyślny **HttpControllerDispatcher**. W tym przykładzie `MessageHandler2` tworzy odpowiedź, a żądania które pasują do "Route2" nigdy nie przechodź do kontrolera. Dzięki temu można zastąpić całego mechanizmu kontrolera interfejsu API sieci Web przy użyciu własnego niestandardowego punktu końcowego.

Alternatywnie program obsługi komunikatów dla trasy można delegować do **HttpControllerDispatcher**, który następnie wysyła do kontrolera.

![](http-message-handlers/_static/image5.png)

Poniższy kod przedstawia sposób konfigurowania tej trasy:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
