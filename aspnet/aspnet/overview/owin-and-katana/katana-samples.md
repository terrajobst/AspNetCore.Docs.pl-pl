---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Przykłady Katana | Dokumentacja firmy Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a>Przykłady Katana
====================
przez [firmy Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Przykłady Katana

**ASP.NET kieruje próbki** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
W niektórych aplikacjach ma Podłączanie składników OWIN w tabeli tras Asp.Net równolegle składniki z systemem innym niż OWIN. Ten przykład przedstawia sposób użycia metody rozszerzenia klasy RouteCollection MapOwinPath i MapOwinRoute dostarczonych przez Microsoft.Owin.Host.SystemWeb.

**Rozgałęzianie potoków próbki** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Potoki przetwarzania żądania OWIN zbędna, nie jest liniowa, można można rozgałęzić do przetwarzania żądań na różne sposoby. W tym przykładzie pokazano, jak utworzyć potok rozgałęziania na podstawie ścieżki żądania lub inne dane żądania, takich jak nagłówki. Składniki te są dostępne w pakiecie nuget Microsoft.Owin.Mapping.

**Przykład niestandardowych Server** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Przedstawia sposób użycia niestandardowego serwera OWIN, gdy hostingu samodzielnego OWIN.

**Osadzonego próbce** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Niektóre serwery OWIN mogą być uruchamiane wewnątrz własnego procesu (&quot;hosta samodzielnego&quot;). W tym przykładzie pokazano, jak rozpocząć korzystanie z narzędzi dostarczanych przez pakiet nuget Microsoft.Owin.Hosting aplikacji OWIN.

**Przykładowe HelloWorld** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN to serwer HTTP abstrakcji interfejsu API, zapewniające przenośność aplikacji na różnych serwerach. W tym przykładzie przedstawiono sposób tworzenia aplikacji Hello World niektórych **proste otoki** wokół pierwotnych abstrakcji OWIN i uruchom go na serwerze sieci web program ASP.NET.

**Przykładowa aplikacja Hello World Raw OWIN** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
W tym przykładzie pokazano, jak aplikacja Hello World przy użyciu **raw** abstrakcji OWIN i uruchom go na serwerze sieci web, takich jak Asp.Net.

**Przykładowe SignalR** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Pokazuje, jak hosta samodzielnego SignalR za pomocą OWIN / Katana. Aby uzyskać więcej informacji na temat własnym hostingu SignalR, zobacz [samouczek: SignalR hosta samodzielnego](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Przykładowe pliki statyczne** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Przedstawia sposób obsługi żądań HTTP dotyczących plików statycznych przy użyciu OWIN / Katana.

**Interfejs API sieci Web** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
W tym przykładzie pokazano, jak udostępniać OWIN w usługach IIS i Dodaj interfejs API sieci Web do potoku OWIN.

**Sieci Web przykładowej gniazda** | [kod źródłowy](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Przedstawia sposób obsługi gniazda sieci Web w OWIN za pomocą [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) klasy.
