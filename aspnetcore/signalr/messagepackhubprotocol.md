---
title: Używać protokołu MessagePack koncentratora SignalR dla platformy ASP.NET Core
author: rachelappel
description: Dodanie protokołu MessagePack koncentratora do SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274992"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Używać protokołu MessagePack koncentratora SignalR dla platformy ASP.NET Core

Przez [Brennan Conroy](https://github.com/BrennanConroy)

W tym artykule przyjęto czytnik jest zapoznać się z tematami w [wprowadzenie](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Co to jest MessagePack?

[MessagePack](https://msgpack.org/index.html) jest szybkie i compact to format serializacji binarnej. Jest przydatne, gdy wydajność i przepustowość są wątpliwości dotyczących, ponieważ spowoduje to utworzenie mniejszych wiadomości w porównaniu do [JSON](https://www.json.org/). Format binarny, dlatego wiadomości są nieczytelne podczas przeglądania dzienników i śladów sieciowych, chyba że bajty są przekazywane za pośrednictwem MessagePack analizatora składni. SignalR ma wbudowaną obsługę MessagePack format i udostępnia interfejsy API dla klienta i serwera do użycia.

## <a name="configure-messagepack-on-the-server"></a>Konfigurowanie MessagePack na serwerze

Aby włączyć protokół MessagePack koncentratora na serwerze, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu w aplikacji. W pliku Startup.cs dodać `AddMessagePackProtocol` do `AddSignalR` wywołanie, aby włączyć obsługę MessagePack na serwerze.

> [!NOTE]
> JSON jest domyślnie włączona. Dodanie MessagePack umożliwia obsługę JSON i MessagePack klientów.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Aby dostosować sposób MessagePack będzie formatowania danych, `AddMessagePackProtocol` przyjmuje delegata do konfigurowania opcji. W tym delegata `FormatterResolvers` właściwości może służyć do konfigurowania opcji serializacji MessagePack. Aby uzyskać więcej informacji na temat rozpoznawania nazw, można znaleźć w bibliotece MessagePack pod [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Atrybuty mogą być używane na obiekty, które chcesz uszeregować do zdefiniowania, jak powinien zostać obsłużony.

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

### <a name="net-client"></a>Klient .NET

Aby włączyć MessagePack w kliencie programu .NET, należy zainstalować `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pakietu i wywołanie `AddMessagePackProtocol` na `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> To `AddMessagePackProtocol` wywołania przyjmuje delegata do konfigurowania opcji tak samo jak serwer.

### <a name="javascript-client"></a>JavaScript klienta

Obsługa MessagePack klienta Javascript jest zapewniana przez `@aspnet/signalr-protocol-msgpack` pakietu NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Po zainstalowaniu pakietu npm, można używana bezpośrednio przez moduł ładujący JavaScript lub zaimportować do przeglądarki, za pomocą odwołań do modułu *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  pliku. W przeglądarce `msgpack5` biblioteki musi się też odwoływać. Użyj `<script>` tag, aby utworzyć odwołania. Biblioteki można znaleźć w folderze *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Korzystając z `<script>` elementu, kolejność jest ważna. Jeśli *signalr-protocol-msgpack.js* odwołuje się przed *msgpack5.js*, występuje błąd podczas próby nawiązania połączenia z MessagePack. *signalr.js* jest również wymagana przed *signalr-protocol-msgpack.js*.

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
> W tej chwili brak opcji konfiguracji protokołu MessagePack na kliencie JavaScript.

## <a name="related-resources"></a>Zasoby pokrewne

* [Wprowadzenie](xref:tutorials/signalr)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
