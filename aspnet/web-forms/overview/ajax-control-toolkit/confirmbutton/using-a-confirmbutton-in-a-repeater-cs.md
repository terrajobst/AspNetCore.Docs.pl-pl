---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Przy użyciu ConfirmButton w powtarzanym (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie ConfirmButton w zestawie narzędzi kontroli AJAX tworzy tak/nie podręcznego, gdy użytkownik kliknie przycisk (w tym LinkButton kontroli). Jest tak tylko wtedy, gdy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a0717083a3c1e285f0c8c4cb8503d2bbe42153b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>Przy użyciu ConfirmButton w powtarzanym (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> Rozszerzenie ConfirmButton w zestawie narzędzi kontroli AJAX tworzy tak/nie podręcznego, gdy użytkownik kliknie przycisk (w tym LinkButton kontroli). Tylko tak po kliknięciu przycisku akcji jest wykonywane, w przeciwnym razie anulowane. To jest również w elementu powtarzanego.


## <a name="overview"></a>Omówienie

Rozszerzenie ConfirmButton w zestawie narzędzi kontroli AJAX tworzy tak/nie podręcznego, gdy użytkownik kliknie przycisk (w tym LinkButton kontroli). Tylko tak po kliknięciu przycisku akcji jest wykonywane, w przeciwnym razie anulowane. To jest również w elementu powtarzanego.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.

Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne. Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Następnie źródła danych jest wymagana. Dla uproszczenia są pobierane tylko pierwsze pięć wpisów w tabeli dostawców AdventureWorks. Należy pamiętać, że podczas korzystania z Kreatora programu Visual Studio, aby utworzyć źródło danych, a nazwy tabeli (`Vendors`) aktualnie nie jest poprawnie poprzedzona z `Purchasing`. Następujący kod jest poprawny:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Następnie można tego źródła danych w ramach elementu powtarzanego. Jak zwykle `DataBinder.Eval()` metoda pobiera dane ze źródła danych. `ConfirmButtonExtender` Kontroli następnie musi się znajdować w `<ItemTemplate>` sekcji powtarzanego, że jest wyświetlany dla każdego wpisu w źródle danych.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![Przycisk Potwierdź widoczna obok każdego wpisu ze źródła danych](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

Przycisk Potwierdź widoczna obok każdego wpisu ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-a-confirmbutton-in-a-repeater-vb.md)
