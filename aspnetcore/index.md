---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Zapoznaj się z wprowadzeniem do rozwiązania ASP.NET Core, czyli międzyplatformowej struktury typu open source o wysokiej wydajności służącej do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: 60f7d64baa0441b90befb2d785999a707e1025c5
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225398"
---
# <a name="introduction-to-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) i [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core to międzyplatformowa struktura typu [open source](https://github.com/aspnet/home) o wysokiej wydajności służąca do tworzenia nowoczesnych aplikacji internetowych opartych na chmurze. Platforma ASP.NET Core umożliwia:

* Kompilowanie aplikacji i usług internetowych, aplikacji [IoT](https://www.microsoft.com/internet-of-things/) i zapleczy mobilnych.
* Używanie ulubionych narzędzi programistycznych w systemach Windows, macOS i Linux.
* Wdrażanie w chmurze lub lokalnie.
* Uruchamianie na platformie [.NET Core lub .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Dlaczego warto korzystać z platformy ASP.NET Core?

Miliony deweloperów korzystają z platformy [ASP.NET 4.x](/aspnet/overview) do tworzenia aplikacji internetowych. ASP.NET Core to przeprojektowana platforma ASP.NET 4.x, w której wprowadzono zmiany architektoniczne w celu stworzenia bardziej zwartej i modułowej struktury.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Tworzenie internetowego interfejsu użytkownika i internetowych interfejsów API przy użyciu wzorca MVC platformy ASP.NET Core

Platforma ASP.NET Core MVC udostępnia funkcje, które umożliwiają tworzenie [internetowych interfejsów API](xref:tutorials/first-web-api) i [aplikacji internetowych](xref:tutorials/razor-pages/index):

* Wzorzec [MVC (Model View Controller)](xref:mvc/overview) ułatwia zapewnienie możliwości testowania aplikacji internetowych i internetowych interfejsów API.
* [Strony Razor](xref:razor-pages/index) (nowość w ASP.NET Core 2.0) to oparty na stronach model programowania, który umożliwia łatwiejsze i bardziej wydajne tworzenie internetowego interfejsu użytkownika.
* [Składnia Razor](xref:mvc/views/razor) zapewnia wydajny język dla [stron Razor](xref:razor-pages/index) i [widoków MVC](xref:mvc/views/overview).
* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.
* Wbudowana obsługa [wiele formatów danych i negocjacji zawartości](xref:web-api/advanced/formatting) umożliwia internetowym interfejsom API obsługę szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.
* [Powiązanie modelu](xref:mvc/models/model-binding) automatycznie mapuje dane z żądań HTTP na parametry metod akcji.
* [Walidacja modelu](xref:mvc/models/validation) automatycznie przeprowadza walidację po stronie klienta i serwera.

## <a name="client-side-development"></a>Programowanie po stronie klienta

Platforma ASP.NET Core bezproblemowo integruje się z popularnymi strukturami i bibliotekami po stronie klienta, takimi jak [Angular](xref:spa/angular), [React](xref:spa/react) czy [Bootstrap](https://getbootstrap.com/). Aby uzyskać więcej informacji, zobacz [Programowanie po stronie klienta](xref:client-side/index).

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>Platforma ASP.NET Core ukierunkowana na platformę .NET Framework

Platforma ASP.NET Core 2.x może jako cel mieć platformę .NET Core lub .NET Framework. Aplikacje platformy ASP.NET Core ukierunkowane na platformę .NET Framework nie są wieloplatformowe &mdash; działają tylko w systemie Windows. Ogólnie rzecz biorąc, platforma ASP.NET Core 2.x jest zbudowana z bibliotek [.NET Standard](/dotnet/standard/net-standard). Aplikacje napisane przy użyciu platformy .NET Standard 2.0 działają wszędzie tam, gdzie obsługiwana jest platforma .NET Standard 2.0.

Platforma ASP.NET Core 2.x jest obsługiwana w wersjach platformy .NET Framework zgodnych z platformą .NET Standard 2.0:

* Zdecydowanie zaleca się platformę .NET Framework 4.7.1 lub nowszą.
* Platforma .NET Framework 4.6.1 lub nowsza.

Platforma ASP.NET Core 3.0 i nowsze wersje będą działać tylko na platformie .NET Core. Aby uzyskać więcej informacji o tej zmianie, zobacz [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Pierwsze spojrzenie na zmiany wprowadzane na platformie ASP.NET Core 3.0).

Jest kilka zalet przyjmowania platformy .NET Core jako docelowej, a ich liczba rośnie z każdym wydaniem. Niektóre z zalet platformy .NET Core nad platformą .NET Framework to:

* Wieloplatformowość. Działa w systemach macOS, Linux i Windows.
* Większa wydajność
* Przechowywanie wersji obok siebie
* Nowe interfejsy API
* Kod open source

Ciężko pracujemy nad zlikwidowaniem rozbieżności między interfejsami API platform .NET Framework i .NET Core. Pakiet [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) udostępnił tysiące interfejsów API specyficznych dla systemu Windows na platformie .NET Core. Te interfejsy API nie były dostępne na platformie .NET Core 1.x.

## <a name="how-to-download-a-sample"></a>Instrukcje: pobieranie pliku

Wiele artykułów i samouczków zawiera linki do kodu przykładowego.

1. [Pobierz plik zip repozytorium ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).
1. Rozpakuj plik *Docs-master.zip*.
1. Adres URL linku do przykładu pomoże Ci przejść do katalogu przykładu.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Platforma ASP.NET Core — podstawy](xref:fundamentals/index)
* [Cotygodniowe podsumowanie ASP.NET Community Standup](https://live.asp.net/) zawiera aktualności o postępach i planach zespołu. Zawiera też informacje o polecanych blogach i oprogramowaniu innych firm.
