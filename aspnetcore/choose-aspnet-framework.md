---
title: "Wybór między ASP.NET i ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak wybrać ASP.NET i ASP.NET Core."
keywords: "Platformy ASP.NET Core podstawy, omówienie"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 875064bd3437acc4e2a53220e19e86431d8c159b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>Wybór między ASP.NET i ASP.NET Core 

Niezależnie od tego, w przypadku tworzenia aplikacji sieci web platformy ASP.NET zawiera rozwiązanie dla Ciebie: z aplikacji sieci web przeznaczonych dla systemu Windows Server, aby małych mikrousług przeznaczonych dla kontenery systemu Linux i wszystko między nimi.

## <a name="aspnet-core"></a>Platformy ASP.NET Core

Platformy ASP.NET Core to open source, międzyplatformowa struktura do tworzenia aplikacji sieci web nowoczesny, oparte na chmurze dla systemu Windows, system macOS lub Linux.

## <a name="aspnet"></a>ASP.NET

ASP.NET to dojrzały platforma, która zawiera wszystkich usług niezbędnych do tworzenia klasy korporacyjnej, na serwerze aplikacji sieci web w systemie Windows.

## <a name="which-one-is-right-for-me"></a>Które z nich jest dla mnie odpowiednia?

| Platformy ASP.NET Core | ASP.NET |
|---|---|
|Tworzenie dla systemu Windows, system macOS lub Linux|Kompilacji dla systemu Windows|
|[Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web platformy ASP.NET Core 2.0. Zobacz też [MVC](xref:mvc/overview) i [interfejs API sieci Web](xref:tutorials/first-web-api)|Użyj [sieci Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [interfejs API sieci Web](https://docs.microsoft.com/aspnet/web-api/), lub [stron sieci Web](https://docs.microsoft.com/aspnet/web-pages)|
|Wiele wersji na maszynie|Jednej wersji na maszynie|
|Tworzenie za pomocą programu Visual Studio, [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), lub [Visual Studio Code](https://code.visualstudio.com/) przy użyciu języka C# lub języka F #|Tworzenie z programem Visual Studio za pomocą C#, VB i F #|
|Większą wydajność niż ASP.NET|Dobrą wydajność|
|[Wybierz środowisko uruchomieniowe .NET Framework lub .NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|Użyj środowiska uruchomieniowego .NET Framework|

## <a name="aspnet-core-scenarios"></a>Scenariusze platformy ASP.NET Core

<!-- update link to Razor Pages mvc movie series when done -->
* [Stron razor](xref:mvc/razor-pages/index) jest zalecane podejście do tworzenia interfejsu użytkownika sieci Web platformy ASP.NET Core 2.0.
* [Witryny sieci Web](xref:tutorials/first-mvc-app/index)
* [Interfejsy API](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a>Scenariusze programu ASP.NET

* [Witryny sieci Web](https://docs.microsoft.com/aspnet/mvc)
* [Interfejsy API](https://docs.microsoft.com/aspnet/web-api)
* [W czasie rzeczywistym](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a>Resources

* [Wprowadzenie do programu ASP.NET](https://docs.microsoft.com/aspnet/overview)
* [Wprowadzenie do platformy ASP.NET Core](xref:index)
