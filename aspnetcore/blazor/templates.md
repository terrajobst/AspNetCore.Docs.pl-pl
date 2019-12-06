---
title: Szablony Blazor ASP.NET Core
author: guardrex
description: Dowiedz się więcej na temat ASP.NET Core szablonów aplikacji Blazor i struktury projektu Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/25/2019
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: bc0ea4a777e8684a7b0925377b8a19a45c2b531c
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879655"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>Szablony Blazor ASP.NET Core

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Platforma Blazor udostępnia szablony do tworzenia aplikacji dla poszczególnych modeli hostingu Blazor:

* Blazor webassembly (`blazorwasm`)
* Serwer Blazor (`blazorserver`)

Aby uzyskać więcej informacji na temat modeli hostingu Blazor, zobacz <xref:blazor/hosting-models>.

Instrukcje krok po kroku dotyczące tworzenia aplikacji Blazor na podstawie szablonu znajdują się w temacie <xref:blazor/get-started>.

## <a name="opno-locblazor-project-structure"></a>Struktura projektu Blazor

Następujące pliki i foldery tworzą Blazor aplikację wygenerowaną na podstawie szablonu Blazor:

* *Program.cs* &ndash; punkt wejścia aplikacji, który konfiguruje ASP.NET Core [hosta](xref:fundamentals/host/generic-host). Kod w tym pliku jest wspólny dla wszystkich ASP.NET Core aplikacji wygenerowanych z szablonów ASP.NET Core.

* *Startup.cs* &ndash; zawiera logikę uruchamiania aplikacji. Klasa `Startup` definiuje dwie metody:

  * `ConfigureServices` &ndash; konfiguruje usługi dla [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. W Blazor aplikacji serwera usługi są dodawane przez wywoływanie <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, a `WeatherForecastService` jest dodawane do kontenera usługi do użycia przez przykładowy składnik `FetchData`.
  * `Configure` &ndash; konfiguruje potok obsługi żądania aplikacji:
    * Blazor webassembly &ndash; dodaje składnik `App` (określony jako element `app` DOM do metody `AddComponent`), który jest głównym składnikiem aplikacji.
    * Serwer programu Blazor
      * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> jest wywoływana w celu skonfigurowania punktu końcowego dla połączenia w czasie rzeczywistym z przeglądarką. Połączenie jest tworzone za pomocą [SignalR](xref:signalr/introduction), który jest strukturą do dodawania funkcji sieci Web w czasie rzeczywistym do aplikacji.
      * [MapFallbackToPage ("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) jest wywoływana w celu skonfigurowania strony głównej aplikacji (*strony/_Host. cshtml*) i włączenia nawigacji.

* *wwwroot/index.html* (Blazor webassembly) &ndash; stronę główną aplikacji zaimplementowaną jako strona HTML:
  * Po wstępnym zażądaniu dowolnej strony aplikacji jest ona renderowana i zwracana w odpowiedzi.
  * Strona określa, gdzie jest renderowany główny składnik `App`. Składnik `App` (*App. Razor*) jest określony jako element `app` dom dla metody `AddComponent` w `Startup.Configure`.
  * Plik JavaScript *_framework/blazor.webassembly.js* jest ładowany:
    * Pobiera środowisko uruchomieniowe platformy .NET, aplikację i zależności aplikacji.
    * Inicjuje środowisko uruchomieniowe, aby uruchomić aplikację.

* *Pages/_Host. cshtml* (Blazor Server) &ndash; stronę główną aplikacji zaimplementowaną jako strona Razor:
  * Po wstępnym zażądaniu dowolnej strony aplikacji jest ona renderowana i zwracana w odpowiedzi.
  * Plik JavaScript *_framework/blazor.Server.js* jest ładowany, który konfiguruje połączenie SignalR w czasie rzeczywistym między przeglądarką a serwerem.
  * Strona hosta określa, gdzie jest renderowany główny składnik `App` (*App. Razor*).

* *App. razor* &ndash; głównym składnikiem aplikacji, która konfiguruje Routing po stronie klienta za pomocą składnika <xref:Microsoft.AspNetCore.Components.Routing.Router>. Składnik `Router` przechwytuje nawigację przeglądarki i renderuje stronę pasującą do żądanego adresu.

* Folder *strony* &ndash; zawiera składniki/strony z obsługą routingu (*Razor*), które tworzą aplikację Blazor. Trasy dla każdej strony są określane za pomocą dyrektywy [`@page`](xref:mvc/views/razor#page) . Szablon zawiera następujące składniki:
  * `Index` (*index. Razor*) &ndash; implementuje stronę główną.
  * `Counter` (*Counter. Razor*) &ndash; implementuje stronę licznika.
  * `Error` (*Error. Razor*, Blazor tylko aplikacja serwera) &ndash; renderowane, gdy wystąpił nieobsługiwany wyjątek w aplikacji.
  * `FetchData` (*FetchData. Razor*) &ndash; implementuje stronę pobieranie danych.

* Folder *udostępniony* &ndash; zawiera inne składniki interfejsu użytkownika ( *. Razor*) używane przez aplikację:
  * `MainLayout` (*MainLayout. Razor*) &ndash; składnik układu aplikacji.
  * `NavMenu` (*NavMenu. Razor*) &ndash; implementuje nawigację po pasku bocznym. Zawiera [składnik NavLink](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), który renderuje linki nawigacji do innych składników Razor. Składnik `NavLink` automatycznie wskazuje wybrany stan podczas ładowania składnika, co pomaga użytkownikowi zrozumieć, który składnik jest aktualnie wyświetlany.

* *_Imports. razor* &ndash; zawiera wspólne dyrektywy Razor do uwzględnienia w składnikach aplikacji ( *. Razor*), takich jak [`@using`](xref:mvc/views/razor#using) dyrektyw dla przestrzeni nazw.

* Folder *danych* (Blazor Server) &ndash; zawiera klasę `WeatherForecast` i implementację `WeatherForecastService`, które zawierają przykładowe dane pogodowe do składnika `FetchData` aplikacji.

* *wwwroot* &ndash; folderem [głównym sieci Web](xref:fundamentals/index#web-root) aplikacji zawierającym publiczne statyczne zasoby aplikacji.

* *appSettings. JSON* (Blazor Server) &ndash; ustawienia konfiguracji dla aplikacji.
