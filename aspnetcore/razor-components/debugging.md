---
title: Debugowanie składniki Razor
author: guardrex
description: Dowiedz się, jak można debugować aplikacje Blazor i składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668127"
---
# <a name="debug-razor-components"></a>Debugowanie składniki Razor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Razor składników ma kilka *wczesnym* obsługę debugowania po stronie klienta Blazor aplikacji uruchomionych na format WebAssembly w przeglądarce Chrome. W trakcie bardzo ograniczona i unpolished tej początkowej obsługi debugowania przedstawia podstawową infrastrukturą debugowania dostarczane razem.

Aby debugować aplikację Blazor po stronie klienta w przeglądarce Chrome:

* Tworzenie aplikacji Blazor w `Debug` konfiguracji (domyślnie dla nieopublikowane aplikacje).
* Uruchom aplikację Blazor w przeglądarce Chrome (wersja 70 lub nowszej).
* Fokus klawiatury w aplikacji (nie w panelu Narzędzia deweloperskie, co prawdopodobnie należy zamknąć mniej mylące środowisko debugowania) wybierz następujący skrót klawiaturowy specyficzne dla Blazor:
  * `Shift+Alt+D` w systemie Windows/Linux
  * `Shift+Cmd+D` W systemie macOS

Uruchom dla programu Chrome przy użyciu zdalnego debugowania, włączone, aby debugować aplikację Blazor. Jeśli zdalne debugowanie jest wyłączone, stronę błędu jest generowany przez przeglądarki Chrome. Strona błędu zawiera instrukcje na temat uruchamiania dla programu Chrome z portem debugowania Otwórz tak, aby serwer proxy debugowania Blazor podłączeniem do aplikacji. *Zamknij wszystkie wystąpienia dla programu Chrome* i ponownie uruchomić przeglądarkę Chrome, zgodnie z instrukcjami.

![Blazor debugowania stronę błędu](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, debugowania skrótu klawiaturowego zostanie otwarta nowa karta debugera. Po chwili *źródeł* karta zawiera listę zestawów .NET w aplikacji. Rozwiń każdy zestaw i Znajdź *.cs*/*.cshtml* dostępna do debugowania plików źródłowych. Ustawianie punktów przerwania, wrócić do karty aplikacji, a punkty przerwania są osiągane. Punkt przerwania — po trafienie w jednym kroku (`F10`) lub wznowić (`F8`) normalnie.

![Debugowanie Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor udostępnia serwer proxy debugowania, który implementuje [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) i rozszerzają protokołu. Informacje dotyczące sieci. Po naciśnięciu klawisza skrótu klawiaturowego debugowania Blazor wskazuje Chrome DevTools serwera proxy. Serwer proxy łączy do okna przeglądarki, w przypadku znalezienia do debugowania (dlatego trzeba włączyć zdalne debugowanie).

Możesz może zastanawiać, dlaczego firma Microsoft nie wystarczy użyć mapy źródła przeglądarki. Map źródeł Zezwalaj na przeglądarkę, aby zamapować skompilowane pliki do ich oryginalnych plików źródłowych. Jednak nie jest mapowany Blazor C# bezpośrednio do JS/WASM (co najmniej nie została jeszcze). Zamiast tego Blazor wykonuje interpretacji IL w przeglądarce, więc map źródeł nie są istotne.

Należy zauważyć, że funkcje debugera są **bardzo ograniczona**. Obecnie można tylko:

* Ustaw i usuwanie punktów przerwania.
* Pojedynczy krok za pomocą kodu lub Wznów (`F8`).
* W *lokalne* wyświetlane, sprawdź wartości wszelkich zmiennych lokalnych typu `int`, `string`, i `bool`.
* Zobaczyć stos wywołań, w tym łańcuchy wywołania, które bardziej szczegółowo w kodzie JavaScript z języka JavaScript w środowisku .NET i .NET.

Możesz *nie*:

* Sprawdź wartości wszelkich zmiennych lokalnych, które nie są `int`, `string`, lub `bool`.
* Sprawdź wartości właściwości klasy lub pola.
* Umieść kursor nad zmienne, aby zobaczyć ich wartości
* Ocena wyrażeń w konsoli.
* Przejść przez wywołania asynchronicznego.
* Wykonywać większości innych zwykłych scenariuszy debugowania.

Rozwój dalszego debugowania scenariusze jest w toku celem zespołu inżynieryjnego.

## <a name="troubleshooting-tip"></a>Porada dotyczącą rozwiązywania problemów

Jeśli pracujesz w błędy, poniższe porady mogą pomóc:

W **debugera** karcie, Otwórz narzędzia deweloperskie w przeglądarce. W konsoli, należy wykonać `localStorage.clear()` do usunięcia żadnych punktów przerwania.
