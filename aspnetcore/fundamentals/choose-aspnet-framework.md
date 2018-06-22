---
title: Wybór między ASP.NET i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wybrać ASP.NET i ASP.NET Core.
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291643"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>Wybór między ASP.NET i ASP.NET Core

Niezależnie od tego, jakiego typu aplikację tworzysz ASP.NET ma dla Ciebie rozwiązanie: od aplikacji sieci web przeznaczonych dla systemu Windows Server, do małych mikrousług przeznaczonych dla kontenerów systemu Linux i wiele innych.

## <a name="aspnet-core"></a>ASP.NET Core

Platformy ASP.NET Core to open source, międzyplatformowa struktura do tworzenia aplikacji sieci web nowoczesny, oparte na chmurze systemu Windows, system macOS lub Linux.

## <a name="aspnet"></a>ASP.NET

ASP.NET to dojrzały platforma, która zawiera wszystkich usług niezbędnych do tworzenia korporacyjnej, aplikacje oparte na serwerze sieci web w systemie Windows.

## <a name="framework-selection"></a>Wybór Framework

Przejrzeć tabelę poniżej, aby określić, które framework jest najbardziej odpowiednią do potrzeb.

| ASP.NET Core | ASP.NET |
|---|---|
|Tworzenie dla systemu Windows, system macOS lub Linux|Tworzenie dla systemu Windows|
|[Stron razor](xref:razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x. Zobacz też [MVC](xref:mvc/overview), [interfejs API sieci Web](xref:tutorials/first-web-api), i [SignalR](xref:signalr/introduction).|Użyj [sieci Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [interfejs API sieci Web](/aspnet/web-api/), [elementów Webhook](/aspnet/webhooks/), lub [stron sieci Web](/aspnet/web-pages)|
|Wiele wersji na maszynie|Jedna wersja na maszynie|
|Tworzenie za pomocą programu Visual Studio, [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu języka C# lub języka F #|Tworzenie z programem Visual Studio za pomocą C#, VB i F #|
|Większa wydajność niż ASP.NET|Dobra wydajność|
|[Wybierz środowisko uruchomieniowe .NET Framework lub .NET Core](/dotnet/articles/standard/choosing-core-framework-server)|Używasz środowiska uruchomieniowego .NET Framework|

## <a name="aspnet-core-scenarios"></a>Scenariusze platformy ASP.NET Core

* [Stron razor](xref:razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x.
* [Witryny sieci Web](xref:tutorials/first-mvc-app/index)
* [Interfejsy API](xref:tutorials/first-web-api)
* [W czasie rzeczywistym](xref:signalr/index)

## <a name="aspnet-scenarios"></a>Scenariusze programu ASP.NET

* [Witryny sieci Web](/aspnet/mvc)
* [Interfejsy API](/aspnet/web-api)
* [W czasie rzeczywistym](/aspnet/signalr)

## <a name="resources"></a>Resources

* [Wprowadzenie do programu ASP.NET](/aspnet/overview)
* [Wprowadzenie do platformy ASP.NET Core](xref:index)
