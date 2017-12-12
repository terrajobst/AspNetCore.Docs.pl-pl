---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "Wykonywanie animacji przy użyciu kodu po stronie klienta (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a>Wykonywanie animacji przy użyciu kodu po stronie klienta (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji również mogą być wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji również mogą być wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnClick>` jednorazowego uruchomienia animacji użytkownik kliknie na panelu. Dodaj dwa animacji można wykonać parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Dla celów demonstracyjnych to animacji (i innych animacji utworzone przy użyciu zestawu narzędzi kontroli) jest wykonywana przy użyciu kodu JavaScript, po uruchomieniu strony. Przede wszystkim należy dostęp do `AnimationExtender` formantu. Biblioteka ASP.NET AJAX zapewnia `$find()` funkcji dla tego zadania:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender` Kontroli przedstawia sformatowanego interfejsu API, tym metody z nazwami identyczne z obsługi zdarzeń, używany w znaczniku XML: `OnClick()`, `OnLoad()`i tak dalej. Na przykład wywołanie elementu `OnClick()` metoda wykonuje animacji w `<OnClick>` elementu `AnimationExtender` sterowania:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

W tym miejscu jest kompletny kod JavaScript po stronie klienta, która emuluje kliknij na panelu po całkowitym załadowaniem strony, należy pamiętać, że `pageLoad()` używana jest nazwa funkcji, która jest wywoływana przez ASP.NET AJAX po stronie i wszystkie zawarte zostały biblioteki JavaScript załadowane.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-animations-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](modifying-animations-from-the-server-side-vb.md)
[dalej](changing-an-animation-using-client-side-code-vb.md)
