---
title: Debuguj ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661708"
---
# <a name="debug-aspnet-core-blazor"></a>Debuguj ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Istnieje *wczesna* pomoc techniczna dla debugowania Blazor webassembly przy użyciu narzędzi deweloperskich przeglądarki w przeglądarkach opartych na chromie (Chrome/Edge). Prace są w toku do:

* W pełni Włącz debugowanie w programie Visual Studio.
* Włącz debugowanie w Visual Studio Code.

Możliwości debugera są ograniczone. Dostępne scenariusze obejmują:

* Ustawianie i usuwanie punktów przerwania.
* Pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu (`F8`).
* Na ekranie *Ustawienia lokalne* Obserwuj wartości wszelkich zmiennych lokalnych typu `int`, `string`i `bool`.
* Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.

*Nie*można:

* Obserwuj wartości dowolnych elementów lokalnych, które nie są `int`, `string`lub `bool`.
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

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> Obsługa debugowania w programie Visual Studio jest wczesnym etapem opracowywania. Debugowanie **F5** nie jest obecnie obsługiwane.

1. Uruchom aplikację webassembly Blazor w konfiguracji `Debug` bez debugowania (**Ctrl**+**F5** zamiast **F5**).
1. Otwórz właściwości debugowania aplikacji (ostatni wpis w menu **debugowanie** ) i skopiuj **adres URL aplikacji**http. Przejdź do adresu HTTP (nie adresu HTTPS) aplikacji przy użyciu przeglądarki opartej na formacie chrom (Edge beta lub Chrome).
1. Umieść fokus klawiatury w aplikacji w oknie przeglądarki, a nie na panelu Narzędzia deweloperskie. Najlepiej jest pozostawić zamkniętą panel Narzędzia deweloperskie w celu wykonania tej procedury. Po rozpoczęciu debugowania możesz ponownie otworzyć panel Narzędzia deweloperskie.
1. Wybierz następujący skrót klawiaturowy dotyczący Blazor:

   * `Shift+Alt+D` w systemie Windows
   * `Shift+Cmd+D` w macOS

   Jeśli zostanie wyświetlony komunikat **nie można znaleźć karty możliwością debugowania Browser**, zobacz [Włączanie debugowania zdalnego](#enable-remote-debugging).
   
   Po włączeniu debugowania zdalnego:
   
   1\. Zostanie otwarte nowe okno przeglądarki. Zamknij poprzednie okno.

   2\. Umieść fokus klawiatury w aplikacji w oknie przeglądarki.

   3 \. Wybierz skrót klawiaturowy dotyczący Blazor w nowym oknie przeglądarki: `Shift+Alt+D` w systemie Windows lub `Shift+Cmd+D` na macOS.

   4\. Zostanie otwarta karta **devtools** w przeglądarce. **Wybierz kartę aplikacji w oknie przeglądarki.**

   Aby dołączyć aplikację do programu Visual Studio, zobacz sekcję [dołączanie do procesu w programie Visual Studio](#attach-to-process-in-visual-studio) .

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. Uruchom aplikację webassembly Blazor w konfiguracji `Debug`, przekazując opcję `--configuration Debug` do polecenia [Run dotnet](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.
1. Przejdź do aplikacji w adresie URL protokołu HTTP pokazanej w oknie powłoki.
1. Umieść fokus klawiatury w aplikacji, a nie na panelu Narzędzia deweloperskie. Najlepiej jest pozostawić zamkniętą panel Narzędzia deweloperskie w celu wykonania tej procedury. Po rozpoczęciu debugowania możesz ponownie otworzyć panel Narzędzia deweloperskie.
1. Wybierz następujący skrót klawiaturowy dotyczący Blazor:

   * `Shift+Alt+D` w systemie Windows
   * `Shift+Cmd+D` w macOS

   Jeśli zostanie wyświetlony komunikat **nie można znaleźć karty możliwością debugowania Browser**, zobacz [Włączanie debugowania zdalnego](#enable-remote-debugging).
   
   Po włączeniu debugowania zdalnego:
   
   1\. Zostanie otwarte nowe okno przeglądarki. Zamknij poprzednie okno.

   2\. Umieść fokus klawiatury w aplikacji w oknie przeglądarki, a nie na panelu Narzędzia deweloperskie.

   3 \. Wybierz skrót klawiaturowy dotyczący Blazor w nowym oknie przeglądarki: `Shift+Alt+D` w systemie Windows lub `Shift+Cmd+D` na macOS.

---

## <a name="enable-remote-debugging"></a>Włącz debugowanie zdalne

Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania w przeglądarce** Chrome. Strona błędu zawiera instrukcje dotyczące uruchamiania programu Chrome z otwartym portem debugowania, dzięki czemu serwer proxy debugowania Blazor może połączyć się z aplikacją. *Zamknij wszystkie wystąpienia programu Chrome* i uruchom ponownie program Chrome zgodnie z instrukcją.

## <a name="debug-the-app"></a>Debugowanie aplikacji

Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji. Rozwiń każdy zestaw i Znajdź *pliki źródłowe/.* *CS* dostępne do debugowania. Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany. Po trafieniu punktu przerwania pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu (`F8`) normalnie.

Blazor udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci. Podczas naciskania skrótu klawiaturowego, Blazor wskazywał DevTools na serwerze proxy. Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).

## <a name="attach-to-process-in-visual-studio"></a>Dołącz do procesu w programie Visual Studio

Dołączanie do procesu aplikacji w programie Visual Studio jest *tymczasowym* scenariuszem debugowania dla Blazor webassembly, podczas gdy debugowanie **F5** jest w trakcie projektowania.

Aby dołączyć proces uruchomionej aplikacji do programu Visual Studio:

1. W programie Visual Studio wybierz kolejno opcje **debuguj** > **Dołącz do procesu**.
1. W polu **Typ połączenia**wybierz pozycję **Chrome devtools Protocol WebSocket (bez uwierzytelniania)** .
1. W przypadku **celu połączenia**Wklej adres http (nie adres https) aplikacji.
1. Wybierz pozycję **Odśwież** , aby odświeżyć wpisy w obszarze **dostępne procesy**.
1. Wybierz proces przeglądarki do debugowania i wybierz pozycję **Dołącz**.
1. W oknie dialogowym **Wybieranie typu kodu** wybierz typ kodu dla konkretnej przeglądarki, do której dołączasz (Edge lub Chrome), a następnie wybierz **przycisk OK**.

## <a name="browser-source-maps"></a>Mapy źródeł przeglądarki

Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta. Jednak Blazor nie są obecnie mapowane C# bezpośrednio do języka JavaScript/WASM. Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Jeśli występują błędy, Poniższa Wskazówka może pomóc:

Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce. W konsoli programu wykonaj `localStorage.clear()`, aby usunąć wszystkie punkty przerwania.
