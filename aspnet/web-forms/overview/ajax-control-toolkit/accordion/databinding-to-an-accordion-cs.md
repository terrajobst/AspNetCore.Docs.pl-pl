---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: "Wiązania z danymi do Accordion (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a>Wiązania z danymi do Accordion (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w samej strony, ale powiązanie ze źródłem danych zapewnia większą elastyczność.


## <a name="overview"></a>Omówienie

Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w samej strony, ale powiązanie ze źródłem danych zapewnia większą elastyczność.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.

Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne. Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony. Aby skorzystać z ograniczoną ilość danych, możemy wybrać tylko pierwsze pięć wpisów w tabeli dostawcy bazy danych AdventureWorks. Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujący kod przedstawia prawidłowa składnia:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Zapamiętaj nazwę źródła danych (ID). Ten sam identyfikator musi być następnie używany w `DataSourceID` właściwości formantu Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

W formancie Accordion, możesz podać szablonów dla różnych części kontrolki, łącznie z nagłówkiem (`<HeaderTemplate>`) i zawartości (`<ContentTemplate>`). W ramach tych elementów, wyświetla dane z danych źródle, przy użyciu `DataBinder.Eval()` metody:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Podczas ładowania strony źródła danych musi być powiązana accordion o tym kodzie po stronie serwera:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Do podpisania tego przykładu, musisz zdefiniować dwa klas CSS, do których istnieją odwołania w formancie Accordion (w jego właściwościach `HeaderCssClass` i `ContentCssClass`). Umieść następujący kod w `<head>` sekcji strony:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Dane w accordion pochodzi bezpośrednio ze źródła danych](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Dane w accordion pochodzi bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-to-an-accordion-cs/_static/image3.png))

>[!div class="step-by-step"]
[Dalej](dynamically-adding-an-accordion-pane-cs.md)
