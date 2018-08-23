---
uid: visual-studio/overview/2013/using-browser-link
title: W programie Visual Studio 2013 za pomocą łączność z przeglądarkami | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: f470aa7e425d16aec3f67d2a0ebb664a3e7eac41
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751863"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Za pomocą łącza przeglądarki w programie Visual Studio 2013
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Łącze przeglądarki jest nową funkcją w programie Visual Studio 2013, która tworzy kanał komunikacyjny między środowiska programistycznego i jeden lub więcej przeglądarek sieci web. Można użyć łącze przeglądarki, aby odświeżyć aplikację sieci web w kilku przeglądarkach jednocześnie, która jest przydatna przy testowaniu obsługiwania wielu przeglądarek.

- [Odśwież w przeglądarce](#browser-refresh)
- [Wyświetlanie na pulpicie nawigacyjnym łącza przeglądarki](#dashboard)
- [Włączanie łączność z przeglądarkami plików Statycznych](#static-html)
- [Wyłączanie łączność z przeglądarkami](#disabling)
- [Jak to działa?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Odśwież w przeglądarce

Za pomocą odświeżyć przeglądarkę można odświeżyć wielu przeglądarek, które są podłączone do programu Visual Studio za pośrednictwem łącza przeglądarki.

Aby użyć, Odśwież przeglądarkę, należy najpierw utworzyć aplikację ASP.NET przy użyciu jednego z szablonów projektu. Debugowanie aplikacji, naciskając klawisz F5 lub klikając ikonę strzałki na pasku narzędzi:

![](using-browser-link/_static/image1.png)

Umożliwia także listy rozwijanej do wybierz określonych przeglądarki dla debugowania.

![](using-browser-link/_static/image2.png)

Aby debugować za pomocą różnych przeglądarkach, wybierz **przeglądanie za pomocą**. W **przeglądanie za pomocą** okno dialogowe, przytrzymaj wciśnięty klawisz CTRL, aby wybrać więcej niż jednej przeglądarki. Kliknij przycisk **Przeglądaj** debugowania przy użyciu wybranej przeglądarki. Łączność z przeglądarkami współpracuje również jeśli przeglądarkę z poza programem Visual Studio i przejdź do adresu URL aplikacji.

![](using-browser-link/_static/image3.png)

Formanty łączność z przeglądarkami znajdują się na liście rozwijanej ikoną strzałkę kolistą. Ikona strzałki jest **Odśwież** przycisku.

![](using-browser-link/_static/image4.png)

Aby zobaczyć, jakie przeglądarki są połączone, umieść kursor myszy nad **Odśwież** przycisk podczas debugowania. Połączonych przeglądarek są wyświetlane w oknie etykietki narzędzia.

![](using-browser-link/_static/image5.png)

Aby odświeżyć połączonych przeglądarek, kliknij **Odśwież** przycisk lub naciśnij klawisze CTRL + ALT + ENTER. Na przykład poniższy zrzut ekranu przedstawia projektu ASP.NET, który został utworzony przy użyciu szablonu projektu MVC 5. Możesz zobaczyć aplikację uruchomioną w dwóch przeglądarek u góry. Na dole projekt jest otwarty w programie Visual Studio.

![](using-browser-link/_static/image6.png)

W programie Visual Studio po zmianie &lt;h1&gt; nagłówek strony głównej:

![](using-browser-link/_static/image7.png)

Po kliknięciu **Odśwież** przycisk, zmiana pojawiła się oba okna przeglądarki:

![](using-browser-link/_static/image8.png)

**Uwagi**

- Aby włączyć łączność z przeglądarkami, ustaw `debug=true` w [ &lt;kompilacji&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elementu w pliku Web.config projektu.
- Aplikacja musi działać na hoście lokalnym.
- Aplikacja musi być przeznaczony dla platformy .NET 4.0 lub nowszy.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Wyświetlanie na pulpicie nawigacyjnym łącza przeglądarki

Pulpit nawigacyjny Browser Link zawiera informacje o połączeniach łączność z przeglądarkami. Aby wyświetlić pulpit nawigacyjny, wybierz menu rozwijane łączność z przeglądarkami (małą strzałkę obok pozycji **Odśwież** przycisk). Następnie kliknij przycisk **nawigacyjnym łącza przeglądarki**.

![](using-browser-link/_static/image9.png)

Pulpit nawigacyjny zawiera listę połączonych przeglądarek i adres URL, do którego nawigację każdą przeglądarkę.

![](using-browser-link/_static/image10.png)

**Wymagania wstępne** sekcja pokazuje wszystkie kroki wymagane do włączenia łączność z przeglądarkami dla tego projektu. Na przykład poniższy zrzut ekranu przedstawia projekt gdzie "debug" jest ustawiona na wartość false w pliku Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Włączanie łączność z przeglądarkami plików Statycznych

Aby włączyć łączność z przeglądarkami na statyczne pliki HTML, należy dodać następujące do pliku Web.config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Ze względu na wydajność należy usunąć ustawienie podczas publikowania projektu.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Wyłączanie łączność z przeglądarkami

Łącze przeglądarki jest domyślnie włączona. Istnieje kilka sposobów, aby ją wyłączyć:

- W menu rozwijanym łączność z przeglądarkami, usuń zaznaczenie pola wyboru **Włącz łącze przeglądarki**. 

    ![](using-browser-link/_static/image12.png)
- W pliku Web.config Dodaj klucz o nazwie "vs: EnableBrowserLink" na wartość "false" w sekcji appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- W pliku Web.config należy ustawić debugowania na wartość false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Jak to działa?

Łączność z przeglądarkami używa [SignalR](../../../signalr/index.md) próba utworzenia kanału komunikacji między Visual Studio i przeglądarki. Po włączeniu łączność z przeglądarkami programu Visual Studio działa jako serwer biblioteki SignalR, wielu klientów (przeglądarki) można łączyć się. Łączność z przeglądarkami rejestruje także moduł HTTP za pomocą platformy ASP.NET. Ten moduł wprowadza specjalne &lt;skryptu&gt; odwołań w każdym żądaniu strony z serwera. Odwołania do skryptu można wyświetlić, wybierając pozycję "Wyświetl źródło" w przeglądarce.

![](using-browser-link/_static/image13.png)

Plików źródłowych nie są modyfikowane. Moduł HTTP wprowadza odwołania do skryptu dynamicznie.

Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa we wszystkich przeglądarkach, [obsługuje SignalR](../../../signalr/overview/getting-started/supported-platforms.md), bez konieczności wtyczkę przeglądarki.
