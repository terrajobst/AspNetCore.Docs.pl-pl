---
uid: single-page-application/overview/templates/hottowel-template
title: Szablon ręczników gorących | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875022"
---
<a name="hot-towel-template"></a>Hot ręczników szablonu
====================
przez [Mads Kristensen](https://github.com/madskristensen)

> Gorących szablonu MVC ręczników została napisana przez Papa Jan
> 
> Należy wybrać wersję, która do pobrania:
> 
> [Szablon MVC ręczników dostępu dla programu Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Szablon MVC gorących ręczników dla programu Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Gorących ręczników: Ponieważ nie chcesz przejść do SPA bez!


Chcesz tworzyć SPA, ale nie można zdecydować, jak zacząć? Użyj gorących ręczników i w sekundach będziesz mieć SPA i wszystkie narzędzia potrzebne do utworzenia na nim!

Gorących ręczników tworzy doskonały punkt początkowy do tworzenia jednej strony aplikacji JEDNOSTRONICOWEJ ASP.NET. Poza pole użytkownik udostępnia modularna struktura do kodu, nawigacji widoku, powiązania danych, zarządzanie danymi sformatowanego i style proste, ale elegancki. Gorących ręczników zawiera wszystko, co jest potrzebne do tworzenia SPA, dzięki czemu można skoncentrować się na aplikacji, nie żmudne procesy.

> Dowiedz się więcej o tworzeniu SPA z [Papa Jan wideo, samouczki i szkolenia Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Struktura aplikacji

Gorących SPA ręczników zawiera folder aplikacji zawierającej pliki JavaScript i HTML, które definiują aplikację.

W folderze aplikacji:

- Durandal
- usługi
- viewmodels
- widoki

Folder aplikacji zawiera zbiór modułów. Te moduły Hermetyzowanie funkcjonalność i zadeklarować zależności w innych modułach. Folder widoków zawiera kod HTML dla aplikacji i viewmodels folder zawiera logikę prezentacji widoków (wspólnego wzorca MVVM). Folder usługi jest idealne rozwiązanie w przypadku których wspólne usługi, które mogą wymagać aplikacji, takie jak pobieranie danych protokołu HTTP lub interakcji lokalnej pamięci masowej. Jest typowe dla wielu viewmodels do ponownego użycia kodu z modułów usługi.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struktura aplikacji po stronie serwera programu ASP.NET MVC

Gorących ręczników opiera się na znanych i wydajnych struktury ASP.NET MVC.

- Aplikacja\_Start
- Zawartość
- Kontrolery
- Modele
- Skrypty
- Widoki

## <a name="featured-libraries"></a>Promowanie bibliotek

- ASP.NET MVC
- ASP.NET Web API
- Optymalizacja sieci Web platformy ASP.NET — tworzenie pakietów i minimalizowanie
- [BREEZE.js](http://Breezejs.com) -zaawansowanych danych zarządzania
- [Durandal.js](http://Durandaljs.com) -nawigacji i kompozycji widoku
- [Knockout.js](http://Knockoutjs.com) -powiązania danych
- [Require.js](http://requirejs.org) -Modułowość AMD i optymalizacji
- [Toastr.js](http://jpapa.me/c7toastr) -komunikatów podręcznych
- [W usłudze Twitter Bootstrap](http://twitter.github.com/bootstrap/) — niezawodny CSS Ustawianie stylów

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalowanie przy użyciu szablonu SPA gorących ręczników programu Visual Studio 2012

Ręczników dynamicznej można zainstalować jako szablon programu Visual Studio 2012. Po prostu kliknij `File`  |  `New Project` i wybierz polecenie `ASP.NET MVC 4 Web Application`. Następnie wybierz pozycję "gorących ręczników jednej strony aplikacji" szablonu i uruchom!

## <a name="installing-via-the-nuget-package"></a>Instalowanie za pośrednictwem pakietu NuGet

Gorących ręczników jest również pakietu NuGet, który wspomaga istniejącego pustego projektu platformy ASP.NET MVC. Po prostu zainstaluj przy użyciu narzędzia Nuget, a następnie uruchom!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Jak zbudować na gorąco ręczników?

Wystarczy uruchomić Dodawanie kodu!

1. Dodaj własny kod po stronie serwera, najlepiej Entity Framework i WebAPI (które są naprawdę widoczne z Breeze.js)
2. Dodawanie widoków `App/views` folderu
3. Dodaj viewmodels do `App/viewmodels` folderu
4. Dodawanie nowych widoków HTML i odcinania powiązania danych
5. Aktualizowanie tras nawigacji w `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Wskazówki HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml jest początkowy trasy i widoku dla aplikacji MVC. Zawiera wszystkie standardowe tagi meta, łącza css i JavaScript odwołania, których można oczekiwać. Treść zawiera jeden `<div>` czyli gdzie całej zawartości (widoki) zostaną umieszczone w przypadku żądania. `@Scripts.Render` Używa Require.js do uruchomienia punktu wejścia dla kodu aplikacji, która jest zawarta w `main.js` pliku. Ekran powitalny zapewnia pokazują, jak utworzyć ekranu powitalnego aplikacji ładowane.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js` Plik zawiera kod, który będzie uruchamiany natychmiast po załadowaniu aplikacji. Jest to, które chcesz zdefiniować trasy nawigacji, ustaw uruchamiania się widoki i wykonać wszelkie ustawienia/uruchamianie takich jak priming danych aplikacji.

`main.js` Plik definiuje kilka modułów w durandal ułatwiające rozpoczęcie aplikacji start. Instrukcja Definiuj pomaga rozpoznać zależności modułów, aby były dostępne dla tej funkcji. Najpierw komunikaty debugowania są włączone, który wysyła komunikaty o zdarzeniach, jakie się, że aplikacja działa w oknie konsoli. Kod app.start informuje durandal framework do uruchomienia aplikacji. Konwencje są ustawiane tak, aby durandal zna wszystkie widoki i viewmodels są zawarte w tych samych folderach nazwanego, odpowiednio. Na koniec `app.setRoot` uruchamiane obciążenia `shell` przy użyciu wstępnie zdefiniowanej `entrance` animacji.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Widoki

Widoki znajdują się w `App/views` folderu.

### <a name="shellhtml"></a>shell.html

`shell.html` Zawiera głównego układu dla kodzie HTML. Wszystkie inne widoków będzie składać się gdzieś po stronie użytkownika `shell` widoku. Udostępnia ręczników gorących `shell` z trzech takich regionów: nagłówek, obszar zawartości i stopki. Każdy z tych obszarów z załadowanymi zawartość formularza innych widoków na żądanie.

`compose` Powiązania nagłówku i stopce występują ustalone w dynamicznej ręczników wskaż `nav` i `footer` widoków odpowiednio. Powiązanie compose dla sekcji `#content` jest powiązany z `router` aktywny element modułu. Innymi słowy po kliknięciu łącza nawigacji jest odpowiedni widok został załadowany w tym obszarze.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` Zawiera łącza nawigacji, aby aplikacja JEDNOSTRONICOWA. Jest to struktura menu rozmieszczenia, np. Często jest to danymi powiązanymi (przy użyciu odcinania) do `router` modułu, aby wyświetlić nawigacji zdefiniowane w `shell.js`. Odcinania szuka wiązania danych atrybutów i wiąże tych `shell` viewmodel w celu wyświetlenia trasy nawigacji i wyświetlania elementu progressbar (przy użyciu Twitter Bootstrap) Jeśli `router` modułu jest w trakcie przechodzenia z jednego widoku do innego (patrz `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML i details.html

Widoki te zawierają HTML, niestandardowe widoki. Gdy `home` łącze w `nav` kliknięciu menu widoku `home` widoku zostaną umieszczone w obszarze zawartości `shell` widoku. Widoki te można rozszerzony lub zastąpione własnych widoków niestandardowych.

### <a name="footerhtml"></a>footer.html

`footer.html` Zawiera kod HTML, który jest wyświetlany w stopce strony, u dołu `shell` widoku.

## <a name="viewmodels"></a>ViewModels

Znaleziono ViewModels `App/viewmodels` folderu.

### <a name="shelljs"></a>shell.js

`shell` Viewmodel zawiera właściwości i funkcje, które są powiązane z `shell` widoku. Często jest to, gdzie znajdują się powiązania z menu nawigacji (zobacz `router.mapNav` logiki).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js i details.js

Te viewmodels zawierają właściwości i funkcje, które są powiązane z `home` widoku. on również zawiera logikę prezentacji dla widoku i jest sklejki między danymi i widoku.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Usługi

Usługi znajdują się w folderze aplikacji i usług. W idealnym przypadku przyszłych usług takich jak moduł dataservice, która jest odpowiedzialna za uzyskanie i przesyłanie danych zdalnych, może zostać umieszczona.

### <a name="loggerjs"></a>logger.js

Udostępnia ręczników gorących `logger` modułu w folderze usługi. `logger` Modułu jest idealny dla rejestrowanie komunikatów do konsoli i użytkownikowi w wyświetlonym wyskakujące powiadomienia.
