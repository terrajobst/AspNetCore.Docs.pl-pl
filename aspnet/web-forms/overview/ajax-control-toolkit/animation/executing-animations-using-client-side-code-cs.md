---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Wykonywanie animacji przy użyciu kodu po stronie klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870742"
---
<a name="executing-animations-using-client-side-code-c"></a>Wykonywanie animacji przy użyciu kodu po stronie klienta (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji również mogą być wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji również mogą być wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnClick>` jednorazowego uruchomienia animacji użytkownik kliknie na panelu. Dodaj dwa animacji można wykonać parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Dla celów demonstracyjnych to animacji (i innych animacji utworzone przy użyciu zestawu narzędzi kontroli) jest wykonywana przy użyciu kodu JavaScript, po uruchomieniu strony. Przede wszystkim należy dostęp do `AnimationExtender` formantu. Biblioteka ASP.NET AJAX zapewnia `$find()` funkcji dla tego zadania:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender` Kontroli przedstawia sformatowanego interfejsu API, tym metody z nazwami identyczne z obsługi zdarzeń, używany w znaczniku XML: `OnClick()`, `OnLoad()`i tak dalej. Na przykład wywołanie elementu `OnClick()` metoda wykonuje animacji w `<OnClick>` elementu `AnimationExtender` sterowania:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

W tym miejscu jest kompletny kod JavaScript po stronie klienta, która emuluje kliknij na panelu po całkowitym załadowaniem strony, należy pamiętać, że `pageLoad()` używana jest nazwa funkcji, która jest wywoływana przez ASP.NET AJAX po stronie i wszystkie zawarte zostały biblioteki JavaScript załadowane.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](modifying-animations-from-the-server-side-cs.md)
> [dalej](changing-an-animation-using-client-side-code-cs.md)
