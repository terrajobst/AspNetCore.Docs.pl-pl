---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Przykłady Katana | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: a26cdf144d02f0b429994df9d7863163807ac7b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827275"
---
<a name="katana-samples"></a>Przykłady Katana
====================
przez [firmy Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Przykłady Katana

**ASP.NET kieruje przykładowe** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
W niektórych aplikacjach można dołączyć składniki OWIN w tabeli trasy Asp.Net równolegle składniki bez OWIN. Ten przykład pokazuje, jak użyć metod rozszerzających RouteCollection MapOwinPath i udostępniane przez Microsoft.Owin.Host.SystemWeb MapOwinRoute.

**Rozgałęzianie potoków przykładowe** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Potoki przetwarzania żądania OWIN czy muszą być liniowego, może zostać rozgałęzione do przetwarzania żądań w różny sposób. W tym przykładzie pokazano, jak do budowy potoku rozgałęziania, na podstawie ścieżki żądania lub inne dane na żądanie, takich jak nagłówki. Te składniki są dostępne w pakiecie nuget Microsoft.Owin.Mapping.

**Przykład niestandardowych Server** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Pokazuje, jak użyć niestandardowego serwera OWIN, gdy hostingu samodzielnego OWIN.

**Przykładowy osadzony** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Niektóre serwery OWIN mogą być uruchamiane wewnątrz własnego procesu (&quot;może być samodzielnie hostowane&quot;). W tym przykładzie pokazano, jak można uruchomić za pomocą narzędzi dostarczanych przez pakiet nuget Microsoft.Owin.Hosting aplikacji OWIN.

**Przykładowe HelloWorld** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN to serwer HTTP abstrakcji interfejsu API, który umożliwia przenoszenia aplikacji na różnych serwerach. W tym przykładzie pokazano, jak napisać aplikację Hello World w niektórych **proste otoki** wokół nieprzetworzone abstrakcji OWIN i uruchom go na serwerze sieci web takich jak ASP.NET.

**Przykładowa aplikacja Hello World pierwotne OWIN** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
W tym przykładzie przedstawiono sposób pisania aplikacji Hello World za pomocą **pierwotne** abstrakcji OWIN i uruchom go na serwerze sieci web, takimi jak Asp.Net.

**Przykładowe SignalR** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Pokazuje, jak na potrzeby samodzielnego hostowania SignalR przy użyciu OWIN / Katana. Aby uzyskać więcej informacji na temat biblioteki SignalR własnym hostingu, zobacz [samouczek: Host samodzielny SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statyczne pliki przykładowe** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Pokazuje, jak do obsługi żądań HTTP dla plików statycznych przy użyciu OWIN / Katana.

**Interfejs API sieci Web** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
W tym przykładzie pokazano, jak hostowanie OWIN w usługach IIS, a następnie Dodaj interfejs API sieci Web do potoku OWIN.

**Sieci Web przykładowej gniazda** | [kodu źródłowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Pokazuje, jak do obsługi gniazda sieci Web w OWIN przy użyciu [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) klasy.
