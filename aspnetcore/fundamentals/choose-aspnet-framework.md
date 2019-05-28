---
title: Wybieranie między ASP.NET 4.x i ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono vs platformy ASP.NET Core. ASP.NET 4.x i jak dokonać wyboru między nimi.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/02/2019
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a51d9946c9e65bd1665c610153f724c6087c9f7f
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251368"
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
|[Strony razor](xref:razor-pages/index) przedstawia zalecane podejście do tworzenia interfejsu użytkownika sieci Web począwszy od platformy ASP.NET Core 2.x. Zobacz też [MVC](xref:mvc/overview), [interfejs API sieci Web](xref:tutorials/first-web-api), i [SignalR](xref:signalr/introduction).|Użyj [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [interfejs API sieci Web](/aspnet/web-api/), [elementów Webhook](/aspnet/webhooks/), lub [stron sieci Web](/aspnet/web-pages)|
|Wiele wersji na maszynie|Jedna wersja na maszynie|
|Programowanie za pomocą programu Visual Studio [programu Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/), lub [programu Visual Studio Code](https://code.visualstudio.com/) przy użyciu C# lubF#|Programowanie za pomocą programu Visual Studio przy użyciu C#, VB, lubF#|
|Wyższą wydajność niż ASP.NET 4.x|Dobra wydajność|
|[Wybierz środowisko uruchomieniowe platformy .NET Framework lub .NET Core](/dotnet/standard/choosing-core-framework-server)|Używasz środowiska uruchomieniowego .NET Framework|

Zobacz [platformy ASP.NET Core przeznaczone dla .NET Framework](xref:index#target-framework) informacje na temat platformy ASP.NET Core 2.x obsługi w programie .NET Framework.

## <a name="aspnet-core-scenarios"></a>Scenariusze platformy ASP.NET Core

* [Witryny sieci Web](xref:tutorials/first-mvc-app/index)
* [Interfejsy API](xref:tutorials/first-web-api)
* [W czasie rzeczywistym](xref:signalr/index)
* [Wdrażanie aplikacji ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>Scenariusze 4.x ASP.NET

* [Witryny sieci Web](/aspnet/mvc)
* [Interfejsy API](/aspnet/web-api)
* [W czasie rzeczywistym](/aspnet/signalr)
* [Tworzenie aplikacji sieci web platformy ASP.NET 4.x na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do platformy ASP.NET](/aspnet/overview)
* [Wprowadzenie do platformy ASP.NET Core](xref:index)
* <xref:host-and-deploy/azure-apps/index>
