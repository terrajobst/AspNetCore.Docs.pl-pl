---
title: Używać protokołu MessagePack Centrum SignalR dla platformy ASP.NET Core
author: tdykstra
description: Dodanie protokołu MessagePack Centrum z SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39094998"
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

### <a name="net-client"></a>Klient modelu .NET

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

MessagePack zapewnia pomoc techniczną dla klienta Javascript `@aspnet/signalr-protocol-msgpack` pakietów Menedżera NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Po zainstalowaniu pakietu npm, moduł mogą być używane bezpośrednio za pośrednictwem modułu ładującego języka JavaScript lub zaimportować do przeglądarki, odwołując się do *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  pliku. W przeglądarce `msgpack5` biblioteki musi się też odwoływać. Użyj `<script>` tag, aby utworzyć odwołania. Biblioteka znajduje się w temacie *node_modules\msgpack5\dist\msgpack5.js*.

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

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
