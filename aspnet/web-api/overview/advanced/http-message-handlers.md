---
uid: web-api/overview/advanced/http-message-handlers
title: Programy obsługi komunikatów HTTP w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566390"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Programy obsługi komunikatów HTTP w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

A *obsługi wiadomości* jest klasa, która odbiera żądania HTTP i zwraca odpowiedź HTTP. Programy obsługi wiadomości pochodzi z klasy abstrakcyjnej **HttpMessageHandler** klasy.

Zazwyczaj szereg obsługi komunikatów są połączone. Pierwsza metoda obsługi odbiera żądanie HTTP, nie przetwarza i przekazuje żądania do następnej procedury obsługi. W pewnym momencie odpowiedzi jest tworzona i rośnie łańcucha. Ten wzorzec jest nazywany *delegowanie* obsługi.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Programy obsługi wiadomości po stronie serwera

Po stronie serwera potok składnika Web API używa niektóre programy obsługi komunikatów wbudowany:

- **HttpServer** pobiera żądania od hosta.
- **HttpRoutingDispatcher** wywołuje żądania na podstawie trasy.
- **HttpControllerDispatcher** wysyła żądanie do kontrolera interfejsu API sieci Web.

Niestandardowe programy obsługi można dodać do potoku. Programy obsługi komunikatów są dobrym dotyczy kompleksowymi, które działają na poziomie HTTP wiadomości (zamiast akcji kontrolera). Może na przykład program obsługi komunikatów:

- Odczyt lub modyfikacja nagłówki żądania.
- Dodaj nagłówek odpowiedzi do odpowiedzi.
- Sprawdzanie poprawności żądań przed dotarciem kontrolera.

