---
title: Zagadnienia dotyczące projektowania interfejsu API SignalR
author: anurse
description: Dowiedz się, jak projektować interfejsy API biblioteki SignalR dla zgodności między wersjami aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903181"
---
# <a name="signalr-api-design-considerations"></a>Zagadnienia dotyczące projektowania interfejsu API SignalR

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)

Ten artykuł zawiera wskazówki dotyczące tworzenia interfejsów API opartych na SignalR.

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>Korzystanie z parametrów obiektów niestandardowych w celu zapewnienia zgodności z poprzednimi wersjami

Dodawanie parametrów do metody koncentratora SignalR (po stronie klienta lub serwera) jest *zmiana powodująca niezgodność*. Oznacza to, że starsze klientów/serwery będą występują błędy podczas próby wywołania metody bez odpowiedniej liczby parametrów. Jednak jest dodawanie właściwości do parametru niestandardowy obiekt **nie** istotnej zmiany. To może służyć do projektowania zgodnych interfejsów API, które są odporne na zmiany po stronie klienta lub serwera.

Na przykład należy wziąć pod uwagę interfejs API po stronie serwera, jak pokazano poniżej:

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

Klient JavaScript wywołania tej metody przy użyciu `invoke` w następujący sposób:

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

Jeśli później dodasz drugi parametr do metody serwera, starsi klienci nie będzie zapewniać wartości tego parametru. Na przykład:

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

Gdy próbuje starego klienta do wywołania tej metody, otrzyma błąd następująco:

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

Na serwerze zostanie wyświetlony komunikat dziennika następująco:

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

Starego klienta wysyłane tylko jeden parametr, ale nowszej interfejsu API serwera wymagane dwa parametry. Przy użyciu niestandardowych obiektów jako parametrów zapewnia większą elastyczność. Umożliwia zmodyfikowanie oryginalny interfejs API, aby użyć niestandardowego obiektu:

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

Teraz klient używa obiektu, aby wywołać metodę:

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

Zamiast opcji dodawania parametr, Dodaj właściwość do `TotalLengthRequest` obiektu:

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

Gdy starego klienta wysyła pojedynczy parametr nadmiarowe `Param2` właściwość zostanie pozostawiony `null`. Można wykryć wiadomością wysłaną przez starszego klienta, sprawdzając `Param2` dla `null` i stosowanie wartości domyślnej. Nowy klient może wysyłać te parametry.

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

Ta sama technika działa dla metody zdefiniowane na komputerze klienckim. Możesz wysłać niestandardowych obiektów po stronie serwera:

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

Po stronie klienta, możesz uzyskać dostęp do `Message` właściwości, a nie za pomocą parametru:

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

Jeśli później zdecydujesz się dodać nadawcy wiadomości do ładunku, Dodaj właściwość do obiektu:

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

Starsi klienci nie oczekiwano `Sender` wartości, dzięki czemu będziesz go zignorować. Nowy klient można go zaakceptować, aktualizując odczytać nową właściwość:

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

W tym przypadku nowego klienta jest również odporny na błędy starego serwera, który nie zapewnia `Sender` wartość. Ponieważ stary serwer nie będzie zapewniać `Sender` wartości, klient sprawdza, czy istnieje ona przed uzyskaniem dostępu do jej.
