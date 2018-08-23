---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Używanie kontrolki ConfirmButton w elemencie powtarzanym (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie ConfirmButton w zestawu narzędzi AJAX Control Toolkit tworzy tak/nie okna podręcznego, gdy użytkownik kliknie przycisk (w tym kontroli LinkButton). Tylko wtedy, gdy tak się...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 38ac40776c2fdd14af046b5e13df0701c71a4ad0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756980"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Używanie kontrolki ConfirmButton w elemencie powtarzanym (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Rozszerzenie ConfirmButton w zestawu narzędzi AJAX Control Toolkit tworzy tak/nie okna podręcznego, gdy użytkownik kliknie przycisk (w tym kontroli LinkButton). Tylko tak, po kliknięciu przycisku akcja jest wykonywana, w przeciwnym razie anulowane. Ponadto jest to możliwe w elemencie powtarzanym.


## <a name="overview"></a>Omówienie

Rozszerzenie ConfirmButton w zestawu narzędzi AJAX Control Toolkit tworzy tak/nie okna podręcznego, gdy użytkownik kliknie przycisk (w tym kontroli LinkButton). Tylko tak, po kliknięciu przycisku akcja jest wykonywana, w przeciwnym razie anulowane. Ponadto jest to możliwe w elemencie powtarzanym.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.

W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Następnie źródło danych jest wymagana. Dla uproszczenia są pobierane tylko pięć pierwszych wpisów w tabeli dostawców AdventureWorks. Należy pamiętać, że podczas korzystania z Kreatora programu Visual Studio, aby utworzyć źródło danych, nazwę tabeli (`Vendors`) aktualnie nie jest poprawnie poprzedzona prefiksem `Purchasing`. Następujące znaczniki jest prawidłowa:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Następnie można używać tego źródła danych w elemencie powtarzanym. Jak zwykle `DataBinder.Eval()` metoda pobiera dane ze źródła danych. `ConfirmButtonExtender` Kontrolki musi zostać następnie umieszczony w obrębie `<ItemTemplate>` części elementu powtarzanego, tak aby pojawił się dla każdego wpisu w źródle danych.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Przycisk Potwierdź pojawia się obok każdego wpisu, ze źródła danych](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Przycisk Potwierdź pojawia się obok każdego wpisu, ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-a-confirmbutton-in-a-repeater-cs.md)
