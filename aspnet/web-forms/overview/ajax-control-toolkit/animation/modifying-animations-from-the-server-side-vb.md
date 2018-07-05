---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modyfikowanie animacji po stronie serwera (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacje mogą również...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: d39a40f2776d5b87bf82d1a6c6282ce920f4a907
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401957"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Modyfikowanie animacji po stronie serwera (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Może także ulec animacji po stronie serwera


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Może także ulec animacji po stronie serwera

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Pozostała część kodu jest uruchamiany po stronie serwera i nie używa znaczników; Zamiast tego używa kodu do tworzenia `AnimationExtender` sterowania:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Jednak zestaw narzędzi kontroli aktualnie nie zapewnia dostęp do interfejsu API do tworzenia indywidualnych animacji. Jednak jest możliwe ustawienie `AnimationExtender`właściwości animacji z ciągiem zawierającym znaczników XML używane podczas przypisywania animacji w sposób deklaratywny. Aby można było utworzyć plik XML, który nie może zawierać `<Animations>` elementu, można użyć XML .NET Framework obsługuje lub zgodnie z poniższym kodem, po prostu podać ciąg:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Na koniec należy dodać `AnimationExtender` sterowania do bieżącej strony, w ramach `<form runat="server">` elementu, upewniając się, że animacja jest dołączony i uruchamia:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![Animacja zostanie utworzony przy użyciu języka C# /VB kod na serwerze](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Animacja zostanie utworzony przy użyciu języka C# /VB kod na serwerze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](triggering-an-animation-in-another-control-vb.md)
> [dalej](executing-animations-using-client-side-code-vb.md)
