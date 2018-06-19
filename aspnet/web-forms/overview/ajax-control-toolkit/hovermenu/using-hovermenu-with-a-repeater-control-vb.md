---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Przy użyciu HoverMenu o kontrolce elementu powtarzanego (VB) | Dokumentacja firmy Microsoft
author: wenz
description: 'Kontrola HoverMenu w zestawie narzędzi kontroli AJAX zapewnia efekt proste menu podręczne: gdy wskaźnik myszy jest przesuwany nad elementem, wyskakującego w specifi...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aa107a7483683d965f3a510e6a43f174a386da0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868402"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Przy użyciu HoverMenu o kontrolce elementu powtarzanego (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Kontrola HoverMenu w zestawie narzędzi kontroli AJAX zapewnia efekt proste menu podręczne: gdy wskaźnik myszy jest przesuwany nad elementem, wyskakującego na określonej pozycji. Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.


## <a name="overview"></a>Omówienie

`HoverMenu` Formantu w zestawie narzędzi kontroli AJAX zapewnia efekt proste menu podręczne: gdy wskaźnik myszy jest przesuwany nad elementem, wyskakującego na określonej pozycji. Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.

Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne. Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony. Aby skorzystać z ograniczoną ilość danych, możemy wybrać tylko pierwsze pięć wpisów w tabeli dostawcy bazy danych AdventureWorks. Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujący kod przedstawia prawidłowa składnia:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Następnie dodaj panel, która służy jako modalnych menu podręczne:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Teraz `HoverMenuExtender` wejścia play. Tak, aby każdy element w źródle danych pobiera własną menu podręczne, rozszerzający musi znajdować się w elemencie powtarzanym `<ItemTemplate>` sekcji. Oto kod znaczników:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Teraz każdy element w źródle danych wyświetla okna podręcznego w prawo (`PopupPosition` atrybut) z opóźnieniem 50 milisekund (`PopDelay` atrybutu).


[![Zostanie wyświetlone hover menu obok każdego elementu w elemencie powtarzanym](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Zostanie wyświetlone hover menu obok każdego elementu w elemencie powtarzanym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-hovermenu-with-a-repeater-control-cs.md)
