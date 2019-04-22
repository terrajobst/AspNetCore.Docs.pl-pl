---
title: Debugowanie Blazor
author: guardrex
description: Dowiedz się, jak można debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/debug
ms.openlocfilehash: 40457b942061fb910a6311af78ff29ac3a1699de
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982793"
---
# <a name="debug-blazor"></a>Debugowanie Blazor

[Daniel Roth](https://github.com/danroth27)

*Wczesne* Obsługa istnieje, do debugowania po stronie klienta Blazor aplikacji uruchomionych na format WebAssembly w przeglądarce Chrome.

Debuger możliwości są ograniczone. Dostępne scenariusze obejmują:

* Ustaw i usuwanie punktów przerwania.
* Pojedynczy krok (`F10`) za pomocą kodu lub Wznów (`F8`) wykonanie kodu.
* W *lokalne* wyświetlane, sprawdź wartości wszelkich zmiennych lokalnych typu `int`, `string`, i `bool`.
* Zobaczyć stos wywołań, w tym łańcuchy wywołania, które bardziej szczegółowo w kodzie JavaScript z języka JavaScript w środowisku .NET i .NET.

Możesz *nie*:

* Sprawdź wartości wszelkich zmiennych lokalnych, które nie są `int`, `string`, lub `bool`.
* Sprawdź wartości właściwości klasy lub pola.
* Umieść kursor nad zmienne, aby zobaczyć ich wartości.
* Ocena wyrażeń w konsoli.
* Przejść przez wywołania asynchronicznego.
* Wykonywać większości innych zwykłych scenariuszy debugowania.

Rozwój dalszego debugowania scenariusze jest w toku celem zespołu inżynieryjnego.

## <a name="procedure"></a>Procedura

Aby debugować aplikację Blazor po stronie klienta w przeglądarce Chrome:

* Tworzenie aplikacji Blazor w `Debug` konfiguracji (domyślnie dla nieopublikowane aplikacje).
* Uruchom aplikację Blazor w przeglądarce Chrome (wersja 70 lub nowszej).
* Fokus klawiatury w aplikacji (nie w panelu Narzędzia dla deweloperów, które prawdopodobnie należy zamknąć mniej mylące środowisko debugowania) wybierz następujący skrót klawiaturowy specyficzne dla Blazor:
  * `Shift+Alt+D` w systemie Windows/Linux
  * `Shift+Cmd+D` W systemie macOS

## <a name="enable-remote-debugging"></a>Włączanie debugowania zdalnego

Jeśli zdalne debugowanie jest wyłączone, **nie można odnaleźć karty przeglądarki debugowania** strony błędu jest generowany przez przeglądarki Chrome. Strona błędu zawiera instrukcje na temat uruchamiania dla programu Chrome z portem debugowania Otwórz tak, aby serwer proxy debugowania Blazor podłączeniem do aplikacji. *Zamknij wszystkie wystąpienia dla programu Chrome* i ponownie uruchomić przeglądarkę Chrome, zgodnie z instrukcjami.

## <a name="debug-the-app"></a>Debugowanie aplikacji

Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, debugowania skrótu klawiaturowego zostanie otwarta nowa karta debugera. Po chwili **źródeł** karta zawiera listę zestawów .NET w aplikacji. Rozwiń każdy zestaw i Znajdź *.cs*/*.razor* dostępna do debugowania plików źródłowych. Ustawianie punktów przerwania, wrócić do karty aplikacji, a punkty przerwania są osiągane, gdy kod jest wykonywany. Punkt przerwania — po trafienie w jednym kroku (`F10`) za pomocą kodu lub Wznów (`F8`) wykonanie kodu w zwykły sposób.

Blazor udostępnia serwer proxy debugowania, który implementuje [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) i rozszerzają protokołu. Informacje dotyczące sieci. Po naciśnięciu klawisza skrótu klawiaturowego debugowania Blazor wskazuje Chrome DevTools serwera proxy. Serwer proxy łączy do okna przeglądarki, w przypadku znalezienia do debugowania (dlatego trzeba włączyć zdalne debugowanie).

## <a name="browser-source-maps"></a>Mapy źródła przeglądarki

Mapy źródła przeglądarki Zezwalaj na używanie przeglądarki zamapować skompilowane pliki do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta. Jednak nie jest obecnie mapowany Blazor C# bezpośrednio do języka JavaScript/WASM. Zamiast tego Blazor wykonuje interpretacji IL w przeglądarce, więc map źródeł nie są istotne.

## <a name="troubleshooting-tip"></a>Porada dotyczącą rozwiązywania problemów

Jeśli pracujesz w błędy, poniższe porady mogą pomóc:

W **debugera** karcie, Otwórz narzędzia deweloperskie w przeglądarce. W konsoli, należy wykonać `localStorage.clear()` do usunięcia żadnych punktów przerwania.