Ten diagram przedstawia dwa niestandardowe programy obsługi dodaje do potoku:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Po stronie klienta HttpClient używa programy obsługi wiadomości. Aby uzyskać więcej informacji, zobacz [obsługi komunikatów HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Programy obsługi wiadomości niestandardowych

Aby napisać program obsługi komunikatów niestandardowych, pochodzi z **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody. Ta metoda ma następującą sygnaturą:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Metoda korzysta z **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**. Typowa implementacja wykonuje następujące czynności:

1. Proces komunikat żądania.
2. Wywołanie `base.SendAsync` można wysłać żądania do wewnętrznego programu obsługi.
3. Wewnętrzny program obsługi zwraca komunikat odpowiedzi. (Ten krok jest asynchroniczne).
4. Przetwarzanie odpowiedzi i przywrócić go do obiektu wywołującego.

Oto przykład prosta:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Wywołanie `base.SendAsync` jest asynchroniczne. Jeśli program obsługi działa żadnych po to wywołanie, użyj **await** — słowo kluczowe, jak pokazano.


Delegujące obsługi można również pominąć wewnętrznym programem obsługi i bezpośrednio utworzyć odpowiedzi:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Jeśli delegowanie obsługi tworzy odpowiedź bez wywoływania elementu `base.SendAsync`, żądanie pomija pozostałego potoku. Może to być przydatne w przypadku obsługi, która weryfikuje żądanie (Tworzenie odpowiedzi błędu).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Dodawanie obsługi do potoku

Aby dodać obsługi wiadomości po stronie serwera, Dodaj program obsługi do **HttpConfiguration.MessageHandlers** kolekcji. Jeśli szablon "Platformy ASP.NET MVC 4 aplikacji sieci Web" są używane do tworzenia projektu, możesz to zrobić to wewnątrz **WebApiConfig** klasy:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Programy obsługi komunikatów są nazywane w tej samej kolejności, w jakiej występują w **MessageHandlers** kolekcji. Ponieważ są one zagnieżdżone, komunikat odpowiedzi porusza się w innym kierunku. Obsługa ostatniej jest pierwszą osobą, która jest wyświetlany komunikat odpowiedzi.

Zwróć uwagę, że nie trzeba ustawić obsługi wewnętrzny; strukturę interfejsu API sieci Web automatycznie łączy programy obsługi wiadomości.

Jeśli jesteś [hostingu samodzielnego](../older-versions/self-host-a-web-api.md), Utwórz wystąpienie **HttpSelfHostConfiguration** klasy i Dodaj do obsługi **MessageHandlers** kolekcji.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Teraz Przyjrzyjmy się przykłady obsługi komunikatów niestandardowych.

## <a name="example-x-http-method-override"></a>Przykład: X-HTTP-Method-Override

X-HTTP-Method-Override jest niestandardowy nagłówek HTTP. Jest on przeznaczony dla klientów, którzy nie mogą wysyłać niektóre typy żądań HTTP, takie jak PUT i DELETE. Zamiast tego klient wysyła żądanie POST i ustawia dla nagłówka X-HTTP-Method-Override żądanej metody. Na przykład:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Oto obsługi wiadomości, który dodaje obsługę X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

W **SendAsync** metody, program obsługi sprawdza, czy komunikat żądania jest wysłanie żądania POST i czy zawiera nagłówek X-HTTP-Method-Override. Jeśli tak, sprawdza poprawność wartości nagłówka, a następnie modyfikuje metodę żądania. Na koniec wywołuje program obsługi `base.SendAsync` do przekazywania wiadomości do następnej procedury obsługi.

Gdy żądanie dotrze **HttpControllerDispatcher** klasy **HttpControllerDispatcher** będzie przekierowywać żądania na podstawie metody żądania zaktualizowane.

## <a name="example-adding-a-custom-response-header"></a>Przykład: Dodawanie nagłówka odpowiedzi niestandardowych

Oto obsługi wiadomości, umożliwiający dodawanie niestandardowego nagłówka do każdej wiadomości odpowiedzi:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Po pierwsze, wywołuje program obsługi `base.SendAsync` do przekazania żądania do obsługi wiadomości wewnętrzny. Wewnętrzny program obsługi zwraca komunikat odpowiedzi, ale pojawia się więc asynchronicznie za pomocą **zadań&lt;T&gt;**  obiektu. Komunikat odpowiedzi nie jest dostępna do `base.SendAsync` kończy asynchronicznie.

W tym przykładzie użyto **await** — słowo kluczowe do wykonywania pracy asynchronicznie po `SendAsync` zakończeniu. Jeśli ma być przeznaczona dla programu .NET Framework 4.0, użyj **zadań**&lt;T&gt;**. ContinueWith** metody:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Przykład: Sprawdzanie klucz interfejsu API

Niektóre usługi sieci web wymaga klientów obejmują klucz interfejsu API w żądaniu. W poniższym przykładzie pokazano, jak sprawdzić obsługi wiadomości żądania prawidłowy klucz interfejsu API:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Ten program obsługi jest szuka klucz interfejsu API w ciągu zapytania identyfikatora URI. (W tym przykładzie przyjęto założenie, że klucz jest statyczny ciąg. Rzeczywistego wykonania prawdopodobnie użyje bardziej złożonych reguł sprawdzania poprawności.) Jeśli ciąg zapytania zawiera klucz, program obsługi przekazuje żądanie do wewnętrznego programu obsługi.

Jeśli żądanie nie zawiera prawidłowego klucza, program obsługi tworzy komunikat odpowiedzi o stanie 403, zabroniony. W takim przypadku nie wywołuje program obsługi `base.SendAsync`, więc wewnętrznym programem obsługi. nigdy nie odbiera żądanie, ani nie kontrolera. W związku z tym kontrolerem można założyć, że wszystkie żądania przychodzące ma prawidłowy klucz interfejsu API.

> [!NOTE]
> Jeśli klucz interfejsu API ma zastosowanie tylko do określonych akcji kontrolera, należy rozważyć użycie filtr akcji zamiast obsługi wiadomości. Filtry akcji Uruchom po wykonaniu routingu identyfikatora URI.


## <a name="per-route-message-handlers"></a>Programy obsługi wiadomości trasy

Programy obsługi zdarzeń w **HttpConfiguration.MessageHandlers** kolekcji są stosowane globalnie.

Alternatywnie można dodać obsługi wiadomości do określonej trasy, podczas definiowania trasy:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

W tym przykładzie, jeśli pasuje do identyfikatora URI żądania "Route2", żądanie jest wysyłane do `MessageHandler2`. Na poniższym diagramie przedstawiono potoku dla tych na dwa sposoby:

![](http-message-handlers/_static/image4.png)

Zwróć uwagę, że `MessageHandler2` zastępuje domyślny **HttpControllerDispatcher**. W tym przykładzie `MessageHandler2` tworzy odpowiedzi i żądania zgodnych "Route2" nigdy nie przechodź do kontrolera. Dzięki temu można zastąpić cały mechanizm kontrolera interfejsu API sieci Web własne niestandardowe punktu końcowego.

Alternatywnie można delegować program obsługi komunikatów dla trasy **HttpControllerDispatcher**, który następnie wysyła do kontrolera.

![](http-message-handlers/_static/image5.png)

Poniższy kod przedstawia sposób konfigurowania tej trasy:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
