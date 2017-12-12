---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: "Obsługa ogłaszania zwrotnego przez formant okna podręcznego o UpdatePanel (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Szczególną uwagę należy zwrócić..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 4445437f25925429d240b7fe2d019855afc52fe0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Obsługa ogłaszania zwrotnego przez formant okna podręcznego o UpdatePanel (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Szczególną uwagę brane odświeżania strony występuje w menu podręczne.


## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Szczególną uwagę brane odświeżania strony występuje w menu podręczne.

## <a name="steps"></a>Kroki

Korzystając z `PopupControl` z odświeżenie strony, `UpdatePanel` może uniemożliwić odświeżania strony spowodowane ogłaszania zwrotnego. Następujący kod definiuje kilka istotnych elementów:

- A `ScriptManager` , aby działa w zestawie narzędzi programu ASP.NET AJAX formantu
- Dwa `TextBox` formantów, które wyzwoli oba okna podręcznego
- A `Panel` formant, który będzie służyć jako menu podręcznego.
- W panelu `Calendar` kontroli jest osadzony w `UpdatePanel` formantu
- Dwa `PopupControlExtender` formantów, które przypisać panelu do pól tekstowych

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrola została ustawiona. Tak, gdy użytkownik zaznaczy datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywana. W ramach tej metody należy pobrać i zapisywane w polu tekstowym bieżącą datę.

Poniżej przedstawiono składnię tego: obiekt przede wszystkim, serwer proxy dla `PopupControlExtender` na stronie musi zostać wygenerowany. Oferuje zestawie narzędzi programu ASP.NET AJAX kontroli `GetProxyForCurrentPopup()` metody. Ta metoda zwraca obiekt obsługuje `Commit()` metodę, która wysyła wartość do kontrolki, która wyzwoliła podręcznego (nie formantu wyzwalająca wywołanie metody!). Następujący kod stanowi wybranej daty jako argument dla `Commit()` metody powodują kod do zapisu zwrotnego wybrana data w polu tekstowym:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Teraz przy każdym kliknięciu na datę w kalendarzu, wybrana data jest wyświetlana w polu tekstowym skojarzone tworzenie formant wyboru daty, które aktualnie znajdują się na wiele witryn sieci Web.


[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

>[!div class="step-by-step"]
[Poprzednie](using-multiple-popup-controls-vb.md)
[dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
