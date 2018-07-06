---
uid: single-page-application/overview/templates/hottowel-template
title: Hot Towel szablonu | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon HotTowel
ms.author: aspnetcontent
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: 6145a34286ef113002b92d722e227005657098d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825140"
---
<a name="hot-towel-template"></a>Szablon hot Towel
====================
przez [: Mads Kristensen](https://github.com/madskristensen)

> Szablon MVC Hot Towel została zapisana przy John Papa
> 
> Wybierz wersję, która do pobrania:
> 
> [Szablon MVC hot Towel dla programu Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Szablon MVC hot Towel dla programu Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel: Ponieważ nie chcesz przejść do SPA bez ani jednego!


Chcesz tworzyć SPA, ale nie można zdecydować, gdzie zacząć? Użyj Hot Towel i w ciągu kilku sekund będziesz mieć SPA i wszystkie narzędzia potrzebne do tworzenia na nim!

Hot Towel tworzy doskonałe punkt początkowy do tworzenia pojedynczej strony aplikacji (SPA) za pomocą platformy ASP.NET. Gotowych użytkownik udostępnia modularna struktura do kodu, nawigacji w widoku, powiązanie danych, zarządzania zaawansowanych danych i style proste, ale elegancki. Hot Towel zawiera wszystko, czego potrzebujesz do tworzenia SPA, dzięki czemu możesz skupić się na aplikacji, a nie podstawami.

> Dowiedz się więcej o tworzeniu SPA z [filmów wideo i samouczki John Papa, a także kursów Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Struktury aplikacji

Hot Towel SPA zawiera folder aplikacji, który zawiera pliki JavaScript i HTML, które definiują aplikację.

Znajdujące się w folderze aplikacji:

- Durandal
- usługi
- modele widoków
- widoki

Folder aplikacji zawiera zbiór modułów. Te moduły zapewniają funkcjonalność hermetyzacji i zadeklarować zależności w innych modułach. Folder widoków zawiera kod HTML dla aplikacji i modele widoków folder zawiera logikę prezentacji widoki (wspólny wzorzec MVVM). Folder usługi jest idealny dla obudowie wspólne usługi, których aplikacja może być konieczne takie jak pobieranie danych protokołu HTTP lub interakcji z magazynu lokalnego. Bardzo często modele wielu widoków do ponownego użycia kodu z modułów usługi.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struktury aplikacji po stronie serwera platformy ASP.NET MVC

Hot Towel opiera się na znanych i wydajnych struktury ASP.NET MVC.

- Aplikacja\_Start
- Zawartość
- Kontrolery
- Modele
- Skrypty
- Widoki

## <a name="featured-libraries"></a>Polecane bibliotek

- ASP.NET MVC
- ASP.NET Web API
- Optymalizacja sieci Web ASP.NET - tworzenie pakietów i minimalizowanie
- [BREEZE.js](http://Breezejs.com) — zarządzanie danymi zaawansowane
- [Durandal.js](http://Durandaljs.com) -nawigacji i widoku kompozycji
- [Struktura Knockout.js](http://Knockoutjs.com) -powiązania danych
- [Require.js](http://requirejs.org) -Modułowość AMD i optymalizacja
- [Toastr.js](http://jpapa.me/c7toastr) -komunikatów podręcznych
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) — niezawodne CSS style

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalowanie za pośrednictwem SPA szablon Hot Towel programu Visual Studio 2012

Hot Towel można zainstalować jako szablon programu Visual Studio 2012. Po prostu kliknij `File`  |  `New Project` i wybierz polecenie `ASP.NET MVC 4 Web Application`. Następnie wybierz pozycję "Hot Towel aplikacji jednostronicowej" szablon i uruchom!

## <a name="installing-via-the-nuget-package"></a>Instalowanie za pośrednictwem pakietu NuGet

Hot Towel jest również pakiet NuGet, który rozszerza istniejący pusty projekt platformy ASP.NET MVC. Po prostu zainstaluj za pomocą narzędzia Nuget, a następnie uruchom!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Jak tworzyć na Hot Towel

Po prostu Rozpocznij, dodając kod!

1. Dodaj własny kod po stronie serwera, najlepiej Entity Framework i WebAPI (które tak naprawdę przykuć uwagę przy użyciu Breeze.js)
2. Dodaj widoki `App/views` folderu
3. Modele widoków, aby dodać `App/viewmodels` folderu
4. Dodawanie powiązania danych HTML i Knockout do nowych widoków
5. Aktualizowanie tras nawigacji w `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Przewodnik po języku HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml jest początkowy trasy i widok dla aplikacji MVC. Zawiera on wszystkich standardowych tagów meta, łączy css i JavaScript odwołań, czego można oczekiwać. Treść zawiera pojedynczy `<div>` czyli, gdzie całej zawartości (widoki) zostaną umieszczone w przypadku żądania. `@Scripts.Render` Używa Require.js do uruchomienia punktu wejścia dla kodu aplikacji, która jest zawarta w `main.js` pliku. Ekran powitalny jest udostępniane na pokazują, jak utworzyć ekran powitalny, podczas ładowania aplikacji.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js` Plik zawiera kod, który będzie uruchamiany natychmiast po załadowaniu aplikacji. Jest to, które chcesz zdefiniować trasy nawigacji, Ustaw użytkownika uruchamiania widoków i wykonać wszelkie ustawienia/uruchamianie takich jak zalewanie danych aplikacji.

`main.js` Plik definiuje kilka modułów durandal firmy, aby ułatwić rozpoczęcie aplikacji start. Zdefiniuj instrukcję pomaga rozpoznać zależności modułów, aby były dostępne dla tej funkcji. Najpierw komunikaty debugowania są włączone, który wysyłać wiadomości o jakie zdarzenia, że aplikacja działa w oknie konsoli. Kod app.start informuje durandal framework, aby uruchomić aplikację. Konwencje są ustawione tak, aby durandal zna wszystkie widoki i modele widoków są zawarte w tych samych folderach nazwanych, odpowiednio. Na koniec `app.setRoot` uruchamiane obciążenia `shell` przy użyciu wstępnie zdefiniowanej `entrance` animacji.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Widoki

Widoki znajdują się w `App/views` folderu.

### <a name="shellhtml"></a>shell.html

`shell.html` Zawiera układem głównym na kodzie HTML. Wszystkie inne widoków będzie składać się gdzieś po stronie usługi `shell` widoku. Udostępnia hot Towel `shell` z trzech takich regionów: nagłówek, obszarem zawartości i stopkę. Każda z tych regionów jest załadowana zawartość formularza inne widoki, gdy wymagane.

`compose` Powiązania w nagłówku i stopce występują ustalone w Hot Towel, aby wskazywał `nav` i `footer` widoków, odpowiednio. Powiązanie compose sekcji `#content` jest powiązany z `router` aktywny element modułu. Innymi słowy po kliknięciu łącza nawigacji jest odpowiedni widok jest ładowany w tym obszarze.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` Zawiera łącza nawigacji dla SPA. Jest to, gdzie struktury menu mogą być umieszczane, na przykład. Często jest to dane powiązane (przy użyciu Knockout) do `router` modułu, aby wyświetlić nawigacji zdefiniowane w `shell.js`. Knockout wyszukuje wiązania danych atrybutów, a także tych, które mają powiązania `shell` viewmodel w celu wyświetlania tras nawigacji i wyświetlania elementu progressbar (przy użyciu usługi Twitter Bootstrap) Jeśli `router` modułu jest w trakcie przechodzenia z jednego widoku do innego (patrz `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML i details.html

Widoki te zawierają HTML do widoków niestandardowych. Gdy `home` łącze w `nav` kliknięciu menu widoku `home` widoku zostaną umieszczone w obszarze zawartości `shell` widoku. Widoki te mogą rozszerzone lub zastąpić widoki niestandardowe.

### <a name="footerhtml"></a>footer.html

`footer.html` Zawiera kod HTML, który pojawia się w stopce w dolnej części `shell` widoku.

## <a name="viewmodels"></a>Modele widoków

Modele widoków znajdują się w `App/viewmodels` folderu.

### <a name="shelljs"></a>shell.js

`shell` Viewmodel zawiera właściwości i funkcje, które są powiązane z `shell` widoku. Często jest to, gdzie znajdują się menu nawigacji powiązania (zobacz `router.mapNav` logiki).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js i details.js

Te modele widoków zawierają właściwości i funkcje, które są powiązane z `home` widoku. on również zawiera logikę prezentacji dla widoku i jest pośredniczącego między danymi a widoku.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Usługi

Usługi znajdują się w folderze aplikacji/usługi. Najlepiej przyszłych usług takich jak moduł usługi danych, który jest odpowiedzialny za uzyskanie i publikowanie danych zdalnych, można umieścić.

### <a name="loggerjs"></a>logger.js

Udostępnia hot Towel `logger` modułu w folderze usługi. `logger` Modułu jest idealny dla rejestrowanie komunikatów do konsoli i użytkownika w menu podręcznym wyskakujące powiadomienia.
