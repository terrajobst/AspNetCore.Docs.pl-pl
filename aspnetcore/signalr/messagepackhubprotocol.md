---
title: Używać protokołu MessagePack koncentratora SignalR dla platformy ASP.NET Core
author: rachelappel
description: Dodanie protokołu MessagePack koncentratora do SignalR platformy ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: b6c33c4da47a19d67bffbaf84f54d59013edadbe
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252501"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="da832-103">Używać protokołu MessagePack koncentratora SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="da832-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="da832-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="da832-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="da832-105">W tym artykule przyjęto czytnik jest zapoznać się z tematami w [wprowadzenie](xref:signalr/get-started).</span><span class="sxs-lookup"><span data-stu-id="da832-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:signalr/get-started).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="da832-106">Co to jest MessagePack?</span><span class="sxs-lookup"><span data-stu-id="da832-106">What is MessagePack?</span></span>

<span data-ttu-id="da832-107">[MessagePack](https://msgpack.org/index.html) jest szybkie i compact to format serializacji binarnej.</span><span class="sxs-lookup"><span data-stu-id="da832-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="da832-108">Jest przydatne, gdy wydajność i przepustowość są wątpliwości dotyczących, ponieważ spowoduje to utworzenie mniejszych wiadomości w porównaniu do [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="da832-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="da832-109">Format binarny, dlatego wiadomości są nieczytelne podczas przeglądania dzienników i śladów sieciowych, chyba że bajty są przekazywane za pośrednictwem MessagePack analizatora składni.</span><span class="sxs-lookup"><span data-stu-id="da832-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="da832-110">SignalR ma wbudowaną obsługę MessagePack format i udostępnia interfejsy API dla klienta i serwera do użycia.</span><span class="sxs-lookup"><span data-stu-id="da832-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="da832-111">Konfigurowanie MessagePack na serwerze</span><span class="sxs-lookup"><span data-stu-id="da832-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="da832-112">Aby włączyć protokół MessagePack koncentratora na serwerze, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da832-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="da832-113">W pliku Startup.cs dodać `AddMessagePackProtocol` do `AddSignalR` wywołanie, aby włączyć obsługę MessagePack na serwerze.</span><span class="sxs-lookup"><span data-stu-id="da832-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="da832-114">JSON jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="da832-114">JSON is enabled by default.</span></span> <span data-ttu-id="da832-115">Dodanie MessagePack umożliwia obsługę JSON i MessagePack klientów.</span><span class="sxs-lookup"><span data-stu-id="da832-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="da832-116">Aby dostosować sposób MessagePack będzie formatowania danych, `AddMessagePackProtocol` przyjmuje delegata do konfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="da832-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="da832-117">W tym delegata `FormatterResolvers` właściwości może służyć do konfigurowania opcji serializacji MessagePack.</span><span class="sxs-lookup"><span data-stu-id="da832-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="da832-118">Aby uzyskać więcej informacji na temat rozpoznawania nazw, można znaleźć w bibliotece MessagePack pod [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="da832-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="da832-119">Atrybuty mogą być używane na obiekty, które chcesz uszeregować do zdefiniowania, jak powinien zostać obsłużony.</span><span class="sxs-lookup"><span data-stu-id="da832-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="da832-120">Skonfiguruj MessagePack na kliencie</span><span class="sxs-lookup"><span data-stu-id="da832-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="da832-121">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="da832-121">.NET client</span></span>

<span data-ttu-id="da832-122">Aby włączyć MessagePack w kliencie programu .NET, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu i wywołanie `AddMessagePackProtocol` na `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="da832-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="da832-123">To `AddMessagePackProtocol` wywołania przyjmuje delegata do konfigurowania opcji tak samo jak serwer.</span><span class="sxs-lookup"><span data-stu-id="da832-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="da832-124">JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="da832-124">JavaScript client</span></span>

<span data-ttu-id="da832-125">Obsługa MessagePack klienta Javascript jest zapewniana przez `@aspnet/signalr-protocol-msgpack` pakietu NPM.</span><span class="sxs-lookup"><span data-stu-id="da832-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="da832-126">Po zainstalowaniu pakietu npm, można używana bezpośrednio przez moduł ładujący JavaScript lub zaimportować do przeglądarki, za pomocą odwołań do modułu *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  pliku.</span><span class="sxs-lookup"><span data-stu-id="da832-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="da832-127">W przeglądarce `msgpack5` biblioteki musi się też odwoływać.</span><span class="sxs-lookup"><span data-stu-id="da832-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="da832-128">Użyj `<script>` tag, aby utworzyć odwołania.</span><span class="sxs-lookup"><span data-stu-id="da832-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="da832-129">Biblioteki można znaleźć w folderze *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="da832-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="da832-130">Korzystając z `<script>` elementu, kolejność jest ważna.</span><span class="sxs-lookup"><span data-stu-id="da832-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="da832-131">Jeśli *signalr-protocol-msgpack.js* odwołuje się przed *msgpack5.js*, występuje błąd podczas próby nawiązania połączenia z MessagePack.</span><span class="sxs-lookup"><span data-stu-id="da832-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="da832-132">*signalr.js* jest również wymagana przed *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="da832-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="da832-133">Dodawanie `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` do `HubConnectionBuilder` skonfiguruje klienta do używania protokołu MessagePack podczas nawiązywania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="da832-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="da832-134">W tej chwili brak opcji konfiguracji protokołu MessagePack na kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="da832-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="da832-135">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="da832-135">Related resources</span></span>

* [<span data-ttu-id="da832-136">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="da832-136">Get Started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="da832-137">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="da832-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="da832-138">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="da832-138">JavaScript client</span></span>](xref:signalr/javascript-client)
