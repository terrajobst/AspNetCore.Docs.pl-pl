---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Przy użyciu TextBoxWatermark w FormView (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant TextBoxWatermark w zestawie narzędzi kontroli AJAX rozszerza pole tekstowe tak, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu go i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 5789ccc4b2c6385f3476857dce139a8b47c5e5e8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872536"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>Przy użyciu TextBoxWatermark w FormView (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Formant TextBoxWatermark w zestawie narzędzi kontroli AJAX rozszerza pole tekstowe tak, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, jest opróżniany. Jeśli użytkownik opuści pole bez wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. To jest również w formancie FormView.


## <a name="overview"></a>Omówienie

`TextBoxWatermark` Formantu w zestawie narzędzi kontroli AJAX rozszerza pola tekstowego, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, jest opróżniany. Jeśli użytkownik opuści pole bez wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Jest to możliwe w ramach również `FormView` formantu.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.

Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne. Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony, która obsługuje `DELETE`, `INSERT` i `UPDATE` instrukcji SQL. Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujący kod przedstawia prawidłowa składnia:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Zapamiętaj nazwę (`ID`) źródła danych, ponieważ będzie on używany w `DataSourceID` właściwość `FormView` formantu. `<InsertItemTemplate>` Sekcji `FormView` zawiera pole tekstowe, w którym zostanie przedłużony `TextBoxWatermarkExtender` formantu. Upewnij się, że rozszerzeń znajduje się w szablonie, a nie poza programem.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Teraz, gdy użytkownik zmieni w trybie wstawiania z `FormView` kontrolować, pola tekstu dla nowego dostawcy jest wstępnie podziękowania dla `TextBoxWatermarkExtender` formantu. Kliknij wewnątrz pola tekstowego umożliwia tekst wypełniacza zniknęły.


[![W polu znak wodny pochodzi z rozszerzeń](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Znak wodny w polu pochodzi z urządzenia extender ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-with-validation-controls-cs.md)
> [dalej](using-textboxwatermark-with-validation-controls-vb.md)
