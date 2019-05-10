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
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="dd7a2-103">Zagadnienia dotyczące projektowania interfejsu API SignalR</span><span class="sxs-lookup"><span data-stu-id="dd7a2-103">SignalR API design considerations</span></span>

<span data-ttu-id="dd7a2-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="dd7a2-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="dd7a2-105">Ten artykuł zawiera wskazówki dotyczące tworzenia interfejsów API opartych na SignalR.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="dd7a2-106">Korzystanie z parametrów obiektów niestandardowych w celu zapewnienia zgodności z poprzednimi wersjami</span><span class="sxs-lookup"><span data-stu-id="dd7a2-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="dd7a2-107">Dodawanie parametrów do metody koncentratora SignalR (po stronie klienta lub serwera) jest *zmiana powodująca niezgodność*.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="dd7a2-108">Oznacza to, że starsze klientów/serwery będą występują błędy podczas próby wywołania metody bez odpowiedniej liczby parametrów.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="dd7a2-109">Jednak jest dodawanie właściwości do parametru niestandardowy obiekt **nie** istotnej zmiany.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="dd7a2-110">To może służyć do projektowania zgodnych interfejsów API, które są odporne na zmiany po stronie klienta lub serwera.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="dd7a2-111">Na przykład należy wziąć pod uwagę interfejs API po stronie serwera, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="dd7a2-112">Klient JavaScript wywołania tej metody przy użyciu `invoke` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="dd7a2-113">Jeśli później dodasz drugi parametr do metody serwera, starsi klienci nie będzie zapewniać wartości tego parametru.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="dd7a2-114">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="dd7a2-115">Gdy próbuje starego klienta do wywołania tej metody, otrzyma błąd następująco:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="dd7a2-116">Na serwerze zostanie wyświetlony komunikat dziennika następująco:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="dd7a2-117">Starego klienta wysyłane tylko jeden parametr, ale nowszej interfejsu API serwera wymagane dwa parametry.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="dd7a2-118">Przy użyciu niestandardowych obiektów jako parametrów zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="dd7a2-119">Umożliwia zmodyfikowanie oryginalny interfejs API, aby użyć niestandardowego obiektu:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="dd7a2-120">Teraz klient używa obiektu, aby wywołać metodę:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="dd7a2-121">Zamiast opcji dodawania parametr, Dodaj właściwość do `TotalLengthRequest` obiektu:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="dd7a2-122">Gdy starego klienta wysyła pojedynczy parametr nadmiarowe `Param2` właściwość zostanie pozostawiony `null`.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="dd7a2-123">Można wykryć wiadomością wysłaną przez starszego klienta, sprawdzając `Param2` dla `null` i stosowanie wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="dd7a2-124">Nowy klient może wysyłać te parametry.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="dd7a2-125">Ta sama technika działa dla metody zdefiniowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="dd7a2-126">Możesz wysłać niestandardowych obiektów po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="dd7a2-127">Po stronie klienta, możesz uzyskać dostęp do `Message` właściwości, a nie za pomocą parametru:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="dd7a2-128">Jeśli później zdecydujesz się dodać nadawcy wiadomości do ładunku, Dodaj właściwość do obiektu:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="dd7a2-129">Starsi klienci nie oczekiwano `Sender` wartości, dzięki czemu będziesz go zignorować.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="dd7a2-130">Nowy klient można go zaakceptować, aktualizując odczytać nową właściwość:</span><span class="sxs-lookup"><span data-stu-id="dd7a2-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="dd7a2-131">W tym przypadku nowego klienta jest również odporny na błędy starego serwera, który nie zapewnia `Sender` wartość.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="dd7a2-132">Ponieważ stary serwer nie będzie zapewniać `Sender` wartości, klient sprawdza, czy istnieje ona przed uzyskaniem dostępu do jej.</span><span class="sxs-lookup"><span data-stu-id="dd7a2-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
