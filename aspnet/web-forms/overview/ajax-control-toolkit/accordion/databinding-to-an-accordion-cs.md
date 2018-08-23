---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Powiązanie danych z kontrolką Accordion (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są deklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 930392bd33cfeb7dec52a6084a5d401a6134a7ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752892"
---
<a name="databinding-to-an-accordion-c"></a>Powiązanie danych z kontrolką Accordion (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale powiązania ze źródłem danych zapewnia większą elastyczność.


## <a name="overview"></a>Omówienie

Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale powiązania ze źródłem danych zapewnia większą elastyczność.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.

W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Następnie dodaj źródła danych do strony. Aby skorzystać z ograniczoną ilością danych, możemy wybrać tylko pięć pierwszych wpisów w tabeli Dostawca bazy danych AdventureWorks. Jeśli są przy użyciu Asystenta ustawień programu Visual Studio, aby utworzyć źródło danych, należy uwzględnić następujące kwestie usterkę w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) przy użyciu `Purchasing`. Następujący kod przedstawia poprawnej składni:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Zapamiętaj nazwę (ID) źródło danych. Ten identyfikator bardzo następnie musi być używany w `DataSourceID` właściwości formantu właściwości Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

W kontrolce właściwości Accordion zapewnieniem szablonów dla różnych części kontrolki, w tym nagłówku (`<HeaderTemplate>`) i zawartości (`<ContentTemplate>`). W ramach tych elementów, wyświetla dane z danych źródłowych, przy użyciu `DataBinder.Eval()` metody:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Po załadowaniu strony właściwości accordion przy użyciu tego kodu po stronie serwera musi być powiązana ze źródłem danych:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Zawierania, w tym przykładzie, należy zdefiniować dwie klasy CSS, które są określone w formancie właściwości Accordion (w jego właściwościach `HeaderCssClass` i `ContentCssClass`). Umieść następujący kod w `<head>` sekcji strony:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Dane w właściwości accordion pochodzą bezpośrednio ze źródła danych](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Dane w właściwości accordion pochodzą bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](dynamically-adding-an-accordion-pane-cs.md)
