---
title: Składniki razor modele obsługi
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i po stronie serwera ASP.NET Razor składniki podstawowe modele obsługi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ffc22201b88b59077a98bd46cbf95e93d2d86e2
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345660"
---
# <a name="razor-components-hosting-models"></a>Składniki razor modele obsługi

Przez [Daniel Roth](https://github.com/danroth27)

Składniki razor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce w środowisku uruchomieniowym .NET oparte na format WebAssembly (*Blazor*) lub po stronie serwera w programie ASP.NET Core (*składniki programu ASP.NET Core Razor*). Niezależnie od tego modelu, aplikacji i składników modelach hostingu *pozostają takie same*. W tym artykule omówiono dostępne modelach hostingu.

## <a name="client-side-hosting-model"></a>Model hostingu po stronie klienta

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce. W tym modelu aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki. Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika. Wszystkie aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu. Zasoby aplikacji można wdrożyć jako pliki statyczne, za pomocą dowolnego serwera sieci web jest preferowana (zobacz [hosta i wdrażanie](xref:host-and-deploy/razor-components/index)).

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, należy użyć **Blazor** lub **Blazor (ASP.NET Core hostowane)** szablony projektów (`blazor` lub `blazorhosted` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie w wierszu polecenia). Dołączonej *components.webassembly.js* skryptu obsługuje:

* Pobieranie środowiska uruchomieniowego .NET, aplikacji oraz jego zależności.
* Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.

Model hostingu w sieci po stronie klienta oferuje wiele korzyści. Blazor po stronie klienta:

* Ma nie zależności po stronie serwera .NET.
* Zawiera zaawansowane interaktywnego interfejsu użytkownika.
* W pełni wykorzystuje zasoby klienta i możliwości.
* Odciążanie pracować z serwera do klienta.
* Obsługuje scenariusze w trybie offline.

Brak wad hostingu po stronie klienta. Blazor po stronie klienta:

* Ogranicza ją do możliwości przeglądarki.
* Wymaga klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa).
* Ma większy rozmiar pobierania i aplikacji dłuższy czas ładowania.
* Ma mniejszą dla dorosłych, środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi (na przykład ograniczenia dotyczące pomocy technicznej Standard platformy .NET i profilowanie).

Program Visual Studio obejmuje **Blazor (platformy ASP.NET Core hostowany)** szablon projektu umożliwiający tworzenie aplikacji Blazor, który jest uruchamiany na format WebAssembly i jest hostowana na serwerze programu ASP.NET Core. Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom, ale to oddzielny proces, w przeciwnym razie. Aplikacja Blazor po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci Web lub połączeniami SignalR.

> [!IMPORTANT]
> Jeśli aplikacja Blazor po stronie klienta jest obsługiwany przez aplikację ASP.NET Core, obsługiwane jako aplikację podrzędnych usług IIS, należy wyłączyć dziedziczone obsługi modułu ASP.NET Core. Ustaw ścieżki podstawowej aplikacji w aplikacji Blazor *index.html* pliku do aliasu usług IIS, używane podczas konfigurowania aplikacji podrzędnej w usługach IIS.
>
> Aby uzyskać więcej informacji, zobacz [ścieżki podstawowej aplikacji](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Model hostingu po stronie serwera

W modelu hostingu po stronie serwera platformy ASP.NET Core Razor składniki aplikacji są wykonywane na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem połączenia SignalR.

![ASP.NET Core Razor składniki po stronie serwera: Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

Aby utworzyć aplikację składniki Razor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć **Blazor (po stronie serwera w programie ASP.NET Core)** szablonu (`blazorserver` przy użyciu [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie w wierszu polecenia). Aplikacji ASP.NET Core obsługuje składniki Razor aplikacji po stronie serwera i ustawia punkt końcowy SignalR, gdy klienci łączą. Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:

* Usługi aparatu Razor składniki po stronie serwera.
* Aplikacja do żądania obsługi potoku.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

*Components.server.js* skryptu&dagger; nawiązuje połączenie z klientem. To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).

Model hostingu w sieci po stronie serwera oferuje wiele korzyści:

* Umożliwia pisanie całej aplikacji przy użyciu platformy .NET i C# przy użyciu modelu składnika.
* Zapewnia rozbudowane interaktywny sposób działania i pozwala uniknąć odświeżenie strony niepotrzebne.
* Ma znacznie mniejszym rozmiarem aplikacji niż aplikacja Blazor po stronie klienta i ładuje znacznie szybciej.
* Składnik logiki można wykorzystać możliwości serwera, takie jak przy użyciu zgodnych interfejsów API dowolnej platformy .NET Core.
* Działa na platformie .NET Core w serwer, więc .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.
* Współpracuje z elastycznej klientów (na przykład przeglądarek, które nie obsługują format WebAssembly i zasobach ograniczonego urządzenia).

Istnieją wad po stronie serwera hostingu:

* Ma czas oczekiwania: Każdy interakcja użytkownika obejmuje przeskok sieci.
* Oferuje Brak obsługi w trybie offline: Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.
* Obniżyła skalowalność: Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.
* Wymaga serwera do obsługi aplikacji programu ASP.NET Core. Wdrożenie bez serwera (na przykład z sieci CDN) nie jest możliwe.

&dagger;*Components.server.js* skryptu jest opublikowana w następującej ścieżce: *bin / {debugowanie | Zlecenia} / {struktury docelowej} /publish/ {Nazwa aplikacji}. Aplikacja/dist/_struktura*.
