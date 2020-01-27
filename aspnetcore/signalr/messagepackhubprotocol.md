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
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a>Użyj protokołu MessagePack Hub w SignalR dla ASP.NET Core

Autor [Brennan Conroy](https://github.com/BrennanConroy)

W tym artykule założono, że czytelnik zna tematy omówione w temacie [wprowadzenie.](xref:tutorials/signalr)

## <a name="what-is-messagepack"></a>Co to jest MessagePack?

[MessagePack](https://msgpack.org/index.html) to binarny format serializacji, który jest szybki i kompaktowy. Jest to przydatne, gdy wydajność i przepustowość są problemem, ponieważ tworzy mniejsze komunikaty w porównaniu z formatem [JSON](https://www.json.org/). Ponieważ jest to format binarny, komunikaty nie są odczytywane podczas przeglądania śladów i dzienników sieci, chyba że bajty są przesyłane przez parser MessagePack. SignalR ma wbudowaną obsługę formatu MessagePack i udostępnia interfejsy API dla klienta i serwera, które mają być używane.

## <a name="configure-messagepack-on-the-server"></a>Konfigurowanie MessagePack na serwerze

Aby włączyć protokół MessagePack Hub na serwerze, zainstaluj pakiet `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` w aplikacji. W pliku Startup.cs Dodaj `AddMessagePackProtocol` do wywołania `AddSignalR`, aby włączyć obsługę MessagePack na serwerze.

> [!NOTE]
> KOD JSON jest domyślnie włączony. Dodanie MessagePack umożliwia obsługę zarówno dla klientów JSON, jak i MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Aby dostosować sposób, w jaki program MessagePack sformatuje dane, `AddMessagePackProtocol` pobiera delegata do konfigurowania opcji. W tym delegatze Właściwość `FormatterResolvers` może służyć do konfigurowania opcji serializacji MessagePack. Aby uzyskać więcej informacji na temat sposobu działania elementów rozpoznawania, odwiedź bibliotekę MessagePack pod adresem [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Atrybuty mogą być używane dla obiektów, które mają być serializowane, aby określić sposób ich obsługi.

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

## <a name="configure-messagepack-on-the-client"></a>Konfigurowanie MessagePack na kliencie

> [!NOTE]
> KOD JSON jest domyślnie włączony dla obsługiwanych klientów. Klienci mogą obsługiwać tylko jeden protokół. Dodanie obsługi MessagePack spowoduje zastąpienie wszelkich wcześniej skonfigurowanych protokołów.

### <a name="net-client"></a>Klient .NET

Aby włączyć MessagePack na kliencie .NET, zainstaluj pakiet `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` i Wywołaj `AddMessagePackProtocol` na `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> To wywołanie `AddMessagePackProtocol` ma delegata do konfigurowania opcji, podobnie jak na serwerze.

### <a name="javascript-client"></a>Klient JavaScript

::: moniker range=">= aspnetcore-3.0"

Obsługa MessagePack dla klienta JavaScript jest zapewniana przez pakiet `@microsoft/signalr-protocol-msgpack` npm. Zainstaluj pakiet, wykonując następujące polecenie w powłoce poleceń:

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Obsługa MessagePack dla klienta JavaScript jest zapewniana przez pakiet `@aspnet/signalr-protocol-msgpack` npm. Zainstaluj pakiet, wykonując następujące polecenie w powłoce poleceń:

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

Po zainstalowaniu pakietu npm można użyć modułu bezpośrednio za pośrednictwem modułu ładującego języka JavaScript lub zaimportowanego do przeglądarki, odwołując się do następującego pliku:

::: moniker range=">= aspnetcore-3.0"

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

W przeglądarce musi być również przywoływana Biblioteka `msgpack5`. Użyj znacznika `<script>`, aby utworzyć odwołanie. Bibliotekę można znaleźć pod adresem *node_modules \msgpack5\dist\msgpack5.js*.

> [!NOTE]
> W przypadku używania elementu `<script>` kolejność jest ważna. Jeśli do *SignalR-Protocol-msgpack. js* odwołuje się przed *msgpack5. js*, wystąpi błąd podczas próby nawiązania połączenia z MessagePack. *sygnałer. js* jest również wymagany przed *SignalR-Protocol-msgpack. js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Dodanie `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` do `HubConnectionBuilder` spowoduje skonfigurowanie klienta do korzystania z protokołu MessagePack podczas nawiązywania połączenia z serwerem.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> W tej chwili nie ma opcji konfiguracji dla protokołu MessagePack na kliencie JavaScript.

## <a name="messagepack-quirks"></a>MessagePack osobliwości

Należy pamiętać o kilku kwestiach dotyczących korzystania z protokołu centrum MessagePack.

### <a name="messagepack-is-case-sensitive"></a>MessagePack jest rozróżniana wielkość liter

W protokole MessagePack jest rozróżniana wielkość liter. Rozważmy na przykład następujące C# klasy:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

W przypadku wysyłania z klienta JavaScript należy użyć nazw właściwości `PascalCased`, ponieważ wielkość liter musi być dokładnie zgodna z C# klasą. Na przykład:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

Używanie nazw `camelCased` nie zostanie prawidłowo powiązane z C# klasą. Można obejść ten krok przy użyciu atrybutu `Key`, aby określić inną nazwę właściwości MessagePack. Aby uzyskać więcej informacji, zobacz [dokumentację MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>Wartość DateTime. Kind nie jest zachowywana podczas serializacji/deserializacji

Protokół MessagePack nie zapewnia sposobu kodowania wartości `Kind` `DateTime`. W związku z tym podczas deserializacji daty, protokół MessagePack Hub zakłada, że data przychodząca jest w formacie UTC. Jeśli pracujesz z wartościami `DateTime` w czasie lokalnym, zalecamy przekonwertowanie na czas UTC przed ich wysłaniem. Konwertuj je z czasu UTC na czas lokalny po ich otrzymaniu.

Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz [#2632 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2632)usługi GitHub.

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>Wartość DateTime. MinValue nie jest obsługiwana przez MessagePack w języku JavaScript

Biblioteka [msgpack5](https://github.com/mcollina/msgpack5) używana przez klienta języka JavaScript SignalR nie obsługuje typu `timestamp96` w MessagePack. Ten typ jest używany do kodowania bardzo dużych wartości daty (bardzo wczesne w przeszłości lub bardzo daleko w przyszłości). Wartość `DateTime.MinValue` jest `January 1, 0001`, która musi być zakodowana w wartości `timestamp96`. W związku z tym wysyłanie `DateTime.MinValue` do klienta JavaScript nie jest obsługiwane. Po odebraniu `DateTime.MinValue` przez klienta JavaScript zostanie zgłoszony następujący błąd:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Zwykle `DateTime.MinValue` jest używany do kodowania "braku" lub `null` wartości. Jeśli trzeba zakodować tę wartość w MessagePack, należy użyć wartości null `DateTime` wartość (`DateTime?`) lub zakodować oddzielną `bool` wartość wskazującą, czy data jest obecna.

Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz [#2228 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2228)usługi GitHub.

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Obsługa MessagePack w środowisku kompilacji "z wyprzedzeniem"

Biblioteka [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) używana przez klienta i serwer platformy .NET używa generowania kodu w celu optymalizacji serializacji. W związku z tym nie jest to obsługiwane domyślnie w środowiskach, w których jest używana Kompilacja "z wyprzedzeniem" (na przykład Xamarin iOS lub Unity). Można używać MessagePack w tych środowiskach przez "wstępne generowanie" kodu serializatora/deserializacji. Aby uzyskać więcej informacji, zobacz [dokumentację MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports). Po wstępnym wygenerowaniu serializatorów można zarejestrować je za pomocą delegata konfiguracji przekazaną do `AddMessagePackProtocol`:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Kontrole typu są bardziej rygorystyczne w MessagePack

Podczas deserializacji protokół JSON będzie wykonywał konwersje typów. Na przykład, jeśli obiekt przychodzący ma wartość właściwości, która jest liczbą (`{ foo: 42 }`), ale właściwość w klasie .NET ma typ `string`, wartość zostanie przekonwertowana. Jednak MessagePack nie wykonuje tej konwersji i zgłosi wyjątek, który może być widoczny w dziennikach po stronie serwera (i w konsoli programu):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz [#2937 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2937)usługi GitHub.

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
