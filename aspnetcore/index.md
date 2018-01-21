---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 6535cc2d7eed099804db5d9783e0f7384bc9fcad
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze. Platforma ASP.NET Core umożliwia:

* Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/internet-of-things/) i zapleczy mobilnych.
* Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.
* Wdrażanie w chmurze lub lokalnie.
* Uruchamianie na platformie [.NET Core lub .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Dlaczego warto korzystać z platformy ASP.NET Core?

Miliony deweloperów korzystają z platformy [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) do tworzenia aplikacji internetowych. ASP.NET Core to przeprojektowana platforma ASP.NET 4.x, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.

Platforma ASP. NET Core oferuje następujące zalety:

* Ujednolicony scenariusz na potrzeby tworzenia internetowego interfejsu użytkownika i internetowych interfejsów API.
* Integracja [nowoczesnych struktur po stronie klienta](xref:client-side/index) i programistycznych przepływów pracy.
* Gotowy do pracy w chmurze, oparty na środowisku [system konfiguracji](xref:fundamentals/configuration/index).
* Wbudowane [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).
* Uproszczony, modułowy potok żądań HTTP zapewniający [wysoką wydajność](https://github.com/aspnet/benchmarks).
* Możliwość hostowania w usługach [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index) lub samodzielnie we własnym procesie.
* Przechowywanie wersji aplikacji obok siebie, gdy platformą docelową jest [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
* Narzędzia, które upraszczają tworzenie nowoczesnych aplikacji internetowych.
* Możliwość kompilowania i uruchamiania w systemach Windows, macOS i Linux.
* Open source i [koncentracja na społeczności](https://live.asp.net/).

Platforma ASP.NET Core jest dostarczana w całości w postaci pakietów [NuGet](https://www.nuget.org/). Dzięki temu można zoptymalizować aplikację, uwzględniając w niej tylko niezbędne pakiety NuGet. Aplikacje ASP.NET Core 2.x, których platformą docelową jest platforma .NET Core, wymagają tak naprawdę tylko [jednego pakietu NuGet](xref:fundamentals/metapackage). Korzyści z mniejszego obszaru powierzchni aplikacji obejmują: większe bezpieczeństwo, ograniczenie obsługi i lepszą wydajność.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core

Platforma ASP.NET Core MVC udostępnia funkcje, które umożliwiają tworzenie [internetowych interfejsów API](xref:tutorials/index#build-web-apis) i [aplikacji internetowych](xref:tutorials/index#build-web-apps):

* Wzorzec [MVC (Model View Controller)](xref:mvc/overview) ułatwia zapewnienie [możliwości testowania](testing/index.md) aplikacji internetowych i internetowych interfejsów API.
* [Strony Razor](xref:mvc/razor-pages/index) (nowość w ASP.NET Core 2.0) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.
* [Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:mvc/razor-pages/index) i [widoków MVC](xref:mvc/views/overview).
* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.
* Wbudowana obsługa [wiele formatów danych i negocjacji zawartości](mvc/models/formatting.md) umożliwia internetowym interfejsom API obsługę szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.
* [Powiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP na parametry metod akcji.
* [Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza weryfikację (walidację) po stronie klienta i serwera.

## <a name="client-side-development"></a>Programowanie po stronie klienta

Platforma ASP.NET Core bezproblemowo integruje się z popularnymi strukturami i bibliotekami po stronie klienta, takimi jak [Angular](xref:spa/angular), [React](xref:spa/react) czy [Bootstrap](xref:client-side/bootstrap). Zobacz [Programowanie po stronie klienta](xref:client-side/index), aby uzyskać więcej informacji.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Samouczki platformy ASP.NET Core](xref:tutorials/index)
* [Platforma ASP.NET Core — podstawy](xref:fundamentals/index)
* [Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) zawiera aktualności o postępach i planach zespołu. Zawiera też informacje o polecanych blogach i oprogramowaniu innych firm.
