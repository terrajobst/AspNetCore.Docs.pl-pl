---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Powiązanie danych z kontrolką Accordion (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są deklarowane w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: e21eeacf776111656eb539b1fef8203db1b761c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805891"
---
<a name="databinding-to-an-accordion-vb"></a>Powiązanie danych z kontrolką Accordion (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale powiązania ze źródłem danych zapewnia większą elastyczność.


## <a name="overview"></a>Omówienie

Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale powiązania ze źródłem danych zapewnia większą elastyczność.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.

W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

Następnie dodaj źródła danych do strony. Aby skorzystać z ograniczoną ilością danych, możemy wybrać tylko pięć pierwszych wpisów w tabeli Dostawca bazy danych AdventureWorks. Jeśli są przy użyciu Asystenta ustawień programu Visual Studio, aby utworzyć źródło danych, należy uwzględnić następujące kwestie usterkę w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) przy użyciu `Purchasing`. Następujący kod przedstawia poprawnej składni:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Zapamiętaj nazwę (ID) źródło danych. Ten identyfikator bardzo następnie musi być używany w `DataSourceID` właściwości formantu właściwości Accordion:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

W kontrolce właściwości Accordion zapewnieniem szablonów dla różnych części kontrolki, w tym nagłówku (`<HeaderTemplate>`) i zawartości (`<ContentTemplate>`). W ramach tych elementów, wyświetla dane z danych źródłowych, przy użyciu `DataBinder.Eval()` metody:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

Po załadowaniu strony właściwości accordion przy użyciu tego kodu po stronie serwera musi być powiązana ze źródłem danych:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Zawierania, w tym przykładzie, należy zdefiniować dwie klasy CSS, które są określone w formancie właściwości Accordion (w jego właściwościach `HeaderCssClass` i `ContentCssClass`). Umieść następujący kod w `<head>` sekcji strony:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Dane w właściwości accordion pochodzą bezpośrednio ze źródła danych](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Dane w właściwości accordion pochodzą bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-to-an-accordion-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](dynamically-adding-an-accordion-pane-cs.md)
> [dalej](dynamically-adding-an-accordion-pane-vb.md)
