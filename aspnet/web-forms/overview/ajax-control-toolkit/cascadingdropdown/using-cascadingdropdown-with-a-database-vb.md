---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Używanie kontrolki CascadingDropDown z bazą danych (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: b90fc8975b6158da838ad6449537a27847650376
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386737"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Używanie kontrolki CascadingDropDown z bazą danych (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. Aby to zrobić można utworzyć usługi sieci web specjalne.


## <a name="overview"></a>Omówienie

Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. (Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Aby to zrobić można utworzyć usługi sieci web specjalne.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.

W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu &lt; `form` &gt; elementu):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

W następnym kroku dwóch kontrolek DropDownList są wymagane. W tym przykładzie używamy dostawcy i skontaktuj się z informacji z AdventureWorks, dlatego utworzymy jedną listę dla dostępnych dostawców i jeden dla dostępnych kontaktów:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Następnie należy dodać dwa rozszerzeń kontrolki CascadingDropDown do strony. Wstawia jedną z pierwszej listy (dostawcami), a druga wypełnia na drugiej liście (kontakty). Należy określić następujące atrybuty:

- `ServicePath`: Adres URL usługi sieci web, zapewniając pozycji na liście
- `ServiceMethod`: Metoda sieci web dostarczanie pozycji na liście
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: Informacje kategorii jest przesyłany do usługi po wywołaniu metody sieci web
- `PromptText`: Tekstu wyświetlanego, gdy asynchronicznie ładowania listy danych z serwera
- `ParentControlID`: (opcjonalnie) tego ładowanie wyzwalaczy bieżącą listę liście rozwijanej nadrzędnego

W zależności od języka programowania, używany zmienia nazwę danej usługi sieci web, ale inne wartości atrybutów są takie same. Oto elementu kontrolki CascadingDropDown z pierwszej listy rozwijanej:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Kontrolek na drugiej liście, należy ustawić `ParentControlID` atrybutu, tak aby zaznaczając daną pozycję w wyzwala listy dostawców załadowanie skojarzonych elementów na liście kontaktów.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Następnie rzeczywista praca odbywa się w usłudze sieci web jest skonfigurowane w następujący sposób. Należy pamiętać, że `[ScriptService]` atrybut jest używany, w przeciwnym razie ASP.NET AJAX, nie można utworzyć serwera proxy JavaScript dostęp do metody sieci web z poziomu kodu skryptu po stronie klienta.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Podpis metody sieci web o nazwie przez kontrolki CascadingDropDown jest następująca:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Dlatego zwracana wartość musi być tablicą typu `CascadingDropDownNameValue` który jest definiowany przez zestaw narzędzi kontroli. `GetVendors()` Metoda jest dość łatwe do wdrożenia: kod nawiązanie połączenia z bazy danych AdventureWorks i zapytanie pierwsze 25 dostawców. Pierwszy parametr w `CascadingDropDownNameValue` Konstruktor jest podpis wpisu listy, a druga wartość (wartość atrybutu w formacie HTML w &lt; `option` &gt; elementu). Oto kod:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Wprowadzenie skojarzone kontakty dla dostawcy (Nazwa metody: `GetContactsForVendor()`) jest nieco trudniejszy. Po pierwsze można określić dostawcę, który został wybrany z pierwszej listy rozwijanej. Zestaw narzędzi kontroli definiuje metodę pomocniczą dla tego zadania: `ParseKnownCategoryValuesString()` metoda zwraca `StringDictionary` element z listy rozwijanej danych:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Ze względów bezpieczeństwa te dane muszą najpierw zweryfikowana. Zatem jeśli zapis dostawcy (ponieważ `Category` pierwszego elementu kontrolki CascadingDropDown zostaje ustalona `"Vendor"`), można pobrać Identyfikatora wybranego dostawcy:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Pozostała część metody jest dość proste, następnie. Identyfikator dostawcy jest używany jako parametr dla zapytania SQL, który pobiera wszystkie skojarzone kontakty dla tego dostawcy. Jeszcze raz, metoda zwraca tablicę typu `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Ładowanie strony ASP.NET, a po krótkiej chwili lista dostawców jest wypełniany 25 wpisów. Wybierz jeden wpis i zwróć uwagę, jak na drugiej liście rozwijanej jest wypełniany danymi.


[![Pierwsza lista jest wypełniana automatycznie](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Pierwsza lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Druga lista jest wypełniana zgodnie z wyborem na pierwszej liście](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Druga lista jest wypełniana zgodnie z wyborem na pierwszej liście ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](filling-a-list-using-cascadingdropdown-vb.md)
> [dalej](presetting-list-entries-with-cascadingdropdown-vb.md)
