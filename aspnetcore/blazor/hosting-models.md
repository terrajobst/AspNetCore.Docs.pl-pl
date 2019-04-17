---
title: Blazor modelach hostingu
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i po stronie serwera ASP.NET Razor składniki podstawowe modele obsługi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/hosting-models
ms.openlocfilehash: 0e7598c9f27201a989d1088764e09461a37beae4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614847"
---
# <a name="blazor-hosting-models"></a>Blazor modelach hostingu

Przez [Daniel Roth](https://github.com/danroth27)

Blazor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce na [format WebAssembly](http://webassembly.org/)— na podstawie środowiska uruchomieniowego .NET (*Blazor*) lub po stronie serwera w programie ASP.NET Core (*składniki aplikacji ASP.NET Core Razor*). Niezależnie od tego modelu, aplikacji i składników modelach hostingu *pozostają takie same*. W tym artykule omówiono dostępne modele hostingu:

* [Blazor po stronie klienta](#client-side-hosting-model)
* [Po stronie serwera platformy ASP.NET Core Razor składników](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>Model hostingu po stronie klienta

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce na format WebAssembly. Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki. Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika. Aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu. Zasoby aplikacji są wdrażane jako pliki statyczne z serwera sieci web lub usługą umożliwia obsługę zawartości statycznej dla klientów.

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, użyj jednej z następujących szablonów:

* **Blazor** ([blazor nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; wdrożony jako zbiór plików statycznych.
* **Blazor (ASP.NET Core hostowane)** ([blazorhosted nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; obsługiwanej przez serwer programu ASP.NET Core. Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom. Aplikacja Blazor po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci web lub [SignalR](xref:signalr/introduction).

Szablony zawierają *components.webassembly.js* skrypt, który obsługuje:

* Pobieranie, środowisko uruchomieniowe platformy .NET, aplikacji i zależności aplikacji.
* Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.

Model hostingu w sieci po stronie klienta oferuje wiele korzyści. Blazor po stronie klienta:

* Ma nie zależności po stronie serwera .NET.
* W pełni wykorzystuje zasoby klienta i możliwości.
* Odciążanie pracować z serwera do klienta.
* Obsługuje scenariusze w trybie offline.

Brak wad hostingu po stronie klienta. Blazor po stronie klienta:

* Ogranicza ją do możliwości przeglądarki.
* Wymaga klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa).
* Ma większy rozmiar pobierania i aplikacji dłuższy czas ładowania.
* Ma mniejszą dla dorosłych, środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi (na przykład ograniczenia w [.NET Standard](/dotnet/standard/net-standard) pomocy technicznej i debugowania).

## <a name="server-side-hosting-model"></a>Model hostingu po stronie serwera

Za pomocą modelu hostingu po stronie serwera aplikacji jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

![Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć platformy ASP.NET Core **składniki Razor** szablonu ([razorcomponents nowe dotnet](/dotnet/core/tools/dotnet-new)). Aplikacja platformy ASP.NET Core hostuje składniki Razor aplikacji po stronie serwera i ustawia punkt końcowy SignalR, gdy klienci łączą. Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:

* Usługi aparatu Razor składniki po stronie serwera.
* Aplikacja do żądania obsługi potoku.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

*Components.server.js* skryptu&dagger; nawiązuje połączenie z klientem. To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).

Model hostingu w sieci po stronie serwera oferuje wiele korzyści. Składniki po stronie serwera Razor:

* Rozmiar aplikacji znacznie mniejszy niż aplikacja Blazor po stronie klienta i ładowane dużo szybciej.
* Można wykorzystać możliwości serwera, w tym przy użyciu zgodnych interfejsów API dowolnej platformy .NET Core.
* Uruchom na platformie .NET Core na serwerze, dzięki czemu .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.
* Współpracuje z elastycznej klientów (na przykład przeglądarek, które nie obsługują format WebAssembly i zasobach ograniczonego urządzenia).
* .NET /C# bazy kodu, w tym kodu składnika aplikacji, nie jest obsługiwane dla klientów.

Brak wad hostingu po stronie serwera. Składniki po stronie serwera Razor:

* Mają wyższe opóźnienie: Każdy interakcja użytkownika obejmuje przeskok sieci.
* Oferty obsługi w trybie offline: Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.
* Ograniczona skalowalność: Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.
* Wymaga platformy ASP.NET Core serwerze do obsługi aplikacji. Wdrożenie bez serwera (na przykład z sieci CDN) nie jest możliwe.

&dagger;*Components.server.js* skryptu jest opublikowana w następującej ścieżce: *bin / {debugowanie | Zlecenia} / {struktury docelowej} /publish/ {Nazwa aplikacji}. Aplikacja/dist/_struktura*.
