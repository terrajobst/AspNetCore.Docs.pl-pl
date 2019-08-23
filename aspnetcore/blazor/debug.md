---
title: Debuguj ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: blazor/debug
ms.openlocfilehash: c3188a1fe1b699b787f7a95630f3918d295d0f68
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69974910"
---
# <a name="debug-aspnet-core-blazor"></a>Debuguj ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

Na potrzeby debugowania Blazor aplikacje po stronie klienta działające w zestawie webassembly w programie Chrome istnieje wczesna pomoc techniczna.

Możliwości debugera są ograniczone. Dostępne scenariusze obejmują:

* Ustawianie i usuwanie punktów przerwania.
* Pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu`F8`().
* Na ekranie *Ustawienia lokalne* Obserwuj wartości wszelkich zmiennych lokalnych typu `int`, `string`, i `bool`.
* Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.

*Nie*można:

* Obserwuj wartości wszelkich zmiennych lokalnych `int`, które nie są, `string`lub `bool`.
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

1. Uruchom aplikację Blazor po stronie klienta w `Debug` konfiguracji. Przekaż opcję do polecenia [Run dotnet](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`. `--configuration Debug`
1. Uzyskaj dostęp do aplikacji w przeglądarce.
1. Umieść fokus klawiatury w aplikacji, a nie na panelu Narzędzia deweloperskie. Po zainicjowaniu debugowania można zamknąć panel Narzędzia deweloperskie.
1. Wybierz następujący skrót klawiaturowy dotyczący Blazor:
   * `Shift+Alt+D`w systemie Windows/Linux
   * `Shift+Cmd+D`na macOS
1. Postępuj zgodnie z instrukcjami wyświetlanymi na ekranie, aby ponownie uruchomić przeglądarkę z włączonym debugowaniem zdalnym.
1. Ponownie wybierz następujący skrót klawiaturowy dotyczący Blazor, aby rozpocząć sesję debugowania:
   * `Shift+Alt+D`w systemie Windows/Linux
   * `Shift+Cmd+D`na macOS

## <a name="enable-remote-debugging"></a>Włącz debugowanie zdalne

Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania w przeglądarce** Chrome. Strona błędu zawiera instrukcje dotyczące uruchamiania programu Chrome z otwartym portem debugowania, dzięki czemu serwer proxy debugowania Blazor może połączyć się z aplikacją. *Zamknij wszystkie wystąpienia programu Chrome* i uruchom ponownie program Chrome zgodnie z instrukcją.

## <a name="debug-the-app"></a>Debugowanie aplikacji

Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji. Rozwiń każdy zestaw i Znajdź pliki źródłowe *CS*/ *. Razor* dostępne do debugowania. Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany. Po trafieniu punktu przerwania pojedynczy krok (`F10`) przez wykonanie kodu kodu lub wznowienia (`F8`).

Blazor udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci. Gdy skrót klawiaturowy debugowania zostanie naciśnięty, Blazor punkty Chrome DevTools na serwerze proxy. Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).

## <a name="browser-source-maps"></a>Mapy źródeł przeglądarki

Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta. Jednak Blazor nie jest obecnie mapowany C# bezpośrednio na język JavaScript/WASM. Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.

## <a name="troubleshooting-tip"></a>Porady dotyczące rozwiązywania problemów

Jeśli występują błędy, Poniższa Wskazówka może pomóc:

Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce. W konsoli programu wykonaj `localStorage.clear()` polecenie, aby usunąć wszystkie punkty przerwania.
