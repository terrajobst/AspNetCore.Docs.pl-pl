---
title: Używać protokołu MessagePack Centrum SignalR dla platformy ASP.NET Core
author: bradygaster
description: Dodanie protokołu MessagePack Centrum z SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902593"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="92fa2-103">Używać protokołu MessagePack Centrum SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92fa2-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="92fa2-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="92fa2-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="92fa2-105">W tym artykule założono, czytelnik jest zapoznać się z tematami omówione w [wprowadzenie](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="92fa2-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="92fa2-106">Co to jest MessagePack?</span><span class="sxs-lookup"><span data-stu-id="92fa2-106">What is MessagePack?</span></span>

<span data-ttu-id="92fa2-107">[MessagePack](https://msgpack.org/index.html) jest formatem serializacji binarnej, który jest szybkie i compact.</span><span class="sxs-lookup"><span data-stu-id="92fa2-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="92fa2-108">Jest to przydatne, gdy wydajność i przepustowość są istotna, ponieważ powoduje to utworzenie wiadomości mniejszych w porównaniu do [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="92fa2-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="92fa2-109">Ponieważ jest to format binarny, komunikaty są nieczytelne podczas przeglądania danych śledzenia sieci i dzienniki, chyba że bajty są przekazywane do analizatora MessagePack.</span><span class="sxs-lookup"><span data-stu-id="92fa2-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="92fa2-110">SignalR ma wbudowaną obsługę formatu MessagePack i udostępnia interfejsy API dla klienta i serwera, do użycia.</span><span class="sxs-lookup"><span data-stu-id="92fa2-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="92fa2-111">Konfigurowanie MessagePack na serwerze</span><span class="sxs-lookup"><span data-stu-id="92fa2-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="92fa2-112">Aby włączyć protokół MessagePack koncentratora na serwerze, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92fa2-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="92fa2-113">W pliku Startup.cs dodać `AddMessagePackProtocol` do `AddSignalR` wywołanie, aby włączyć obsługę MessagePack na serwerze.</span><span class="sxs-lookup"><span data-stu-id="92fa2-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="92fa2-114">JSON jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="92fa2-114">JSON is enabled by default.</span></span> <span data-ttu-id="92fa2-115">Dodanie MessagePack umożliwia obsługę zarówno w formacie JSON, jak i MessagePack klientów.</span><span class="sxs-lookup"><span data-stu-id="92fa2-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="92fa2-116">Aby dostosować sposób formatowania danych, przez MessagePack `AddMessagePackProtocol` przyjmuje obiekt delegowany do konfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="92fa2-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="92fa2-117">W tym delegatów `FormatterResolvers` właściwość może służyć do konfigurowania opcji serializacji MessagePack.</span><span class="sxs-lookup"><span data-stu-id="92fa2-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="92fa2-118">Aby uzyskać więcej informacji na temat sposobu działania rozpoznawania nazw, można znaleźć w bibliotece MessagePack pod [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="92fa2-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="92fa2-119">Atrybuty może służyć do obiektów, potrzebne do serializacji w celu zdefiniowania, jak powinno zostać obsłużone.</span><span class="sxs-lookup"><span data-stu-id="92fa2-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="92fa2-120">Skonfiguruj MessagePack na kliencie</span><span class="sxs-lookup"><span data-stu-id="92fa2-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="92fa2-121">JSON jest domyślnie włączone dla obsługiwanych klientów.</span><span class="sxs-lookup"><span data-stu-id="92fa2-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="92fa2-122">Klienci może obsługiwać tylko jeden protokół.</span><span class="sxs-lookup"><span data-stu-id="92fa2-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="92fa2-123">Dodanie obsługi MessagePack zastąpi dowolnego wcześniej skonfigurowane protokoły.</span><span class="sxs-lookup"><span data-stu-id="92fa2-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="92fa2-124">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="92fa2-124">.NET client</span></span>

<span data-ttu-id="92fa2-125">Aby włączyć MessagePack w kliencie programu .NET, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu i wywołania `AddMessagePackProtocol` na `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="92fa2-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="92fa2-126">To `AddMessagePackProtocol` wywołanie przyjmuje obiekt delegowany do konfigurowania opcji, podobnie jak serwer.</span><span class="sxs-lookup"><span data-stu-id="92fa2-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="92fa2-127">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="92fa2-127">JavaScript client</span></span>

<span data-ttu-id="92fa2-128">MessagePack zapewnia pomoc techniczną dla klienta JavaScript `@aspnet/signalr-protocol-msgpack` pakietów Menedżera npm.</span><span class="sxs-lookup"><span data-stu-id="92fa2-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="92fa2-129">Po zainstalowaniu pakietu npm, moduł mogą być używane bezpośrednio za pośrednictwem modułu ładującego języka JavaScript lub zaimportować do przeglądarki, odwołując się do *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* pliku.</span><span class="sxs-lookup"><span data-stu-id="92fa2-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="92fa2-130">W przeglądarce `msgpack5` biblioteki musi się też odwoływać.</span><span class="sxs-lookup"><span data-stu-id="92fa2-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="92fa2-131">Użyj `<script>` tag, aby utworzyć odwołania.</span><span class="sxs-lookup"><span data-stu-id="92fa2-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="92fa2-132">Biblioteka znajduje się w temacie *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="92fa2-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="92fa2-133">Korzystając z `<script>` elementu, kolejność jest ważna.</span><span class="sxs-lookup"><span data-stu-id="92fa2-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="92fa2-134">Jeśli *signalr-protocol-msgpack.js* odwołuje się przed *msgpack5.js*, występuje błąd podczas próby nawiązania połączenia z MessagePack.</span><span class="sxs-lookup"><span data-stu-id="92fa2-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="92fa2-135">*signalr.js* jest również wymagane przed *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="92fa2-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="92fa2-136">Dodawanie `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` do `HubConnectionBuilder` skonfiguruje klienta do używania protokołu MessagePack podczas nawiązywania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="92fa2-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="92fa2-137">W tej chwili nie istnieją żadnych opcji konfiguracji protokołu MessagePack na kliencie języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="92fa2-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="92fa2-138">Osobliwości MessagePack</span><span class="sxs-lookup"><span data-stu-id="92fa2-138">MessagePack quirks</span></span>

<span data-ttu-id="92fa2-139">Istnieje kilka kwestii, które należy zwrócić uwagę podczas korzystania z protokołu Centrum MessagePack.</span><span class="sxs-lookup"><span data-stu-id="92fa2-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="92fa2-140">MessagePack jest uwzględniana wielkość liter</span><span class="sxs-lookup"><span data-stu-id="92fa2-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="92fa2-141">Protokół MessagePack jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="92fa2-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="92fa2-142">Na przykład, należy wziąć pod uwagę następujące C# klasy:</span><span class="sxs-lookup"><span data-stu-id="92fa2-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="92fa2-143">Podczas wysyłania z klienta JavaScript, należy użyć `PascalCased` nazwy właściwości, ponieważ musi odpowiadać wielkości liter C# dokładnie klasy.</span><span class="sxs-lookup"><span data-stu-id="92fa2-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="92fa2-144">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="92fa2-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="92fa2-145">Za pomocą `camelCased` nazwy nie będą poprawnie powiązać C# klasy.</span><span class="sxs-lookup"><span data-stu-id="92fa2-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="92fa2-146">Można obejść to za pomocą `Key` atrybutu, aby określić inną nazwę dla właściwości MessagePack.</span><span class="sxs-lookup"><span data-stu-id="92fa2-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="92fa2-147">Aby uzyskać więcej informacji, zobacz [dokumentacji języka CSharp MessagePack](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="92fa2-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="92fa2-148">DateTime.Kind nie są zachowywane podczas serializacji/deserializacji</span><span class="sxs-lookup"><span data-stu-id="92fa2-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="92fa2-149">Protokół MessagePack nie umożliwiają kodowanie `Kind` wartość `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="92fa2-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="92fa2-150">W rezultacie podczas deserializacji daty, protokołu Centrum MessagePack przyjęto założenie, że przychodzące Data jest w formacie UTC.</span><span class="sxs-lookup"><span data-stu-id="92fa2-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="92fa2-151">Jeśli pracujesz z `DateTime` wartości według czasu lokalnego, firma Microsoft zaleca się przejście na czas UTC przed ich wysłaniem.</span><span class="sxs-lookup"><span data-stu-id="92fa2-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="92fa2-152">Przekonwertuj je względem czasu UTC na czas lokalny podczas ich odbierania.</span><span class="sxs-lookup"><span data-stu-id="92fa2-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="92fa2-153">Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz GitHub problem [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="92fa2-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="92fa2-154">DateTime.MinValue, nie jest obsługiwana przez MessagePack w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="92fa2-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="92fa2-155">[Msgpack5](https://github.com/mcollina/msgpack5) biblioteki używane przez klienta SignalR JavaScript nie obsługuje `timestamp96` wpisz MessagePack.</span><span class="sxs-lookup"><span data-stu-id="92fa2-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="92fa2-156">Ten typ służy do kodowania wartości od początku bardzo duże (lub bardzo wcześnie w przeszłości bardzo daleko w przyszłości).</span><span class="sxs-lookup"><span data-stu-id="92fa2-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="92fa2-157">Wartość `DateTime.MinValue` jest `January 1, 0001` musi być zakodowany za `timestamp96` wartość.</span><span class="sxs-lookup"><span data-stu-id="92fa2-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="92fa2-158">Ze względu na to, wysyłając `DateTime.MinValue` do JavaScript klienta nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="92fa2-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="92fa2-159">Gdy `DateTime.MinValue` zostanie odebrana przez klienta JavaScript jest zgłaszany następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="92fa2-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="92fa2-160">Zazwyczaj `DateTime.MinValue` służy do kodowania "Brak" lub `null` wartość.</span><span class="sxs-lookup"><span data-stu-id="92fa2-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="92fa2-161">Do zakodowania tę wartość w MessagePack, należy użyć dopuszczający wartości null `DateTime` wartość (`DateTime?`) lub zakodować oddzielnego `bool` wartość wskazującą, czy data jest obecne.</span><span class="sxs-lookup"><span data-stu-id="92fa2-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="92fa2-162">Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz GitHub problem [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="92fa2-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="92fa2-163">Obsługa MessagePack w środowisku kompilacji "z wyprzedzeniem of-time"</span><span class="sxs-lookup"><span data-stu-id="92fa2-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="92fa2-164">[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) używanej przez klienta platformy .NET i serwer biblioteki używa generowania kodu, aby zoptymalizować serializacji.</span><span class="sxs-lookup"><span data-stu-id="92fa2-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="92fa2-165">W rezultacie nie jest obsługiwana przez domyślny w środowiskach korzystających z kompilacji "z wyprzedzeniem of-time" (na przykład platformy Xamarin iOS lub Unity).</span><span class="sxs-lookup"><span data-stu-id="92fa2-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="92fa2-166">Istnieje możliwość używania MessagePack w tych środowiskach, generując"przed" kodu serializator/Deserializator.</span><span class="sxs-lookup"><span data-stu-id="92fa2-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="92fa2-167">Aby uzyskać więcej informacji, zobacz [dokumentacji języka CSharp MessagePack](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="92fa2-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="92fa2-168">Po wygenerowaniu wstępnie serializatory, możesz zarejestrować ich za pomocą delegowanego konfiguracji przekazywana do `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="92fa2-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="92fa2-169">Ten typ są bardziej rygorystyczne w MessagePack</span><span class="sxs-lookup"><span data-stu-id="92fa2-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="92fa2-170">Protokół Centrum JSON będzie wykonywać konwersje typów podczas deserializacji.</span><span class="sxs-lookup"><span data-stu-id="92fa2-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="92fa2-171">Na przykład, jeśli obiekt przychodzące ma wartość właściwości jest ona liczbą (`{ foo: 42 }`), ale właściwość klasy .NET jest typu `string`, wartość zostanie przekonwertowany.</span><span class="sxs-lookup"><span data-stu-id="92fa2-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="92fa2-172">Jednak MessagePack nie wykonać tę konwersję i zgłosi wyjątek, który może być widoczny w dziennikach po stronie serwera (i w konsoli):</span><span class="sxs-lookup"><span data-stu-id="92fa2-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="92fa2-173">Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz GitHub problem [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="92fa2-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="92fa2-174">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="92fa2-174">Related resources</span></span>

* [<span data-ttu-id="92fa2-175">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="92fa2-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="92fa2-176">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="92fa2-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="92fa2-177">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="92fa2-177">JavaScript client</span></span>](xref:signalr/javascript-client)
