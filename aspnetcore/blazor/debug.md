---
title: Debuguj ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/debug
ms.openlocfilehash: 9fc3f1d2dd7dc79d2ba3d64bff6e0f92ac2cf6dc
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391180"
---
# <a name="debug-aspnet-core-blazor"></a>Debuguj ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Istnieje *wczesna* pomoc techniczna dla debugowania Blazor aplikacje webassembly działające w zestawie webassembly w przeglądarce Chrome.

Możliwości debugera są ograniczone. Dostępne scenariusze obejmują:

* Ustawianie i usuwanie punktów przerwania.
* Pojedynczy krok (`F10`) przez wykonanie kodu lub wznowienie (`F8`).
* Na ekranie *Ustawienia lokalne* Obserwuj wartości wszelkich zmiennych lokalnych typu `int`, `string` i `bool`.
* Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.

*Nie*można:

* Zwróć uwagę na wartości dowolnych ustawień regionalnych, które nie są `int`, `string` lub `bool`.
* Obserwuj wartości właściwości lub pól klas.
* Umieść kursor nad zmiennymi, aby zobaczyć ich wartości.
* Oceń wyrażenia w konsoli programu.
* Krok w ramach wywołań asynchronicznych.
* Wykonuj większość innych typowych scenariuszy debugowania.

Programowanie dalszych scenariuszy debugowania jest tym samym punktem zespołu inżynierów.

## <a name="prerequisites"></a>Wymagania wstępne

Debugowanie wymaga jednej z następujących przeglądarek:

* Google Chrome (wersja 70 lub nowsza)
* Podgląd Microsoft Edge ([kanał dev Edge](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Procedura

1. Uruchom aplikację webassembly Blazor w konfiguracji `Debug`. Przekaż opcję `--configuration Debug` do polecenia [Run dotnet](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.
1. Uzyskaj dostęp do aplikacji w przeglądarce.
1. Umieść fokus klawiatury w aplikacji, a nie na panelu Narzędzia deweloperskie. Po zainicjowaniu debugowania można zamknąć panel Narzędzia deweloperskie.
1. Wybierz następujący skrót klawiaturowy dotyczący Blazor:
   * `Shift+Alt+D` w systemie Windows/Linux
   * `Shift+Cmd+D` w macOS
1. Postępuj zgodnie z instrukcjami wyświetlanymi na ekranie, aby ponownie uruchomić przeglądarkę z włączonym debugowaniem zdalnym.
1. Ponownie wybierz następujący skrót klawiaturowy dotyczący Blazor, aby rozpocząć sesję debugowania:
   * `Shift+Alt+D` w systemie Windows/Linux
   * `Shift+Cmd+D` w macOS

## <a name="enable-remote-debugging"></a>Włącz debugowanie zdalne

Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania w przeglądarce** Chrome. Strona błędu zawiera instrukcje dotyczące uruchamiania programu Chrome z otwartym portem debugowania, dzięki czemu serwer proxy debugowania Blazor może połączyć się z aplikacją. *Zamknij wszystkie wystąpienia programu Chrome* i uruchom ponownie program Chrome zgodnie z instrukcją.

## <a name="debug-the-app"></a>Debugowanie aplikacji

Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji. Rozwiń każdy zestaw i Znajdź plik *. cs*/ *.* pliki źródłowe Razor dostępne do debugowania. Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany. Po trafieniu punktu przerwania pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu (`F8`) normalnie.

Blazor udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci. Gdy skrót klawiaturowy debugowania zostanie naciśnięty, Blazor punkty Chrome DevTools na serwerze proxy. Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).

## <a name="browser-source-maps"></a>Mapy źródeł przeglądarki

Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta. Jednak Blazor nie jest obecnie mapowany C# bezpośrednio na język JavaScript/WASM. Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.

## <a name="troubleshooting-tip"></a>Porady dotyczące rozwiązywania problemów

Jeśli występują błędy, Poniższa Wskazówka może pomóc:

Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce. W konsoli programu wykonaj `localStorage.clear()`, aby usunąć wszystkie punkty przerwania.
