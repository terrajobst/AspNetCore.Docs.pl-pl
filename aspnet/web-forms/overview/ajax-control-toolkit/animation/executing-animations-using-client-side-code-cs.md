---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Wykonywanie animacji przy użyciu kodu po stronie klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Wykonywanie animacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b58579a2a3b4849a69337600492e2b1a69175513
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389133"
---
<a name="executing-animations-using-client-side-code-c"></a>Wykonywanie animacji przy użyciu kodu po stronie klienta (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Wykonywanie animacji może być też wywoływane przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Wykonywanie animacji może być też wywoływane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnClick>` do uruchamiania animacji po użytkownik klika przycisk na panel. Dodaj dwa animacji do wykonania parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Celach demonstracyjnych ta animacja (i inne animacje utworzone przy użyciu zestawu narzędzi kontroli) jest wykonywane przy użyciu kodu JavaScript, po uruchomieniu na stronie. Po pierwsze: konieczny jest dostęp do `AnimationExtender` kontroli. Biblioteka rozszerzeń ASP.NET AJAX zapewnia `$find()` funkcji dla tego zadania:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender` Kontroli udostępnia zaawansowane interfejsy API, włączając w to metody z nazwami identyczne do obsługi zdarzeń, używany w znaczników XML: `OnClick()`, `OnLoad()`i tak dalej. Na przykład, wywołanie `OnClick()` metoda jest wykonywana animacji w ramach `<OnClick>` elementu `AnimationExtender` sterowania:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Oto kompletny kod JavaScript po stronie klienta, który symuluje kliknij w panelu po całkowitym załadowaniu strony, należy pamiętać, że `pageLoad()` używana jest nazwa funkcji, które jest wywoływane przez ASP.NET AJAX po stronie, a wszystkie dołączone zostały bibliotek JavaScript załadowane.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Animacja jest uruchamiany natychmiast, bez kliknięcia myszą](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Animacja jest uruchamiany natychmiast, bez kliknięcie myszą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](modifying-animations-from-the-server-side-cs.md)
> [dalej](changing-an-animation-using-client-side-code-cs.md)
