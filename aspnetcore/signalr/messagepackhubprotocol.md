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
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400674"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Używać protokołu MessagePack Centrum SignalR dla platformy ASP.NET Core

Przez [Brennan Conroy](https://github.com/BrennanConroy)

W tym artykule założono, czytelnik jest zapoznać się z tematami omówione w [wprowadzenie](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Co to jest MessagePack?

[MessagePack](https://msgpack.org/index.html) jest formatem serializacji binarnej, który jest szybkie i compact. Jest to przydatne, gdy wydajność i przepustowość są istotna, ponieważ powoduje to utworzenie wiadomości mniejszych w porównaniu do [JSON](https://www.json.org/). Ponieważ jest to format binarny, komunikaty są nieczytelne podczas przeglądania danych śledzenia sieci i dzienniki, chyba że bajty są przekazywane do analizatora MessagePack. SignalR ma wbudowaną obsługę formatu MessagePack i udostępnia interfejsy API dla klienta i serwera, do użycia.

## <a name="configure-messagepack-on-the-server"></a>Konfigurowanie MessagePack na serwerze

Aby włączyć protokół MessagePack koncentratora na serwerze, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu w aplikacji. W pliku Startup.cs dodać `AddMessagePackProtocol` do `AddSignalR` wywołanie, aby włączyć obsługę MessagePack na serwerze.

> [!NOTE]
> JSON jest domyślnie włączona. Dodanie MessagePack umożliwia obsługę zarówno w formacie JSON, jak i MessagePack klientów.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Aby dostosować sposób formatowania danych, przez MessagePack `AddMessagePackProtocol` przyjmuje obiekt delegowany do konfigurowania opcji. W tym delegatów `FormatterResolvers` właściwość może służyć do konfigurowania opcji serializacji MessagePack. Aby uzyskać więcej informacji na temat sposobu działania rozpoznawania nazw, można znaleźć w bibliotece MessagePack pod [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Atrybuty może służyć do obiektów, potrzebne do serializacji w celu zdefiniowania, jak powinno zostać obsłużone.

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

## <a name="configure-messagepack-on-the-client"></a>Skonfiguruj MessagePack na kliencie

> [!NOTE]
> JSON jest domyślnie włączone dla obsługiwanych klientów. Klienci może obsługiwać tylko jeden protokół. Dodanie obsługi MessagePack zastąpi dowolnego wcześniej skonfigurowane protokoły.

### <a name="net-client"></a>Klient .NET

Aby włączyć MessagePack w kliencie programu .NET, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu i wywołania `AddMessagePackProtocol` na `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> To `AddMessagePackProtocol` wywołanie przyjmuje obiekt delegowany do konfigurowania opcji, podobnie jak serwer.

### <a name="javascript-client"></a>Klient JavaScript

MessagePack zapewnia pomoc techniczną dla klienta JavaScript `@aspnet/signalr-protocol-msgpack` pakietów Menedżera npm.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Po zainstalowaniu pakietu npm, moduł mogą być używane bezpośrednio za pośrednictwem modułu ładującego języka JavaScript lub zaimportować do przeglądarki, odwołując się do *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* pliku. W przeglądarce `msgpack5` biblioteki musi się też odwoływać. Użyj `<script>` tag, aby utworzyć odwołania. Biblioteka znajduje się w temacie *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Korzystając z `<script>` elementu, kolejność jest ważna. Jeśli *signalr-protocol-msgpack.js* odwołuje się przed *msgpack5.js*, występuje błąd podczas próby nawiązania połączenia z MessagePack. *signalr.js* jest również wymagane przed *signalr-protocol-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Dodawanie `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` do `HubConnectionBuilder` skonfiguruje klienta do używania protokołu MessagePack podczas nawiązywania połączenia z serwerem.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> W tej chwili nie istnieją żadnych opcji konfiguracji protokołu MessagePack na kliencie języka JavaScript.

## <a name="messagepack-quirks"></a>Osobliwości MessagePack

Istnieje kilka kwestii, które należy zwrócić uwagę podczas korzystania z protokołu Centrum MessagePack.

### <a name="messagepack-is-case-sensitive"></a>MessagePack jest uwzględniana wielkość liter

Protokół MessagePack jest rozróżniana wielkość liter. Na przykład, należy wziąć pod uwagę następujące C# klasy:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Podczas wysyłania z klienta JavaScript, należy użyć `PascalCased` nazwy właściwości, ponieważ musi odpowiadać wielkości liter C# dokładnie klasy. Na przykład:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

Za pomocą `camelCased` nazwy nie będą poprawnie powiązać C# klasy. Można obejść to za pomocą `Key` atrybutu, aby określić inną nazwę dla właściwości MessagePack. Aby uzyskać więcej informacji, zobacz [dokumentacji języka CSharp MessagePack](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime.Kind nie są zachowywane podczas serializacji/deserializacji

Protokół MessagePack nie umożliwiają kodowanie `Kind` wartość `DateTime`. W rezultacie podczas deserializacji daty, protokołu Centrum MessagePack przyjęto założenie, że przychodzące Data jest w formacie UTC. Jeśli pracujesz z `DateTime` wartości według czasu lokalnego, firma Microsoft zaleca się przejście na czas UTC przed ich wysłaniem. Przekonwertuj je względem czasu UTC na czas lokalny podczas ich odbierania.

Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz GitHub problem [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime.MinValue, nie jest obsługiwana przez MessagePack w języku JavaScript

[Msgpack5](https://github.com/mcollina/msgpack5) biblioteki używane przez klienta SignalR JavaScript nie obsługuje `timestamp96` wpisz MessagePack. Ten typ służy do kodowania wartości od początku bardzo duże (lub bardzo wcześnie w przeszłości bardzo daleko w przyszłości). Wartość `DateTime.MinValue` jest `January 1, 0001` musi być zakodowany za `timestamp96` wartość. Ze względu na to, wysyłając `DateTime.MinValue` do JavaScript klienta nie jest obsługiwane. Gdy `DateTime.MinValue` zostanie odebrana przez klienta JavaScript jest zgłaszany następujący błąd:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Zazwyczaj `DateTime.MinValue` służy do kodowania "Brak" lub `null` wartość. Do zakodowania tę wartość w MessagePack, należy użyć dopuszczający wartości null `DateTime` wartość (`DateTime?`) lub zakodować oddzielnego `bool` wartość wskazującą, czy data jest obecne.

Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz GitHub problem [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Obsługa MessagePack w środowisku kompilacji "z wyprzedzeniem of-time"

[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) używanej przez klienta platformy .NET i serwer biblioteki używa generowania kodu, aby zoptymalizować serializacji. W rezultacie nie jest obsługiwana przez domyślny w środowiskach korzystających z kompilacji "z wyprzedzeniem of-time" (na przykład platformy Xamarin iOS lub Unity). Istnieje możliwość używania MessagePack w tych środowiskach, generując"przed" kodu serializator/Deserializator. Aby uzyskać więcej informacji, zobacz [dokumentacji języka CSharp MessagePack](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Po wygenerowaniu wstępnie serializatory, możesz zarejestrować ich za pomocą delegowanego konfiguracji przekazywana do `AddMessagePackProtocol`:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Ten typ są bardziej rygorystyczne w MessagePack

Protokół Centrum JSON będzie wykonywać konwersje typów podczas deserializacji. Na przykład, jeśli obiekt przychodzące ma wartość właściwości jest ona liczbą (`{ foo: 42 }`), ale właściwość klasy .NET jest typu `string`, wartość zostanie przekonwertowany. Jednak MessagePack nie wykonać tę konwersję i zgłosi wyjątek, który może być widoczny w dziennikach po stronie serwera (i w konsoli):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz GitHub problem [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
