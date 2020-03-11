---
title: Wybieranie między ASP.NET 4.x i ASP.NET Core
author: rick-anderson
description: Wyjaśnia ASP.NET Core a ASP.NET 4. x oraz jak wybierać między nimi.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/12/2020
no-loc:
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a7280b59578ee1d96edeeccf9c9df0b0e4eb4eb8
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665243"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>Wybieranie między ASP.NET 4.x i ASP.NET Core

Platforma ASP.NET Core to przeprojektowana platforma ASP.NET 4.x. W tym artykule przedstawiono różnice między nimi.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core to architektura typu open source i między platformami do tworzenia aplikacji sieci web Nowoczesna, oparta na chmurze systemie Windows, macOS lub Linux.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x to dojrzała platforma, która zapewnia usługi potrzebne do tworzenia przeznaczonych dla przedsiębiorstw oparte na serwerze aplikacji sieci web na Windows.

## <a name="framework-selection"></a>Wybór Framework

W poniższej tabeli porównano platformy ASP.NET Core, platformy ASP.NET 4.x.

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|Tworzenie dla systemu Windows, system macOS lub Linux|Tworzenie dla systemu Windows|
|[Razor Pages](xref:razor-pages/index) jest zalecanym podejściem do tworzenia interfejsu użytkownika sieci Web w przypadku ASP.NET Core 2. x. Zobacz również [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api)i [SignalR](xref:signalr/introduction).|Korzystanie z [formularzy sieci Web](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), internetowego [interfejsu API](/aspnet/web-api/), elementów [webhook](/aspnet/webhooks/)lub [stron sieci Web](/aspnet/web-pages)|
|Wiele wersji na maszynie|Jedna wersja na maszynie|
|Programowanie za pomocą [programu Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/)lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu C# lubF#|Programowanie w programie [Visual Studio](https://visualstudio.microsoft.com/vs/) przy użyciu C#programu, VB lubF#|
|Wyższą wydajność niż ASP.NET 4.x|Dobra wydajność|
|[Korzystanie z środowiska uruchomieniowego platformy .NET Core](/dotnet/standard/choosing-core-framework-server)|Używasz środowiska uruchomieniowego .NET Framework|

Aby uzyskać informacje na temat obsługi programu ASP.NET Core 2. x w systemie .NET Framework, zobacz [ASP.NET Core określania celu .NET Framework](xref:index#target-framework) .

## <a name="aspnet-core-scenarios"></a>Scenariusze platformy ASP.NET Core

* [Zaufany](xref:tutorials/first-mvc-app/index)
* [Interfejsy API](xref:tutorials/first-web-api)
* [W czasie rzeczywistym](xref:signalr/introduction)
* [Wdrażanie aplikacji ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>Scenariusze 4.x ASP.NET

* [Zaufany](/aspnet/mvc)
* [Interfejsy API](/aspnet/web-api)
* [W czasie rzeczywistym](/aspnet/signalr)
* [Tworzenie aplikacji sieci Web ASP.NET 4. x na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do ASP.NET](/aspnet/overview)
* [Wprowadzenie do ASP.NET Core](xref:index)
* <xref:host-and-deploy/azure-apps/index>
