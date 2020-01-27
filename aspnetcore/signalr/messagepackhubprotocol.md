---
title: Użyj protokołu MessagePack Hub w SignalR dla ASP.NET Core
author: bradygaster
description: Dodaj protokół MessagePack Hub do SignalRASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 1b01357233a9b95a5da052d92e30232c94e78a78
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727225"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="a8c42-103">Użyj protokołu MessagePack Hub w SignalR dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8c42-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="a8c42-104">Autor [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="a8c42-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="a8c42-105">W tym artykule założono, że czytelnik zna tematy omówione w temacie [wprowadzenie.](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="a8c42-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="a8c42-106">Co to jest MessagePack?</span><span class="sxs-lookup"><span data-stu-id="a8c42-106">What is MessagePack?</span></span>

<span data-ttu-id="a8c42-107">[MessagePack](https://msgpack.org/index.html) to binarny format serializacji, który jest szybki i kompaktowy.</span><span class="sxs-lookup"><span data-stu-id="a8c42-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="a8c42-108">Jest to przydatne, gdy wydajność i przepustowość są problemem, ponieważ tworzy mniejsze komunikaty w porównaniu z formatem [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="a8c42-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="a8c42-109">Ponieważ jest to format binarny, komunikaty nie są odczytywane podczas przeglądania śladów i dzienników sieci, chyba że bajty są przesyłane przez parser MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="a8c42-110"> ma wbudowaną obsługę formatu MessagePack i udostępnia interfejsy API dla klienta i serwera, które mają być używane.</span><span class="sxs-lookup"><span data-stu-id="a8c42-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="a8c42-111">Konfigurowanie MessagePack na serwerze</span><span class="sxs-lookup"><span data-stu-id="a8c42-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="a8c42-112">Aby włączyć protokół MessagePack Hub na serwerze, zainstaluj pakiet `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8c42-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="a8c42-113">W pliku Startup.cs Dodaj `AddMessagePackProtocol` do wywołania `AddSignalR`, aby włączyć obsługę MessagePack na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8c42-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="a8c42-114">KOD JSON jest domyślnie włączony.</span><span class="sxs-lookup"><span data-stu-id="a8c42-114">JSON is enabled by default.</span></span> <span data-ttu-id="a8c42-115">Dodanie MessagePack umożliwia obsługę zarówno dla klientów JSON, jak i MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="a8c42-116">Aby dostosować sposób, w jaki program MessagePack sformatuje dane, `AddMessagePackProtocol` pobiera delegata do konfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="a8c42-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="a8c42-117">W tym delegatze Właściwość `FormatterResolvers` może służyć do konfigurowania opcji serializacji MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="a8c42-118">Aby uzyskać więcej informacji na temat sposobu działania elementów rozpoznawania, odwiedź bibliotekę MessagePack pod adresem [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="a8c42-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="a8c42-119">Atrybuty mogą być używane dla obiektów, które mają być serializowane, aby określić sposób ich obsługi.</span><span class="sxs-lookup"><span data-stu-id="a8c42-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="a8c42-120">Konfigurowanie MessagePack na kliencie</span><span class="sxs-lookup"><span data-stu-id="a8c42-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="a8c42-121">KOD JSON jest domyślnie włączony dla obsługiwanych klientów.</span><span class="sxs-lookup"><span data-stu-id="a8c42-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="a8c42-122">Klienci mogą obsługiwać tylko jeden protokół.</span><span class="sxs-lookup"><span data-stu-id="a8c42-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="a8c42-123">Dodanie obsługi MessagePack spowoduje zastąpienie wszelkich wcześniej skonfigurowanych protokołów.</span><span class="sxs-lookup"><span data-stu-id="a8c42-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="a8c42-124">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="a8c42-124">.NET client</span></span>

<span data-ttu-id="a8c42-125">Aby włączyć MessagePack na kliencie .NET, zainstaluj pakiet `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` i Wywołaj `AddMessagePackProtocol` na `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8c42-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="a8c42-126">To wywołanie `AddMessagePackProtocol` ma delegata do konfigurowania opcji, podobnie jak na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8c42-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="a8c42-127">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8c42-127">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8c42-128">Obsługa MessagePack dla klienta JavaScript jest zapewniana przez pakiet `@microsoft/signalr-protocol-msgpack` npm.</span><span class="sxs-lookup"><span data-stu-id="a8c42-128">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="a8c42-129">Zainstaluj pakiet, wykonując następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a8c42-129">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8c42-130">Obsługa MessagePack dla klienta JavaScript jest zapewniana przez pakiet `@aspnet/signalr-protocol-msgpack` npm.</span><span class="sxs-lookup"><span data-stu-id="a8c42-130">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="a8c42-131">Zainstaluj pakiet, wykonując następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a8c42-131">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="a8c42-132">Po zainstalowaniu pakietu npm można użyć modułu bezpośrednio za pośrednictwem modułu ładującego języka JavaScript lub zaimportowanego do przeglądarki, odwołując się do następującego pliku:</span><span class="sxs-lookup"><span data-stu-id="a8c42-132">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8c42-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="a8c42-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8c42-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="a8c42-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="a8c42-135">W przeglądarce musi być również przywoływana Biblioteka `msgpack5`.</span><span class="sxs-lookup"><span data-stu-id="a8c42-135">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="a8c42-136">Użyj znacznika `<script>`, aby utworzyć odwołanie.</span><span class="sxs-lookup"><span data-stu-id="a8c42-136">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="a8c42-137">Bibliotekę można znaleźć pod adresem *node_modules \msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="a8c42-137">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="a8c42-138">W przypadku używania elementu `<script>` kolejność jest ważna.</span><span class="sxs-lookup"><span data-stu-id="a8c42-138">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="a8c42-139">Jeśli do *SignalR-Protocol-msgpack. js* odwołuje się przed *msgpack5. js*, wystąpi błąd podczas próby nawiązania połączenia z MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-139">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="a8c42-140">*sygnałer. js* jest również wymagany przed *SignalR-Protocol-msgpack. js*.</span><span class="sxs-lookup"><span data-stu-id="a8c42-140">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="a8c42-141">Dodanie `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` do `HubConnectionBuilder` spowoduje skonfigurowanie klienta do korzystania z protokołu MessagePack podczas nawiązywania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a8c42-141">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="a8c42-142">W tej chwili nie ma opcji konfiguracji dla protokołu MessagePack na kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8c42-142">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="a8c42-143">MessagePack osobliwości</span><span class="sxs-lookup"><span data-stu-id="a8c42-143">MessagePack quirks</span></span>

<span data-ttu-id="a8c42-144">Należy pamiętać o kilku kwestiach dotyczących korzystania z protokołu centrum MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-144">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="a8c42-145">MessagePack jest rozróżniana wielkość liter</span><span class="sxs-lookup"><span data-stu-id="a8c42-145">MessagePack is case-sensitive</span></span>

<span data-ttu-id="a8c42-146">W protokole MessagePack jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a8c42-146">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="a8c42-147">Rozważmy na przykład następujące C# klasy:</span><span class="sxs-lookup"><span data-stu-id="a8c42-147">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="a8c42-148">W przypadku wysyłania z klienta JavaScript należy użyć nazw właściwości `PascalCased`, ponieważ wielkość liter musi być dokładnie zgodna z C# klasą.</span><span class="sxs-lookup"><span data-stu-id="a8c42-148">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="a8c42-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8c42-149">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="a8c42-150">Używanie nazw `camelCased` nie zostanie prawidłowo powiązane z C# klasą.</span><span class="sxs-lookup"><span data-stu-id="a8c42-150">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="a8c42-151">Można obejść ten krok przy użyciu atrybutu `Key`, aby określić inną nazwę właściwości MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-151">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="a8c42-152">Aby uzyskać więcej informacji, zobacz [dokumentację MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="a8c42-152">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="a8c42-153">Wartość DateTime. Kind nie jest zachowywana podczas serializacji/deserializacji</span><span class="sxs-lookup"><span data-stu-id="a8c42-153">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="a8c42-154">Protokół MessagePack nie zapewnia sposobu kodowania wartości `Kind` `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="a8c42-154">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="a8c42-155">W związku z tym podczas deserializacji daty, protokół MessagePack Hub zakłada, że data przychodząca jest w formacie UTC.</span><span class="sxs-lookup"><span data-stu-id="a8c42-155">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="a8c42-156">Jeśli pracujesz z wartościami `DateTime` w czasie lokalnym, zalecamy przekonwertowanie na czas UTC przed ich wysłaniem.</span><span class="sxs-lookup"><span data-stu-id="a8c42-156">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="a8c42-157">Konwertuj je z czasu UTC na czas lokalny po ich otrzymaniu.</span><span class="sxs-lookup"><span data-stu-id="a8c42-157">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="a8c42-158">Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz [#2632 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2632)usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8c42-158">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="a8c42-159">Wartość DateTime. MinValue nie jest obsługiwana przez MessagePack w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8c42-159">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="a8c42-160">Biblioteka [msgpack5](https://github.com/mcollina/msgpack5) używana przez klienta języka JavaScript SignalR nie obsługuje typu `timestamp96` w MessagePack.</span><span class="sxs-lookup"><span data-stu-id="a8c42-160">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="a8c42-161">Ten typ jest używany do kodowania bardzo dużych wartości daty (bardzo wczesne w przeszłości lub bardzo daleko w przyszłości).</span><span class="sxs-lookup"><span data-stu-id="a8c42-161">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="a8c42-162">Wartość `DateTime.MinValue` jest `January 1, 0001`, która musi być zakodowana w wartości `timestamp96`.</span><span class="sxs-lookup"><span data-stu-id="a8c42-162">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="a8c42-163">W związku z tym wysyłanie `DateTime.MinValue` do klienta JavaScript nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a8c42-163">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="a8c42-164">Po odebraniu `DateTime.MinValue` przez klienta JavaScript zostanie zgłoszony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="a8c42-164">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="a8c42-165">Zwykle `DateTime.MinValue` jest używany do kodowania "braku" lub `null` wartości.</span><span class="sxs-lookup"><span data-stu-id="a8c42-165">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="a8c42-166">Jeśli trzeba zakodować tę wartość w MessagePack, należy użyć wartości null `DateTime` wartość (`DateTime?`) lub zakodować oddzielną `bool` wartość wskazującą, czy data jest obecna.</span><span class="sxs-lookup"><span data-stu-id="a8c42-166">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="a8c42-167">Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz [#2228 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2228)usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8c42-167">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="a8c42-168">Obsługa MessagePack w środowisku kompilacji "z wyprzedzeniem"</span><span class="sxs-lookup"><span data-stu-id="a8c42-168">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="a8c42-169">Biblioteka [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) używana przez klienta i serwer platformy .NET używa generowania kodu w celu optymalizacji serializacji.</span><span class="sxs-lookup"><span data-stu-id="a8c42-169">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="a8c42-170">W związku z tym nie jest to obsługiwane domyślnie w środowiskach, w których jest używana Kompilacja "z wyprzedzeniem" (na przykład Xamarin iOS lub Unity).</span><span class="sxs-lookup"><span data-stu-id="a8c42-170">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="a8c42-171">Można używać MessagePack w tych środowiskach przez "wstępne generowanie" kodu serializatora/deserializacji.</span><span class="sxs-lookup"><span data-stu-id="a8c42-171">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="a8c42-172">Aby uzyskać więcej informacji, zobacz [dokumentację MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="a8c42-172">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="a8c42-173">Po wstępnym wygenerowaniu serializatorów można zarejestrować je za pomocą delegata konfiguracji przekazaną do `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="a8c42-173">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="a8c42-174">Kontrole typu są bardziej rygorystyczne w MessagePack</span><span class="sxs-lookup"><span data-stu-id="a8c42-174">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="a8c42-175">Podczas deserializacji protokół JSON będzie wykonywał konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="a8c42-175">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="a8c42-176">Na przykład, jeśli obiekt przychodzący ma wartość właściwości, która jest liczbą (`{ foo: 42 }`), ale właściwość w klasie .NET ma typ `string`, wartość zostanie przekonwertowana.</span><span class="sxs-lookup"><span data-stu-id="a8c42-176">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="a8c42-177">Jednak MessagePack nie wykonuje tej konwersji i zgłosi wyjątek, który może być widoczny w dziennikach po stronie serwera (i w konsoli programu):</span><span class="sxs-lookup"><span data-stu-id="a8c42-177">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="a8c42-178">Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz [#2937 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2937)usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8c42-178">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="a8c42-179">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="a8c42-179">Related resources</span></span>

* [<span data-ttu-id="a8c42-180">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="a8c42-180">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="a8c42-181">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="a8c42-181">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a8c42-182">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8c42-182">JavaScript client</span></span>](xref:signalr/javascript-client)
