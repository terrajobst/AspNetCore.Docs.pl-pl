---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze. Platforma ASP.NET Core umożliwia:

* Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/en-us/internet-of-things/) i zapleczy mobilnych.
* Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.
* Wdrażanie w chmurze i lokalne
* Uruchamianie na platformie [.NET Core lub .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Dlaczego warto korzystać z platformy ASP.NET Core?

Miliony deweloperów używają platformy ASP.NET do tworzenia aplikacji internetowych. Platforma ASP.NET Core to przeprojektowana platforma ASP.NET, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.

Platforma ASP. NET Core oferuje następujące zalety:

* Ujednolicony scenariusz na potrzeby tworzenia internetowego interfejsu użytkownika i internetowych interfejsów API.
* Integracja [nowoczesnych struktur po stronie klienta](xref:client-side/index) i przepływów pracy programowania.
* Gotowy do pracy w chmurze, oparty na środowisku [system konfiguracji](xref:fundamentals/configuration/index).
* Wbudowane [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).
* Uproszczony, zapewniający wysoką wydajność, modułowy potok żądania HTTP.
* Możliwość hostowania w ramach usług IIS lub samodzielnego hostowania we własnym procesie.
* Możliwość działania na platformie [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), która obsługuje prawdziwe równoległe używanie wielu wersji obok siebie.
* Narzędzia, które upraszczają tworzenie nowoczesnych aplikacji internetowych.
* Możliwość kompilowania i uruchamiania w systemach Windows, macOS i Linux.
* Open source i koncentracja na społeczności.

Platforma ASP.NET Core jest dostarczana w całości w postaci pakietów [NuGet](https://www.nuget.org/). Dzięki temu można zoptymalizować aplikację w celu uwzględnienia tylko tych pakietów NuGet, które są potrzebne. Korzyści z mniejszego obszaru powierzchni aplikacji obejmują: większe bezpieczeństwo, ograniczenie obsługi i lepszą wydajność.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core

Platforma ASP.NET Core MVC udostępnia funkcje, które ułatwiają tworzenie [internetowych interfejsów API](xref:tutorials/index#building-web-apis) i [aplikacji internetowych](xref:tutorials/index#building-web-applications):

* Wzorzec [MVC (Model View Controller)](xref:mvc/overview) ułatwia zapewnienie [możliwości testowania](testing/index.md) aplikacji internetowych i internetowych interfejsów API.
* [Strony Razor](xref:mvc/razor-pages/index) (nowość w wersji 2.0) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.
* [Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:mvc/razor-pages/index) i [widoków MVC](xref:mvc/views/overview).
* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.
* Wbudowana obsługa [wiele formatów danych i negocjacji zawartości](mvc/models/formatting.md) umożliwia internetowym interfejsom API obsługę szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.
* [Wiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP do parametrów metod akcji.
* [Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza weryfikację (walidację) po stronie klienta i po stronie serwera.

## <a name="client-side-development"></a>Programowanie po stronie klienta

Platformę ASP.NET Core zaprojektowano w celu bezproblemowej integracji z rozmaitymi platformami po stronie klienta, w tym [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) i [Bootstrap](xref:client-side/bootstrap). Zobacz [Programowanie po stronie klienta](client-side/index.md), aby uzyskać więcej informacji.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Samouczki platformy ASP.NET Core](xref:tutorials/index)
* [Platforma ASP.NET Core — podstawy](xref:fundamentals/index)
* [Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) obejmuje informacje o postępie i planach zespołu, poleca nowe blogi i oprogramowanie innych firm.
