---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Przy użyciu CascadingDropDown z bazy danych (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4fd5d2b28540f6e2f751da9fb9d0df5f9995c20b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Przy użyciu CascadingDropDown z bazy danych (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. Aby to zrobić można utworzyć usługi sieci web specjalnych.


## <a name="overview"></a>Omówienie

Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Aby to zrobić można utworzyć usługi sieci web specjalnych.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.

Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne. Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu &lt; `form` &gt; element):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

W następnym kroku wymagane są dwa formanty DropDownList. W tym przykładzie używamy dostawcy i skontaktuj się z informacji z AdventureWorks, dlatego utworzymy jedną listę dostępnych dostawców i jedną dla kontaktów, dostępne:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Następnie dwóch rozszerzeń CascadingDropDown musi być dodane do strony. Wstawia jedną z pierwszej listy (dostawcy), a innego wypełnia druga lista (kontaktów). Musi być ustawione następujące atrybuty:

- `ServicePath`: Adres URL usługi sieci web dostarczania pozycji na liście
- `ServiceMethod`: Dostarczanie pozycji na liście metody sieci web
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: Kategoria informacje przesyłane podczas wywoływania metody sieci web
- `PromptText`: Tekst wyświetlany, gdy asynchronicznie podczas ładowania danych listy z serwera
- `ParentControlID`: (opcjonalnie) nadrzędnej listy rozwijanej liście ładowania tego wyzwalaczy bieżącej listy

W zależności od języka programowania, używany zmienia nazwę danej usługi sieci web, ale wszystkie wartości atrybutów są takie same. Oto elementu CascadingDropDown dla pierwszej listy rozwijanej:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Należy ustawić rozszerzeń formantu druga lista `ParentControlID` atrybutu, aby wybranie wpis w wyzwalaczy listy dostawców ładowania skojarzonych elementów z listy kontaktów.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Rzeczywista praca odbywa się następnie usługi sieci web, który został ustawiony w następujący sposób. Należy pamiętać, że `[ScriptService]` atrybut jest używany, w przeciwnym razie ASP.NET AJAX nie można utworzyć serwera proxy JavaScript dostęp do metody sieci web z poziomu kodu skryptu po stronie klienta.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Podpis metody sieci web o nazwie przez CascadingDropDown wygląda następująco:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Dlatego zwracana wartość musi być tablicą typu `CascadingDropDownNameValue` którego jest definiowana za pomocą zestawu narzędzi kontroli. `GetVendors()` Metody jest bardzo łatwa do wdrożenia: kod nawiązuje połączenie z bazą danych AdventureWorks i wysyła zapytanie do dostawcy pierwsze 25. Pierwszy parametr w `CascadingDropDownNameValue` Konstruktor jest jego wartość podpis wpis listy, a drugi (wartość atrybutu w formacie HTML w &lt; `option` &gt; elementu). Oto kod:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Pobieranie skojarzone kontakty dla dostawcy (Nazwa metody: `GetContactsForVendor()`) jest nieco trudniejszy. Po pierwsze należy określić dostawcę, który został wybrany z pierwszej listy rozwijanej. Zestaw narzędzi do sterowania definiuje metodę pomocniczą dla tego zadania: `ParseKnownCategoryValuesString()` metoda zwraca `StringDictionary` element z listy rozwijanej danych:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Ze względów bezpieczeństwa danych musi najpierw zostać zweryfikowany. Tak, jeśli istnieje wpis dostawcy (ponieważ `Category` ma ustawioną właściwość pierwszego elementu CascadingDropDown `"Vendor"`), można pobrać identyfikator wybranego dostawcy:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Pozostałe metody jest dość proste, następnie. Identyfikator dostawcy jest używany jako parametr dla zapytania SQL, które pobiera wszystkie kontakty skojarzone dla tego dostawcy. Ponownie, metoda zwraca tablicę typu `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Ładowanie strony ASP.NET, a po chwili krótkich listy dostawców jest wypełniony 25 wpisów. Wybierz jedną pozycję i zwróć uwagę, jak drugi listy rozwijanej jest wypełniony danych.


[![Pierwsza lista jest wypełniane automatycznie](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Pierwsza lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Druga lista jest wypełniana zgodnie z wyborem z pierwszej listy](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Druga lista jest wypełniana zgodnie z wyborem z pierwszej listy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](filling-a-list-using-cascadingdropdown-vb.md)
> [dalej](presetting-list-entries-with-cascadingdropdown-vb.md)
