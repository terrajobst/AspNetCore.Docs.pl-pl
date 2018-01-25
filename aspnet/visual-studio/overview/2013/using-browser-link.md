---
uid: visual-studio/overview/2013/using-browser-link
title: "Przy użyciu Browser Link w programie Visual Studio 2013 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-browser-link-in-visual-studio-2013"></a>Przy użyciu Browser Link w programie Visual Studio 2013
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Łącze przeglądarki jest nową funkcją w programie Visual Studio 2013, tworzącym kanał komunikacji między Środowisko deweloperskie i co najmniej jeden przeglądarki sieci web. Można użyć łącze przeglądarki, aby odświeżyć aplikacji sieci web w wielu przeglądarkach jednocześnie, która jest przydatna przy testowaniu różnych przeglądarkach.

- [Odśwież przeglądarkę](#browser-refresh)
- [Wyświetlanie pulpicie nawigacyjnym łącza przeglądarki](#dashboard)
- [Włączanie łącze przeglądarki plików Statycznych](#static-html)
- [Wyłączanie łącza przeglądarki](#disabling)
- [Jak to działa?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Odśwież przeglądarkę

Z Odśwież przeglądarkę można odświeżyć wiele przeglądarek, które są połączone z programu Visual Studio za pośrednictwem łącza przeglądarki.

Aby użyć Odśwież przeglądarkę, należy najpierw utworzyć aplikacji ASP.NET przy użyciu tych szablonów projektu. Debugowanie aplikacji, naciskając klawisz F5 lub klikając ikonę strzałki na pasku narzędzi:

![](using-browser-link/_static/image1.png)

Można też użyć listy rozwijanej do wybrania określonej przeglądarki do debugowania.

![](using-browser-link/_static/image2.png)

Aby debugować z różnych przeglądarkach, wybierz **przeglądanie za pomocą**. W **przeglądanie za pomocą** okna dialogowego, przytrzymaj klawisz CTRL, aby wybrać więcej niż jednej przeglądarki. Kliknij przycisk **Przeglądaj** do debugowania z wybranej przeglądarki. Łącze przeglądarki również działa, jeśli przeglądarki z zewnętrznego programu Visual Studio i przejdź do adresu URL aplikacji.

![](using-browser-link/_static/image3.png)

Formanty łączy przeglądarki znajdują się w ikoną cykliczne strzałkę listy rozwijanej. Ikona strzałki jest **Odśwież** przycisku.

![](using-browser-link/_static/image4.png)

Aby zobaczyć, które przeglądarki są połączone, umieść kursor myszy nad **Odśwież** przycisk podczas debugowania. Podłączonych przeglądarek są wyświetlane w oknie etykietka narzędzia.

![](using-browser-link/_static/image5.png)

Aby odświeżyć podłączonych przeglądarek, kliknij przycisk **Odśwież** przycisk lub naciśnij klawisze CTRL + ALT + ENTER. Na przykład poniższy zrzut ekranu przedstawia projektu ASP.NET, który został utworzony za pomocą szablonu projektu MVC 5. Widać aplikacji działający w przeglądarkach dwóch u góry. Na dole projekt jest otwarty w programie Visual Studio.

![](using-browser-link/_static/image6.png)

W programie Visual Studio po zmianie &lt;h1&gt; nagłówek strony głównej:

![](using-browser-link/_static/image7.png)

Po kliknięciu **Odśwież** przycisk zmiany znajdowały się w oba okna przeglądarki:

![](using-browser-link/_static/image8.png)

**Uwagi**

- Aby włączyć łącze przeglądarki, ustaw `debug=true` w [ &lt;kompilacji&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elementu w pliku Web.config dla projektu.
- Aplikacja musi być uruchomiona na hoście lokalnym.
- Aplikacja musi wskazywać .NET 4.0 lub nowszy.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Wyświetlanie pulpicie nawigacyjnym łącza przeglądarki

Na pulpicie nawigacyjnym łącza przeglądarki zawiera informacje dotyczące połączeń łącze przeglądarki. Aby wyświetlić pulpit nawigacyjny, wybierz menu rozwijane łącze przeglądarki (małą strzałkę obok **Odśwież** przycisku). Następnie kliknij przycisk **nawigacyjnym łącza przeglądarki**.

![](using-browser-link/_static/image9.png)

Pulpit nawigacyjny zawiera listę podłączonych przeglądarek i adres URL, do którego nawigację każdą przeglądarkę.

![](using-browser-link/_static/image10.png)

**Wymagania wstępne** sekcji przedstawiono wszystkie czynności, aby włączyć łącze przeglądarki dla tego projektu. Na przykład poniższy zrzut ekranu przedstawia projekt gdzie "debug" ma wartość false w pliku Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Włączanie łącze przeglądarki plików Statycznych

Aby włączyć łącze przeglądarki dla statyczne pliki HTML, należy dodać następujące do pliku Web.config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Ze względu na wydajność Usuń to ustawienie podczas publikowania projektu.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Wyłączanie łącza przeglądarki

Łącze przeglądarki jest domyślnie włączona. Istnieje kilka sposobów, aby ją wyłączyć:

- W menu rozwijanym łącze przeglądarki, usuń zaznaczenie pola wyboru **Włącz łącze przeglądarki**. 

    ![](using-browser-link/_static/image12.png)
- W pliku Web.config Dodaj klucz o nazwie "vs: EnableBrowserLink" o wartości "false" w sekcji appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- W pliku Web.config należy ustawić debugowania na wartość false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Jak to działa?

Łącze przeglądarki używa [SignalR](../../../signalr/index.md) próba utworzenia kanału komunikacji między Visual Studio i przeglądarki. Po włączeniu łącze przeglądarki, programu Visual Studio działa jako wielu klientów (przeglądarki) mogą łączyć się z serwerem SignalR. Łącze przeglądarki rejestruje również moduł protokołu HTTP z programem ASP.NET. Ten moduł injects specjalne &lt;skryptu&gt; odwołań w każdym żądaniu strony z serwera. Zobacz z odwołań do skryptów, wybierając polecenie "Wyświetl źródło" w przeglądarce.

![](using-browser-link/_static/image13.png)

Plików źródłowych nie są modyfikowane. Moduł HTTP dynamicznie injects odwołań do skryptów.

Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa na wszystkie przeglądarki który [SignalR obsługuje](../../../signalr/overview/getting-started/supported-platforms.md), bez konieczności wtyczki przeglądarki.
